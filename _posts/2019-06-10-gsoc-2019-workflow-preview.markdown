---
layout: posts
title:  "App Inventor Chart Components: Workflow Preview"
date:   2019-06-10 01:47:00 +0200
category: Open Source
tags: open-source gsoc appinventor project
---

## Overview
Following up on the [Chart prototypes]({{ site.baseurl }}{% post_url 2019-06-02-gsoc-2019-prototype %}), this post will focus on previewing the current progress
on the Charts components in [App Inventor][appinventor].

This post will focus on the Components from the User perspective, meaning that implementation details will not be covered in this post.

## Component previews
To complete the prototype and depict the idea of the Charts, the prototype was further extended to include Bar Charts and Pie Charts alongside Line Charts.
Although the work for Bar and Pie Charts has been delayed to focus efforts and define the concepts on one type of Chart, they have been made to present
to the developer team to give an idea of how they would look like.


<figure class="half">
    <a href="{{ site.baseurl }}/assets/images/posts/gsoc2019/preview/prototype/DesignerBarChartPreview.png"><img src="{{ site.baseurl }}/assets/images/posts/gsoc2019/preview/prototype/DesignerBarChartPreview.png" alt="Designer Bar Chart Preview Image"></a>
    <a href="{{ site.baseurl }}/assets/images/posts/gsoc2019/preview/prototype/DesignerPieChartPreview.png"><img src="{{ site.baseurl }}/assets/images/posts/gsoc2019/preview/prototype/DesignerPieChartPreview.png" alt="Designer Pie Chart Preview Image"></a>
    <figcaption>Bar Chart & Pie Chart in Designer</figcaption>
</figure>

<figure class="half">
  <a href="{{ site.baseurl }}/assets/images/posts/gsoc2019/preview/prototype/DesignerChartBlocks.png"><img src="{{ site.baseurl }}/assets/images/posts/gsoc2019/preview/prototype/DesignerChartBlocks.png" alt="Designer Add Entry Blocks Preview"></a>
  <figcaption>Designer Blocks example</figcaption>
</figure>

<figure class="half">
  <a href="{{ site.baseurl }}/assets/images/posts/gsoc2019/preview/prototype/AndroidChartPreview.png"><img src="{{ site.baseurl }}/assets/images/posts/gsoc2019/preview/prototype/AndroidChartPreview.png" alt="Android Charts Preview"></a>
  <figcaption>Android Charts preview</figcaption>
</figure>


## Chart workflow redefinition
In the last post, two prototype versions were shown. In fact, however, the idea was redefined slightly from the two proposed models. Together with the developer team,
we have made the final decision to use a 2-component model, where a Chart could be added, and then Data would be attached to the Chart separately. However, a further
decision was to simply allow to drag & drop Data components on a Chart, providing ease of use and setup when working with Charts. The image summarizes the idea:

<a href="{{ site.baseurl }}/assets/images/posts/gsoc2019/preview/workflow/DataDragging.gif"><img src="{{ site.baseurl }}/assets/images/posts/gsoc2019/preview/workflow/DataDragging.gif" alt="Chart Data component drag & drop animation"></a>

<figure class="half">
  <a href="{{ site.baseurl }}/assets/images/posts/gsoc2019/preview/workflow/ChartDataComponents.png"><img src="{{ site.baseurl }}/assets/images/posts/gsoc2019/preview/workflow/ChartDataComponents.png" alt="Designer Chart component hierarchy"></a>
  <figcaption>Note how the Chart Data components are attached to the Chart itself</figcaption>
</figure>

## Multiple Data components
With the drag & drop workflow implementation, naturally, the next step was to implement multiple Data component support in the Charts themselves. Each Data component can be manipulated
individually, and they would then be updated in the Chart separately.

So if we add some entries into 3 attached Data components (following from the example of the previous section) using blocks, the Chart would look like this in the Android implementation:

<figure class="half">
  <a href="{{ site.baseurl }}/assets/images/posts/gsoc2019/preview/workflow/MultipleDataSeries.png"><img src="{{ site.baseurl }}/assets/images/posts/gsoc2019/preview/workflow/MultipleDataSeries.png" alt="Android multiple Data components"></a>
</figure>


## Data Properties
Two properties have been implemented for the Data components so far -- the color and the label. These properties have also been made responsive in the Designer interface to provide
visual feedback to the user on how the Data would look like.

<a href="{{ site.baseurl }}/assets/images/posts/gsoc2019/preview/workflow/DataStylingColorLabel.gif"><img src="{{ site.baseurl }}/assets/images/posts/gsoc2019/preview/workflow/DataStylingColorLabel.gif" alt="Designer Data component property responsiveness animation"></a>

### Android Chart Properties
Since the Charts are different in Android than in the browser, they require a different implementation.
In Android, if we apply some data properties to our Line Chart, currently it will look as follows:

<figure class="half">
  <a href="{{ site.baseurl }}/assets/images/posts/gsoc2019/preview/workflow/MultipleDataSeriesBasicStyling.png"><img src="{{ site.baseurl }}/assets/images/posts/gsoc2019/preview/workflow/MultipleDataSeriesBasicStyling.png" alt="Android multiple Data Series properties example image"></a>
</figure>

In the future, the Designer Chart components and the Android Chart components will be made to look more alike. But for now, the similarity is quite close.

### Data Adding
Support has also been added to allow the User to quickly enter some data in a form field to add to the Chart. For now, this is quite basic, and only y-values can be specified,
meaning that the first entered value will correspond to x value 1, the second to x value 2 and so on.

<a href="{{ site.baseurl }}/assets/images/posts/gsoc2019/preview/workflow/DataEntryBasic.gif"><img src="{{ site.baseurl }}/assets/images/posts/gsoc2019/preview/workflow/DataEntryBasic.gif" alt="Designer Data entry animation"></a>


## Stay tuned for more!
This has been a short preview of how the Chart components look and function so far to get a feeling of the general workflow of the Chart and Data components. A future blog post will
follow describing some design choices and implementation details.

[appinventor]: https://appinventor.mit.edu/explore/