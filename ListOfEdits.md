This is a list of essential changes done to different files in the forked repo. You can choose to clone the original repo and selectively apply any of those edit.

## ucl_thesis.cls

### Adding a Subtitle in the main page

In line `412` add this to be able to add a subtitle

```diff
\def\department#1{\gdef\@department{#1}}
\def\@department{\@latex@warning@no@line{No \noexpand\department given}}
+\def\subtit#1{\gdef\@subtit{#1}}
+\def\@subtit{\@latex@warning@no@line{No \noexpand\subtit given}}
```

then a bit lower

```diff
\gdef \@department{}%
\gdef \@title{}%
+\gdef \@subtit{}%
\let \maketitle = \relax%
```

then a bit more down

```diff
\begin{center}%
+    {\huge \bfseries \@title}\\[1em]%
+    {\huge \mdseries \@subtit}\\[5em]%
    {\Large \itshape \@author}\\%
\end{center}%
\vfill%
```

### Add an option to switch to the name of the course

Around line `120`

```diff
\newcommand \@doctor{Doctor of Philosophy}
\newcommand \@master{Master of Philosophy}
\newcommand \@mres{Master of Research}
\newcommand \@degree@string{\@doctor}
+\newcommand \@course@title{University College London}
+\newcommand \@course@title@preposition{of}
```

and then around line `180`

```diff
A dissertation submitted in partial fulfillment \\
of the requirements for the degree of \\
\textbf{\@degree@string} \\
-in \\
+\@course@title@preposition \\
-\textbf{University College London}.
+\textbf{\@course@title}.
```

## MainPackages.tex

Here you can add any other packages you want to use, I added a couple.

### For using SVG figures

For adding svg files exported from illustrator for example use this, as referenced [here](https://tex.stackexchange.com/a/129854).

Check [Readme](README.md) for requirements prior to using this package.

```
\usepackage[inkscapeversion=1]{svg}
```

### Tables

For simple tables I used booktabs, for more advanced tables look into using something like `tabularx` as mentioned [here](https://tex.stackexchange.com/a/163071).

```
%tables
\usepackage{booktabs}
```

### Pseudo-code

For creating styled pseudo-code and removing the endlines that are on by default use, as referenced [here](https://tex.stackexchange.com/a/111171).

```
%package for writing pseudo code
\usepackage{algorithm}
\usepackage[noend]{algpseudocode}
```

### Code Syntax Highlighting

For code highlighting, we use `Minted`. `tango` and `bw` are the styles I chose to try, `tango` is currently the active one since the other is commented out.

The last line increases the size of the line numbers next to the code.

Check [Readme](README.md) for requirements prior to using this package.

```
\usepackage{minted}
\usemintedstyle{tango}
%\usemintedstyle{bw}
\renewcommand{\theFancyVerbLine}{\sffamily{\footnotesize\arabic{FancyVerbLine}:}}
```

### Bibliography

- For bibliography

  ```
  \usepackage{har2nat}
  \usepackage{fancyhdr}
  \setcitestyle{citesep={;}, aysep={,}}
  ```

  `\usepackage{bibentry}` is already used in the template, so no need to add it.

### Gird of figures

For creating grid of figures/pictures

```
\usepackage{graphicx}
\usepackage{subcaption}
\usepackage{wrapfig}
\usepackage[autostyle]{csquotes}
```

## BibSettings.tex

I changed the style, you can choose to change it based on your guidelines.

```diff
-bibliographystyle{unsrt}
+\bibliographystyle{agsm}
```

## Main.tex

```diff
-\documentclass[12pt,phd,a4paper,oneside]{ucl_thesis}
+\documentclass[12pt,phd,final,a4paper,oneside]{ucl_thesis}
```

Right after loading the packages, define where your pictures and figure will be located. This means whenever you reference an image, it will expect that its located in a folder called `fig` in your root folder.

```diff
\input{MainPackages}
+%identify where figure and picture are
+\graphicspath{{fig/}}
+%if you are using the svg package do the same
+\svgpath{{fig/}}
```

Comment out the no bibliography trick

```diff
\begin{document}

-\nobibliography*
+%\nobibliography*
```

I used Mendely to generate my bibliography as a `Thesis.bib` file in my root folder.

```diff
-\bibliography{example}
+% This line manually adds the Bibliography to the table of contents.
+% The fact that \include is the last thing before this ensures that it
+% is on a clear page, and adding it like this means that it doesn't
+% get a chapter or appendix number.
+%\addcontentsline{toc}{chapter}{Bibliography} %this line is already activated in this template

+% Actually generates your bibliography.
+\bibliography{./Thesis}

% All done. \o/
\end{document}
```
