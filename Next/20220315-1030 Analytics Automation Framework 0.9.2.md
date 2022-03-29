# Vision
Become the standard for analytics automation.

# Mission
Release 0.9.2 of the CrossCompute Analytics Automation Framework.

# Owner
Roy Hyunjin Han

# Roles
Computational Engineer: Roy

# Context
The framework is a self-contained server that analysts can use to prototype their automations before publishing them to the marketplace.

# Timeframe
20220315 - 20220415: 1 month

# Budget
N/A

# Objectives
1. Restrict visible automations and batches and change script behavior using cookies.
2. Implement script.schedule.
3. Restore upload functionality.

# Habits

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

## Saturday 20220326-1245 - 20220326-1300: 15 minutes

- [Cancelled] Make tutorial example for Kashfi tutorial
- [Cancelled] Update run-onsset example to read from variables.dictionary
- [Cancelled] Run OnSSET
- [Cancelled] Check why settlements.csv is not saving

## Monday 20220328-1200 - 20220328-1215: 15 minutes

I think that each field should potentially have an attached label. Or perhaps only input fields. The label should be able to be turned off or on. The question is whether the label shows by default or not.

    Option 1: Show label by default for input and not for output and use flex layout by default for input
        ADV: better default user experience with less work
        DIS: inconsistent behavior between input and output

        id: SMTP_PASSWORD
        view: password
        configuration:
          label:

    Option 2: Require activation to show label
        ADV: more control
        ADV: consistent behavior between input and output
        DIS: slightly more verbose configuration files

        id: SMTP_PASSWORD
        view: password
        configuration:
          label: SMTP Password

    _ Option 3: Show label by default for some views like string but not table
        DIS: inconsistent behavior between views

    Option 4: layout: auto vs manual in display
    Option 5: layout: auto vs manual in input or output
    Option 6: layout: auto if not templates otherwise manual

I think the person will usually want the label for input but not for output. The question is also what to show for the label.

    Option 1: Show raw variable id
    Option 2: Show variable id with normalize key and title
    Option 3: Show only label defined in configuration

# Schedule

- [4] Release invisibleroads-macros-log 1.0.6
- [2] Implement checkbox view
- [5] Review issues for crosscompute

# Tasks

- [ ] Restore select view

    Save variables.dictionary for extra variables
    Create example with multiple scripts
    Consider specifying empty environment variables for send-emails

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
# Lessons