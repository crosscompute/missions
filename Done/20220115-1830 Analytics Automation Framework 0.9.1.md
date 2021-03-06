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
20220115-1845 - 20220215-1845: 1 month

# Budget
N/A

# Objectives
1. [Done] Pre-compile notebooks to scripts.
2. [Done] Accept custom root template.
3. [Done] Signal to a variable whether it is being printed.

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

## Monday 20220131-1815 - 20220131-1830: 15 minutes

    render_input(options)
    render_input(d)

    o = Option(id='')
    d = D(id='')
    o = Options(id='')

    RenderingOptions
    RenderingDictionary

I see that besides collections.namedtuple, there is now typing.NamedTuple and dataclass. We will use dataclass.

```
@dataclass
class RenderingConfiguration
class RenderingOptions
class RenderingDictionary
class ElementConfiguration
_ class ElementDictionary
_ class RenderConfiguration

view.render(c)
view.render(e)
view.render(o)

web_element
dom_element
```

    + Move render options into x: VariableElement

## Tuesday 20220201-1315 - 20220201-1330: 15 minutes

    + Expand variables in configure_with
    + Expand variables in serve_with
    + Expand variables in run_with

## Sunday 20220206-1945 - 20220206-2000: 15 minutes

    _ template.folder
    _ cookiecutter.folder
    _ base.folder
    reference.folder

```
REJECTED
batches:
  - name: {country_name}
    folder: batches/{country_name | slug}
    configuration:
      folder: batches/djibouti
      path: datasets/batches.csv
  - name: {country_name}
    folder: batches/{country_name | slug}
    configuration:
      reference:
        folder: batches/djibouti
      path: datasets/batches.csv
  - name: {country_name}
    folder: batches/{country_name | slug}
    reference:
      folder: batches/djibouti
    variables:
      - id: population_growth_rate
        data:
          python: __import__('numpy').arange(0, 1, 0.1)
      - id: population_growth_rate
        generator: __import__('numpy').arange(0, 1, 0.1)
      - id: population_growth_rate
        python: __import__('numpy').arange(0, 1, 0.1)
      - id: population_growth_rate
        data: __import__('numpy').arange(0, 1, 0.1)

ACCEPTED
batches:
  - name: {country_name}
    folder: batches/{country_name | slug}
    reference:
      folder: batches/djibouti
    variables:
      - id: population_growth_rate
        code: __import__('numpy').arange(0, 1, 0.1)
      - id: urban_rural_ratio
        code: [0.1, 0.4, 0.7, 1.0]
  - name: {country_name}
    folder: batches/{country_name | slug}
    reference:
      folder: batches/djibouti
    configuration:
      path: datasets/batches.csv
```

## Monday 20220207-0915 - 20220207-0930: 15 minutes

I think I found a browser quirk. It seems like sometimes the refresh uses the cache. Sometimes the cached file is the css file. Sometimes the cache file is the html file. And it leads to unpredictable behavior. Server-side everything is fine. The solution I think is to specify cache-control for development mode.

    + Check why updating font-size in the css was not working
    + Specify cache-control header if in development mode

## Monday 20220207-1430 - 20220207-1445: 15 minutes

    execution_time_in_seconds
    _ execution_time_in_milliseconds

    + Decide format of debug/variables.dictionary
        execution_time_in_seconds
        return_code

## Tuesday 20220208-1515 - 20220208-1530: 15 minutes

    _ Use itertools.product for variables

## Tuesday 20220208-2200 - 20220208-2215: 15 minutes

    _ Option 1: Load variables in order
    Option 2: Load backwards and only use folder and reference if missing

## Wednesday 20220209-2130 - 20220209-2145: 15 minutes

It is not for the view to decide whether the data is asynchronous. The data may or may not be asynchronous and the view will need to decide how to handle it.

## Thursday 20220210-1630 - 20220210-1645: 15 minutes

    + Add title text to each html route
    + Check templates

## Friday 20220211-0100 - 20220211-0115: 15 minutes

- Automation definition defines an automation
- Automation configuration configures one or more automations & file does the configuring
- Batch definition defines a batch
- Batch configuration configures one or more batches & file does the configuring
- Variable definition defines a variable
- Variable configuration configures a variable & file does the configuring
- Template definition defines a template

## Friday 20220211-0700 - 20220211-0715: 15 minutes

## Friday 20220211-1315 - 20220211-1330: 15 minutes

    + Use reference for default values if defined
    + Use configuration path for specific value sets
    + Consider warning if batch folder or name is not unique
    + Consider how to handle format_path not handling Path
    + Restore routines/configuration.py

## Saturday 20220212-1900 - 20220212-1915: 15 minutes

## Monday 20220214-1745 - 20220214-1800: 15 minutes

    + Check automation_definition
    + Check batch_definition
    + Check template_definition
    + Check variable_definition
    + Check routes
        + automation
        + stream
    + Check routines
        + automation.py
        + batch.py
        + configuration.py
        + interface.py
        + log.py
        + variable.py
    _ Implement variables.id, variables.code
    + Check unique automation name and uri

## Tuesday 20220215-1300 - 20220215-1315: 15 minutes

    + Split batches.csv into variables.dictionary for each batch using reference.folder

## Wednesday 20220216-1045 - 20220216-1100: 15 minutes

    + Randomize Histograms
    + Compute Logarithms
    + Show Maps
    + Map Schools
    + Add Numbers
    + Ask Question
    + Send Emails
    + Find Places
    + Paint Letters
    + Gather Locations
    + Manage Locations
    + Test each example

## Wednesday 20220216-1445 - 20220216-1500: 15 minutes

    + Accept custom root template
    + Pre-compile notebooks to scripts

## Wednesday 20220216-2345 - 20220217-0000: 15 minutes

    _ Option 1: Have crosscompute serve the logs and use server sent events
    _ Option 2: Get the logs from subprocess and use server sent events
    Option 3: Save the logs to disk and poll periodically

## Thursday 20220217-0730 - 20220217-0745: 15 minutes

## Friday 20220218-1000 - 20220218-1015: 15 minutes

    + Try increasing default waitress threads

There is a problem with the way that waitress is handling the event streams. It is not closing the connections.

I am thinking that maybe we should just revert to long polling.

## Friday 20220218-1100 - 20220218-1115: 15 minutes

## Saturday 20220219-0815 - 20220219-0830: 15 minutes

The problem with server sent events is that it ties up waitress resources. I do think the most universally supported method is simple polling. And we are only creating this version for the development of automations, not their use.

    _ /s/abc/_
    _ /s/abc/-
    _ /s/abc/~
    /s/abc/crosscompute
    _ /s/abc/c

    _ /-
    _ /~
    _ /polls
    _ /states
    _ /updates
    _ /changes
    /mutations

    / = root
    /- = poll mirror

    _ Consider whether to substitute '' if environment variable is missing

## Sunday 20220220-0830 - 20220220-0845: 15 minutes

    + Update run to continue with next automation despite exception
    _ Stick with EventSource for now but remove loop

## Tuesday 20220222-2230 - 20220222-2245: 15 minutes

    + Add support for script.path = abc.ipynb and pre-compile abc.ipynb to abc.py before running batches
    + Revert to polling

## Wednesday 20220223-0945 - 20220223-1000: 15 minutes

```
scripts:
  - command:
    folder:
  - path:
    folder:
```

I think that for each batch, we will run all of the scripts in sequence. Another question is whether each of the scripts will share the same input and output folders. I think they should.

## Wednesday 20220223-1215 - 20220223-1230: 15 minutes

## Wednesday 20220223-1300 - 20220223-1315: 15 minutes

    + Convert all examples so that scripts is a list
    + Make it possible for scripts to be a list

## Thursday 20220224-1030 - 20220224-1045: 15 minutes

It seems that if a variable is changed in one process, it does not automatically change in the other process.

- [Done] Get the jupyterlab button working to re-run the automation

## Tuesday 20220308-1045 - 20220308-1100: 15 minutes

## Tuesday 20220308-2030 - 20220308-2045: 15 minutes

- [Done] Test links with texts with single quotes
- [Done] Prototype js refreshVariable(variableId)
- [Cancelled] Prototype js refreshTemplate(templateId)
- [Done] Prototype js refresh for each view

## Tuesday 20220308-2030 - 20220308-2045: 15 minutes

## Tuesday 20220308-2200 - 20220308-2215: 15 minutes

- [Done] Prototype lookup table of variable_path to uri + variable_id
- [Done] Prototype lookup table of template_path to uri + template_id

## Wednesday 20220309-1545 - 20220309-1600: 15 minutes

- [Done] Implement see_mutation to use lookup table to signal refresh
- [Done] Refresh selectively for variables

# Schedule

# Tasks

# Milestones

# Lessons

- Make functions that are easy to test.
