---
layout: post
title:  "Spreadsheet? More than a spreadshhet."
date:   2021-11-29
categories: "Application"
excerpt: “Keikai Spreadsheet - filling data automatically to your Excel templates.”

index-intro: "Explore possibilties when leveraging the best of spreadsheet."

image: "2021-11-more_than_spreadsheet/more_than_spreadsheet.png"
tags: tutorial
author: "Stella Wu"
authorImg: "/images/author/stellaw.png"
authorDesc: "Marketing, Keikai."


imgDir: "/images/2021-11-more_than_spreadsheet"
javadoc: "https://keikai.io/javadoc/5.0.0/"
---
<!--
images come from https://drive.google.com/open?id=17EEz_BuTVsTSeAA3a8AakyMspVSd_OEb made with draw.io
-->

Spreadsheets have been out for more than 40 years, but are still beloved by experts in all fields dealing with numbers, forecast, finance, and all kinds of data analysis, crediting its unique value - allowing users to compute and analyze data without having to program. 

During these 40 years, the internet, web, ajax, mobile, and front-end technologies have evolved way further, while the spreadsheet, either being offline or online, stays pretty much the same in terms of its functionality and look & feel.

Here comes the question: Can we expect more from today’s modern spreadsheet than the classic Excel or google sheets?

In this talk, we are going to look at several use cases combining the best of both worlds - the spreadsheet and the evolving Web.


## One shoe will not fit everyone. One CELL will not fit everything.

Spreadsheets are useful for arranging data and charts most of the time, but we would also encounter situations where we need to include long text, images, or other supplemental information to explain what’s going on in the cells and formulas.

With a traditional spreadsheet, we would try to fit this supplemental information in a cell, a cell comment, another sheet, or adding a link going to an external application. But what if we can seamlessly display this information on the same view?

![]({{ site.baseurl }}/{{page.imgDir}}/lXDAcAD.gif)


Read more to learn how you can slide in an HTML panel or pop up a window right on your sheet:
<a href="https://keikai.io/blog/p/Give-your-classic-spreadsheet-a-modern-touch.html/" target="_blank">Give your classic spreadsheet a modern touch</a> 


## Bring in intuitive web input experience

When rating on a website, we would expect to click on a row of stars or smilies. When selecting the budget range on a booking site, we would expect to see a slider where we can drag the handle to specify the desired range. But when it comes to working on a spreadsheet, it’s just a big table and all we can do is input numbers.

![]({{ site.baseurl }}/{{page.imgDir}}/front.png)

Here, one twist we can add to Keikai and improve user experience is intuitive input controls. Through mapping data with appropriate controls, Keikai could help to integrate intuitive input controls into a spreadsheet. To learn more about how to incorporate the functionalities, please visit: <a href="https://keikai.io/blog/p/insheet-control.html" target="_blank">Bringing intuitive input controls to the web spreadsheet user experience with Java</a>.


## Visualize formula dependencies

The core value of a spreadsheet lies in its ability to perform formula calculations, and the majority of spreadsheet users are doing their finance or forecasting models based on these formulas. Excel does have a built-in tool allowing you to track the dependent and ascendant of the formulas. However, there’s no convenient way of looking at the overall relationships between the formulas.

![]({{ site.baseurl }}/{{page.imgDir}}/custom_chart_cover.png)

By integrating a Listbox, we can see all dependencies in a table. By integrating a Network Chart, we can easily display the formula dependencies in a chart. Isn’t it amazing?
Read more on: <a href="https://keikai.io/blog/p/visualize-spreadsheet-formula.html" target="_blank">How I Visualize My Spreadsheet Formula Dependencies in a Chart</a>.


## Leverage Java power & maximize the efficiency
 A staple of Excel “search and retrieve” formula is VLOOKUP. It is a reasonable formula to use in isolation, but for the purpose of filtering and merging a large set of values, it has a major issue: each cell using this formula will multiply the processing time. A simple way of thinking about it is that if a single VLOOKUP takes X milliseconds to complete, a sheet with N Cells using VLOOKUP will use (X)N milliseconds to complete.

![]({{ site.baseurl }}/{{page.imgDir}}/merge_workflow.png)

Now, Keikai’s Java server reliance allows you to run java code on sheet events and implement efficient algorithms instead of using Excel’s built-in chaining functions. This means we can create efficient workflows that make full use of both the relations between cells in the sheet and the power of a full coding language. Read more: <a href="https://keikai.io/blog/p/budget-merge.html" target="_blank">Excel UI with Java flexibility: merging spreadsheet data in Keikai</a>.


# Summary
We hope these examples inspire you to think out of the box when leveraging the best of spreadsheets and the Web. Creativity is unlimited and Keikai is the shortcut! 



[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
