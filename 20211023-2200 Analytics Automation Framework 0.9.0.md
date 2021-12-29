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
- Work on framework on live stream at https://twitch.tv/crosscompute on Monday, Wednesday, Friday from 4pm to 5pm EST.

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

## Tuesday 20211109-0930 - 20211109-0945: 15 minutes

    + Define server sent events endpoint
    + Implement auto reload when file changes (30 minutes)
        + Use watchgod to watch for file changes
        + Restart server
        _ Rerun script if script changes (not md, css, yml)

## Tuesday 20211109-1515 - 20211109-1530: 15 minutes

    + Clean up serve script (50 minutes)
    _ Consider defining views as a class for renderer globals
    + Put link to livestream above

## Tuesday 20211109-1530 - 20211109-1545: 15 minutes

## Tuesday 20211109-1645 - 20211109-1700: 15 minutes

## Wednesday 20211110-0715 - 20211110-0730: 15 minutes

    + Restore automation batch report file route
    + Restore automation batch report route

## Wednesday 20211110-1400 - 20211110-1415: 15 minutes

    + Rename automation_dictionaries to automation_definitions
    + Rename batch_dictionaries to batch_definitions

    _ Option 1: style_dictionaries
    _ Option 2: style_uris
    Option 3: style_definitions

We need to eventually write unit tests.

## Wednesday 20211110-1630 - 20211110-1645: 15 minutes

Let's draft a plan for restoring maps functionality and test how to have the map display in print.

'''
<script src='https://api.mapbox.com/mapbox-gl-js/v2.6.0/mapbox-gl.js'></script>
<link href='https://api.mapbox.com/mapbox-gl-js/v2.6.0/mapbox-gl.css' rel='stylesheet' />
'''

    + Evaluate test map page
    + Check how to let map be printed in firefox

## Wednesday 20211110-1630 - 20211110-1645: 15 minutes

## Friday 20211112-1315 - 20211112-1330: 15 minutes

    + Check maps example

    map-pydeck-screengrid
    _ map-pydeck-screen-grid

Let's get the mapbox example working today and then we can try screengridlayer afterwards.

    _ Option 1: Use regular expression to match .css
    _ Option 2: Hardcode .css into route
    Option 3: Serve styles at /styles/x.css

STYLE_ROUTE was conflicting with ECHOES_ROUTE so we put STYLE_ROUTE into a separate namespace.

    + See what happens if we try to run and serve the show-maps example

    _ Option 1: Put script inline after each html
    Option 2: Gather scripts at bottom

## Friday 20211112-1415 - 20211112-1430: 15 minutes

    Option 1: Get view renderer instance
        for each view renderer, generate additional styles and scripts and htmls
    _ Option 2: Make new view renderer instance
        use class static variables

    ImageRender
    ImageRenderer
    ImageRendererView
    ImageView
    ImageViewRenderer

    _ self.variable_views
    _ self.variable_view_by_view
    _ self.render_by_view
    _ self.render_by_view_name
    _ self.renderers
    _ self.view_by_view_name
    _ self.view_by_name
    self.renderer_by_view
    self.renderer_by_view_name
    self.variable_view_by_view_name
    self.variable_view_by_name

    variable_view.render(type_name, variable_definition)
    _ variable_view.render(variable_definition, type_name)

    variable_view = self.variable_view_by_name[view_name]
    variable_view.render(type_name, variable_definition)

    variable_view.style_definitions
    variable_view.script_definitions

## Monday 20211115-1600 - 20211115-1615: 15 minutes

    + Check current state

There seem to be two issues:

1. the page is requesting styles at the old route
2. not all style definitions have a path

That is strange, I thought I fixed these issues already though. Maybe I forgot to commit changes somewhere.

    + Fix issue of page requesting styles at old route

    Option 1: Use get in find_item
    _ Option 2: Specify dummy path

    _ Option 1: Update find_item to support string indexing
    _ Option 2: Define separate function
    Option 3: Use loop

    + Fix how to handle if not all style definitions have path

## Monday 20211115-1645 - 20211115-1700: 15 minutes

First, we will define the ImageView inside the module temporarily and we will separate the packages later.

What is the purpose of defining an AbstractBaseClass? It does protect against instantiation of the default. But perhaps in this case, we want a default implementation of render?

    Option 1: Make VariableView an AbstractBaseClass
    Option 2: Make VariableView a real class

What should we do when a variable view is requested but not installed?

    [YES] Option 1: Log error
    Option 2: Skip rendering
    _ Option 3: Show variable id in brackets

There might be cases where the author will want to render a report and not install all the dependencies, in which case we should gracefully skip the variable views that require the dependencies.

    _ NoView
    _ GhostView
    _ EmptyView
    _ HiddenView
    _ LostView
    _ SkippedView
    _ DefaultView
    _ VariableView
    NullView

    Option 1: Use dictionary
        def render(self, type_name, variable_definition):
    Option 2: Use keyword arguments
        def render(self, type_name, variable_id, variable_data, variable_path):
    _ Option 3: Use tuple
        def render(self, type_name, (variable_id, variable_data, variable_path)):
        def render(self, type_name, {variable_id, variable_data, variable_path}):

I don't think Option 3 is supported.

    + Define ImageView

Okay, today we defined a dummy ImageView. Next time, we will implement a dummy MapMapboxView and MapPyDeckScreenGridView.

## Tuesday 20211116-1630 - 20211116-1645: 15 minutes

    MapMapboxView.index
    _ MapMapboxView.__i
    _ MapMapboxView._i
    _ MapMapboxView.count
    _ MapMapboxView.i
    _ MapMapboxView.variable_count
    _ MapMapboxView.variable_index

    map-mapbox-0
    _ map-mapbox0
    _ map-mapbox-1
    _ map-mapbox1

    v0
    _ variable0

    Option 1: script_definitions in variable view
    _ Option 2: script_definitions in map mapbox view

    _ Option 1: Put all scripts into script_definitions
    Option 2: Separate scripts by type

    Option 1
        script_uris
        script_texts
    _ Option 2:
        script_definitions
            uri
            text

## Tuesday 20211116-1715 - 20211116-1730: 15 minutes

    + Rename body_content to content_html
    + Rename styles to style_uris
    + Rename scripts to script_uris
    + Add script_texts

    Option 1: initialize
    _ Option 2: __init__ with super

    _ add
    _ merge
    _ append
    _ merge_inplace
    append_once
    _ append_uniquely

It seems like the multiple initializations of the views is causing in the index.

## Wednesday 20211117-1300 - 20211117-1315: 15 minutes

    _ Rendition
    PageView
    _ Page
    _ page.add_style
    _ page.add_script

    page_view
    page_view.add_style
    page_view.add_script
    page_view.render

## Monday 20211122-1045 - 20211122-1100: 15 minutes

    css_uris
    style_uris

    html_text
    _ html_txt
    _ content_html
    js_text
    _ js_txt

    content_text
    html_text

    + Rename style_uri to css_uri
    + Rename script_uri to js_uri
    + Rename content_html to html_text
    + Rename script_text to js_text
    + Let author specify style in settings

    Option 1: Precompute bounds
    _ Option 2: Compute bounds on the fly

We should probably precompute bounds.

    + Experiment with map.getSource('mysource').tileBounds.bounds (doesn't work)

## Friday 20211126-1245 - 20211126-1300: 15 minutes

What we decided is to enable settings.path and have the script writer save variable specific settings there.

    Option 1: Load settings path from server side
    _ Option 2: Load settings path via AJAX

    _ Find js code to auto frame
    + Pre compute center and zoom
    + Implement variable_view.render
    + Return variable_view.style_definitions
    + Return variable_view.script_definitions
    _ Consider renaming body_content to content
    + Separate render_variable_from into a separate method
    + Add map to render_variable_from
    + Restore maps functionality
    + Define MapMapboxView

When we come back, we will implement MapPyDeckScreenGridView and MarkdownView.

It seems like we need to let the author define custom javascript code for their map.

## Saturday 20211127-1145 - 20211127-1200: 15 minutes

## Sunday 20211128-1045 - 20211128-1100: 15 minutes

What is the expected behavior for crosscompute serve.yml?

    _ Option 1: Show CrossCompute Examples in title
        /b/usa-maine
        DIS
            Adds inconsistency if user later adds multiple automations
    Option 2: Show CrossCompute Examples as one of the automations
        /a/crosscompute-examples/b/usa-maine
    _ Option 3: Start at /a/crosscompute-examples
        /a/crosscompute-examples/b/usa-maine

## Sunday 20211128-1700 - 20211128-1715: 15 minutes

What if an automation has no output? I think if an automation has no output, then we can rightfully skip it.

    + Decide conditions under which to skip an automation
    + Find the place where we load automations

    _ Consider renaming imports to automations

    Option 1: imports
    _ Option 2: automations

I think imports is clearer than automations.

Imports complicate the process of resolving stylesheets. Can the importing configuration override the stylesheet? Yes.

    Option 1: Load all configurations first
    _ Option 2: Load one at a time

I think Option 1 is simpler.

## Monday 20211129-1415 - 20211129-1430: 15 minutes

I have a problem. If the imports are nested, then we will need to resolve paths and other things properly. Paths are simple as they are relative to the path of the configuration folder. But what about stylesheets? Should we include the base stylesheets? Is it possible to override a stylesheet? I am thinking yes. Then for that to work, we need to load the current automation's stylesheets, then include the base stylesheets.

    BASE
    DISPLAY.STYLES
    IMPORTS
        import 1
        display.styles for 1
        imports
            import a
            display.styles for a

    display.styles for a
    display.styles for 1
    DISPLAY.STYLES

    _ Option 1: Store nested imports (complex to navigate)
    Option 2: Store reference to parent
    _ Option 3: Store absolute css uris resolved at load (redundant but fast)

It seems like we can store a reference to the parent.

    imports
    _ includes

## Monday 20211129-1545 - 20211129-1600: 15 minutes

    + Prototype code to load automation definitions
    + Skip an automation if it has no output
    + Load automation definitions from imports
    + Make it possible to serve multiple automations
    + Enable imports for serving multiple reports

## Monday 20211129-2115 - 20211129-2130: 15 minutes

    _ html_text
    _ markdown_text
    _ content_text
    body_text

    + Rename html_text to body_text

## Tuesday 20211130-1645 - 20211130-1700: 15 minutes

    _ def get_env_variable
    _ def get_environment
    _ def get_environment_variable
    _ def get_env
    _ def get_env_value
    def get_environment_value

## Tuesday 20211130-1745 - 20211130-1800: 15 minutes

    + Define MapPyDeckScreenGridView

## Wednesday 20211201-1345 - 20211201-1400: 15 minutes

```
batches:
  - name: {organization_name}
    folder: batches/{organization_name | normalize_key}
    variables:
      - id: organization_name
        path: datasets/organizations.txt
```

## Wednesday 20211201-1615 - 20211201-1630: 15 minutes

    + Fix watchgod on current folder ''

What if we want certain combinations of the variables?

```
batches:
  - name: {organization_name}
    folder: batches/{organization_name | normalize_key}
    variables:
      - id: organization_name
        path: datasets/organizations.csv
```

Here we can assume that the column name is the id.

What kind of example can we use here?

For pydeck screengrid, we can get a series of locations, perhaps fire stations.

## Thursday 20211202-1000 - 20211202-1015: 15 minutes

    + Draft configuration file serve.yml

## Thursday 20211202-1515 - 20211202-1530: 15 minutes

    + make svg template of keyboard
    + use input of text

## Thursday 20211202-1545 - 20211202-1600: 15 minutes

    + Substitute variables into template
    + Render template
    + Convert the configuration file into end html (30 minutes)
    + Have the server serve style.css (10 minutes)
    + Work on image substitution (45 minutes)
    + Consider how to let report creator specify alternate fonts
    + Consider renaming resource to automation in docs
    + Process old tasks
    + Define test for paint-letters
    + Define batches for paint-letters in proverbs.txt
    
## Thursday 20211202-1630 - 20211202-1645: 15 minutes

    + Draft run.ipynb for paint-letters
    + Make image choropleth example
    + Draft example with batches template -- report

## Friday 20211203-1600 - 20211203-1615: 15 minutes

I think we will finish the examples and then get the examples working.

## Friday 20211203-1745 - 20211203-1800: 15 minutes

    + Find a good open dataset

I think we will use public school locations.

- https://catalog.data.gov/en/dataset/public-school-locations-current
- https://catalog.data.gov/dataset/detroit-public-libraries

## Saturday 20211204-0945 - 20211204-1000: 15 minutes

I think we will have the script download the data if it does not exist.

Let's write it in such a way that there is no dependency on GDAL.

I rewrote it to not have a dependency on GDAL, but I think I will keep the dependency on invisibleroads-macros-disk

## Sunday 20211205-1115 - 20211205-1130: 15 minutes

    + Make smaller version of public school locations
    + Map public school locations
    + Make example that uses view: map-pydeck-screengrid -- widget

## Sunday 20211205-1145 - 20211205-1200: 15 minutes

I want to preserve the convention that plurals are lists and singulars are dictionaries.

    + Rename settings to configuration

I think we need a different example for markdown. Maybe we should use a simple example that does not rely on the internet. We could have a simple chat bot.

    ask-question

    + Make example that uses view: markdown -- widget

Now the challenge is to get the examples working.

## Sunday 20211205-1245 - 20211205-1300: 15 minutes

## Sunday 20211205-1530 - 20211205-1545: 15 minutes

    _ Option 1: Expand batch folders all at once
    Option 2: Expand batch folders one at a time

I think that in both cases, we can solve the problem by defining `yield_batches`.

## Sunday 20211205-1645 - 20211205-1700: 15 minutes

    _ Option 1: Use jinja2 filters
    Option 2: Parse on our own

## Sunday 20211205-2130 - 20211205-2145: 15 minutes

## Monday 20211206-0945 - 20211206-1000: 15 minutes

    + Test that we can run paint-letters
    + Make paint-letters example work with crosscompute
    + Make ask-question example work with crosscompute
    + Implement batches template
    + Allow commenting in batches path file

## Monday 20211206-1615 - 20211206-1630: 15 minutes

    + Make map-schools example work with crosscompute
    + Define StringView
    + Define MarkdownView
    + Test that stylesheet is not reloading
    + Force stylesheet reload
    + Make dynamic map example using screengridlayer
    + Render output
        + Prepare input folder by saving data in input variable paths
        + Load output variable definitions from output folder

    crosscompute
        # Walk through new configuration
        # Serve
        # Run in background
    crosscompute serve.yml
        # Serve
        # Run in background
    crosscompute serve.yml --run --force
        # Run in foreground
    crosscompute serve.yml --debug
        # Start debugger

    + Increase watchgod debounce interval

    python -m crosscompute

    Option 1: Define scripts/launch.py (separate script)
        crosscompute.scripts.launch:do
        ADV
            error in launch.py will not affect other submodules
            more consistent with run.py serve.py configure.py
        DIS
    _ Option 2: Define scripts.launch (function in __init__)
        ADV
        DIS
            less accessible when debugging
            error in __init__ will break other submodules
            might not be obvious place to look for other developers

I think in general __init__.py files should be light.

## Monday 20211206-1945 - 20211206-2000: 15 minutes

## Tuesday 20211207-0800 - 20211207-0815: 15 minutes

    _ wait_and_run(f'http://localhost:{args.port}', webbrowser.open)
    _ run_on_ready(f'http://localhost:{args.port}', webbrowser.open)
    _ run_on_load(f'http://localhost:{args.port}', webbrowser.open)
    _ run_if_ready(f'http://localhost:{args.port}', webbrowser.open)
    run_when_ready(f'http://localhost:{args.port}', webbrowser.open)

## Tuesday 20211207-1030 - 20211207-1045: 15 minutes

    + Add serve first
    + Combine run and serve into the crosscompute command line script

    Option 1:
        setup:
          dependencies:
          command:

    _ Option 2:
        script:
          dependencies:
          setup:

## Monday 20211213-1645 - 20211213-1700: 15 minutes

Where does caching happen in a web request?

- browser client (firefox, google chrome, safari, brave)
- server proxy (nginx, apache)
- corporate proxies (squid)

    Option 1: Use current time
    Option 2: Use content hash
    Option 3: Use modification time

    _ Option 1: Cache bust with timestamp query
        DIS: did not seem to work with firefox
    _ Option 2: Cache bust in the file name using content hash
        DIS: could make retrieval more complicated
    Option 3: Add response header to no-store, no-cache
    _ Option 4: Add nginx options to prevent proxy caching

    display
        html
            head
                path
    _ display
        head
        header

    display
        html
          - head: <link rel="">

    Option 1: jinja2 template
    _ Option 2: section path
    _ Option 3: structured injection (see above)
        DIS: could get annoying for js

## Wednesday 20211215-1615 - 20211215-1630: 15 minutes

Here are my main goals today:

1. finish the cache busting for stylesheets by adding no cache response headers
2. get forms working using multiprocessing queue for the self contained server
3. get the jupyterlab button working to re-run the automation and start the server

How about allowing preconnect?

    display:
        path: base.jinja2

    display:
        base:
            path: base.jinja2

I think the base template option is nice, but I think it would be even better to have some simple fallback in case the user doesn't want to create a separate base template. Let me think about it.

    Option A
        display:
            links:
                - rel: preconnect
                  href: https://fonts.googleapis.com
                - rel: preconnect
                  href: https://fonts.gstatic.com
                  crossorigin
            js:
                path: x.js
    Option B
        display:
            base:
                path: base.jinja2
            head:
                path: head.jinja2

I guess this could work as an alternative to specifying a derived base template.

I actually think it would be good to support both options.

    _ Remove support for display.styles.uri

## Wednesday 20211215-1645 - 20211215-1700: 15 minutes

I think in order not to be distracted, I should just implement display.base.path, handle file not found in yield_data_by_id

I also want to think through a good way to show live output. I don't really want to send data over server side events, though we could. If we do, we would probably need to keep track of all the listeners. Here is where the architecture of nodejs shines because the code for echoes is much simpler.

## Wednesday 20211215-1800 - 20211215-1815: 15 minutes

I think we should go with the content hash.

    + Combine serve and run into launch
    + Let user choose to run before serve to make debugging easier

I think caching is fine. We just want to break the cache in certain situations.

    _ Experiment with no cache response header

## Wednesday 20211215-1830 - 20211215-1845: 15 minutes

We have to make modifications in two places:

    + see_style
    get_display_configuration

    + Change see_style to match style to file based on uri
    + Change get_display_configuration to define uri using hash
    + Experiment with content hash in filename
    + Fix cache busting for stylesheets somehow
    + Rewrite see style to get style path from uri match
    + Force firefox to reload stylesheet

## Wednesday 20211215-2300 - 20211215-2315: 15 minutes

    + Handle file not found in yield_data_by_id
    + Strip spaces from csv keys

## Thursday 20211216-1030 - 20211216-1045: 15 minutes

    + Check whether we can attach a tmux inside a jupyterlab terminal
        Yes we can

We have at least 1-2 months of development before we can consider the package production ready.

We should get the backend working first.

    + Experiment with using a multiprocessing queue to run a worker

    runner
    _ worker

I actually like runner better than worker now.

## Friday 20211217-1100 - 20211217-1115: 15 minutes

## Friday 20211224-1015 - 20211224-1030: 15 minutes

Today we are going to build the mini worker queue that people can use while prototyping their automation.

The idea is to make a self-contained package for prototyping automations.

Where do we define the automation_queue?

    _ Option 1: Pass into Automation
        DIS: I don't think this is the right place to put it
    Option 2: Pass into serve and get_app
    _ Option 3: Define within Automation
        DIS: Does not allow for using a differently defined queue

## Friday 20211224-1115 - 20211224-1130: 15 minutes

    + Draft automation_queue
    + Prepare run folder using prepare_batch_folder

    Option 1: Put runs in runs
    _ Option 2: Put runs in batches

My feeling is that the two should be separate.

    add-integers
        batches
        tests
        runs

The runs folder can either be inside the configuration folder or it can be an absolute path.

    _ Option 1: Enumerate run ids
    Option 2: Randomize run ids

## Friday 20211224-1115 - 20211224-1130: 15 minutes

I forgot how to load a POST request in pyramid.

    + Define dummy POST /a/{automation_slug}.json
    + Put job on runner_queue or run_queue or worker_queue or automation_queue

I removed the exists check. I think I put that there in case the user modifies batch parameters manually. I wanted to preserve the manually modified batch parameters. But I think that won't happen very much if at all.

    + POST /a/{automation_slug}.json
        put job on queue
    + launch process that works through queue
        runs (how to specify folder)
    + Separate runs folder
    + Randomize run ids

## Friday 20211224-1445 - 20211224-1500: 15 minutes

## Friday 20211224-2030 - 20211224-2045: 15 minutes

An interesting question is whether to require batches as an initial starting point or whether it is allowed to have an automation without any batches. I think for now we will require batches.

## Saturday 20211225-0745 - 20211225-0800: 15 minutes

## Saturday 20211225-0945 - 20211225-1000: 15 minutes

I am debating whether to allow input and output variables to appear in the same template.

    { x | input }

The question is how to gather and submit. Maybe that could be custom js. But I do think it is useful to have a class for all input.

I think we decided to have render_input and render_output in the view class. After that, the view class will mostly be finalized and we can begin to split them into packages.

## Tuesday 20211228-1415 - 20211228-1430: 15 minutes

Today we need to get markdown forms working, then split the views. We will also work on the analytics content draft for tomorrow.

    _ Option 1: parse in get_batch_definitions_from_path
    Option 2: parse in yield_data_by_id

    data_by_id = parse_data_by_id(data_by_id, variable_definitions)

    + Parse using view in data_by_id
    + Update all views using new render and parse specs

## Tuesday 20211228-1600 - 20211228-1615: 15 minutes

    + Render form
    + Add link for input to template
    _ Define input and output template filters
    _ Allow input and output variables in the same template by allowing { x | input }

The way that we retrieve the variable data depends on the view that was used to render the variable.

    _ Option 1: Store data in value
        DIS: may result in invalid html
    _ Option 2: Store data in data attributes
        DIS: requires duplicating values to data
    Option 3: Use custom js depending on the variable view to retrieve the data
        I think this is the solution.

```
1. For each input element, get view class and call get_{view_name}_data.

get_number_data
get_string_data
get_image_data

2. Send POST request.
3. Redirect to either log or debug or output.
4. Let the creator customize the above behavior with a js override.
```

## Tuesday 20211228-1630 - 20211228-1645: 15 minutes

How do we let the creator customize the above behavior?

    <button class="run">Run</button>

Should we let the creator customize the above behavior?

Well for now, let's just show a simple run button that redirects to the output page. We could define this as a web component but for now let's stick to vanilla js.

We are returning to vanilla js because of a profound dislike of the sluggishness of react.

## Tuesday 20211228-2330 - 20211228-2345: 15 minutes

    Option 1: Put everything in form
    Option 2: Assemble request dynamically

# Schedule

    Submit post request
    Get forms working for the self contained server

# Tasks

    Handle http errors using add_exception_view

    Get the jupyterlab button working to re-run the automation and start the server
    Need to be able to re-run automation from jupyterlab

    Implement support for display.base.path
    Restore parallel runs
    Show live log output

    Need to be able to see errors
    Redirect to root if 404 on refresh
    Start an automation from an example template

    Restore upload functionality

    Add TableView

    Think of a good way for the user to specify custom html in head

    Limit refresh event to files that actually affect the view
    Check if file has actually changed
    Check whether jupyter autosaves files and updates modification time
    Prevent unnecessary file reloads because of jupyterlab autosaves
    Document steps need to take when creating a new automation

    Initialize variable_view_by_name using classes
    Initialize variable_view_by_name using classes loaded from importlib
    Consider pre initializing views for error checking

    Make it possible to navigate back from report
    Do not show navigation in print
    Do not open reports in target blank

    Make it possible to change fill color for geometries in MapMapboxView
    Consider separate option for printer friendly map
    Check that map displays in print
    Check that screengrid displays in print

## Phase 1

    Pull from repository if option is enabled
    Restore forms functionality

## Phase 2

    Move img code into crosscompute-image (30 minutes)
    Separate into packages
        Experiment with importlib.metadata
        Experiment with different design patterns for the view plugins

    Define TextView

    Check if sse event triggers onmessage and onevent
    Make a new queue for each new see_echoes
    Publish echoes when other files change
    Test if we could have app_iter block on a coroutine
    Reload server and page when source files change

    Make it possible for author to override mapbox version

    Continue thinking through decisions for user experience for preview vs fullscreen
    Decide how to let the user override the default preview for a huge dataset

    Draft script to render input

    Decide whether to eager load or lazy load full variable definitions

    Render title in template
    Render description in template

    Review legacy code to check whether we missed anything

    Consider crosscompute:// url scheme

    Consider how to make it possible to click on different svg elements and show information
    Show how to center home content
    Show how to center report content

# Milestones
Highlight accomplishments.

# Lessons
Give feedback on how we can do better next time.
