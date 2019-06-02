---
layout: posts
title:  "App Inventor Chart Components: Prototype"
date:   2019-06-02 01:47:00 +0200
category: Open Source
tags: open-source gsoc appinventor project
---

## Overview
In continuation to the previous [previous blog post]({% post_url 2019-06-01-gsoc-2019-first-steps %}), this post will focus on a Line Chart Component
prototype in [App Inventor][appinventor].

The goal of prototyping in this case is to first determine whether the chosen approach is the right way to go, or whether revisions are needed to the concepts
and design itself. For this reason, a few prototypes have been devised. From the previous post, two different general models for the components were presented --
one where the Chart component holds both the view & the data, and the other where the view & the data are divided into two components, one being the Chart component,
and the other being the Data component. The prototypes focus on these two models.

## Features
Since the aim is to develop some simple prototypes, the Chart is not really intricate with features. A few features have been added for example purposes. The following
criteria are satisfied in the prototypes:

* (x, y) entries can be added to the Chart via an App Inventor block
* The background color of the Chart can be changed
* The description label of the Chart can be set
* The Chart's height and width can be changed
* A responsive UI component is present in the Designer window

## Differences
With these features in mind, let's first look at the differences between the two prototypes.

The left images correspond to the 2 Component model, and the right images correspond to the 1 Component model.

<figure class="half">
    <a href="/assets/images/posts/gsoc2019/preview/prototype/CategoryChartandData.png"><img src="/assets/images/posts/gsoc2019/preview/prototype/CategoryChartandData.png"></a>
    <a href="/assets/images/posts/gsoc2019/preview/prototype/CategoryChartOnly.png"><img src="/assets/images/posts/gsoc2019/preview/prototype/CategoryChartOnly.png"></a>
    <figcaption>Difference in Component selection</figcaption>
</figure>

<figure class="half">
    <a href="/assets/images/posts/gsoc2019/preview/prototype/LineChartProperties.png"><img src="/assets/images/posts/gsoc2019/preview/prototype/LineChartProperties.png"></a>
    <a href="/assets/images/posts/gsoc2019/preview/prototype/LineChartNoDataProperties.png"><img src="/assets/images/posts/gsoc2019/preview/prototype/LineChartNoDataProperties.png"></a>
    <figcaption>Difference in Properties (note the ChartData property)</figcaption>
</figure>

<figure class="half">
    <a href="/assets/images/posts/gsoc2019/preview/prototype/EntryAddingBlockData.png"><img src="/assets/images/posts/gsoc2019/preview/prototype/EntryAddingBlockData.png"></a>
    <a href="/assets/images/posts/gsoc2019/preview/prototype/EntryAddingBlockChart.png"><img src="/assets/images/posts/gsoc2019/preview/prototype/EntryAddingBlockChart.png"></a>
    <figcaption>Difference in blocks (note that the AddEntry block is on the Chart on the right)</figcaption>
</figure>

## 2 Component Model Implementation
With these differences in mind, the implementation itself does not differ that greatly. Since the 2 component model is a bit more intricate, this post will focus on this
model's implementation. The entire implementation will not be covered, and only some important key aspects will be detailed here. For the interested readers, see the
[pull request][pr-basic-charts] in my repository for the full changes.

The Android library used for the Charts is [MPAndroidChart][mpandroidchart].

### Visible & Non-visible Components
Since this model aims at separating the data from the view, the view is the component that is visible, and the data is non-visible
in the GUI.

It is a standard in App Inventor's component class hierarchy to provide AndroidVisibleComponent and AndroidNonVisibleComponent
classes. Thus, the Chart should be an extension from the AndroidVisibleComponent class, and the Data should be an extension form the
AndroidNonVisibleComponent class.

### Abstractions
Now, let's take a look at the abstract classes for the Chart and the Data components.

{% highlight java %}
public abstract class ChartDataBase<T extends ChartData> extends AndroidNonvisibleComponent {

    protected T chartData;
{% endhighlight %}

{% highlight java %}
public abstract class ChartBase<T extends Chart, D extends ChartDataBase> extends AndroidViewComponent {

    protected T view;
    protected D data;
{% endhighlight %}

Note that the object types *Chart* and *ChartData* are classes of MPAndroidChart. The *ChartDataBase* in *ChartBase* is the *ChartDataBase* that
we define, however!

The idea is to make use of generics to allow us to treat the objects in Chart implementations as the actual needed objects. For instance,
take a look at the signature of the LineChart component class:

{% highlight java %}
public final class LineChart extends ChartBase<com.github.mikephil.charting.charts.LineChart, LineChartData>
{% endhighlight %}

The 'T' becomes the LineChart class of the MPAndroidChart, and the 'D' becomes the LineChartData component that is an extension of ChartDataBase.

With this structure, we have a convenient way of representing the hierarchy in the implementation.

### Adding Entries
Let us now move on to adding the entries. In this model, all the data operations should be moved to the Data components. For now, the decision
was to store all the logic in LineChartData, and do proper abstractions later on, when more design decisions have been accepted (especially on
the other data set types).

In LineChartData, the following method has been added:
{% highlight java %}
    /**
     * Adds entry to the Line Data Series
     *
     * @param x - x value of entry
     * @param y - y value of entry
     */
    @SimpleFunction(description = "Adds (x, y) point to the Line Data.")
    public void AddEntry(int x, int y) {
        Entry entry = new Entry(x, y);

        if (chartData.getDataSetCount() == 0) {
            LineDataSet dataSet = new LineDataSet(new ArrayList<Entry>(), "Data");
            dataSet.setColor(Color.BLACK);
            dataSet.setCircleColor(Color.BLACK);
            dataSet.addEntry(entry);
            chartData.addDataSet(dataSet);
        } else {
            chartData.getDataSetByIndex(0).addEntryOrdered(entry);
            chartData.notifyDataChanged();
        }

        refreshCharts();
    }
{% endhighlight %}

Since this is a prototype, right now the aim is to have at most one data set (hence the if statement in the code).

Initially, an entry using the x and y values is constructed. Then, an if statement is done to check if an entry already exists
(if it does not, then there is no data set). If the entry does not exist, a new data set is created, the first entry is added,
and that dataset is added to the chartData object instance.

If a dataset already exists, the entry is simply added to the chartData. Note the *addEntryOrdered* call here. If we do not use entry
ordered, then adding lower x values will result in the Line Chart going backwards, which is not what we want. This method version
guarantees that the entries will be sorted after insertion, and thus the Line Chart is not broken. Then we notify that the chart
data has been changed to refresh the Chart.

The final line refreshes the Charts, and we will take a look at this in a few sections.

### Chart and ChartData linking
Charts are linked with the ChartData by having a ChartData property. ChartData objects can then be attached to Charts.

Due to ChartData concrete types depending on the Chart type, the method to set the property has to be written in the implemented
classes due to issues with dynamic types when compiling the code.

As such, we have the following method to add a LineChartData component to a LineChart:
{% highlight java %}
    @SimpleProperty(category = PropertyCategory.BEHAVIOR,
            description = "")
    @DesignerProperty(editorType = PropertyTypeConstants.PROPERTY_TYPE_COMPONENT + ":com.google.appinventor.components.runtime.LineChartData")
    public void ChartData(LineChartData data) {
        // Remove this Chart from previous LineChartData component
        if (data != null) {
            data.removeChart(this);
        }

        // Add Chart to LineChartData component
        data.addChart(this);

        this.data = data;
        view.setData(this.data.getChartData());
        view.invalidate();
    }
{% endhighlight %}

The *removeChart* and *addChart* methods will be covered in the next section.

What interests us here is the setData and invalidate methods. setData sets the *ChartData* object (which is part of MPAndroidChart)
to the Chart, thus effectively loading the data, and invalidate refreshes the Chart to display the newly added data.

### Chart Refreshing
One problem that arises when using the library and implementing this model is the need to refresh the Chart after adding the data.
However, in order to do so, access to the actual Chart view is needed, but the Data Set is decoupled from the Chart.

The approach is then to create Chart component references from the Data component in order to refresh the Charts.

The following method was written in ChartBase:
{% highlight java %}
    /**
     * Refreshes the Chart to react to Data Set changes.
     */
    public void Refresh() {
        view.notifyDataSetChanged();
        view.invalidate();
    }
{% endhighlight %}


The method is made public so the Data Set component can call it. Now let's look at the relevant code in ChartDataBase:
{% highlight java %}
    protected HashSet<ChartBase> charts;

    /**
     * Add Chart component to observe by this Chart Data component.
     *
     * @param chart  Chart component
     */
    public void addChart(ChartBase chart) {
        charts.add(chart);
    }

    /**
     * Refreshes all the Charts that use this Chart Data component.
     * Called whenever there are changes to the underlying Chart data object.
     */
    protected void refreshCharts() {
        for (ChartBase chart : charts) {
            if (chart != null) {
                chart.Refresh();
            }
        }
{% endhighlight %}

The implementation is rather simple. We simply keep a HashSet containing ChartBase classes. There is a method to add a new Chart
to be observed by the Data component, and there is a method to refresh all the observed Charts, so that their data is updated in
the view.

A removeChart method was also later added to remove Charts that no longer use the data set:
{% highlight java %}
    /**
     * Removes a Chart Component to be observed by this Chart Data component
     *
     * @param chart  Chart component
     */
    public void removeChart(ChartBase chart) {
        charts.remove(chart);
    }
{% endhighlight %}

## Designer UI Component
The final step of the simple prototype version is to implement a responsive UI component.

[Charba][charba] was chosen as the library for the implementation of the component, which is a
[GWT][gwt] wrapper library of the well-known [Chart.js][chart-js] charting library.

The component is still in an early stage, and is mainly intended for presentation purposes, so
details of the implementation will not be covered here. For the curious readers, the code
changes are available on a [pull request][pr-mock-basic-charts].

The basic idea is to initialize the Line Chart component from the library, add placeholder data,
style it to look similar to the MPAndroidChart representation (although styling was not given 
priority right now, more can be done on that part) and add functionality to respond to changes.

The end result is summarized in this gif:
![Mock Component Preview](/assets/images/posts/gsoc2019/preview/MockChartCharbaPreview.gif)

**Stay tuned for more!**

[appinventor]: https://appinventor.mit.edu/explore/
[mpandroidchart]: https://github.com/PhilJay/MPAndroidChart
[pr-basic-charts]: https://github.com/lightingft/appinventor-sources/pull/18
[pr-mock-basic-charts]: https://github.com/lightingft/appinventor-sources/pull/23
[charba]: https://github.com/pepstock-org/Charba
[gwt]: http://www.gwtproject.org/
[chart-js]: https://www.chartjs.org/