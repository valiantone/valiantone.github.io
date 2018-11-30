---
layout: post
title: "TeXifying your Jupyter Reports - Part 2"
date: "2018-08-27 17:28:56 +0200"
categories: jekyll update
published: true
excerpt_separator: <!--more-->
---
> In the previous post, we demonstrated how to set-up and debug the `nbconvert` directive available as part of the Jupyter Project. In this follow-up we cover template driven report-generation that allow for simplistic reports delivered in your favorite format - HTML, PDF or Markdown!
<!--more-->
## The worst is behind us!
If you've been following our post series on creating reports with _Jupyter's_ `nbconvert`, you've experienced the annoyance that is getting `nbconvert` up and running. Fortunately, if you've followed our walkthrough,  you not only have _LaTex_ configured on your machine but should have successfully generated a PDF report.

The first thing you'll notice with your PDF is that it follows the _Article_ template (defined within your _Tex_ software distribution bundle). _LaTex_ articles are minimalistic and follow a _box-layout_ model to define formatting, positioning and other layout options. Now, unfortunately, customization of these templates is not a well documented matter; with most current instructions for template tailoring residing on question-answering communities like StackOverflow and GitHub issues.

To ease you transition over to creating custom report formats, we offer a brief _'Getting Started'_ guide which should cover the following topics:
+ Creating new templates
+ Overriding parent templates for desired results
+  Configuring Jupyter - a workspace for your templates
+ From PDFs to other formats
