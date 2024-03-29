# Log

## Tuesday 20220125-1645 - 20220125-1700: 15 minutes

    pip install -U cookiecutter
    cookiecutter https://github.com/jupyterlab/extension-cookiecutter-js
    cookiecutter https://github.com/jupyterlab/extension-cookiecutter-ts

    _ Start a JupyterLab extension project from scratch using extension-cookiecutter-js
    + Start a JupyterLab extension project from scratch using extension-cookiecutter-ts

It seems like what we need to do is name the python project as `jupyterlab_crosscompute` and then later change the pypi package name.

    + Set name as jupyterlab-crosscompute

I don't think the extension structure has changed much since the last time we updated this extension.

    + [Review hello-world example](https://github.com/jupyterlab/extension-examples/tree/master/hello-world)

## Tuesday 20220125-1945 - 20220125-2000: 15 minutes

    _ Option 1:
        Preview
        Render
        Deploy
    Option 2:
        Launch
        Render
        Deploy
    _ Option 3:
        Run
        Render
        Deploy

    + Define dummy command Launch
    + Define dummy command Render
    + Define dummy command Deploy
    + [Review commands example](https://github.com/jupyterlab/extension-examples/tree/master/commands)
    + [Review command palette example](https://github.com/jupyterlab/extension-examples/blob/master/command-palette/README.md)
    + Add launch, render, deploy to command palette under category CrossCompute

Time for a break!

## Thursday 20220127-0930 - 20220127-0945: 15 minutes

## Thursday 20220127-1130 - 20220127-1145: 15 minutes

    + Find where the right sidebar panel is defined
    + Add an empty sidebar panel

## Thursday 20220127-1330 - 20220127-1345: 15 minutes

    + Add icon to sidebar panel

    _ cc-sidebar
    _ crosscompute-area
    _ crosscompute-sandbox
    _ crosscompute
    _ crosscompute-widget
    _ crosscompute-workspace
    _ crosscompute-panel

    crosscompute-sidebar

Why should we use a class instead of an id for styling? Would it be possible that multiple crosscompute sidebars are added? It could be possible, if multiple automations are being run. But then why not handle everything in the same panel? I think it is safe to assume that there will be only one crosscompute-sidebar.

    + Make the sidebar panel background look like the other sidebar panels

I am also not sure whether we need to use the lumino layout classes.

However the convention seems to be to style everything using classes. Not sure why.

It seems like we have a choice between rendering the widget as a lumino widget or react widget.

For now, these are details.

We need to walk the dog.

    + Define .crosscompute-Window
    + Add a Launch button to the sidebar panel

We cannot append widgets to dom nodes but we can put widgets in panels.

## Friday 20220128-1030 - 20220128-1045: 15 minutes

    + Connect the Launch switch with a server route (POST)

It seems there are two ways to get the file browser path:

```javascript
function getFileBrowserPath(): string {
  try {
    let leftSidebarItems = shell.widgets('left');
    let fileBrowser = leftSidebarItems.next();
    while (fileBrowser.id !== 'filebrowser') {
      fileBrowser = leftSidebarItems.next();
    }
  return (fileBrowser as any).model.path;
  } catch (err) {}
}

const basePath =
((args['basePath'] as string) ||
(args['cwd'] as string) ||
browserFactory?.defaultBrowser.model.path) ??
'';
```

    _ Option 1: [Get file browser widget and access model](https://github.com/jupyterlab/jupyterlab-git/blob/f2abd5250f22657427a73e0b11340033917320bf/src/gitMenuCommands.ts#L44-L71)
    Option 2: [Use browser factory](https://github.com/jupyterlab/jupyterlab/blob/master/packages/console-extension/src/index.ts#L559)

## Friday 20220128-1130 - 20220128-1145: 15 minutes

Kashfi says to make the side panel pay attention to which automation we are in the file browser.

    + Connect the Launch switch with a server route (POST)

## Monday 20220131-1815 - 20220131-1830: 15 minutes

    + Disable render and deploy for now

## Tuesday 20220201-0915 - 20220201-0930: 15 minutes

    + See if we can load the jupyterlab settings
    + Get value for ServerApp.ip
    + Update to use python to launch

## Tuesday 20220201-1015 - 20220201-1030: 15 minutes

    + Get randomly available port
    + Check that we can properly launch an automation

## Tuesday 20220201-1230 - 20220201-1245: 15 minutes

## Tuesday 20220201-1300 - 20220201-1315: 15 minutes

    + Revert to serving crosscompute directly instead of via subprocess

    _ Option 1: Render error by directly modifying div
    Option 2: Use lumino signal
    Option 3: Use react

Lumino signaling is based on callbacks while React is based on declarative state. I think in this case, React will be better.

In the previous version, we listened to server sent events to update the log component.

I don't think I can use React in JupyterLab to communicate across widgets though. I think we will need to use lumino signaling.

## Tuesday 20220201-1745 - 20220201-1800: 15 minutes

I have two options on how to implement panel.

    Option 1: Implement panel as ReactWidget + UseSignal on pathChanged or modelChanged event
        DIS: cannot use lumino switch widget
    _ Option 2: Update panel in the modelChanged callback and use the ui-components/switch widget

    pathChanged
        update model
        signal refresh
    error from fetch
        update model
        signal refresh
    success from fetch
        update model
        signal refresh

I want to do a fetch on pathChanged to get the automation information, including the path to the batch configuration. I think that if I eventually want to add more user experience enhancements to the panel in the future (such as renaming the automation), it would be better to use the ReactWidget.

That means that we are going to change the panel to be a ReactWidget. We are going to define a signal in the widget so that other events can update the model.

## Tuesday 20220201-1930 - 20220201-1945: 15 minutes

## Wednesday 20220202-2215 - 20220202-2230: 15 minutes

    + Load configuration on each path change
    + Define automationBody.onUpdate
    + Check how we can subscribe to filebrowser updates
    + Say automation name and version in sidebar based on file browser path

## Thursday 20220203-1215 - 20220203-1230: 15 minutes

    Edit Configuration
    Edit Batches
    _ Edit Automation Configuration
    _ Edit Batches Configuration
    _ Edit Batch Configuration

    + Render error
    + Debug "format not supported for configuration"
    + Get automation to show on load
    + Show default message if configuration not found
    _ Change switch color to green
    _ Add description captions on hover over launch button

## Thursday 20220203-1530 - 20220203-1545: 15 minutes

    + Add launch button
    + Implement Edit Batches

## Friday 20220204-1230 - 20220204-1245: 15 minutes

    + Use flex for launch button
    + Implement mock experience for launch button
    + Make uri clickable

## Saturday 20220205-1130 - 20220205-1145: 15 minutes

```
45689─45691─┬─45693
            ├─45695─┬─45697
            │       ├─{45698} waitress thread
            │       ├─{45699} waitress thread
            │       ├─{45700} waitress thread
            │       └─{45701} waitress thread
            └─{45696}
```

I feel like there are too many processes in the command line script. Let's identify which one is doing what.

```
2022-02-05 12:11:13 .macros.process DEBUG started serve process 51217
2022-02-05 12:11:13 .macros.process DEBUG started run process 51218
2022-02-05 12:11:13 .macros.process DEBUG started browser daemon process 51219
2022-02-05 12:11:13 .macros.process DEBUG started server process 51221
2022-02-05 12:11:13 .macros.process DEBUG started worker daemon process 51222

root (51215)─┬─serve (51217)─┬─ browser daemon (51219)
             │               ├─ server (51221)─┬─ worker daemon (51222)
             │               │                 ├─ waitress {thread} (51224)
             │               │                 ├─ waitress {thread} (51225)
             │               │                 ├─ waitress {thread} (51226)
             │               │                 └─ waitress {thread} (51227)
             │               └─ watch {thread via watchgod.main.awatch} (51223)
             └─run (51218)
```

I learned about different process start methods:
- https://stackoverflow.com/questions/63424251/multiprocessing-in-python-what-gets-inherited-by-forkserver-process-from-paren
- http://www.bnikolic.co.uk/blog/python/parallelism/2019/11/13/python-forkserver-preload.html
- https://pythonspeed.com/articles/python-multiprocessing/

The conclusion is that there are currently 3 ways that multiprocessing can start a process: 
- fork (does not copy threads into child process)
- spawn (copies everything but is slow)
- forkserver (specify what to copy into the child process with `set_forkserver_preload`)

## Saturday 20220205-1430 - 20220205-1445: 15 minutes

## Sunday 20220206-1045 - 20220206-1100: 15 minutes

	+ See how we can kill the child processes
		+ Try using subprocess
		_ Record child process ids
    + Fix daemonic process error
    _ Revert to running crosscompute directly instead of via subprocess
    + Check that we can properly stop the automation server

## Monday 20220207-1430 - 20220207-1445: 15 minutes

I am thinking that we should reuse the repository crosscompute-prints.

    _ crosscompute-prints
    _ crosscompute-printer
    crosscompute-printers

We decided to use puppeteer instead of pyppeteer because of lagging changes in puppeteer.

    + Write a script that prints a page given a url and a target path
    + Get the url and the target path from the command line

```bash
curl -X POST localhost:8718/api/users -d "id=a&token=bc"
```

    + Write a script that starts a server and prints the POST parameters
    + Update framework to update variables.dictionary in `debug_folder`

## Wednesday 20220216-1100 - 20220216-1115: 15 minutes

    _ Option 1: Define oldFolder in model
        DIS: Seems oldFolder is more UI than data
    Option 2: Define oldFolder in AutomationControl
        DIS: Need to pass either browser folder or current folder

Let's try modelling both approaches. I think we need to use Option 2 because it is possible that openFolder is used in different ways.

It seems like browser.model.cd does not accept absolute paths.

    _ Option 1: Compute relpath in browser

Actually, it seems like the definition of '/' is with respect to the folder where we started JupyterLab.

    + Make it possible to go back after jumping to a batch folder

## Wednesday 20220216-1230 - 20220216-1245: 15 minutes

## Wednesday 20220216-1445 - 20220216-1500: 15 minutes

## Thursday 20220217-1000 - 20220217-1015: 15 minutes

    _ automationModel.launch.text
    _ automationModel.launch.log
    automationModel.launch.log.text

    + Define temporary folder for logs
    + Delete temporary folder on exit
    + Save stdout and stderr to log file
    + Define LogHandler.get
    + Poll LogHandler.get on launch
    + Populate automationModel.launch.log.text
    + Render stdout and stderr for launch

## Friday 20220218-1015 - 20220218-1030: 15 minutes

    + Use proxy if https

## Saturday 20220219-0845 - 20220219-0900: 15 minutes

    + Revert namespace to jupyterlab-crosscompute

## Tuesday 20220222-0715 - 20220222-0730: 15 minutes

    + Restart from cookiecutter
    + Use dirdiff to review differences
    + Test cookiecutter version
    + Restore side panel
    + Render updated path

## Tuesday 20220222-1100 - 20220222-1115: 15 minutes

    + Render automation information
    + Restore batch information

## Tuesday 20220222-2230 - 20220222-2245: 15 minutes

    + Restore launch control
    + Add link to edit configuration if there is a configuration error
    + Fix issue with concurrent logs

## Sunday 20220306-1630 - 20220306-1645: 15 minutes

I believe we can get the output url for each batch from the automation definition. Then I want to check that each output url is active and that the script has finished running. Next, we will print each url. Finally, we will gather the pdfs in a zip and trigger a download if possible or render a link.

The first step is to write this simple script. Technically, I think this script could be written in nodejs.

## Monday 20220307-0445 - 20220307-0500: 15 minutes

We decided to use nodejs because puppeteer is better supported than pyppeteer. In the future, we could have an alternate plugin that uses pyppeteer. I think both implementations could be in a package called crosscompute-renderers-pdf. Or we could have separate packages called crosscompute-renderers-pdf-puppeteer and crosscompute-renderers-pdf-pyppeteer. For now, I think we can simply have crosscompute-renderers-pdf. I do not see the need for split packages. Or it could even just be in crosscompute-renderers.

We also decided that the node script will take a CSV or json file and render the pdfs to a folder. The csv or json has the batch names and uris. We will also add a route that lists all batch names and uris, perhaps as a json. The node script will check each debug uri and if the batch is finished, then it will generate a pdf of the output uri. We may also need to revive the page number footer. Finally, we will need to integrate the code with the extension. Option 1 is to have the render button duplicate the launch. Option 2 is to have the render button require a successful launch. I think we should go with option 1, because the render requires different settings than launch, such as no auto refresh.

## Monday 20220307-0515 - 20220307-0530: 15 minutes

Actually, I do not think it needs to be a route.

- [Cancelled] Define route /batches.json that gets all batch names and uris
- [Done] Write a python script that gets all batch names and uris

## Monday 20220307-0530 - 20220307-0545: 15 minutes

Why call it renderers anyway? Why not just return to the old name printers? Let's stick with printers. The reason is that it has fewer syllables.

- [Cancelled] Rename crosscompute-printers to crosscompute-renderers

Another option is that we can make the nodejs script super lightweight as a server. Specify an output folder when starting the folder. Then make requests that specify the uri and name.

## Monday 20220307-0730 - 20220307-0745: 15 minutes

- [Done] Write a nodejs script that is a basic server that generates pdfs given a uri and a name

## Monday 20220307-0800 - 20220307-0815: 15 minutes

I don't think puppeteer can really be used asynchronously.

## Monday 20220307-1130 - 20220307-1145: 15 minutes

- [Done] Write a python script that checks whether debug has `return_code`
- [Done] Write a python script that takes batches.json and checks debug then prints output

## Monday 20220307-1145 - 20220307-1200: 15 minutes

- [Done] Rename crosscompute-printers to crosscompute-printers-pdf
- [Done] Implement crosscompute --print pdf --prints-folder /tmp/a

## Monday 20220418-1200 - 20220418-1215: 15 minutes

- [Done] Restore running batches in parallel using ThreadPoolExecutor

# Schedule

# Tasks

- [ ] Update extension to show render button
- [ ] Update extension to show render uri
- [ ] Implement print
- [ ] Document `pip install jupyterlab-spreadsheet-editor`
- [ ] Add links to open scripts in side panel
