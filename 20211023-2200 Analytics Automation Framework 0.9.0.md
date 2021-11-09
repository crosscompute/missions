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

## Friday 20211029-0800 - 20211029-1000: 120 minutes

By the end of this session, we should have a prototype script that takes the randomize histograms example report and renders the output batches linked to the index page. I also want to render a map or two. We will then take the prototype script and merge it into our open source framework.

I think we first need to finish drafting the batch code, then put everything together for images. Then we can go back to maps.

We actually do not need to load the input variables to run a batch. All we need to do is specify the input and output folders.

## Friday 20211029-1500 - 20211029-1515: 15 minutes

It does not look there are any significant changes to subprocess.

    + Review if there are any changes to subprocess

We wrote an experiment once to capture the live subprocess output and stream it via server sent events. We should merge that functionality in this release.

[There is the repeated question of whether or not to use `shell=True`](https://docs.python.org/3/library/subprocess.html#security-considerations). I think we can safely assume that the person should know whether or not to trust external code and perhaps we can offer a way for the user to run external code in sandboxes or containers if something like podman is available.

    Use shell=True
        ADV
            if the person is running external code anyway then it does not matter
        DIS
            could result in shell injections

I think in 0.7 we tried shell=False and in 0.8 we used shell=True because of complications while splitting the command string.

To be honest, I can't think of a good reason for shell=True and it seems like shell=False is the default, so maybe we should revert to shell=False for 0.9.

Actually, it looks like we can't run the following command because $(...) is a bash construct if shell=False. And I think we ended up with this syntax because we were trying overcome race conditions for jupyter lab/notebook.

It is tempting to combine the template and code in the notebook, but I think separation of template and code is important.

    python -c "$(jupyter nbconvert run.ipynb --to script --stdout)"

    jupytext --to py notebook.ipynb --opt comment_magics=false
    jupytext --to script notebook.ipynb --opt comment_magics=false
    mv notebook.py notebook.ipy
    ipython notebook.ipy

I'm wary of using jupytext. Well let's experiment with it anyway. We don't really need the reverse direction of py to ipynb.

I think it was a design decision mistake to make ipynb json format.

Right now we are thinking about whether there is an alternative to using the bash $(...) syntax for running notebooks, which we did to enable parallel execution during report generation. The issue with writing to a file is that the file path would need to be unique. I think we need to stick with shell=True and the $(...) syntax. People should be running this system on Linux anyway.

    + Check if there is a better way to run notebooks in 2021

I need to restore the part about preserving some of the environment.

I forgot that the environment variables need to be relative to the configuration folder or script folder. Which?

Oh that is why there is the `script_folder`. Note that `batch_folder` is relative to the configuration folder.

    script_folder -- relative to configuration_folder
    batch_folder -- relative to configuration_folder
    CROSSCOMPUTE_INPUT_FOLDER -- relative to script_folder

So I do think we need to recompute relative paths.

    + Test run batches/a manually

We successfully ran a batch manually.

    + Draft batch code
    + Run command for each batch

## Friday 20211029-1645 - 20211029-1700: 15 minutes

Let's visualize the end result again. I think we are going to move the live output from the jupyterlab dialog into the browser window instead.

1. User clicks the CrossCompute button in JupyterLab
2. Run crosscompute in the same folder as the notebook
3. Find the configuration file
4. !! Run batches if display == output or show input form if display == input
5. !! Wait for the server to be up and start the browser
6. (Eventually) show live output in the browser if the batches are still running
7. Auto download pdf batch if the user has a premium subscription

Today we are going to productionize 2 through 5.

    jupyter nbconvert --to script *

    + Save all prototype notebooks as scripts
    + Write pseudocode

## Friday 20211029-1730 - 20211029-1745: 15 minutes

	/ = index
	/a/randomize-histograms = automation
	/a/randomize-histograms/b/a = batch
	/a/randomize-histograms/b/a/o/histogram-1d.png = batch file
	/a/randomize-histograms/b/a/o/histogram-2d.png = batch file
	/a/randomize-histograms/b/b

    index
    home
    _ root

    + Combine steps 4 to 5 into a single script
    + Generate output for each batch
    + Save our draft jupyter notebook as a script
    + Draft a script that generates the batches for the randomize-histograms example report

## Monday 20211101-1600 - 20211101-1615: 15 minutes

We want to keep the philosophy of minimizing our dependencies. However, I prefer pyramid over flask because pyramid apps can be building blocks for more complicated apps and that is how we are designing this framework.

    + Test current state of prototype script
    + Note next steps

    + Print link to server in command line

Should we launch the server before or after the batches run?

    Option 1: Launch server before batches run
        ADV - User can more easily track and preview parallel runs
    _ Option 2: Launch server after batches run

It seems that what we need to do is render the home first, then start the batch runs thread and finally launch the server. But I think debugging is easier in the main thread. So I think the script runs should be in the main thread, unless the user chooses to run the batches in parallel. And the server will be a thread or separate process.

    /a/randomize-histograms

    _ Option 1: see_automation returns {'automation': {'name'}}
        ADV More resilient to future changes
        DIS
    Option 2: see_automation returns {'name'}
        ADV Slightly less verbose and more consistent with REST api
        DIS

    + Show links to all batches in home

## Tuesday 20211102-1615 - 20211102-1700: 45 minutes

    + Evaluate the current state of the script

Let's focus on the core functionality before we get stuck in any details.

    scripts
        launch.py
        configure.py
        run.py
        serve.py
        _ initialize.py
    macros
    routines

To simplify development, we are going to separate the functions of the command-line script into separate parts. My priority today is to add map functionality and potentially separate the functionality into the crosscompute-map-mapbox package.

## Tuesday 20211102-1645 - 20211102-1700: 15 minutes

We don't have a lot of time right now, so let's just focus on computing all batches.

We'll write all the code first and then we can merge duplicates into functions later after we get the functionality working.

```
$ cat $(which jupyter)
#!/home/invisibleroads/.virtualenvs/crosscompute/bin/python
# -*- coding: utf-8 -*-
import re
import sys
from jupyter_core.command import main
if __name__ == '__main__':
    sys.argv[0] = re.sub(r'(-script\.pyw|\.exe)?$', '', sys.argv[0])
        sys.exit(main())
```

By examining the jupyter script, we can understand why we do not need to carry `VIRTUAL_ENV` into the subprocess environment because it is running the `python` for the virtualenv that was used to install jupyter.

    #!/home/invisibleroads/.virtualenvs/crosscompute/bin/python

    + Compute all batches

We wrote a rough script to compute all batches.

## Wednesday 20211103-1115 - 20211103-1130: 15 minutes

The main goal today is to get all the code ready for a pre-release candidate on pypi or perhaps just github.

    DEBUG
    INFO
    WARNING
    ERROR
    CRITICAL

    [empty] logging.ERROR
    -v      logging.WARNING
    -vv     logging.INFO
    -vvv    logging.DEBUG

## Wednesday 20211103-1300 - 20211103-1315: 15 minutes

    _ # ~/.cache/crosscompute/randomize-histograms/runs
    # ~/.crosscompute/randomize-histograms/runs

    + Define run_batch
    + Clean up the batches script
    + Run notebook / script

    _ normalize_path
    _ prepare_path
    _ clean_path
    _ sanitize_path
     _redact_path
    format_path

    join('~', relpath('/home/invisibleroads/Projects', expanduser('~')))
    _ re.sub(r'^' + expanduser('~'), COMMAND_LINE_HOME, x)

```
In [1]: import re
In [2]: from os.path import join, relpath, expanduser
In [3]: from invisibleroads_macros_log import HOME_FOLDER_SHORT_PATH
In [4]: x = '/home/invisibleroads/Projects'
In [5]: re.sub(r'^' + expanduser('~'), HOME_FOLDER_SHORT_PATH, x)
In [6]: join(HOME_FOLDER_SHORT_PATH, relpath(x, expanduser('~')))
In [7]: timeit re.sub(r'^' + expanduser('~'), HOME_FOLDER_SHORT_PATH, x)
9.04 µs ± 164 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)
In [8]: timeit join(HOME_FOLDER_SHORT_PATH, relpath(x, expanduser('~')))
25.3 µs ± 476 ns per loop (mean ± std. dev. of 7 runs, 10000 loops each)
```

I was originally thinking that join and relpath would be more platform independent if the original path is from a different platform, but I think both versions would suffer from the same problem of not being able to translate the other platform's path. So let's just revert to the faster version.

    + Replace home folder with squiggly ~ for logging.info

## Wednesday 20211103-1500 - 20211103-1515: 15 minutes

    + Define configure_logging
    + Define configure_argument_parser_for_logging

We are going to implement run and serve separately first and then combine them in launch.

## Wednesday 20211103-1530 - 20211103-1545: 15 minutes

    _ Option 1: Expose all waitress arguments with pass through
        ADV Easier
    Option 2: Cherry pick with wrapper
        ADV Can make independent of waitress

    [empty] logging.INFO
    -qq logging.CRITICAL
    -q logging.ERROR
    -v logging.WARNING
    -vv logging.DEBUG

What I don't like about this arrangement is that it uses up two command line arguments. However, it is intuitive and more inline with conventions.

    _ OPTION 1
        [empty] INFO
        -l      CRITICAL
        -ll     ERROR
        -lll    WARNING
        -llll   INFO
        -lllll  DEBUG

    _ OPTION 2
        [empty] INFO
        -l      CRITICAL
        -ll     ERROR
        -lll    WARNING
        -llll   DEBUG

    _ OPTION 3
        [empty] INFO
        -L      CRITICAL
        -LL     ERROR
        -LLL    WARNING
        -LLLL   INFO
        -LLLLL  DEBUG

    OPTION 4
        [empty] logging.INFO
        -qq logging.CRITICAL
        -q logging.ERROR
        -v logging.WARNING
        -vv logging.DEBUG

    _ OPTION 5
        [empty] INFO
        -i      CRITICAL
        -ii     ERROR
        -iii    WARNING
        -iiii   INFO
        -iiiii  DEBUG

    serve.py -iiiii
    serve.py -q
    serve.py -qq
    serve.py -v
    serve.py -vv
    crosscompute -iiiii
    crosscompute -q
    crosscompute -v
    crosscompute -vv

I think we should favor convention in this case.

## Thursday 20211104-0800 - 20211104-0815: 15 minutes

    + Restore barebones home view

    /
    /a/{automation_slug}
    /a/{automation_slug}/b/{batch_slug}
    /a/{automation_slug}/b/{batch_slug}/{variable_type}/{variable_path}

    Decide route for serving styles

    /style.css
    /favicon.ico
    /robots.txt
    _ /styles.css
    _ /~/styles.css
    _ /~/style.css
    _ /-/styles.css
    _ /-/style.css

    + Include CSS

## Thursday 20211104-0945 - 20211104-1000: 15 minutes

The question is, should we provide a default style or let the user be purely barebones?

For now, I think purely barebones is fine.

## Thursday 20211104-1145 - 20211104-1200: 15 minutes

    + Restore images functionality
    + Render css

## Thursday 20211104-1600 - 20211104-1700: 60 minutes

    + Evaluate the current state of the serve script (5 minutes)

I think the script re-runs should be on demand, not automatic because the script re-runs could be intense.

I am actually very tempted to clean up the server code. But let's focus on our chosen task.

    _ /e
    /echoes
    _ /events
    _ /sse
    _ /sses

Should each report have its own echoes endpoint? Or should the echoes endpoint be universal? I am thinking that the endpoint should be universal because we want the framework to have a small memory footprint. Should it be enabled by default? Yes. But it can be disabled perhaps by the --static argument.

    Option 1: Use messages
        ADV: more compact, uses less data and cpu
        DIS
    _ Option 2: Use events
        ADV: might be more maintainable and easier to understand
        DIS: less compact

## Thursday 20211104-1700 - 20211104-1715: 15 minutes

    _ Option 1: Use innerHTML
        ADV: easier
    Option 2: Use data and selective update
        ADV: more compact

## Thursday 20211104-1700 - 20211104-1715: 15 minutes

    + Restore server sent events endpoint toy experiment

I realized that restarting the server process itself might be a little more complicated. We could potentially use mmerickel's hupper but it seems like hupper has a lot of extra functionality that we might not need.

## Friday 20211105-1515 - 20211105-1530: 15 minutes

    _ Option 1: Use hupper
        Implement wrapper around watchgod
    Option 2: Use our own method
        Use subprocess and terminate()

```
from pyramid.config import Configurator
c = Configurator()
app = c.make_wsgi_app()

from waitress import serve
serve(app, port=7000)

from multiprocessing import Process
def f():
    print('http://127.0.0.1:7000')
    serve(app, port=7000)
p = Process(target=f)
p.start()
p.is_alive()
p.terminate()
p.is_alive()
# p.kill()
# p.is_alive()
```

It might be naive, but I think multiprocessing.Process will work for us.

    + Review hupper
    + Experiment with using multiprocessing to start and stop waitress.serve
    + Test whether we can restart waitress.serve using either a thread or process

We made such a mess yesterday that I think we will need to write the serve script from scratch again.

It seems like watchgod is already using multiprocessing and has defined a run_process with restart functionality.

	+ Review watchgod functionality

	Option 1: Put reload functionality into automation.serve
	_ Option 2: Make reload functionality separate

I think we can safely use watchgod.run_process.

	+ Reload server when any file in configuration_folder changes

The problem with this implementation is the server becomes the main process. I think it is fine for now.

	_ Option 1: Put our views into Automation
	Option 2: Put our views into a separate view class

	automation.views.see_home
	_ automation._see_home

What should be the degree of coupling between Automation and AutomationViews?

	Option 1: style['urls']
		content
		style['urls']
		style['text']
		_ style['external_urls']
		_ style['internal_text']
	_ Option 2: style_urls
		head_style
		body_content
	_ Option 3: stylesheet_urls

## Friday 20211105-1745 - 20211105-1800: 15 minutes

	_ self.views.configure(config)
	_ config.include(self.views)  # Ideal but doesn't work
	_ config.include(self.views.configure)
	_ config.include(self.views.add_routes)
	_ config.include(self.views.configure_routes_and_views)
	_ config.include(self.views.add_routes_and_views)
	config.include(self.views.includeme)

	_ Option 1: AutomationViews(automation)
	Option 2: AutomationViews(configuration, configuration_folder)
	_ Option 3: AutomationViews(configuration_path)

I think the strong coupling of Option 1 is acceptable here. Although Option 2 is easier to test.

Currently we are stuck because we have the view defined in AutomationViews but style_urls would need to be in Automation.serve, which seems messy. Ideally the configuration of style_urls in jinja2 globals should happen within AutomationViews.

## Monday 20211108-1415 - 20211108-1430: 15 minutes

By the end of today, we should have updated the serve script and integrated map-mapbox functionality.

    + Review the current state of the code

    Option 1: Finish auto reload
    _ Option 2: Integrate maps
    _ Option 3: Clean up serve script

First, we will finish auto reload via server sent events. Then we will integrate maps and then clean up the serve script if we have time.

Should we differentiate between server reload and file reload?

It looks like FileResponse dutifully reloads the file from disk and the response is not cached.

    + Check whether style.css is cached

Then we need to differentiate between whether the server is in development or production mode rather than static vs dynamic because the server is still dynamic always.

    _ Option 0: serve.py -d(ev)
    _ Option 1: serve.py -d(evelop)
    _ Option 2: serve.py -d(evelopment)
    Option 3: serve.py --production

## Monday 20211108-1430 - 20211108-1445: 15 minutes

We decided to keep the thread in the main server for now. We can expose an option in launch to run before serve to make it easier for the author to debug.

    _ Launch server in separate thread or process before running batches

We might want to differentiate between restarting the server and sending an update event. Restarting the server only really needs to happen during development.

Default behavior should be to restart the server and send both reload and update events.

    --production (disables server restart)
    --static (disables server sent events)

I think that for now, the echoes endpoint can be in AutomationViews. I see no clear reason for why /echoes should be in a separate class yet.

It seems wasteful to have two watch processes.

We will have two watch processes for now and we can optimize later. One watch process is to restart the server and the other is to send a server sent event. In production, the server sent events server will be a separate process, in which we could potentially have a single watch thread that sends both server sent events and restarts the server. The problem is how to consolidate the two in a way that is compatible with app_iter.

We could potentially have a coroutine and the app_iter blocks on the coroutine/generator.

The problem with this queue implementation is that I think there are multiple instances of see_echoes, but only one of them will get it. Do we need a separate queue for each instance? That seems like exactly what was done in https://stackoverflow.com/questions/31267366/how-can-i-implement-a-pub-sub-pattern-using-multiprocessing.

Maybe the complication is trying to send an event when the server is reloading. I think the better way to tackle this is to have an event handler client side that detects dropped connections and tries to reconnect and then detects whether a refresh is needed or not.

I think it was a mistake to tackle this auto reload problem right now.

Let's make a plan for how we will tackle this issue.

```
In [8]: timeit datetime.now().isoformat()
2.1 µs ± 35.7 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)

In [9]: import time

In [10]: timeit time.time()
168 ns ± 1.44 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)
```

    + Store a timestamp when the server initializes
    + Send the timestamp when a client connects
    + Reload server when the configuration file changes
    + Refresh page if the timestamp has changed

We made a bit of a mess again. We will clean it up later.

# Schedule

    Make a new queue for each new see_echoes
    Publish echoes when other files change

    Test if we could have app_iter block on a coroutine

    Define server sent events endpoint
    Implement auto reload when file changes (30 minutes)
        Use watchgod to watch for file changes
        Restart server
        _ Rerun script if script changes (not md, css, yml)
    Reload server and page when source files change

    Move img code into crosscompute-image (30 minutes)

    Clean up serve script (50 minutes)
    Restore maps functionality
    Restore forms functionality

    Separate into packages
        Experiment with importlib.metadata
        Experiment with different design patterns for the view plugins
    Consider defining views as a class for renderer globals

    Combine serve and run into launch
    Let user choose to run before serve to make debugging easier

# Tasks

    Continue thinking through decisions for user experience for preview vs fullscreen
    Decide how to let the user override the default preview for a huge dataset

    Draft script to render input

    Render output
        Prepare input folder by saving data in input variable paths
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


    Put link to livestream above
    Consider how to let report creator specify alternate fonts
    Consider renaming resource to automation in docs

    Consider crosscompute:// url scheme

# Milestones
Highlight accomplishments.

# Lessons
Give feedback on how we can do better next time.
