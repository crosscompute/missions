# Vision
Describe your long-term goal for the project.

# Mission
Define a short-term goal that you can complete within two weeks.

# Owner
State your name.

# Roles
Identify who is responsible for what.

# Context
Explain why the mission is important.

# Timeframe
Set estimated and actual dates for when you start and finish.

# Budget
Itemize estimated and actual costs including compensation.

# Objectives
Specify at most three goals that must be achieved for the mission to be successful.

# Habits
Detail what, when, where you will practice progress toward objectives.

# Log

## Tuesday 20220315-1115 - 20220315-1130: 15 minutes

```
display:
  configuration:
    _ page-break-templating
    _ template-page-break
    _ page-break-between-templates
    _ break-between-templates
    _ page-numbers
    _ page-numbering
    _ page-number-alignment
    page-break: template
    page-number: footer
  layout: auto
  format: pdf

display:
  configuration:
    page-break: template
    page-number: footer
  layout: auto
  format: pdf

printer:
  page-break: template
  page-number: footer

print:
  page-break: template
  page-number: footer
```

## Tuesday 20220315-1145 - 20220315-1200: 15 minutes

- [Done] Test that adding page breaks works ([done via CSS](https://forum.crosscompute.com/t/how-to-add-page-breaks/174))
- [Cancelled] Add page break option
- [Cancelled] Add support for page breaks
- [Done] Test that adding page numbers works

## Wednesday 20220316-1545 - 20220316-1600: 15 minutes

- [Done] Add page number option
- [Done] Update appa-automations
- [Done] Add support for page numbers

## Saturday 20220319-0700 - 20220319-0715: 15 minutes

- [Done] Remove crosscompute 0.9.1.5 from PyPI
- [Done] Remove crosscompute-printers-pdf 0.3.0 from PyPI

# Schedule

# Tasks

- [ ] Restore select view

    Save variables.dictionary for extra variables
    Check why settlements.csv is not saving
    Update run-onsset example to read from variables.dictionary
    Run OnSSET

    Create example with multiple scripts

    Consider specifying empty environment variables for send-emails
    Make tutorial example for Kashfi tutorial

    Restore running batches in parallel using ThreadPoolExecutor
    Support { x | label }
    Support display.layout = auto

    Document TableView
    Consider caching a run if caching is enabled in configuration
    Redirect to log if it is defined
    Redirect to output once variables.dictionary exists in debug_folder
        execution_time_in_seconds
    Consider allowing alt text for ImageView
    Update the framework so we can specify a custom base and root template
    Update documentation

    Update the framework to make it possible to restrict visible automations and batches based on a token via display.authorization.function = check.authorize and display.authorization.groups
    Consider using cookie prefixes for secure authentication
        https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie#cookie_prefixes

    Load the token from a cookie or from the url
    Save authorization keys into input folder or debug folder or auth folder or authorization path so that scripts can use it

    Update the framework to make it possible to define a script.schedule
    Implement programmable forms with progressive disclosure in the framework 
    Create a checkbox input view

    Restore upload functionality

    Consider how to let user choose auto formatted input and output templates
        display.layout = auto (default)
        display.layout = none

    Restore parallel runs
    Handle http errors using add_exception_view

    Need to be able to see errors
    Document steps for creating a new automation

    Make it possible to navigate back from report
    Do not show navigation in print

    Consider separate option for printer friendly map
    Check that map displays in print
    Check that screengrid displays in print

    Pull from repository if option is enabled

    Make it possible for author to override mapbox version
    Review legacy code to check whether we missed anything
    Consider crosscompute:// url scheme
    Make it possible to click on different svg elements and show information

    Consider splitting views into crosscompute-views to allow alternate versions of all the view plugins
    Consider cookiecutter
    Consider compatibility with gunicorn


# Milestones
Highlight accomplishments.

# Lessons
Give feedback on how we can do better next time.
