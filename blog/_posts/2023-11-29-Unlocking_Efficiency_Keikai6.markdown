---
layout: post
title:  "Keikai 6 Coming Soon with Grouping, Highlighting, and Autocompletion"
date:   2023-12-4
categories: "Intro"
excerpt: “Keikai 6 bringing Grouping, Syntax Highlighting and Formula Autocomplete”

index-intro: "Explore upcoming Keikai 6 new features: Grouping, Syntax Highlighting and Formula Autocomplete"

image: "2023-11-Unlocking_Efficiency_Keikai6/Keikai-6-beta.png"
tags: tutorial
author: "Rebecca Lai"
authorImg: "/images/author/rebecca.jpg"
authorDesc: "RD, Keikai."


imgDir: "/images/2023-11-Unlocking_Efficiency_Keikai6"
javadoc: "https://keikai.io/javadoc/6.0.0/"
---
<!--
images come from https://drive.google.com/open?id=17EEz_BuTVsTSeAA3a8AakyMspVSd_OEb made with draw.io
-->

Keikai spreadsheet is a fast and modern ajax web spreadsheet component that brings Excel functionalities into your web applications. We are excited to share that Keikai 6.0.0-Beta, coming with a new set of enhancements to elevate your spreadsheet experience, will be available very soon. In this blog post, we will explore three of its key features.


## Row & Column Grouping

Have you ever felt overwhelmed by complex spreadsheets? This is when row and column grouping comes to your rescue. Creating groups of up to eight levels and displaying data at the ideal level of detail are just a few lines of code away with our new server-side Range API.

```java
<zk>
<spreadsheet id="ss" ... />
	<zscript><![CDATA[
		io.keikai.api.Ranges.range(ss.getSelectedSheet(), "B:E").group();
		io.keikai.api.Ranges.range(ss.getSelectedSheet(), "2:5").group();
		io.keikai.api.Ranges.range(ss.getSelectedSheet(), "3:4").group();
		io.keikai.api.Ranges.range(ss.getSelectedSheet(), "3:4").collapse();
	]]></zscript>
</zk>
```

Once created, each group is automatically equipped with UI controls, providing an intuitive way for users to collapse and expand groups while navigating spreadsheets.
<style>
.videowrapper {
    float: none;
    clear: both;
    width: 100%;
    position: relative;
    padding-bottom: 56.25%;
    padding-top: 25px;
    height: 0;
}
.videowrapper iframe {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
}
</style>
<div class="videowrapper">
<iframe src="https://www.youtube.com/embed/9vERtnUg6fg?si=krFygwsfKggPXDYQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>


Additionally, the feature is fully compatible with Excel groups, making it seamless to import Excel spreadsheets with groups into Keikai.


## Formula Syntax Highlighting

Now that we have successfully simplified complex spreadsheets with row and column groups, let’s move on to tackle complex formulas, which can be equally daunting. With our syntax highlighting, the different parts of a formula can be easily differentiated at a glance, significantly improving its readability. On top of that, the simultaneous highlighting of the cells and ranges referenced in the editing formula also enables users to visualize the relationships between cells.

<div class="videowrapper">
<iframe src="https://www.youtube.com/embed/eDFGhSttzmk?si=Vh8j1v4tLVFua01S" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>


## Formula Autocomplete & Argument Hints

Finally, with an extra 55 built-in functions added in 6.0.0-Beta, we developed the formula autocomplete and augment hints to help exploit the full potential of our extensive function collection. Simply begin typing a formula into a cell or the formula bar, the UI will dynamically generate a drop-down list of matching functions, in which you can easily insert your desired function with a mouse click or arrow keys followed by the tab key. After inserting a function, a tooltip will automatically display its list of required and optional arguments to guide you through the formula editing process. 

<div class="videowrapper">
<iframe src="https://www.youtube.com/embed/3xq6AZ_LYus?si=irN8zonrDCCuBhb0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

# Summary
We hope your projects can benefit from the three features above along with the other 6.0.0-Beta enhancements. We would love to hear from you, so feel free to share your feedback with us at info@keikai.io!



[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
