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

# Schedule

    Make example configuration file

    Work on image substitution
        Make example markdown
        Make end html
            Make html from markdown
            Insert images into html
        Work in JupyterLab to convert the configuration file into end html

# Tasks

    Put link to livestream above

# Milestones
Highlight accomplishments.

# Lessons
Give feedback on how we can do better next time.
