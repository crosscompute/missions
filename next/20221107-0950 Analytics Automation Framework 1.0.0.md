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

## Friday 20230120-1515 - 20230120-1530: 15 minutes

- [Done] Use `get_data_from`
- [Done] Remove `b.get_data_from_request`
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

# Schedule

- [ ] Combine with refresh and with restart into `with_production`
- [ ] Stop mutations if there are no expected updates
- [ ] Consider replacing code == 'c' with either constant or enum
- [ ] Finish migration from pyramid to fastapi

# Tasks

- [ ] Let scripts have different behavior with or without identities
- [ ] Consider making it possible to load identities without groups
- [ ] Reconsider token vs cookie
- [ ] Use websockets > eventstream > polling
- [ ] Test that we can override root template
- [ ] Update default styling for automations
- [ ] Restore upload functionality
- [ ] Restore missing examples
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
- [ ] Update the framework to make it possible to restrict visible automations and batches based on a token via display.authorization.function = check.authorize and display.authorization.groups
- [ ] Consider using cookie prefixes for secure authentication https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie#cookie_prefixes
- [ ] Load the token from a cookie or from the url
- [ ] Save authorization keys into input folder or debug folder or auth folder or authorization path so that scripts can use it
- [ ] Implement programmable forms with progressive disclosure in the framework
- [ ] Handle http errors using `add_exception_view`
- [ ] Need to be able to see errors
- [ ] Document steps for creating a new automation
- [ ] Pull from repository if option is enabled
- [ ] Make it possible for author to override mapbox version
- [ ] Review legacy code to check whether we missed anything
- [ ] Consider crosscompute:// url scheme
- [ ] Make it possible to click on different svg elements and show information
- [ ] Consider splitting views into crosscompute-views to allow alternate versions of all the view plugins
- [ ] Consider cookiecutter
- [ ] Consider compatibility with gunicorn
- [ ] Autogenerate datasets/batches.csv if requested during configure
- [ ] Implement count/index variables
- [ ] Consider making it possible to embed via ajax instead of iframe
- [ ] Update example forms
- [ ] Implement ipynb forms

# Lessons
