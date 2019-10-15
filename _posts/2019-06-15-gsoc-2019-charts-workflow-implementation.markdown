---
layout: posts
title:  "App Inventor Chart Components: Workflow Implementation"
date:   2019-06-15 22:27:00 +0200
category: Open Source
tags: open-source gsoc appinventor project
---

## Overview
In the last post, I have previewed the [workflow]({{ site.baseurl }}{% post_url 2019-06-10-gsoc-2019-workflow-preview %}) of the Charts components that I am working on
for [App Inventor][appinventor]. In this post, I will dive into some implementation details and design choices.

For the curious readers, the code changes revolving this post are available in a [pull request][charts-data-core-pr].

## Core Issue
The toughest aspect of designing the Chart components is making the workflow intuitive and providing easy and good usability to the users.
The main issue that I encountered while developing the core features is adapting to the defined specifications of presenting the components
to the users.

One design choice that was made is to have Chart and Data components separately, as the reader may recall from the previous posts.
Following up on the design choice, the next decision made was to have Data components be quite broad and applicable to many different
types of Charts. So, for example, a Data component for x and y coordinates should  be applicable to all coordinate-based Charts.

Now, this causes some problems, because internally, the Charting libraries used ([MPAndroidChart][mpandroidchart] and [Chart.js][chartjs])
have one class type per Chart to represent the Data. Although there are some abstractions in between and both libraries have their
hierarchies, there are certain properties that are only available in certain Data classes, and the libraries are not exact
in their implementation. Since one of the goals is to also provide some customization options for the
Data components, and some data adding needs to be treated differently, this essentially means that there needs to be some way
to abstract the library classes from the Data component itself.

The initial simplistic approach initially would have been having Data components for Charts (e.g. Line Data component for Line Chart),
however, with the decision that was described being made, this was no longer an option.

## Separating Data functionality
Initially, the key idea of the separation of the components (Chart and Data) was that one component would handle the view, and
the other one would handle the data.

My idea was to extend upon this concept. Since we cannot put the Data instances coming from the libraries directly into the Data component,
I have thought of making the Data component simply a **controller** that would handle user requests and handle linking the view with the
data, while also updating the view upon property changes.

One might notice that this is starting to kind of look like the [Model View Controller][mvc] pattern. That is indeed quite the aim, although
a looser implementation on the pattern was aimed for to adapt for various needs of the project.

Now we have the controller. The Chart component would, of course, serve as the **view**, as described earlier. The functionality involved in
the Chart component class should only be related to the Chart GUI functionality itself, and generally not contain any data-related logic, although
time might tell that the class might be responsible for a bit more than that.

And now comes the important bit on how I decided to fix the mentioned core issue. The idea is to define **model** classes, which would hold the
underlying data structure of the library Data classes, and be responsible for all the Data logic. Essentially, this makes it so that our controller
(the Data component class) does not even have to care on what type of concrete Data it is operating. All it does is make the calls to the model, and
the model handles it from there.

The following diagram summarizes the interactions and functionality of the classes:
<figure>
    <a href="{{ site.baseurl }}/assets/images/posts/gsoc2019/design/ChartComponentLinkingWorkflow.png"><img src="{{ site.baseurl }}/assets/images/posts/gsoc2019/design/ChartComponentLinkingWorkflow.png" alt="Chart Component Linking Workflow"></a>
</figure>


## Implementation of the classes
Let us now take a look at how the classes are represented in the code. To adapt to the design decision, the [prototype]({{ site.baseurl }}{% post_url 2019-06-02-gsoc-2019-prototype %})
implementation has been refactored, and of course, LineData is no longer a component (due to reasons mentioned in a previous section)

### Chart Component (View)

{% highlight java %}
public abstract class ChartBase<T extends Chart, D extends ChartData> extends AndroidViewComponent implements ComponentContainer {

    protected T view;
    protected D data;

    // ...

    /**
     * Creates a new Chart Model object instance.
     *
     * @return  Chart Model instance
     */
    public abstract ChartModel createChartModel();
{% endhighlight %}

{% highlight java %}
public final class LineChart extends ChartBase<com.github.mikephil.charting.charts.LineChart, LineData> {

    /**
     * Creates a new Line Chart component.
     *
     * @param container container, component will be placed in
     */
    public LineChart(ComponentContainer container) {
        super(container);

        view = new com.github.mikephil.charting.charts.LineChart(container.$context());
        data = new LineData();
        view.setData(data);

        // ...
    }

    @Override
    public LineChartModel createChartModel() {
        return new LineChartModel(data);
    }
  }
{% endhighlight %}

The Chart components themselves are quite simple. First off, the ChartBase component has two generics -- one for
the Chart and one for the Data, both of which come from the library.

The LineChart constructor handles creating the view, creating the data and assigning the data to the view.

The part that might be a bit confusing here is the createChartModel. But in fact, it is pretty straight forward.
Since each Chart has its corresponding ChartModel class, we put the method in the Chart component class, and
then it can be called by the controller to instantiate the proper Chart Model class instance.

We pass in the data instance to the ChartModel because then we have the same data component referenced in the
ChartModel, allowing manipulation of data from a class that is not the Chart component, essentially allowing
us to put the data logic into the Chart Model class.


### Data Component (controller)

{% highlight java %}
@SimpleObject
public abstract class ChartDataBase implements Component {
    protected ChartBase container;
    protected ChartModel chartModel;

    private String label;
    private int color;

    /**
     * Creates a new Chart Data component.
     */
    protected ChartDataBase(ChartBase chartContainer) {
        this.container = chartContainer;

        chartModel = chartContainer.createChartModel();

        // Set default values
        Color(Component.COLOR_BLACK);
        Label("");
    }

    /**
     * Specifies the data series color as an alpha-red-green-blue integer.
     *
     * @param argb  background RGB color with alpha
     */
    @DesignerProperty(editorType = PropertyTypeConstants.PROPERTY_TYPE_COLOR,
            defaultValue = Component.DEFAULT_VALUE_COLOR_BLACK)
    @SimpleProperty
    public void Color(int argb) {
        color = argb;
        chartModel.setColor(color);
        refreshChart();
    }

    /**
     * Refreshes the Chart view object.
     */
    protected void refreshChart() {
        container.Refresh();
    }
}
{% endhighlight %}

Looking at ChartDataBase, one thing to notice right away that there are no generic parameters here.
The Chart and the Model object instances both use the abstract types, since in the controller, we do not
really care about the underlying subclass type.

In the constructor, note the line that creates the ChartModel. Essentially, following from the previous
section, it can now be seen that the ChartModel will indeed be instantiated and set to the right type,
all while keeping the details away from the Data component.

Also note how we barely handle much logic here either. In the Color function, we call the chartModel's
method to do it for us, since the ChartModel has direct access to the data object instances, which also
handle setting the styling options.

{% highlight java %}
public final class CoordinateData extends ChartDataBase {
    /**
     * Creates a new Coordinate Data component.
     */
    public CoordinateData(ChartBase chartContainer) {
        super(chartContainer);
    }

    /**
     * Adds entry to the Data Series.
     *
     * @param x - x value of entry
     * @param y - y value of entry
     */
    @SimpleFunction(description = "Adds (x, y) point to the Coordinate Data.")
    public void AddEntry(float x, float y) {
        chartModel.addEntry(x, y);

        // Refresh Chart with new data
        refreshChart();
    }
}
{% endhighlight %}

CoordinateData was created with the intention of storing x and y coordinates as the data. For now, it was kept simple, and
only one method was added, which is adding an entry based on the specified x and y parameters. Note again how the adding
of data is propagated up to the model to handle the data logic.

### Model class
{% highlight java %}
public abstract class ChartModel<T extends DataSet, D extends ChartData> {
    protected D data;
    protected T dataset;

    /**
     * Initializes a new ChartModel object instance.
     *
     * @param data  Chart data instance
     */
    protected ChartModel(D data) {
        this.data = data;
    }

    public T getDataset() {
        return dataset;
    }

    public ChartData getData() {
        return data;
    }

    /**
     * Adds (x, y) entry to the data set.
     *
     * @param x  x value
     * @param y  y value
     */
    public abstract void addEntry(float x, float y);

    /**
     * Changes the color of the data set.
     *
     * @param argb  new color
     */
    public void setColor(int argb) {
        dataset.setColor(argb);
    }
}
{% endhighlight %}

Now we move on to the class that handles the logic. The abstract class is kept rather simple, and
most future methods to be added to this class will generally be abstract, since the functionality
depends on a case-by-case basis depending on the Data class.

The ChartModel has two generic parameters - the DataSet and the ChartData, both of which come
from the library. There are instances and getters for both of them.

Since this is the class that will handle the logic, the setColor method directly changes the
color of the Data Series.

One interesting point to make is the abstract addEntry method. This is, in fact, an abstract
class, and there might be data models that would not have an (x, y) parameters as a valid
options for the addEntry method. However, we need it in the abstract method so the controller
could call the functionality directly. The idea I have thought of applying later is simply
throwing an exception in the overridden method if the option is not supported.

{% highlight java %}
public class LineChartModel extends ChartModel<LineDataSet, LineData> {
    /**
     * Initializes a new LineChartModel object instance.
     *
     * @param data  Line Chart Data object instance
     */
    public LineChartModel(LineData data) {
        super(data);
        dataset = new LineDataSet(new ArrayList<Entry>(), "");
        this.data.addDataSet(dataset);
    }

    /**
     * Adds a (x, y) entry to the Line Data Set.
     *
     * @param x  x value
     * @param y  y value
     */
    public void addEntry(float x, float y) {
        Entry entry = new Entry(x, y);
        dataset.addEntryOrdered(entry);
    }

    @Override
    public void setColor(int argb) {
        super.setColor(argb);
        dataset.setCircleColor(argb);
    }
}
{% endhighlight %}

Looking at the subclass for the LineChart, the key idea is to extend upon the required methods and initialize the proper
objects.

As can be noted in the constructor, the dataset is initialized to the proper type, and then it is added to the data
component. The dataset is added to the data component of the Chart, following a 1-to-N Chart-to-Data model.

#### Redefining the concept of the Data component
Note the implementation of the LineChartModel, specifically the part where we add the Data Series to the Chart's data.

Previously, there was an idea of having a single Data component attach to a Chart component. This kind of implementation was also done in
the [prototype]({{ site.baseurl }}{% post_url 2019-06-02-gsoc-2019-prototype %}). However, the concept was redefined to allow attaching multiple Data
components to simplify the process and allow for responsive components in the Designer view.

## Designer Components
Recall that the Charts in the Designer are different from the ones in Android, and as such, a different implementation is needed.
However, in fact, the same design was implemented for the Designer components, therefore there are just a few differences
that I will touch upon in this section.

### Similar Styling
In order to make the Designer and Android Charts similar, some additional code is written to set options that would make the
Designer Charts representative.

Here is an example snippet, which should be fairly self-explanatory:
{% highlight java %}
  chartWidget.getOptions().getTitle().setDisplay(true);
  chartWidget.getOptions().getLegend().getLabels().setBoxWidth(20);
  chartWidget.getOptions().getLegend().setPosition(Position.BOTTOM);
{% endhighlight %}

Various other styling changes are done to make the Chart components similar across the Designer and Android.

### Attaching Data to the Chart
Although the implementations of the Designer Charts and the Android Charts follow the same ideas,
there are a few differences with regards to attaching Data components to the Chart.

First let's take a look at the abstract MockChartData class:


{% highlight java %}
public abstract class MockChartData extends MockVisibleComponent {
    private static final String PROPERTY_COLOR = "Color";
    private static final String PROPERTY_LABEL = "Label";

    // Temporary placeholder for the Chart Data image
    private InlineHTML labelWidget;

    protected MockChartModel chartModel;
    protected MockChart chart;

    // ...

    /**
     * Adds the Mock Chart Data component to the specified Mock Chart component
     * @param chart  Chart Mock component to add the data to
     */
    public void addToChart(MockChart chart) {
        // Set widget to invisible
        labelWidget.setVisible(false);
        labelWidget.setWidth("0");
        labelWidget.setHeight("0");

        // Set references for Chart view and Chart model
        this.chart = chart;
        this.chartModel = chart.createChartModel();

        // Set the properties to the Data Series
        setDataSeriesProperties();

        // Refresh the Chart view
        refreshChart();
    }

    @Override
    protected void onSelectedChange(boolean selected) {
        super.onSelectedChange(selected);
        removeStyleDependentName("selected"); // Force remove styling
    }

    @Override
    public void onRemoved() {
        super.onRemoved();
        chartModel.removeDataSeriesFromChart();
        refreshChart();
    }

    /**
     * Refreshes the Chart view.
     */
    protected void refreshChart() {
        chart.chartWidget.update();
    }
}
{% endhighlight %}

As one might notice, some things are very similar. We have the same Color and Label properties, we have
Chart Model and Chart instances and we create the Chart Model in the same way as in the Android implementation.

However, the adding of the Data components is a bit different. We have the addToChart method, which
is called whenever the Data component is actually added to the Chart.

The method initially hides the widget's UI representation. The reason we do this is because when we drag the
Data component onto the Chart, it should perform an action, rather than be attached visually. But we
want a visual representation of the widget so the user sees where they are dragging the Data.
For now, this was made a labelWidget component (simply text) as a placeholder, which will most likely
be changed to a representative image later on.

The rest of the method sort of acts like a constructor. The reason for this is the fact that the component
is only truly initialized when it has the Chart object that it relates to. The reason we do not put most
of the code in the constructor is because the Data component is initialized even before it is added to the
Chart (while dragging), so it sort of complicates the situation, and we need to move the code to the
addToChart method.

Finally, let's see how the Data component is actually added on to the Chart. First, let's look at the MockChart
class:
{% highlight java %}
abstract class MockChart<C extends AbstractChart> extends MockContainer {

    // ...

    /**
     * Creates a new instance of a visible component.
     *
     * @param editor editor of source file the component belongs to
     * @param type  type String of the component
     * @param icon  icon of the component
     */
    protected MockChart(SimpleEditor editor, String type, ImageResource icon) {
        super(editor, type, icon, new MockChartLayout());
    }
}
{% endhighlight %}

First of all, note that the MockChart inherits from the MockContainer. A MockContainer in the App Inventor system
is basically a UI component that contains multiple elements inside it. These multiple elements in our case
are the Data components. In fact, what we are essentially doing is adding UI components inside the MockChart.
While one may not think of the Data components as UI components, we need this representation for the reason mentioned above
(Data components need a visual representation to guide the user)

Then the other important part is the super() constructor call. Note the MockChartLayout instantiation, which we look at next:

{% highlight java %}
public class MockChartLayout extends MockLayout {

    // ...

    @Override
    boolean onDrop(MockComponent source, int x, int y, int offsetX, int offsetY) {
        if (source instanceof MockCoordinateData) {
            container.addComponent(source);
            ((MockCoordinateData)source).addToChart((MockChart) container);
            return true;
        }

        return false;
    }
}
{% endhighlight %}

The MockChartLayout is essentially responsible for handling the layout that is inside the Chart component.
Since we are dealing with a Container component here, it is a requirement to specify a Layout object instance
to use for the Container. In this class, we typically specify some event handling for the children of the
container.

Now we get to the part where the drop functionality of the drag & drop process is handled. We have an
onDrop event, with its own parameters.

What this overridden method basically does is check if the component dropped onto the Chart is
of the required type (a MockCoordinateData component, meaning we only support CoordinateData components
for now). If the source is indeed a CoordinateData component, it is added to the container
(meaning the Data component is added as a child to the Chart in the Designer) and the addToChart
method discussed earlier is called form the Data component.

## Stay tuned for more!
This has been a rather in-depth explanation of the Chart workflow implementation. More updates will follow
on the status of the project. Stay tuned!


[appinventor]: https://appinventor.mit.edu/explore/
[mpandroidchart]: https://github.com/PhilJay/MPAndroidChart
[chartjs]: https://github.com/chartjs
[mvc]: https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller
[charts-data-core-pr]: https://github.com/elatoskinas/appinventor-sources/pull/37