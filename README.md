# How to Use Template

This is a template that allows you to render a quarto document into the template 
format for the FYE, which currently uses `icml2023`. This guide will give you some 
example code for installing and using the template. 

# Setup
First, to download the template, type the following into your terminal:

>quarto use template eviekimmy/UCSC-STATS-TEMPLATE

This will give you some prompts for installing:


>? Do you trust the authors of this template (Y/n) › Yes <br>
>? Create a subdirectory for template? (Y/n) › No <br>
>[✓] Downloading <br>
>[✓] Unzipping <br>
>
>The template requires the following changes to extensions: <br>
>UCSC Statistics Template   [Install]    (format) <br>
>? Would you like to continue (Y/n) › Yes <br>


Once this is done, add the following to the YAML of your .qmd document:

```
---
title: 'YOUR TITLE'
author: 'YOUR NAME'
documentclass: article
classoption: twocolumn
format: 
  pdf: 
    theme: ucsc-stats-template
    keep-tex: true
---
```
The `keep-tex` command is optional. This allows you the original `.tex` file 
that is generated before rendering to pdf. This is useful for catching any latex 
issues, as well as for making slight adjustments to the format before rendering 
and turning in. (NOTE: If you are making edits to the `.tex` file, make sure to 
not render `.qmd`, as this will override any changes you made. Or, you can 
rename the `.tex` file you are editing and turn the renamed file in). 

# Formatting
## Abstract
Include the following into the YAML:

```
abstract: |
  the content of your abstract here
```

The `|` allows for extra text for anything indented underneath. 

## Code Chunk Options
For quarto, the preferred way is to use the `#|` format
```
#| echo: false
#| eval: true
```
These go at the start of your code block. I would always include `echo` and `eval` 
as options in your r chunk. `echo` tells quarto whether you want the code to be 
pasted into the document text or not, and `eval` tells you whether quarto should 
run the code. Most often, `echo: false`. The state for `eval` is 
useful if you decide that you no longer want a table or figure to be 
include in your report, but still want the flexibility of pasting it back in 
later. I also typically run a simulation in its own code chunk and save it to 
csv, which I will read in later. This allows me to set `eval: false` on the 
simulation code and have my document render faster when I make slight 
adjustments. 

Some other options are `warning` and `message`, and `error`, for 
controlling warning outputs, message outputs, and whether to run code with 
errors. 

## Figures
There are two types of figures that you can use: figures that take up one 
column, and figures that take up the entire page width. For column-wide figures,
include the following options at the start of your code chunk:

```
#| echo: false 
#| message: false
#| warning: false
#| label: fig-label
#| fig-asp: 0.4 
#| fig-pos: 'htbp'
#| fig-cap: 'CAPTION HERE'
```

Ensure that the label starts with `fig-`. These can be referenced in your quarto 
document by typing `@fig-label`. The `fig-asp` option is the `height/width` 
ratio of the figure. 0.4 seems to be a good ratio for most plots, and plots that 
are faceted (i.e. include `facet_wrap`) are usually appealing when `fig-asp` is 
0.6.  

Page-width figures are usually for when you want multiple subfigures. Include 
the following options at the start of your code chunk:

```
#| echo: false 
#| message: false 
#| label: fig-label
#| layout-ncol: 2 
#| fig-env: figure* 
#| fig-asp: 0.4
#| fig-pos: 'htbp' 
#| fig-cap: 'MAIN FIGURE CAPTION HERE' 
#| fig-subcap: 
#| - 'CAPTION FOR SUBFIGURE 1' 
#| - 'CAPTION FOR SUBFIGURE 2' 
#| - 'ETC' 
```

The option `layout-ncol` specifies how many columns it wants to have for the 
figures. So, for example, if you want to include three subfigures, setting 
`layout-ncol: 2` will paste two of the subfigures next to each other and the 
other on the bottom left. The figures will be pasted by row. For this example, 
this means the first figure in your chunk will be pasted in the top-left 
position, the second figure in the top-right, and the third in the bottom left. 
The options `fig-cap` and `fig-subcap` are self-explanatory. When referencing 
the subfigures, add a dash and the numbered figure you want after the label. So, 
if you want to reference the first subfigure generated in the figure, type `@fig-label-1`. 

# Tables
Quarto allows for multiple ways to include a table. For now, 
I will focus on tables generated from r code. If you would like to learn more 
about generating tables in line, 
[click here](https://quarto.org/docs/authoring/tables.html). First, ensure that you have the `kableExtra` package loaded in your document. 
This can be done using the following:

```
install.packages('kableExtra')
library(kableExtra)
```

Next, add the following options to the top of your code chunk:

```
#| echo: false
#| message: false
#| warning: false
#| label: tbl-label
#| tbl-cap: 'CAPTION HERE'
```

Ensure that the label starts with `tbl-`. These can be referenced in your quarto 
document by typing `@tbl-label`. After you have the `table` you want to generate, 
pipe the following:
```
table %>% kbl(booktabs=T) # need tidyverse loaded to use
table |> kbl(booktabs=T)  # base R pip operator that is slightly more limited
```

Currently there is no support for `longtables`. For now, tables only take up the 
column width of the document. Also, make sure that the text within the table is 
not too wide, as this will run into the other column and interfere with the 
other text. 

# Citations and Bibliography
First, ensure the citations are saved to a `.bib` file. Then, add the following 
in the YAML:

```
bibliography: YOURCITATIONS.bib
```

These can be referenced with `@citation`, where citation is the label you gave 
the citation in your `.bib` file. For example, If using `example-citations.bib`, 
I would type `@douglass_jacksfilms_2021` if I wanted to reference 
[Jacksfilm's meltdown over the Pizza Planet Truck](https://www.tiktok.com/@jackstiks/video/6985246283925572870):

> "iT sHoUlD oNlY bE iN tOy sToRy 😭" @douglass_jacksfilms_2021


# Referencing Inline Code
Adding \`{r} CODE HERE \` allows you to run inline code in your document. So, for 
example, you can define a variable that contains a posterior credible interval 
like so: 

```
# takes 95% symmetric credible interval and rounds to three digits
interval = posterior %>% quantile(c(0.025, 0.075)) %>% round(3)
```

and reference it in you text like this:

> The credible interval for the distribution is [\`{r} interval\`]. 





