---
title: TeXifying your Jupyter Reports
author: Zubin J
pubDatetime: 2017-05-08T12:05:00Z
postSlug: latexify-jupyter-reports
featured: true
draft: false
tags:
  - data science, tools
description:
  Turn your Jupyter notebooks in to beautiful PDF reports!
---

> The Jupyter project offers a host of great features; not only can you embed mathematical formulae in your cells with MathJax; you can convert the entirety of your notebook into simple and clean looking reports. With the `nbconvert` tool you can convert your notebooks into a whole suite of different formats - python, markdown and pdf.
<!--more-->

However despite the functionality offered, converting to pdf can be a bit of a hassle due to dependency on `LaTex`, a powerful tool that can convert a markup language into a document. In principle all it takes is a single command to run the directive for conversion however due to dependencies on the forementioned `latex` package and other graphical libraries, the command is more likely to throw up a world of different errors.

So let's assume you got that to run, output: you have a new report thats looks... wait are those lines of code overflowing the page margin? And why is it expanding all my dataframes to cover over 45 pages? Yes, generating automated reports can be a pain at times and that's why we've put together this guide to highlight the most important aspects of report generation, pre-processing and post-processing.

In part 1 of this series we walk through the installation and setup guide to get your `nbconvert` tool running. In part 2 we will cover some basic templating to perform simple but effective generation of pdf documents.

## Welcome to the Jungle
First let's do a quick whirlwind tour of all the possible errors. The conversion module can be kicked off with the following directive.


~~~~
jupyter nbconvert --to PDF path/to/example.ipynb
~~~~

So let's run that and see what we have..


```python
nbconvert.utils.pandoc.PandocMissing: Pandoc wasn't found.
Please check that pandoc is installed:
http://pandoc.org/installing.html
```

If that error message makes little sense, well there's no need to panic; the most common of errors may sound daunting but all it's telling you is that your system is missing pandoc. Pandoc is a document conversion tool with multi-format support specializing in converting from one markup format to another. Conveniently you needn't have the entire pandoc suite of tools on your system, you only need LaTex as we mentioned earlier. So let's get LaTex installed. This however depends on your operating system. If you're on one of those operating systems that makes cute noises on startup and takes forever for system resources to load, this guide will probably be of little help to you.

But seriously for Windows users, getting any flavor of LaTex installed is simpler than on any other OS (yeah, that's a first). Our recommendation would be [MiKTeX](https://miktex.org/download), if you follow all the instructions on the official project page you should have a fulll installation of LaTex with all supporting packages. Now, add the system installation path to your environment variables and you should be ready to go!

### Mac and UNIX Users

The first step is to get the base LaTex package installed before we can proceed with exploring further error messages.
#### On Mac
> brew cask install mactex

This gets rid of the annoying Pandoc message hovever throws up a new error of its own.
```python
OSError: xelatex not found on PATH, if you have not installed xelatex you may need to do so. Find further instructions at https://nbconvert.readthedocs.io/en/latest/install.html#installing-tex.
```
Now, this error is due to a permission setting that was introduced in El Capitan. Under El Caitan and later releases (Sierra) writing to `/usr/` is no longer permitted. As a results the `/usr/textbin` path is removed - this causes complications since the symbolic link to `pdlatex` is now missing. The fix for this is quite simple really, all you need to do is relocate the link to the new default location (`/Library/TeX/texbin`) for TeX and hence reconfigure the TeX applications to look for directives in this location.

The following snippet by user [Moriambr](https://tex.stackexchange.com/users/89949/moriambar) [here](https://tex.stackexchange.com/questions/274179/mactex-error-xelatex-command-not-found) addresses the issue of correcting the link for the `TeXLive` utility which fixed the problem in our case.
~~~~
Reconfiguring TeXLive Utility: Version 1.23 of TEX Live Utility automatically detects the loss of /usr/texbin under El Capitan and prompts the user to automatically fix the problem. Previous versions need manual fixing:

+ Open TeXLive Utility Preferences
+ Click on Choose...
+ Press Shift + cmd + g
+ Enter /Library/TeX and click ok
+ Double click on texbin
~~~~

That's all you need to set LaTex up correctly on OSX.

#### On Unix/Linux variants
Depending on the flavor of your system, use the default system package manager to get the `texlive` package. In this post we will assume Ubuntu as the choice of variant.
> sudo apt-get install texlive texlive-latex-extra pandoc -y

**Note:** The atomic command mentioned above will perform a complete installation for Ubuntu users. There are certain different types of errors that you may still come across but all of them arise from a combination of missing packages, the most common being:

```python
! LaTeX Error: File `adjustbox.sty' not found.
```
This error arises when the `texlive-latex-extra` package is missing, use your packet manager to install this support package for fonts, images and layouts.

### Running the nbconvert directive
That does it! You're now ready to run the convert command from earlier, the console may throw a few warnings but those can be safely ignored.
```python
jupyter nbconvert --to PDF path/to/example.ipynb
```

In part one of this post series, we've outlined how to go about setting up LaTex correctly on your machine irrespective of the underlying architecture. We also did a quick run-down of some common errors and fixes. This should get you started with compiling simpler pdf reports however there's still some advanced functionality to be covered. Customizing your reports can be as challenging due to lack of strong documentation. In Part 2 of this series we will continue to explore template based conversion and some common capabilities such as suppressing output in your reports.
