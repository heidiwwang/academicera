<%*
// Fetch metadata from the central config file
const configPage = tp.file.find_tfile("APA_Config.md");
const cfg = app.metadataCache.getFileCache(configPage)?.frontmatter;
-%>
---
title: "<% tp.file.title %>"
aliases:
  - "<% cfg.doc_title %>"
created: <% tp.date.now("YYYY-MM-DD") %>
modified: <% tp.date.now("YYYY-MM-DD") %>
tags:
  - project/assignment
csl: <% cfg.csl_path %>
bibliography: <% cfg.bib_dir %>/<% cfg.bib_filename %>
geometry: <% cfg.geometry %>
mainfont: "<% cfg.mainfont %>"
fontsize: <% cfg.fontsize %>
linestretch: <% cfg.linestretch %>
header-includes:
  - \renewcommand{\maketitle}{}
  - \usepackage{fancyhdr}
  - \pagestyle{fancy}
  - \fancyhf{}
  - \rhead{\thepage}
  - \renewcommand{\headrulewidth}{0pt}
  - \usepackage{xurl}
  - \usepackage{microtype}
  - \urlstyle{same}
include-before:
  - |
    \begin{titlepage}
      \vspace*{2in}
      \begin{center}
        \textbf{\large <% cfg.doc_title %>}\\[2em]
        <% cfg.author_name %>\\[1em]
        <% cfg.institution %>\\[1em]
        <% cfg.course_code %>\\[1em]
        <% cfg.instructor %>\\[1em]
        <% cfg.due_date %>
      \end{center}
    \end{titlepage}
    \newpage
---

# <% cfg.doc_title %>

Write body text here.

\clearpage

# References

\begingroup
\raggedright
\setlength{\parindent}{-0.5in}
\setlength{\leftskip}{0.5in}
\setlength{\parskip}{0.5em}

<div id="refs"></div>

\endgroup
