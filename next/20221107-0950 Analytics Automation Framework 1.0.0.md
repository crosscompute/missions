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

# Schedule

- [ ] Prototype dynamic checkbox
- [ ] Finish migration to fastapi
- [ ] Reconsider token vs cookie
- [ ] Option 1: Have the GET request save token in cookie
- [ ] Let scripts have different behavior with or without identities
- [ ] Implement basic `see_automation`
- [ ] Implement basic `see_automation_batch_mode`
- [ ] Implement basic `see_automation_batch_mode_variable`
- [ ] Implement basic `run_automation`
- [ ] Implement qrcode view

# Tasks

- [ ] Consider making it possible to load identities without groups
- [ ] check requests dependency
- [ ] Use websockets > eventstream > polling
- [ ] Test that we can override root template
- [ ] Update default styling for automations
- [ ] Embed css in page for local css
- [30] Restore upload functionality
- [ ] Restore missing examples
- [ ] Consider making it possible to set break-inside auto for table label and table
- [ ] Implement checkbox view
- [ ] Review issues for crosscompute
- [ ] Restore select view
- [ ] Make it possible to make a tool from a spreadsheet
- [ ] Create example with multiple scripts
- [ ] Consider specifying empty environment variables for send-emails
- [ ] Document TableView
- [ ] Consider caching a run if caching is enabled in configuration
- [ ] Redirect to log if it is defined
- [ ] Redirect to output once variables.dictionary exists in `debug_folder`
- [ ] Consider allowing alt text for ImageView
- [ ] Update documentation
- [ ] Update the framework to make it possible to restrict visible automations and batches based on a token via display.authorization.function = check.authorize and display.authorization.groups
- [ ] Consider using cookie prefixes for secure authentication https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie#cookie_prefixes
- [ ] Load the token from a cookie or from the url
- [ ] Save authorization keys into input folder or debug folder or auth folder or authorization path so that scripts can use it
- [ ] Update the framework to make it possible to define a script.schedule
- [ ] Implement programmable forms with progressive disclosure in the framework 
- [ ] Create a checkbox input view
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

# Lessons
