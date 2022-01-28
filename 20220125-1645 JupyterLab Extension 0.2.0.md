# Log

## Tuesday 20220125-1645 - 20220125-1700: 15 minutes

    pip install -U cookiecutter
    cookiecutter https://github.com/jupyterlab/extension-cookiecutter-js
    cookiecutter https://github.com/jupyterlab/extension-cookiecutter-ts

    _ Start a JupyterLab extension project from scratch using extension-cookiecutter-js
    + Start a JupyterLab extension project from scratch using extension-cookiecutter-ts

It seems like what we need to do is name the python project as jupyterlab_crosscompute and then later change the pypi package name.

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

# Schedule

    Connect the Launch switch with a server route (POST)

# Tasks

    Update the server route to start the crosscompute self-contained server
