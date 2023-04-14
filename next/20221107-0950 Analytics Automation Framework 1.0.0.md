# Problem

Updating web apps takes too much time.

# People

- Roy Hyunjin Han, CrossCompute

# Timeframe

20221107 - 20221231: 2 months

# Log

## Monday 20221107-1000 - 20221107-1015: 15 minutes

We need a way to accelerate the process of creating, updating and publishing a specific class of web apps.

- [Done] Define `WITH_REFRESH`
- [Done] Define `ROOT_URI`
- [Done] Define `title_text`
- [Done] Define `automations`
- [Done] Update `site_name` from configuration
- [Done] Update `automations` from configuration
- [Done] List automations in root

## Tuesday 20221108-1730 - 20221108-1745: 15 minutes

I think that today I will finish the fastapi migration so that we can return to work on the platform.

## Wednesday 20221109-1630 - 20221109-1645: 15 minutes

- Option 1: Have the GET request save token in cookie
- [Canceled] Option 2: Relay `_token` to POST

## Friday 20221111-1515 - 20221111-1530: 15 minutes

I realized that the way it is implemented now, the parent configuration defines the tokens and the child configuration tokens are ignored. I think this is the right behavior actually.

On second thought, no. The child configurations should be able to define identities independently.

In both cases, the parent configuration should override the child configurations where there is an overlap.

## Friday 20221111-1615 - 20221111-1630: 15 minutes

We found an issue where tokens can keep uniqueness but cookies can't.

## Saturday 20221119-1245 - 20221119-1300: 15 minutes

- [Done] Consider restoring separate style css or force style reload on change
- [Done] Pass `root_uri` in template environment

## Tuesday 20221206-0630 - 20221206-0645: 15 minutes

The only time I can really get work done is in the morning. Once my afternoon babysitting shift starts, all bets are off!

## Tuesday 20221206-0945 - 20221206-1000: 15 minutes

It seems like datalist is an extension of the input element.

- [Done] Experiment with the html5 datalist

```
_ view: text-checkbox
_ view: checkboxes
_ view: checkbox2text
_ view: dynamic-checkbox
_ view: checkbox-dynamic
_ view: checkbox-text
view: checkbox
view: datetime
```

- [Canceled] Prototype js to populate checkboxes based on query string

```
Option 1: configuration['options'] is standard
_ Option 2: configuration['options'] is view-specific
```

## Wednesday 20221207-1445 - 20221207-1500: 15 minutes

I have been thinking about making it possible to configure checkboxes from GET parameters. On second thought, I don't think this is a good idea. What we need is a kind of multi step wizard, where the elements of the second form are dynamically generated based on the answers in the first form. This could of course be done with a custom app, but we are trying to make this possible with the framework.

What are the options for dynamic checkboxes?

```
_ Option 1: GET parameters
_ Option 2: output
_ Option 3: log
_ Option 4: input
Option 5: configuration path
```

There is no reason to spread a template over multiple files only to be joined later. Separate template files means separate screens.

## Thursday 20221208-1045 - 20221208-1100: 15 minutes

The question is that when a RadioView renders as output, should we just get a text box or a disabled radio? I am thinking that it should be a disabled radio. It is the same for a checkbox.

- [Done] Check that we can load checkbox
- [Canceled] Prototype dynamic checkbox

We decided that we can make a checkbox dynamic by updating the variable configuration file.

- [Done] Implement qrcode view

```
$ function f() {console.log('hey'); return [1, 2]}
$ for (const x of f()) {console.log(x)}
hey
1
2
```

It seems that, at least in firefox, the second part of `of` is evaluated only once.

- [Done] Check that we can save checkbox

## Friday 20221209-1045 - 20221209-1100: 15 minutes

```
_ variable_definition.parent_mode
_ variable_definition.child_mode
_ variable_definition.route_mode
_ variable_definition.page_mode
_ variable_definition.element_mode
_ variable_definition.mode_name
_ variable_definition.page
_ variable_definition.mode
_ variable_definition.render_mode
_ variable_definition.page_id

variable_definition.step_name
variable_definition.mode_name

see_automation_batch
_ see_automation_batch_mode
see_automation_batch_page
_ see_automation_batch_type
_ see_automation_batch_section
_ see_automation_batch_part

step
_ phase
_ part
_ mode
```

## Saturday 20221210-1515 - 20221210-1530: 15 minutes

- [Done] Differentiate between `mode_name` for location vs `mode_name` for rendering

## Wednesday 20221214-1415 - 20221214-1430: 15 minutes

Checkboxes do not render for input for a run.

```
_ Option 1: Get configuration from first batch
Option 2: Make options from values.
```

## Tuesday 20221220-1445 - 20221220-1500: 15 minutes

```
_ site_settings
_ site_values
_ site_dictionary
_ site_mutables
site_variables
constants
variables
```

## Wednesday 20221221-1430 - 20221221-1445: 15 minutes

Current Flow

1. Server detects change in configuration, template, style or variable
2. Server looks up URI corresponding to change, URI = which automation, which batch, which step
3. Client keeps asking server whether my page changed (polling or server side events or websockets)
4. Server sends mutation information to tell the client that is on the page URI that it has changed
5. Client retrieves changes

- Option 1: Server only notifies changes but does not send change content
- Option 2: Server sends change content at the same time of the notification

Reason chose option 1 is a) security and b) bandwidth.

## Wednesday 20221221-1615 - 20221221-1630: 15 minutes

```
if code == InfoCode.CONFIGURATION:
if code == Info.CONFIGURATION:
if code == CONFIGURATION_CODE:
if code == CONFIGURATION_INFO_CODE:
if code == INFO_CODE_BY_NAME['configuration']:
if code == CONFIGURATION_CODE:
if code == 'c':
```

## Thursday 20221222-1715 - 20221222-1730: 15 minutes

Actually, `root_path`/`root_uri` seems to be working properly. There is a difference between `uri_prefix` and `root_uri` for proxies. One note is that the nginx reverse proxy needs a trailing slash:

```
  location /api/v1 {
    # proxy_pass http://localhost:8000/app;
    proxy_pass http://localhost:8000/;
    # proxy_pass http://localhost:8000; <-- does not work
  }
```

## Thursday 20221222-2215 - 20221222-2230: 15 minutes

- [Done] Remove `request_params` from DiskBatch

When there are no expected updates:
1. is production (stable configuration, templates, styles)
2. no update interval defined
3. all variables have loaded

`--no-development`

## Saturday 20230114-0615 - 20230114-0630: 15 minutes

I would really like to finish the migration to fastapi as well as deal with some other issues with the framework. Then we can finally return to upgrading the platform into a proper marketplace.

A few days ago, Kashfi and I found that there are issues with refreshing the map, radio button and checkboxes.

- [Done] Make a dummy automation for testing map refresh

I am not able to recreate the race condition locally, but here is the offending code:

```
v0.on('load', () => {
  const m = v0;
  m.addSource('v0', {'type': 'geojson', 'data': '/a/refresh-variables-test/b/standard/o/features'});
  m.addLayer({'type': 'circle', 'paint': {'circle-color': 'yellow'}, 'id': 'v0', 'source': 'v0'});
  jumpToBounds(m, [-30.073116676200726, -49.34924846903043, 79.9379367854184, -27.8821395183992]);
});
registerElement('features', async function () {
  try {
    await refreshMapMapbox('v0', '/a/refresh-variables-test/b/standard/o/features', v0);
  } catch {
  }
});
```

## Saturday 20230114-1245 - 20230114-1300: 15 minutes

- Option 1: Stall until load runs (uses cpu unnecessarily)
- Option 2: Add callback if not loaded

I can't think of other solutions.

```
# option 2
on load
  if flag set
    refresh
if loaded
refresh right away
if not loaded
set flag
```

Option 1 would actually be simpler. Let's go with option 1.

```
# option 1
while not loaded
  sleep
refresh
```

## Saturday 20230114-2245 - 20230114-2300: 15 minutes

- [Done] Fix race condition when refreshing map
- [Done] Fix radio refresh
- [Done] Fix checkbox refresh

## Sunday 20230115-0815 - 20230115-0830: 15 minutes

## Monday 20230116-1400 - 20230116-1415: 15 minutes

- [Done] Restrict variables to step

## Tuesday 20230117-2030 - 20230117-2045: 15 minutes

## Thursday 20230119-0745 - 20230119-0800: 15 minutes

I've been dragging on this fastapi migration. Let's finish it this week.

## Thursday 20230119-1130 - 20230119-1145: 15 minutes

- [Done] Use `get_data_from`
- [Done] Remove `b.get_data_from_request`

```
_ uniquify_with_order
_ deduplicate_with_order
_ make_unique(xs)
_ make_unique_with_order
_ make_unique_order
_ keep_unique_order
get_unique
get_unique_order
```

## Friday 20230120-1515 - 20230120-1530: 15 minutes

- [Done] Replace `self.mode_name` with `element.mode_name`
- [Done] Pass `request_params` to `_get_step_page_outer_dictionary`
- [Done] Implement basic `see_automation`
- [Done] Implement basic `see_automation_batch_mode`
- [Done] Implement basic `see_automation_batch_mode_variable`
- [Done] Implement basic `run_automation`
- [Done] Check requests dependency
- [Canceled] Embed css in page for local css
- [Done] Implement checkbox view
- [Done] Redirect to log if it is defined
- [Done] Redirect to output once variables.dictionary exists in `debug_folder`
- [Done] Update the framework to make it possible to define a script.schedule

## Thursday 20230126-1215 - 20230126-1230: 15 minutes

- [Done] Make test for refresh
- [Done] Add console logs for each event

I think what is happening is that an exception in a refreshVariable function is preventing other refreshVariables in the same pack from running.

- [Canceled] Option 1: trust that each refresh variable callback will catch its errors
- Option 2: have fallback

I see that there is an error in refreshTable and that is preventing the rest of the code from runnning if the table does not exist.

- [Done] check that each refresh variable catches
- [Done] Fix json error

## Friday 20230127-1945 - 20230127-2000: 15 minutes

```
async function f() {
  throw 'hey';
}

async function g() {
  try {
    await f();
  } catch (e) {
    console.error(e);
  }
}

g()
```

## Wednesday 20230201-0300 - 20230201-0315: 15 minutes

```
_ AssetFolder
_ AssetCache
AssetStorage
_ AssetStore
_ AssetMine
```

## Wednesday 20230201-1945 - 20230201-2000: 15 minutes

```
_ crosscompute --production
_ crosscompute --is-production
_ crosscompute --no-development
_ crosscompute --no-develop
crosscompute --no-restart
_ crosscompute --no-changes
```

## Monday 20230206-1300 - 20230206-1315: 15 minutes

```
_ resolve_package_path
_ get_package_path
get_package_file_path
get_asset_path
```

- [Done] Combine with refresh and with restart into `--no-restart`
- [Done] Stop mutations if there are no expected updates
- [Done] Finish migration from pyramid to fastapi
- [Done] Read https://advancedweb.hu/how-to-avoid-uncaught-async-errors-in-javascript/
- [Done] Test that we can override root template

## Wednesday 20230208-1800 - 20230208-1815: 15 minutes

```
Option 1: Update uris in `style_definitions` on refresh if with prefix
    PRO:
    CON: more memory use
Option 2: Strip request uri if with prefix
    PRO:
    CON: might remove wrong part
Option 3: Define left normalize function in `find_item`
    PRO:
    CON: adds some processing time
```

I think we should stick with option 2.

- [Done] Check why style is not loading for jupyterlab launches
- [Done] Fix favicon.ico when `root_uri` is defined

## Monday 20230213-1515 - 20230213-1530: 15 minutes

It looks like exceptions need to be handled within each process when using multiprocessing.

## Monday 20230213-2030 - 20230213-2045: 15 minutes

- [Done] Restore args.environment initialization
- [Done] Kill podman containers if interrupted

## Wednesday 20230215-0515 - 20230215-0530: 15 minutes

```
crosscompute(236180)─┬─serve(236185)─┬─ZOMBIE(236189)─┬─{crosscompute}(236191)
                     │               │                └─{crosscompute}(236217)
                     │               ├─server(236198)─┬─worker(236204)
                     │               │                ├─{crosscompute}(236209)
                     │               │                ├─{crosscompute}(236210)
                     │               │                ├─{crosscompute}(236211)
                     │               │                └─{crosscompute}(236212)
                     │               └─{crosscompute}(236219)
                     └─run(236186)

20230215-051820 DEBUG __init__.start:13 started serve process 236185
20230215-051820 DEBUG __init__.start:13 started run process 236186
20230215-051820 DEBUG __init__.start:13 started server process 236198
20230215-051820 INFO automation.run_batch:173 Watch Machine 0.1.0 batches/standard running
20230215-051820 DEBUG __init__.start:13 started worker process 236204

HERE IS THE ZOMBIE PROCESS
crosscompute(236189)---{crosscompute}(236191)
```

```
crosscompute(237083)─┬─serve(237088)─┬─ZOMBIE(237091)─┬─{crosscompute}(237093)
                     │               │                └─{crosscompute}(237119)
                     │               ├─crosscompute(237104)─┬─worker(237109)
                     │               │                      ├─{crosscompute}(237112)
                     │               │                      ├─{crosscompute}(237113)
                     │               │                      ├─{crosscompute}(237114)
                     │               │                      └─{crosscompute}(237115)
                     │               └─{crosscompute}(237107)
                     └─run(237089)
```

## Thursday 20230216-0400 - 20230216-0415: 15 minutes

Remember that KeyboardInterrupt is not of the Exception class.

## Thursday 20230216-2230 - 20230216-2245: 15 minutes

```
view: json
_ view: javascript
_ view: js
```

## Friday 20230217-1200 - 20230217-1215: 15 minutes

## Saturday 20230218-0445 - 20230218-0500: 15 minutes

- [Done] Replace registerElement with registerFunction
- [Done] Define registerCallback
- [Done] Update refreshVariable

## Tuesday 20230228-1345 - 20230228-1400: 15 minutes

## Wednesday 20230301-2000 - 20230301-2015: 15 minutes

- [Done] Experiment with sse-starlette

## Thursday 20230302-1515 - 20230302-1530: 15 minutes

- [Done] Define dummy sse endpoint /streams/mutations.json

There should only be a single point where the runs/prints are triggered otherwise there will be duplicates.

```
flags['/a/add-numbers/b/standard']['run'] = False
flags['/a/add-numbers/b/standard']['print'] = True
counts['/a/add-numbers/b/standard'] += 1

When a batch connects,
Check batch
If the batch run is obsolete, reset flag and queue run and print
If the batch print is obsolete, reset flag and queue print
Add uri to counts

When a batch disconnects,
Remove uri from counts

If there is a configuration or script change,
Mark batch run obsolete
Mark batch print obsolete

If there is a template or style change,
Mark batch print obsolete

If there a batch run or batch print in the queue,
if the flag is dirty,
skip it
```

## Friday 20230303-0930 - 20230303-0945: 15 minutes

## Friday 20230303-1100 - 20230303-1115: 15 minutes

Specifying an event actually sends more data in a server side event than not specifying an event.

```
# use onmessage
data: you

VS

# use addeventlistener
event: hey
data: you
```

## Monday 20230306-1515 - 20230306-1530: 15 minutes

- [Done] Move mutations to streams

## Tuesday 20230307-1000 - 20230307-1015: 15 minutes

- [Done] Move mutations to streams
- [Done] Define view: pdf
- [Done] Replace migrations polling with sse

```
Option 1: Print all but prioritize batches that are being watched
    CON: Might run batches unnecessarily
Option 2: Lazy load everything
    CON: Causes user to wait
Option 3: Trigger run and print if obsolete separate from worker queue
```

The absolute simplest thing might be to simply trigger a re-run whenever the pdf is loaded.

How can we detect if a batch is obsolete?

A run is obsolete when either a configuration or script changes. A print is obsolete when either a run changes or a template or style changes.

Obsolete means that the internal change is newer than the external render. That means that we need to timestamp both the internal change and the external render. We have two kinds of renders: a run and a print. That means we need to timestamp the batch run and the batch print. We also timestamp the changes.

Is a run obsolete? Get the timestamp of the batch run. Search the changes for the batch. If there are newer changes, then the batch run is obsolete. The same goes for print.

```
batch uri => run timestamp, print timestamp
batch uri => changes => latest change timestamp
if obsolete, then put batch run or print on queue (might result in duplicate runs)
```

The worker queue should probably check whether the run or print is already up-to-date and then skip it if it is. The timestamp of a run or print should be recorded when the run or print is started, not at the end.

Actually, I think there are two worker queues, one for runs and another for batches. We should actually be paying attention to the batches, not the runs.

## Tuesday 20230307-1415 - 20230307-1430: 15 minutes

The problem is that the run has the old configuration. The run needs to restart when serve restarts.

We also need to adjust run to be based on batches rather than automations.

Should we separate the worker from the runner? No, I think they should be the same. But let's adjust the worker so that it is based on popping from a list.

## Friday 20230310-1500 - 20230310-1515: 15 minutes

```
Option 1: Handle recurring automations in `process_loop`
Option 2: Have a separate process that feeds tasks into the worker
```

## Sunday 20230312-1045 - 20230312-1100: 15 minutes

## Wednesday 20230322-2200 - 20230322-2215: 15 minutes

## Thursday 20230323-1530 - 20230323-1545: 15 minutes

```
/a/make-qr/b/abc/p/pdf
_ /a/make-qr/b/abc/pdf

batch['prints'] = [{
    'uri':
}]
```

I am thinking that pdf should be in the output page.

## Sunday 20230326-0130 - 20230326-0145: 15 minutes

- [Done] Fix imports
- [Done] Update work to pop from a list

## Sunday 20230326-1000 - 20230326-1015: 15 minutes

I am trying to decide whether `print` is a step or if it is a separate section. On the one hand, print is a step because it happens after the output is generated. On the other hand, print is a separate section because unlike the other steps, the scripts do not have access to read or write the print variables.

But if print is a step, the user can configure how the template is displayed. It is likely that the user will want to configure how the template is displayed, both in terms of text and style.

I think print should be a step.

# Tuesday 20230328-1600 - 20230328-1615: 15 minutes

- [Canceled] Restore prints
- [Done] Use prints folder instead of folder unless run
- [Canceled] Update run to push to a list
- [Done] Update POST run to push to a list
- [Canceled] Update streams to push to a list

## Tuesday 20230328-1600 - 20230328-1615: 15 minutes

I am thinking of an alternate structure. I am now thinking that the queue or list should be limited to only on-demand runs and that obsolete detection and interval re-runs should be done separately.

## Wednesday 20230329-1945 - 20230329-2000: 15 minutes

## Thursday 20230330-1315 - 20230330-1330: 15 minutes

- Option 1: Use manager.list to pass definitions
- Option 2: Modify definitions manually in both server and worker process

Option 1 does not work because I am trying to modify a dictionary (`clock.time_by_key` that should actually be a manager dict. So I could pass the manager and initialize a clock for each. But in the end, I think I concluded that manual runs do not need live preview.

- [Done] Regenerate print preview on save script, template, style
- [Done] Use code == f for scripts to prevent page refresh

## Friday 20230331-1145 - 20230331-1200: 15 minutes

## Friday 20230331-1345 - 20230331-1400: 15 minutes

- [Done] Set style for print page calc(100vh - 3.5em)
- [Canceled] Consider restoring live print preview for manual run
- [Done] Detect command file path changes

## Friday 20230331-1515 - 20230331-1530: 15 minutes

- [Done] Show link to open print preview for manual run
- [Done] Find why add-numbers is not refreshing when batches is empty

## Saturday 20230401-1500 - 20230401-1515: 15 minutes

Now the problem is that the input variables are getting saved during prepare batch, which is triggering a re-run.

- [Canceled] Option 1: prepare batch when the run is created before task is created and have actual run wait until prepare batch finishes (will cause duplicate runs)
- [Canceled] Option 2: re-save input variables only if there are changes (this will still trigger a refresh)
- [Canceled] Option 3: start the run time exactly at the run time (this will cause duplicate runs and does not solve the problem)
- [Canceled] Option 4: ignore input variable changes during run (may cause some valid refreshes to be skipped)
- [Canceled] Option 5: ignore input variable changes completely
- Option 6: skip input variable changes that happened during prepare batch by adding another timer for prepare batch
- [Canceled] Option 7: unlearn input variable paths temporarily during prepare batch (i kind of like this option, but then prepare batch needs to know about memory, which is not clean code)

## Saturday 20230401-1700 - 20230401-1715: 15 minutes

- [Done] Fix manual run

## Saturday 20230401-1915 - 20230401-1930: 15 minutes

- [Done] Remove flicker when template is updated for print page

## Saturday 20230401-1945 - 20230401-2000: 15 minutes

- [Done] Add pygments css (either directly or via cdn) for syntax highlighting

## Monday 20230403-0645 - 20230403-0700: 15 minutes

## Monday 20230403-1200 - 20230403-1215: 15 minutes

- [Done] Add download link to file that has customized download name for batch
- [Done] Test refresh on config change
- [Done] Test refresh on script change
- [Done] Test refresh on template change
- [Done] Test refresh on style change
- [Done] Test automation interval
- [Done] Test automation strict interval
- [Done] Test manual run/print

All tests pass with self testing. Now we will set up a server for internal testing.

## Monday 20230403-1230 - 20230403-1245: 15 minutes

- [Done] Consider replacing code == 'c' with either constant or enum
- [Done] Update the framework to make it possible to restrict visible automations and batches based on a token via display.authorization.function = check.authorize and display.authorization.groups
- [Done] Load the token from a cookie or from the url
- [Done] Save authorization keys into input folder or debug folder or auth folder or authorization path so that scripts can use it
- [Done] Implement programmable forms with progressive disclosure in the framework
- [Canceled] Handle http errors using `add_exception_view`
- [Canceled] Make it possible for author to override mapbox version
- [Canceled] Consider compatibility with gunicorn
- [Canceled] Implement ipynb forms
- [Done] Update example forms
- [Done] Update label in all content

## Monday 20230403-1400 - 20230403-1415: 15 minutes

## Tuesday 20230404-1130 - 20230404-1145: 15 minutes

- [Done] Differentiate between datasets and scripts
- [Done] Check that dataset changes will trigger re-runs

```bash
while [ 1 ]; do ls -l run.ipynb; sleep 1; done
```

## Tuesday 20230404-1345 - 20230404-1400: 15 minutes

- [Done] Refresh command string if script changed
- [Done] Check 503 error after config file update (only redirect if 404)

## Wednesday 20230405-1230 - 20230405-1245: 15 minutes

## Wednesday 20230405-1545 - 20230405-1600: 15 minutes

We are running into a problematic race condition when the script runs too fast and the input variable is marked as having been modified after the run finishes. This creates an endless loop.

- [Canceled] Option 1: Ignore changes to input variables
- Option 2: Expand time window by disk poll time

## Wednesday 20230405-2230 - 20230405-2245: 15 minutes

- [Done] Fix `remove_path`
- [Done] Fix grok
- [Done] Test crosscompute --print
- [Done] Add symbolic links in batches if prints are run from command line
- [Done] Check print after manual run
- [Done] Test print using pdf printer
- [Done] Add more specific classes in css example

## Thursday 20230406-0200 - 20230406-0215: 15 minutes

```
{ uri: automation-x://324abc }
_ { uri: crosscompute://324abc }
_ { file: crosscompute://abc123 }
_ { file: test-variable-views://abc123 }
_ { file: automation-x://abc123 }
_ { file: ABC }
```

## Friday 20230414-1500 - 20230414-1515: 15 minutes

```
_ { "uri": "crosscompute://a/automation-x/f/abc123" }
_ { "uri": "crosscompute://automation-x/abc123" }
_ { "uri": "crosscompute://automation-x/f/abc123" }
_ { "uri": "crosscompute://f/abc123" }
_ { "uri": "crosscompute://abc123" }
_ { "uri": "file://abc123" }
_ { "uri": "google://abc123" }
_ { "uri": "/files/abc123" }
{ "uri": "/f/abc123" }
```

## Friday 20230414-1630 - 20230414-1645: 15 minutes

- [Done] Restore upload functionality view=file

# Schedule

# Tasks

- [ ] Communicate when something is running/printing
- [ ] Restore missing examples
- [ ] Add guard to migrations sse
- [ ] Send data from migrations sse
- [ ] Show seconds ticking if in log
- [ ] Let scripts have different behavior with or without identities
- [ ] Consider making it possible to load identities without groups
- [ ] Reconsider token vs cookie
- [ ] Use websockets > eventstream > polling
- [ ] Update default styling for automations
- [ ] Consider making it possible to set break-inside auto for table label and table
- [ ] Review issues for crosscompute
- [ ] Restore select view
- [ ] Make it possible to make a tool from a spreadsheet
- [ ] Create example with multiple scripts
- [ ] Consider specifying empty environment variables for send-emails
- [ ] Document TableView
- [ ] Consider caching a run if caching is enabled in configuration
- [ ] Consider allowing alt text for ImageView
- [ ] Update documentation
- [ ] Consider using cookie prefixes for secure authentication https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie#cookie_prefixes
- [ ] Need to be able to see errors
- [ ] Document steps for creating a new automation
- [ ] Pull from repository if option is enabled
- [ ] Review legacy code to check whether we missed anything
- [ ] Consider crosscompute:// url scheme
- [ ] Make it possible to click on different svg elements and show information
- [ ] Consider splitting views into crosscompute-views to allow alternate versions of all the view plugins
- [ ] Consider cookiecutter
- [ ] Autogenerate datasets/batches.csv if requested during configure
- [ ] Implement count/index variables
- [ ] Consider making it possible to embed via ajax instead of iframe for consistent styling

# Lessons
