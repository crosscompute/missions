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

# Schedule

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
