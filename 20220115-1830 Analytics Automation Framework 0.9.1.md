# Vision
Become the standard for analytics automation.

# Mission
Release 0.9.1 of the CrossCompute Analytics Automation Framework.

# Owner
Roy Hyunjin Han

# Roles
Computational Engineer: Roy

# Context
The framework is a self-contained server that analysts can use to prototype their automations before publishing them to the marketplace.

# Timeframe
20220115-1845 - 2022

# Budget
N/A

# Objectives
1. Pre-compile notebooks to scripts.
2. Accept custom root template.
3. Signal to a variable whether it is being printed.

# Habits
- Work on the system from 6am to 8am everyday.

# Log

## Saturday 20220115-1830 - 20220115-1845: 15 minutes

    + Make it possible to change fill color for geometries in MapMapboxView

    _ render_output(is_print=True)
    _ render_output(is_printing=True)
    _ render_output(with_printer=True)
    render_output(for_print=True)

    /a/A/b/B/o
    /a/A/b/B/o?p&t=TOKEN
    _ /a/A/b/B/p
    _ /a/A/b/B/o?for-print
    _ /a/A/b/B/o?print
    _ /a/A/b/B/o?with_printer
    _ /a/A/b/B/o?with-printer

## Sunday 20220116-0000 - 20220116-0015: 15 minutes

## Monday 20220117-1215 - 20220117-1230: 15 minutes

    _ class Request
    _ class WebRequest
    _ class VariablesRequest
    _ class CrossComputeRequest
    _ class RenderRequest
    _ class RenderingRequest
    _ class ModeRequest
    _ class BatchRequest
    _ class AutomationRequest
    _ mode_request = ModeRequest()

## Tuesday 20220118-0915 - 20220118-0930: 15 minutes

    + Support /a/A/b/B/o?p
    + Make it possible to signal whether a page is being printed

    _ HrefView
    _ HyperLinkView
    _ DownloadView
    LinkView

    view: download
    configuration:
        text: about our products

    view: link
    configuration:
        text: about our products
        download: x.csv

    id: places
    view: link
    configuration:
        text: about our products
        download: places.csv

    + Implement LinkView
    + Document LinkView

## Thursday 20220120-1430 - 20220120-1445: 15 minutes

## Friday 20220121-2030 - 20220121-2045: 15 minutes

    + Restore the table output view to render JSON
    + Implement TableView

## Monday 20220124-1700 - 20220124-1715: 15 minutes

# Schedule

    Document TableView
    Consider caching a run if caching is enabled in configuration

    Redirect to log if it is defined
    Redirect to output once variables.dictionary exists in debug_folder
        execution_time_in_seconds

    Consider allowing alt text for ImageView

    Update the framework so we can specify a custom base and root template
    Add support for script.path = abc.ipynb and pre-compile abc.ipynb to abc.py before running batches
    Restore running batches in parallel using ThreadPoolExecutor

    Update documentation

# Tasks

    Move render options into element_options or variable_options or render_options
    Support { x | label }
    Support display.layout = auto

    Update the framework to make it possible to restrict visible automations and batches based on a token via display.authorization.function = check.authorize and display.authorization.groups
    Consider using cookie prefixes for secure authentication
        https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie#cookie_prefixes

    Load the token from a cookie or from the url
    Save authorization keys into input folder or debug folder or auth folder or authorization path so that scripts can use it

    Update the framework to make it possible to define a script.schedule
    Implement programmable forms with progressive disclosure in the framework 
    Create a checkbox input view

    Get the jupyterlab button working to re-run the automation
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

# Milestones

# Lessons
