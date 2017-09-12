R Markdown Format for reveal.js Presentations
================

-   [Overview](#overview)
-   [Rendering](#rendering)
-   [Display Modes](#display-modes)
-   [Incremental Bullets](#incremental-bullets)
-   [Appearance and Style](#appearance-and-style)
-   [Slide Transitions](#slide-transitions)
-   [Slide Backgrounds](#slide-backgrounds)
-   [2-D Presenations](#d-presenations)
-   [Reveal Options](#reveal-options)
-   [Figure Options](#figure-options)
-   [MathJax Equations](#mathjax-equations)
-   [Document Dependencies](#document-dependencies)
-   [Reveal Plugins](#reveal-plugins)
-   [Advanced Customization](#advanced-customization)
-   [Shared Options](#shared-options)

Overview
--------

This repository provides an [R Markdown](http://rmarkdown.rstudio.com) custom format for [reveal.js](http://lab.hakim.se/reveal-js/#/) HTML presentations.

You can use this format in R Markdown documents by installing this package as follows:

``` r
install.packages("revealjs", type = "source")
```

To create a [reveal.js](http://lab.hakim.se/reveal-js/#/) presentation from R Markdown you specify the `revealjs_presentation` output format in the front-matter of your document. You can create a slide show broken up into sections by using the `#` and `##` heading tags (you can also create a new slide without a header using a horizontal rule (`----`). For example here's a simple slide show:

    ---
    title: "Habits"
    author: John Doe
    date: March 22, 2005
    output: revealjs::revealjs_presentation
    ---

    # In the morning

    ## Getting up

    - Turn off alarm
    - Get out of bed

    ## Breakfast

    - Eat eggs
    - Drink coffee

    # In the evening

    ## Dinner

    - Eat spaghetti
    - Drink wine

    ## Going to sleep

    - Get in bed
    - Count sheep

Rendering
---------

Depending on your use case, there are 3 ways you can render the presentation.

1.  RStudio
2.  R console
3.  Terminal (e.g., bash)

### RStudio

When creating the presentation in RStudio, there will be a `Knit` button right below the source tabs. By default, it will render the current document and place the rendered `HTML` file in the same directory as the source file, with the same name.

Note: Unlike the the other slideshow outputs, the slideshow viewer popup from RStudio will be blank, to view the slide show click the `open in browser` button, and the slide show will render in your default web browser.

### R Console

The `Knit` button is actually calling the `rmarkdown::render()` function. So, to render the document within the R console:

``` r
rmarkdown::render('my_reveal_presentation.Rmd')
```

There are many other output tweaks you can use by directly calling `render`. You can read up on the [documentation](https://cran.r-project.org/web/packages/rmarkdown/rmarkdown.pdf) for more details.

### Command Line

When you need the presentation to be rendered from the command line:

``` bash
Rscript -e "rmarkdown::render('my_reveal_presentation.Rmd')"
```

Display Modes
-------------

The following single character keyboard shortcuts enable alternate display modes:

-   `'f'` enable fullscreen mode

-   `'o'` enable overview mode

Pressing `Esc` exits all of these modes.

Incremental Bullets
-------------------

You can render bullets incrementally by adding the `incremental` option:

    ---
    output:
      revealjs::revealjs_presentation:
        incremental: true
    ---

If you want to render bullets incrementally for some slides but not others you can use this syntax:

    > - Eat eggs
    > - Drink coffee

Appearance and Style
--------------------

There are several options that control the appearance of revealjs presentations:

-   `theme` specifies the theme to use for the presentation (available themes are "default", "simple", "sky", "beige", "serif", "solarized", "blood", "moon", "night", "black", "league" or "white").

-   `highlight` specifies the syntax highlighting style. Supported styles include "default", "tango", "pygments", "kate", "monochrome", "espresso", "zenburn", and "haddock". Pass null to prevent syntax highlighting.

-   `center` specifies whether you want to vertically center content on slides (this defaults to false).

-   `smart` indicates whether to produce typographically correct output, converting straight quotes to curly quotes, `---` to em-dashes, `--` to en-dashes, and `...` to ellipses. Note that `smart` is enabled by default.

For example:

    ---
    output:
      revealjs::revealjs_presentation:
        theme: sky
        highlight: pygments
        center: true
    ---

Slide Transitions
-----------------

You can use the `transition` and `background_transition` optoins to specify the global default slide transition style:

-   `transition` specifies the visual effect when moving between slides. Available transitions are "default", "fade", "slide", "convex", "concave", "zoom" or "none".

-   `background_transition` specifies the background transition effect when moving between full page slides. Available transitions are "default", "fade", "slide", "convex", "concave", "zoom" or "none".

For example:

    ---
    output:
      revealjs::revealjs_presentation:
        transition: fade
    ---

You can override the global transition for a specific slide by using the data-transition attribute, for example:

    ## Use a zoom transition {data-transition="zoom"}

    ## Use a faster speed {data-transition-speed="fast"}

You can also use different in and out transitions for the same slide, for example:

    ## Fade in, Slide out {data-transition="slide-in fade-out"}

    ## Slide in, Fade out {data-transition="fade-in slide-out"}

Slide Backgrounds
-----------------

Slides are contained within a limited portion of the screen by default to allow them to fit any display and scale uniformly. You can apply full page backgrounds outside of the slide area by adding a data-background attribute to your slide header element. Four different types of backgrounds are supported: color, image, video and iframe. Below are a few examples.

    ## CSS color background {data-background=#ff0000}

    ## Full size image background {data-background="background.jpeg"}

    ## Video background {data-background-video="background.mp4"}

    ## Embed a web page as a background {data-background-iframe="https://example.com"}

Backgrounds transition using a fade animation by default. This can be changed to a linear sliding transition by specifying the `background-transition: slide`. Alternatively you can set data-background-transition on any slide with a background to override that specific transition.

2-D Presenations
----------------

You can use the `slide_level` option to specify which level of heading will be used to denote individual slides. If `slide_level` is 2 (the default), a two-dimensional layout will be produced, with level 1 headers building horizontally and level 2 headers building vertically. For example:

    # Horizontal Slide 1

    ## Vertical Slide 1

    ## Vertical Slide 2

    # Horizontal Slide 2

With this layout horizontal navigation will proceed directly from "Horizontal Slide 1" to "Horizontal Slide 2", with vertical navigation to "Vertical Slide 1", etc. presented as an option on "Horizontal Slide 1".

Reveal Options
--------------

Reveal.js has many additional options to conifgure it's behavior. You can specify any of these options using `reveal_options`, for example:

    ---
    title: "Habits"
    output:
      revealjs::revealjs_presentation:
        self_contained: false
        reveal_options:
          slideNumber: true
          previewLinks: true
    ---

You can find documentation on the various available Reveal.js options here: <https://github.com/hakimel/reveal.js#configuration>.

Figure Options
--------------

There are a number of options that affect the output of figures within reveal.js presentations:

-   `fig_width` and `fig_height` can be used to control the default figure width and height (7x5 is used by default)

-   `fig_retina` Specifies the scaling to perform for retina displays (defaults to 2, which currently works for all widely used retina displays). Note that this only takes effect if you are using knitr &gt;= 1.5.21. Set to `null` to prevent retina scaling.

-   `fig_caption` controls whether figures are rendered with captions

For example:

    ---
    title: "Habits"
    output:
      revealjs::revealjs_presentation:
        fig_width: 7
        fig_height: 6
        fig_caption: true
    ---

MathJax Equations
-----------------

By default [MathJax](http://www.mathjax.org/) scripts are included in reveal.js presentations for rendering LaTeX and MathML equations. You can use the `mathjax` option to control how MathJax is included:

-   Specify "default" to use an https URL from the official MathJax CDN.

-   Specify "local" to use a local version of MathJax (which is copied into the output directory). Note that when using "local" you also need to set the `self_contained` option to false.

-   Specify an alternate URL to load MathJax from another location.

-   Specify null to exclude MathJax entirely.

For example, to use a local copy of MathJax:

    ---
    title: "Habits"
    output:
      revealjs::revealjs_presentation:
        mathjax: local
        self_contained: false
    ---

To use a self-hosted copy of MathJax:

    ---
    title: "Habits"
    output:
      revealjs::revealjs_presentation:
        mathjax: "http://example.com/mathjax/MathJax.js?config=TeX-AMS-MML_HTMLorMML"
    ---

To exclude MathJax entirely:

    ---
    title: "Habits"
    output:
      revealjs::revealjs_presentation:
        mathjax: null
    ---

Document Dependencies
---------------------

By default R Markdown produces standalone HTML files with no external dependencies, using data: URIs to incorporate the contents of linked scripts, stylesheets, images, and videos. This means you can share or publish the file just like you share Office documents or PDFs. If you'd rather have keep depenencies in external files you can specify `self_contained: false`. For example:

    ---
    title: "Habits"
    output:
      revealjs::revealjs_presentation:
        self_contained: false
    ---

Note that even for self contained documents MathJax is still loaded externally (this is necessary because of it's size). If you want to serve MathJax locally then you should specify `mathjax: local` and `self_contained: false`.

One common reason keep dependencies external is for serving R Markdown documents from a website (external dependencies can be cached separately by browsers leading to faster page load times). In the case of serving multiple R Markdown documents you may also want to consolidate dependent library files (e.g. Bootstrap, MathJax, etc.) into a single directory shared by multiple documents. You can use the `lib_dir` option to do this, for example:

    ---
    title: "Habits"
    output:
      revealjs::revealjs_presentation:
        self_contained: false
        lib_dir: libs
    ---

Reveal Plugins
--------------

You can enable various reveal.js plugins using the `reveal_plugins` option. Plugins currently supported include:

<table>
<colgroup>
<col width="38%" />
<col width="61%" />
</colgroup>
<thead>
<tr class="header">
<th>Plugin</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><a href="https://github.com/hakimel/reveal.js/#speaker-notes">notes</a></td>
<td>Present per-slide notes in a separate browser window.</td>
</tr>
<tr class="even">
<td><a href="http://lab.hakim.se/zoom-js/">zoom</a></td>
<td>Zoom in and out of selected content with Alt+Click.</td>
</tr>
<tr class="odd">
<td><a href="https://github.com/hakimel/reveal.js/blob/master/plugin/search/search.js">search</a></td>
<td>Find a text string anywhere in the slides and show the next occurrence to the user.</td>
</tr>
<tr class="even">
<td><a href="https://github.com/rajgoel/reveal.js-plugins/tree/master/chalkboard">chalkboard</a></td>
<td>Include handwritten notes within a presentation.</td>
</tr>
</tbody>
</table>

Note that the use of plugins requires that the `self_contained` option be set to false. For example, this presentation includes both the "notes" and "search" plugins:

    ---
    title: "Habits"
    output:
      revealjs::revealjs_presentation:
        self_contained: false
        reveal_plugins: ["notes", "search"]
    ---

You can specify additional options for the `chalkboard` plugin using `reveal_options`, for example:

    ---
    title: "Habits"
    output:
      revealjs::revealjs_presentation:
        self_contained: false
        reveal_plugins: ["chalkboard"]
        reveal_options:
          chalkboard:
            theme: whiteboard
            toggleNotesButton: false
    ---

Advanced Customization
----------------------

### Includes

You can do more advanced customization of output by including additional HTML content or by replacing the core pandoc template entirely. To include content in the document header or before/after the document body you use the `includes` option as follows:

    ---
    title: "Habits"
    output:
      revealjs::revealjs_presentation:
        includes:
          in_header: header.html
          before_body: doc_prefix.html
          after_body: doc_suffix.html
    ---

### Pandoc Arguments

If there are pandoc features you want to use that lack equivilants in the YAML options described above you can still use them by passing custom `pandoc_args`. For example:

    ---
    title: "Habits"
    output:
      revealjs::revealjs_presentation:
        pandoc_args: [
          "--title-prefix", "Foo",
          "--id-prefix", "Bar"
        ]
    ---

Documentation on all available pandoc arguments can be found in the [pandoc user guide](http://johnmacfarlane.net/pandoc/README.html#options).

Shared Options
--------------

If you want to specify a set of default options to be shared by multiple documents within a directory you can include a file named `_output.yaml` within the directory. Note that no YAML delimeters or enclosing output object are used in this file. For example:

**\_output.yaml**

``` yaml
revealjs::revealjs_presentation:
  theme: sky
  transition: fade
  highlight: pygments
```

All documents located in the same directory as `_output.yaml` will inherit it's options. Options defined explicitly within documents will override those specified in the shared options file.
