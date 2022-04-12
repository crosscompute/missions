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
    _ Option 4: layout: auto vs manual in display
    Option 5: layout: auto vs manual in input or output
    _ Option 6: layout: auto if not templates otherwise manual

I think the person will usually want the label for input but not for output. The question is also what to show for the label.

    Option 1: Show raw variable id
    Option 2: Show variable id with normalize key and title
    Option 3: Show only label defined in configuration

## Tuesday 20220329-1630 - 20220329-1645: 15 minutes

    configuration:
      download-name: x.txt
      link-text: people

    configuration:
      download-filename:
      link-text:

    configuration:
      file-name:
      href-text:

    configuration:
      file-name:
      link-text:

## Friday 20220401-1245 - 20220401-1300: 15 minutes

- [Done] Release invisibleroads-macros-log 1.0.6

```
OLD
run_automation
    DiskAutomation.run logs on exception
    print_with raises then logs on exception in do

_run_batch
    run_automation raises on exception
    work logs on exception

NEW
execute _run_batch in parallel

Option 1:
raise exception in _run_batch
handle in result

Option 2:
log in _run_batch
return error dictionary

Option 3:
return error dictionary

Option 4:
raise exception in _run_batch if we could not run the script
log if we could run but there was an error in the script
handle in result
print only if there were no exceptions
```

## Friday 20220401-1500 - 20220401-1515: 15 minutes

```
environment:
  _ execution: process
  _ execution: thread
  _ execution: single
  _ concurrency: process
  _ concurrency: thread
  _ concurrency: single
  _ pool: process
  _ pool: thread
  _ pool: none
  batch: process
  batch: thread
  batch: single
```

## Friday 20220401-1830 - 20220401-1845: 15 minutes

## Monday 20220404-1400 - 20220404-1415: 15 minutes

- [Done] Restore running batches in parallel using ThreadPoolExecutor

## Monday 20220404-1530 - 20220404-1545: 15 minutes

It seems like double quotes in the downloaded file name are simply not supported in certain browsers.

- [Done] Check why double quotes not working in link name
- [Done] Update view: link examples
- [Done] Update link configuration

## Wednesday 20220406-1400 - 20220406-1415: 15 minutes

```
configuration:
  name:
  text:

configuration:
  file-name:
  link-text:

configuration:
  label:

configuration:
  label-text:
```

## Wednesday 20220406-2015 - 20220406-2030: 15 minutes

## Thursday 20220407-0930 - 20220407-0945: 15 minutes

    _ title_conservatively
    _ title_cautiously
    _ title_safely
    title_simply

- [Done] Consider adding process_text to normalize_key
- [Done] Implement label-text

## Thursday 20220407-1015 - 20220407-1030: 15 minutes

    _ with_label
    _ use_label
    add_label
    label
    label_html
    add_label_html

## Friday 20220408-0830 - 20220408-0845: 15 minutes

    class
    _ layout
    _ css-class
    _ css

    _ class: auto-layout
    _ class: flex
    _ class: raw-layout
    _ class: vertical
    _ class: vertical-layout
    _ class: layout-auto
    _ class: layout-flex
    _ class: l-vertical
    _ class: flex-vertical
    class:
    class: layout-vertical

## Friday 20220408-1145 - 20220408-1200: 15 minutes

## Friday 20220408-2245 - 20220408-2300: 15 minutes

```
input:
  layout: auto

display:
  layouts:
    - mode: input
      type: raw

display:
  layouts:
    - input: raw
```

## Saturday 20220409-0900 - 20220409-0915: 15 minutes

I think that all the modes should have a consistent layout of auto, with labels and vertical flex, unless specified otherwise. I think that layouts belong in the display section.

Layout is auto by default. That includes labels and vertical flex. Layout of raw has no labels or vertical flex. Or maybe our layouts could correspond to form, tool, widget, dashboard, report.

## Saturday 20220409-1315 - 20220409-1330: 15 minutes

```
display:
  styles:
  templates:
  pages:
    - id: automation
      configuration:
        design: input
        design: output
        design: none
        _ main: input
        _ main: output
        _ main: batches
        _ main: output batches
        _ main: input batches
        _ content: input
        _ content: output
        _ content: batches
        _ content: output batches
        _ content: input batches
        _ body: input
        _ body: output
        _ body: batches
        _ body: output batches
        _ body: input batches
    - id: input
      configuration:
        _ label: true
        _ labelled: true
        _ labelling: true
        _ design-name: flex-vertical
        _ layout-name: flex-vertical
        _ theme-name: flex-vertical
        _ theme: flex-vertical
        _ layout: flex-vertical
        _ with-labels: true
        design: flex-vertical
    - id: output
      configuration:
        design: none
        _ with-labels: false
    - id: log
      configuration:
    - id: debug
      configuration:

_ display:
  layouts:
    - id: automation
    - id: input
    - id: output
    - id: log
    - id: debug

_ display:
  designs:
    - id: automation
    - id: input
    - id: output
    - id: log
    - id: debug

_ display:
  modes:
    - id: automation
    - id: input
    - id: output
    - id: log
    - id: debug
```

The default design is flex-vertical. The user can set the default design to none.

When design is none, there are no labels unless explicitly specified. The flex-vertical design includes labels.

- [Cancelled] Implement display.pages configuration.main

## Saturday 20220409-1445 - 20220409-1500: 15 minutes

## Saturday 20220409-1530 - 20220409-1545: 15 minutes

- [Done] Implement dataset links

Use design flex-vertical by default, in which cases labels are on (unless overridden) and we have flex css for main. But if design is none, then omit the css and labels.

- [Done] Handle display.pages id=input design=none
- [Done] Handle display.pages id=output design=none
- [Done] Handle display.pages id=input design=flex-vertical
- [Done] Handle display.pages id=output design=flex-vertical
- [Done] Include form css unless design=none

## Tuesday 20220412-1500 - 20220412-1515: 15 minutes

- [Done] Handle display.pages id=automation design=none
- [Done] Handle display.pages id=automation design=input
- [Done] Handle display.pages id=automation design=output
- [Done] Implement display.pages configuration.design

- [Cancelled] Support { x | label }
- [Cancelled] Support display.layout = auto
- [Done] Update the framework so we can specify a custom base and root template
- [Done] Consider how to let user choose auto formatted input and output templates
- [Done] Restore parallel runs
- [Done] Make it possible to navigate back from report
- [Done] Do not show navigation in print
- [Done] Consider separate option for printer friendly map
- [Done] Check that map displays in print
- [Done] Check that screengrid displays in print

# Schedule

- [0] Implement checkbox view
- [1] Restrict visible automations and batches and change script behavior using cookies

# Tasks

- [30] Implement script.schedule
- [40] Restore upload functionality
- [ ] Implement ipynb forms
- [ ] Review issues for crosscompute
- [ ] Restore select view

    Save variables.dictionary for extra variables
    Create example with multiple scripts
    Consider specifying empty environment variables for send-emails

    Document TableView
    Consider caching a run if caching is enabled in configuration
    Redirect to log if it is defined
    Redirect to output once variables.dictionary exists in debug_folder
        execution_time_in_seconds
    Consider allowing alt text for ImageView
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

    Handle http errors using add_exception_view

    Need to be able to see errors
    Document steps for creating a new automation

    Pull from repository if option is enabled

    Make it possible for author to override mapbox version
    Review legacy code to check whether we missed anything
    Consider crosscompute:// url scheme
    Make it possible to click on different svg elements and show information

    Consider splitting views into crosscompute-views to allow alternate versions of all the view plugins
    Consider cookiecutter
    Consider compatibility with gunicorn

    Autogenerate datasets/batches.csv if requested during configure
    Implement count/index variables

# Milestones
# Lessons
