# Vision
Make it easy for municipalities to set up their own office of data analytics.

# Mission
Release CrossCompute Analytics Automation Framework 0.9.0.

# Owner
Roy Hyunjin Han

# Roles
Software Engineer: Roy

# Context
The CrossCompute Analytics Automation Framework makes it easy to transform your Jupyter Notebook into a web-based form or report.

# Timeframe
20211023-2230 - 20211109-2230: 2 weeks

# Budget
N/A

# Objectives
1. Implement crosscompute-image.
2. Implement crosscompute-map-mapbox.
2. Implement crosscompute-markdown.

# Habits
- Work on framework every morning from 6am to 8am EST.
- Work on framework on live stream on Monday, Wednesday, Friday from 4pm to 5pm EST.

# Log

## Saturday 20211023-2230 - 20211023-2245: 15 minutes

Will we have other plugins besides view plugins? Actually, we do have other crosscompute packages. So we need to specify crosscompute-view / crosscompute-views.

Now the question is whether to call it crosscompute-view-image or crosscompute-views-image.

    _ crosscompute-image
    _ crosscompute-view-image
    crosscompute-views-image

I think we need to call the plugins `crosscompute-views-image`.

## Saturday 20211023-2245 - 20211023-2300: 15 minutes

    + Clone repositories
        + crosscompute
        + crosscompute-docs
        + crosscompute-examples

    + Rename plugins
        crosscompute-image => crosscompute-views-image
        crosscompute-map-mapbox => crosscompute-views-map-mapbox
        crosscompute-table => crosscompute-views-table

I don't think we need to use stevedore anymore.

## Saturday 20211023-2315 - 20211023-2330: 15 minutes

Okay so far we identified input variables we can use to generate a single example image.

    mean, variance, value_count, bin_count, random_seed

The next step is to update the configuration file to specify those input variables. But I think we need to break and continue tomorrow.

## Sunday 20211024-1800 - 20211024-1815: 15 minutes

    mean, variance, value_count, bin_count, random_seed

    + Make example configuration file
        + Define input variables
        + Define output variables

    + Make example markdown template file
    + Define tests/standard
    + Define tests/batches/a
    + Define tests/batches/b
    + Make html from markdown

I think that last time, we decided to do the variable substitution before converting the markdown to html.

For the report css, should we make the css inline or make it available as a separate file? I think in this version, we should make it a separate file.

## Monday 20211025-1545 - 20211025-1600: 15 minutes

Today we are going to work on the image plugin.

    crosscompute-image
    crosscompute-image-extension
    crosscompute-views-image

    + Estimate completion times
    + Make html template
    + Download images
    + Insert images into html
    + Make end html (5 minutes)

We might want to group variable definitions by path, which is what I think we did before.

I think we previously loaded stuff into variable_data. That was because we were using React. I wonder whether we will load the data differently this time.

    variable
        id
        view
        path
        data
            value

I forgot why we differentiated between data.value and data.uri etc.

The idea is that we can convert a variable definition into html.

    configuration file =>
    variable definition =>
    html

I guess the question is whether we should still have the nested structure in data or just use data straight.

Looking at our old code, it looks like we had the following keys inside data.

    value
    dataset
    file
    path

I think we did this because we wanted to load files and datasets dynamically.

Let's think through what our input variable and output variable should look like here. For now, let's load it raw and we can think whether to complicate it again later. I like the simplicity of the non-nested format.

    input
        variables
            id: mean
            view: number
            path: variables.json
            data: 0

    output
        variables
            id: histogram-1d
            view: image
            path: histogram-1d.png
            data: 

Previously, we loaded image data using b64 into data.value. I don't know if we should do that again here. It was very inefficient. Ideally, it should be a regular url just like how img.src normally renders. And the server should serve the image. That means we will need to think about the url structure.

I want the URL to be easy to read aloud.

    /a/randomize-histograms/b/a/f/histogram-1d.png
    _ /a/randomize-histograms/batches/a/files/histogram-1d.png
    _ /a/randomize-histograms/batches/a/-/histogram-1d.png
    _ /automations/randomize-histograms/batches/a/
    _ /-/randomize-histograms/batches/a/
    _ /~/randomize-histograms/batches/a/
    _ /analytics/randomize-histograms/batches/a/
    _ /resources/randomize-histograms/batches/a/

    Option 1:
        output
            variables
                - data: /a/randomize-histograms/b/a/f/histogram-1d.png
    Option 2: 
        output
            variables
                - data
                    url: /a/randomize-histograms/b/a/f/histogram-1d.png

For now, let's put src directly into data. That leaves open the option of binary64 image data.

    input
        variables
            id: mean
            view: number
            path: variables.json
            data: 0

    output
        variables
            id: histogram-1d
            view: image
            path: histogram-1d.png
            data: /a/randomize-histograms/b/a/f/histogram-1d.png

Before we load this format, let's verify that we have enough information to convert the above data structures into html or any other output.

    input
        variables
            id: mean
            view: number
            path: variables.json
            data: 0

    <input class="input mean" type="number" value="0">

    output
        variables
            id: histogram-1d
            view: image
            path: histogram-1d.png
            data: /a/randomize-histograms/b/a/f/histogram-1d.png

    <img class="output histogram-1d" src="/a/randomize-histograms/b/a/f/histogram-1d.png">

Let's write pseudocode for our next steps.

I think we can have even more lazy loading. We should start by loading variable ids but we should not try to load the data or prepare the variable unless we can confirm that we actually need to render it.

    Option 1: Set data eagerly for each variable definition
        ADV: Is efficient about paths -- can load a path once
        DIS: Might end up loading paths that are not actually used
    _ Option 2: Set data lazily only if a template requests it
        ADV: only loads if in template
        DIS: Might load a path twice

To be honest, I think the likelihood is low that a user will define lots of variables that do not render, unless those variable definitions are log or debug variables. 

I think previously, we had files stored in the cloud and served them directly from google cloud.

How do we address Option 2 DIS Might load a path twice? We could keep files open or cached in memory, but that would increase memory requirements and we want servers to have a low memory profile.

Actually, we want to optimize render time, so all the complex data conversions should be pre-done. So I don't we should do lazy loading.

I can't really make up my mind about either way so let's table that decision.

## Monday 20211025-1715 - 20211025-1730: 15 minutes

Usually a report will be generated once and viewed multiple times. I think we also need to generate summaries for both input and output variables.

I want to keep a variable definition pretty lean. That means that large binary data should be in a file and loaded externally via uri and not in the variable definition itself.

For today let's solve the problem of making the randomize-histograms example functional.

When the server gets a request for the randomize-histograms automation batch a, it should load the configuration for batch a. Let's think how that configuration would look.

    Option 1: Cache pre-rendered html templates
        ADV - fast render
    Option 2: Load json for batch and render on fly server side
        ADV - Can quickly render to other formats?
        ADV - Can update template? -- but that won't happen often unless the person is trying to customize the report
    Option 3: Load json for batch and render on fly client side
        ADV - compatible with frameworks like react
        ADV - results in a more programmable web by exposing an api
            _ /a/randomize-histograms/b/a/i/v/mean
            _ /a/randomize-histograms/b/a/i.json
            _ /a/randomize-histograms/b/a/input.json
            /a/randomize-histograms/b/a.json

We can actually do both, render on fly and cache for the best of both worlds.

So it clear that we need to pregenerate the batch configuration. So we need to get the data values eagerly.

## Monday 20211025-1745 - 20211025-1800: 15 minutes

Okay now I think we are ready to move forward with render input.

Regarding finding variable ids in the template, we have some options actually.

    _ Option 1: Search for all instances of { ... } and then match to variable ids
    Option 2: For each variable id, search for instances of that specific { variable_id }

Previously we did option 1, but I think option 2 by help us ignore false positives while preserving the squiggly brackets.

    + Draft pseudocode for input render

When we finish this exercise, we should have a script that render the raw input form. Eventually we will implement a progressive disclosure user experience.

The question is... should we store the configuration file in its entirety or keep the configuration and data files separate. I am thinking that they should be separate and we can merge the dictionaries later.

Don't worry if the code is messy now. We clean up later.

When we render the default form, we have options:

    Option 1: Prepare default template
        ADV: Can later be customized by user
        DIS: Slightly less efficient
    _ Option 2: Render to html directly
        DIS: Creates a separate path

I think regular expressions are faster than the html rendering part. Plus I can compile the regex pattern.

    _ Option 1: Render html first, then substitute
    Option 2: Check if variable id matches, then render html and substitute

The way we are doing these substitutions could result in problems if the inner rendered HTML gets replaced. I think we can use placeholders. The issue is to make sure those placeholders are unique.

    + Render input
        + Load input variable definitions
        + For each path,
            + For each variable definition,
                + Set data (EAGER)
        + Find matching ids in template
            + Render html from variable definition
            + Substitute html into template
        + Turn template into markdown
    + Draft notebook to render input

We break now! Next time, we will productionize the code from the notebook into a script.

Today I learned that you can specify a function instead of a replacement string in `re.sub`.

## Wednesday 20211027-1600 - 20211027-1830: 150 minutes

    number
    image
    table
    table-datatable
    video
    audio
    calendar-?
    map-mapbox

It seems like importlib.metadata is a replacement for stevedore.

    _ Option 1:
        - id: settlements
          view: map
          path: settlements.geojson
          preview: image
          preview_path: settlements.png
    _ Option 2:
        - id: settlements
          view: image
          path: settlements.png
          fullview: map
          fullview_path: settlements.geojson
    _ Option 3:
      serve.yml
        - id: settlements-preview
          view: image
          path: settlements.png
          href: settlements
        - id: settlements
          view: map
          path: settlements.geojson
      report.md
        { settlements-preview }
    _ Option 5:
      serve.yml
        - id: settlements-preview
          view: image
          path: settlements.png
          settings:
            id: settlements
            # linkId: settlements
            # hrefId: settlements
        - id: settlements
          view: map
          path: settlements.geojson
      report.md
        { settlements-preview }
    _ Option 6:
      serve.yml
        - id: settlements-preview
          view: table
          path: settlements-preview.csv

          target_id: settlements
          targetId: settlements
          target: settlements

          href: settlements
          link: settlements

          _ click_id: settlements
        - id: settlements
          view: table
          path: settlements.csv
      report.md
        { settlements-preview }
    Option 4:
      serve.yml
        - id: settlements-preview
          view: image
          path: settlements.png
          link: settlements
        - id: settlements
          view: map
          path: settlements.geojson
      report.md
        { settlements-preview }
    Option 7:
      serve.yml
        - id: settlements-preview
          view: image
          path: settlements.png
        - id: settlements-link
          view: markdown
          path: settlements.md
        - id: settlements
          view: map
          path: settlements.geojson
      report.md
        { settlements-preview }
        { settlements-link }

    _ UX Option 1: Make entire preview clickable and navigate to link
        DIS: Can click on image by mistake
        DIS: Functionality might be too hidden unless there is a highlight
    UX Option 2: Expose a specific button or target that is clickable and that navigates to link
        ADV: prevents inadvertent clicking which can get annoying
        ADV: Visually obvious

    + Think through user experience for preview vs fullscreen

## Wednesday 20211027-1715 - 20211027-1730: 15 minutes


# Schedule

    Run command for each batch
    Generate output for each batch

    Save our draft jupyter notebook as a script
    Draft a script that generates the batches for the randomize-histograms example report

    Experiment with importlib.metadata
    Experiment with different design patterns for the view plugins

    Continue thinking through decisions for user experience for preview vs fullscreen
    Decide how to let the user override the default preview for a huge dataset

    Draft script to render input

    Render output
        Prepare input folder by saving data in input variable paths
        Run notebook / script
        Load output variable definitions from output folder


    Decide whether to eager load or lazy load full variable definitions


    Substitute variables into template
    Render title in template
    Render description in template
    Render template
    Convert the configuration file into end html (30 minutes)

    Have the server serve style.css (10 minutes)
    Work on image substitution (45 minutes)

    Review legacy code to check whether we missed anything

# Tasks

    Put link to livestream above
    Consider how to let report creator specify alternate fonts

# Milestones
Highlight accomplishments.

# Lessons
Give feedback on how we can do better next time.
