---
layout: post
title:  "Pivot Challenge: Can you analyze this aggregated data?"
meta-title: "Pivot Challenge: Can you analyze this aggregated data? | Ottava"
date:   2024-01-09
categories: "Stories"

index-intro: "Explain the differences between tabular and aggregated data and explore Ottava's innovative approach to analyzing pivoted data directly."
category-intro: "Explain the differences between tabular and pivoted data and explore Ottava's innovative approach to analyzing aggregated data directly."
image: "2024-1/pivot-challenge-cover.jpg"
tags: pivot, pivot table, excel pivot table, pivot table in Excel, unpivot, data analysis, pivot charts
author: "Jean Yen"
authorImg: "/images/author/jean.png"


imgDir: "2024-1"
---

![a pivot table]({{ site.baseurl }}/images/{{page.imgDir}}/pivot-challenge-cover.jpg)

My mom is a school teacher; one day, she showed me the class grades and asked me how she could re-arrange them to see how students were doing across Q1 and Q2 and find out which students were falling behind on any specific subject.

![a grade book table]({{ site.baseurl }}/images/{{page.imgDir}}/report-card.png)

Easy peasy." I was more than confident. Pivot tables and pivot charts are for slice and dice; I immediately thought about using them. I am not an expert, but I've used Excel pivot table at work a few times. I opened up my laptop and input the grade data, starting to drag the pivot table. 

“Hum,” I noticed I could only drag the subjects into the columns and rows. I could not drag students’ names. And when I selected the ELA (Q1) and ELA (Q2), the charts didn’t look the way I expected.

![an Excel pivot table]({{ site.baseurl }}/images/{{page.imgDir}}/pivot1.png)

Not what I want. I couldn’t compare the scores of each student between Q1 and Q2.

I switched the rows and columns and tweaked them here and there. No luck.

![an Excel pivot table]({{ site.baseurl }}/images/{{page.imgDir}}/pivot2.png)

It is not giving me the data I wanted; no average score for each student, and I couldn’t compare the scores of a specific subject.

>*I stumbled upon several forums and finally concluded that this grade book is not something I can slice and dice with pivot table in Excel directly. I will first have to convert it into tabular data before I can pivot it.*

Tabular data, what exactly is it? 

![comparing tabular and pivoted data]({{ site.baseurl }}/images/{{page.imgDir}}/tabluar-vs-pivoted.png "Example of Tabular data vs. Pivoted(aggregated) data")

Tabular data (left) has a row header indicating the meaning of each column, followed by rows of data, either text or numbers. It is well-structured and can be easily filtered, sorted and analyzed. 

On the other hand, the grade book I have (right) is pivoted or aggregated data - it has one or more row headers, one or more column headers, and many numbers in the main grid. In most cases, it looks like a summarized report.

/* ==Further Reading Banner: Pivot and Unpivot */

I've tried to use other popular spreadsheet apps like Tableau, and they all have the same requirement - you have to unpivot or shape your data in tabular format before you can even start to analyze them. In Excel, this unpivot process is normally done with advanced tools like Power Query. Unpivot can also be done using Python or other advanced platforms.

**How inconvenient!**
Wouldn't it be great if we could explore the grade book directly without having to convert it back to the tabular data?

That’s where the Ottava approach comes in to change the game.

Ottava is designed to work with pivoted or aggregated data directly, such as this grade book, sales reports, or an annual budget. Simply input the data as it is in the most natural way into the main grid in Ottava, and it will intelligently suggest appropriate pivot charts for you to pick from. 

This brings two advantages: first, obviously, skipping the unpivot, or pivoted-to-tabular transformation, saves time and makes the process of analyzing pivoted data easier.

Secondly, the pivoted table preserves more information.
If you take a look at the pivoted data below:

![]({{ site.baseurl }}/images/{{page.imgDir}}/report-card2.png)

You can tell the hierarchy from the table. For example, you can tell there are two classes, A and B, where John and Peter belong to Class A, and Anna and Cindy belong to Class B. Also, you can tell that there are 2 quarters, and in each quarter, students study 3 subjects. 

With this information, we can assume that the user having this table would want to compare the grades between Class A and Class B; or compare the grades across Q1 and Q2.

If you turn your pivoted data into tabular data, such hierarchy information is no longer there, and all fields are treated equally. A tool that analyzes this tabular data has less information to predict what the user really wants to visualize.
 
Ottava’s distinctive capability of analyzing pivoted data streamlines the data analysis process, saving valuable time and providing users with a seamless experience. Most importantly, now, my mom can easily visualize and explore her grade book with Ottava all by herself!

If this sounds like something you’ve been waiting for, give it a try and let us know what you think!


/* ==Ottava Signup Banner */