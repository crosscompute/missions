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

# Schedule

    Make html template
    Download images
    Insert images into html
    Make end html

    Work in JupyterLab to convert the configuration file into end html
        Load input variables
        Run notebook / script
        Load output variables
        Substitute variables into template
        Render template
            Render title

    Work on image substitution
    Have the server serve style.css

# Tasks

    Put link to livestream above

# Milestones
Highlight accomplishments.

# Lessons
Give feedback on how we can do better next time.
