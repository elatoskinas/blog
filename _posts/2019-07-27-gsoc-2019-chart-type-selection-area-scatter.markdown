---
layout: posts
title:  "App Inventor Chart Components: Chart Type Selection, Area & Scatter Charts"
date:   2019-07-27 23:34:00 +0200
category: Open Source
tags: open-source gsoc appinventor project
---

## Overview
Following up on the [App Inventor][appinventor] [Chart components project]({{ site.baseurl }}{% post_url 2019-06-01-gsoc-2019-first-steps %}),
this blog post will focus on a specific workflow aspect that allows to select the Chart type dynamically.

A design decision was accepted to allow to select the Chart type on the fly, rather than having to use a different
component for a different Chart type each time. This effectively means that there is a single Chart component in
the entire application, but the component itself holds all the possible Chart type options.

## Component Design
While the major focus is implementing a Chart type selection system, in fact the toughest part was adapting and
finalizing the final component design that would be fit for this task. Following up on
[one of the previous posts]({{ site.baseurl }}{% post_url 2019-06-15-gsoc-2019-charts-workflow-implementation %}), I have decided to make
an extension that would be more adaptable to this specific case.

<figure>
    <a href="{{ site.baseurl }}/assets/images/posts/gsoc2019/design/ChartDataComponentsDesign.png"><img src="{{ site.baseurl }}/assets/images/posts/gsoc2019/design/ChartDataComponentsDesign.png" alt="Chart Component Design"></a>
</figure>

### Designer Component layer
The designer component layer is the public interface visible to the user. This comprises of the Chart
component (with the type property to select a Chart), and the attachable Data components. This has not
changed much from the previous design, apart from these components now being considered part of one
coherent layer.

### Functionality layer
The bottom part is the functionality layer. The bulk of the GUI and data logic lives here,
and this is not visible to the user, but rather an internal part of the components.

#### Chart View
The Chart component directly defines the Chart View type, since the type property is
selected via the component itself. The View objects are made with the intention to form
a hierarchy, and ultimately, a subclass should exist for each possible Chart type (Line,
Bar, Pie, etc.)

Logic wise, the view is responsible for initializing the appropriate Chart view, as well
as pass on any Chart-related logic (which mostly comprises of Chart GUI functions)
from the Chart component.

Finally, the Data Model is instantiated by the View, since the type of the Data model required
is defined by the type of the Chart (e.g. Pie Charts have different data types than Line Charts)

This is the most important addition of the design (iterated form the previous design), since
it allows to abstract the Chart component away from the underlying view, effectively separating
the concerns of the library implementation and App Inventor's needs. This also allows to be flexible
when introducing new Chart types.

#### Chart Data Model
In terms of the Data Model, it pretty much remains the same as in the previously discussed design (in
the post mentioned at the beginning of the Component Design section). The Data Model handles all the
data logic, and most functionality is passed to this Data Model from the Data components.

## Type Property
With the Chart component design finalized, the next step was to introduce the Type property, which looks as follows:

<figure>
    <a href="{{ site.baseurl }}/assets/images/posts/gsoc2019/preview/workflow/ChartTypeSelection.png"><img src="{{ site.baseurl }}/assets/images/posts/gsoc2019/preview/workflow/ChartTypeSelection.png" alt="Chart Type Selection dropdown menu"></a>
</figure>

### Workflow
The underlying workflow of the Type property is a bit more complex than it might appear at first. What actually happens is the following:
1. The type property dropdown is opened, and the Chart type is selected.
2. The old Chart view is removed from the Designer window
3. A new Chart view is initialized, which corresponds to the new Chart type
4. The Data Components are re-attached to the new Chart view
5. The properties for the new Chart view are set accordingly as they were on the previous Chart view (e.g. background color)

To the user, it looks as follows:
<figure>
    <a href="{{ site.baseurl }}/assets/images/posts/gsoc2019/preview/workflow/ChartTypeSelectionPreview.gif"><img src="{{ site.baseurl }}/assets/images/posts/gsoc2019/preview/workflow/ChartTypeSelectionPreview.gif" alt="Chart Type Selection preview animation"></a>
</figure>

#### Code snippet
The snippets of the code for the Designer Chart component in terms of the type property change is as follows:

{% highlight java %}
    /**
     * Sets the type of the Chart to the newly specified value.
     * @param value  new Chart type
     */
    private void setTypeProperty(String value) {
        // Update type
        type = Integer.parseInt(value);

        // Keep track whether this is the first time that
        // the Chart view is being initialized
        boolean chartViewExists = (chartView != null);

        // Remove the current Chart Widget from the root panel (if present)
        if (chartViewExists) {
            rootPanel.remove(chartView.getChartWidget());
        }

        // Create a new Chart view based on the supplied type
        chartView = createMockChartViewFromType(type);

        // Add the Chart Widget to the Root Panel (as the first widget)
        rootPanel.insert(chartView.getChartWidget(), 0);

        // Chart view already existed before, so the new Chart view must
        // be reinitialized.
        if (chartViewExists) {
            reinitializeChart();
        }
    }

    /**
     * Creates and returns a new MockChartView object based on the type
     * (integer) provided
     * @param type  Chart type (integer representation)
     * @return new MockChartView object instance
     */
    private MockChartView createMockChartViewFromType(int type) {
        switch(type) {
            case ComponentConstants.CHART_TYPE_LINE:
                return new MockLineChartView();
            case ComponentConstants.CHART_TYPE_SCATTER:
                return new MockScatterChartView();
            case ComponentConstants.CHART_TYPE_AREA:
                return new MockAreaChartView();
            // ...
            default:
                // Invalid argument
                throw new IllegalArgumentException("type:" + type);
        }
    }
{% endhighlight %}

The setTypeProperty method is invoked when the property is changed. Since all properties are passed in as Strings,
the value is parsed, the old Chart view is removed (if it exists), the new one is created and added, and the
Chart is reinitialized.

With the help of the Chart View abstraction, this became a far more simple task than it would be otherwise, since
we do not have to worry about handling the View itself!

## Area & Scatter Charts
Before the Type property initialization, I had decided to work only on the Line Chart to focus the concepts on
one single Chart type to not overcomplicate matters.

However, with the component design finalized, I figured it would be a good idea to invest into implementing
more Chart types, which would also allow generalizing the code a bit more to support other Chart types.

Since Area and Scatter Charts are very close to Line Charts (they share the same data type), I decided to
first go for these two Chart types.

### Model & View hierarchy
To incorporate Area and Scatter Charts, I came up with the following hierarchy (orange & dashed boxes
represent abstract classes):

<figure>
    <a href="{{ site.baseurl }}/assets/images/posts/gsoc2019/design/ChartHierarchyPointCharts.png"><img src="{{ site.baseurl }}/assets/images/posts/gsoc2019/design/ChartHierarchyPointCharts.png" alt="Point Chart Hierarchy diagram"></a>
</figure>

A few observations:
* The Point Chart abstraction was made due to Scatter Charts and Line-based Charts sharing a lot of functionality.
The data accepted by the Charts is essentially the same, so it would otherwise result in quite a bit of redundancy.
* Line and Area Charts are essentially the same in both the Desigenr and Android chosen libraries. The only difference
is the Area Chart has the fill property enabled, and the fill colors are handled there. However, for simplicity to the
users, this was added as an additional type.
* The design is also made to be extendible for the future Charts. The Point Chart was also made primarily for this reason,
since Charts like the Pie Chart will not share a lot of functionality with the Point-based Charts.
* The View & Model hierarchies are the same.


### Model creation in views
Recall that the Chart Data component would get a Chart Data Model reference as follows:
{% highlight java %}
    // Creates a ChartDataModel based on the current
    // Chart type being used.
    chartDataModel = chart.createChartModel();
{% endhighlight %}

To extend on the previous point of the Chart Model creation being handled by the views, here are a few examples of
how the Model is created in the Views themselves:
{% highlight java %}
public class ScatterChartView extends PointChartView<ScatterChart, ScatterData> {

  // ...

  @Override
  public ChartDataModel createChartModel() {
    return new ScatterChartDataModel(data);
  }
}

public class LineChartView extends LineChartViewBase {
    // ...

    @Override
    public ChartDataModel createChartModel() {
        return new LineChartDataModel(data);
    }
}
{% endhighlight %}

By defining the View and Model classes together, the library functionality is essentially abstracted away from the user-controlled
components themselves, making the code more modular, extendible and separated.

### Chart previews
Here is a preview of the Charts in both Android and the Designer (recall that different libraries are used,
and some alterations have been made to make them look similar):

<figure>
    <a href="{{ site.baseurl }}/assets/images/posts/gsoc2019/preview/views/PointCharts.png"><img src="{{ site.baseurl }}/assets/images/posts/gsoc2019/preview/views/PointCharts.png" alt="Point Chart previews (Android & Designer)"></a>
</figure>

For the curiuos readers, the main pull requests related to these features can be found here:
* [Component Redesign][pr-charts-redesign]
* [Area Chart][pr-area-chart]
* [Scatter Chart][pr-scatter-chart]

## Stay tuned for more!
It has been a while since the creation of this post. I have been keeping quite busy with the project, but do expect updates on the project soon!
The next post will most likely follow up on the Data importing methods, since that has been the focus of the last few weeks. Stay tuned!


[appinventor]: https://appinventor.mit.edu/explore/
[pr-charts-redesign]: https://github.com/lightingft/appinventor-sources/pull/78
[pr-area-chart]: https://github.com/lightingft/appinventor-sources/pull/86
[pr-scatter-chart]: https://github.com/lightingft/appinventor-sources/pull/101