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
1. [Done] Restrict visible automations and batches and change script behavior using cookies.
2. [Done] Implement script.schedule.
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

## Tuesday 20220412-1730 - 20220412-1745: 15 minutes

- [Done] Release crosscompute 0.9.2
- [Done] Release crosscompute-views-map
- [Done] Release jupyterlab-crosscompute

## Monday 20220418-1600 - 20220418-1615: 15 minutes

- [Done] Search for available port unless specified, starting with the default

## Thursday 20220421-1230 - 20220421-1245: 15 minutes

```
ACCEPT
DROP
REJECT

authorizations:
  - id: automation
  - id: batch
authorization:
  authorization:
  default:
  root:
  automation: run, see
  batch:
  run:
  mode:
  variable:
  style:
permissions
clearances:
authorization:
  default: DROP
  configuration:
    see-automation:
    see-batch: 
    run-batch: 

POST /authorizations.json
    data_by_id = {} => {token}

see root or not
see automation or not
see batch or not
see run or not
see mode or not
see variable or not (protected by see batch)
see style or not
see mutations or not
```

If a token is passed via cookie, bearer or `_token` get parameter, then put into authorization folder for script.

- Authorize see batch -- check whether token variable value matches batch variable value
- whether it is listed
- whether it is rendered
- whether can run

    _ Option 1: { variables: {} }
    Option 2: { aa, bb }

- secure by ip
- secure by variable value

```
firewall-cmd --permanent --add-rich-rule="rule family='ipv4' source address='192.168.0.111' reject"

_ zone
group

authorization:
  groups:
    - id: gods
    - id: leaders
    - id: members
    - id: spectators
  default:
    source:
      address: *
      action: reject
    source:
      address: 192.168.0.111
      action: accept
  configuration:
    see-batch:
      group: spectators
```

## Thursday 20220428-2045 - 20220428-2100: 15 minutes

## Thursday 20220428-2300 - 20220428-2315: 15 minutes

I am thinking that the top-level authorization should override imported authorizations.

## Friday 20220429-1130 - 20220429-1145: 15 minutes

## Friday 20220429-1130 - 20220429-1145: 15 minutes

Use auth configuration if it is defined, otherwise check parent.

I am thinking that maybe we should use something other than crontab syntax for the schedule.

## Friday 20220429-1845 - 20220429-1900: 15 minutes

I am now thinking that instead of script.schedule we can have script.interval = 5 minutes. The question is where to implement the interval.

    Option 1: environment.interval
    _ Option 2: script.interval

    script.interval = 5 minutes
    script.interval = 1 minute
    script.interval = 1 week
    script.interval = 1 day
    script.interval = 2 days
    environment.interval = 1 minute

    _ Option 1: Use schedule
    Option 2: Roll our own schedule

I think that if I can just finish environment.interval during this flight, then it will be a success.

- [Cancelled] Save variables.dictionary for extra variables

## Sunday 20220501-1230 - 20220501-1245: 15 minutes

```
https://github.com/crosscompute/crosscompute-examples/tree/1db460a1d9adf11ca28732b4e7fea7aa0890d264
+ add-integers
compare-timestamps
convert-timestamps
count-words
find-primes
find-primes-scala
flip-wkt
transform coordinates
https://github.com/crosscompute/crosscompute-examples/tree/b341994d9353206fe0ee6b69688e70999bc2196e
reports/google-trends
reports/load-profiles
tools/add-columns
+ tools/add-numbers
tools/add-watermark-pdf
tools/compare-datestamps
tools/convert-timestamps
tools/count-assets
tools/count-geometries
tools/count-words
tools/encrypt-pdf
tools/extract-images-pdf
tools/extract-pdf-pages
tools/find-primes
tools/flip-wkt
tools/map-addresses
tools/onsset-test
tools/remove-password-pdf
tools/set-points
tools/shift-timezones
tools/subset-features
tools/survey-infrastructure
tools/test-errors
tools/transform-coordinates
https://github.com/crosscompute/crosscompute-examples/tree/31536cfd33acbd79a18ed5059941c83250bb95e9
automate.yml
dashboards/watch-machine/automate.yml
forms/encourage-creativity/automate.yml
forms/encourage-exercise/automate.yml
reports/compute-logarithms/automate.yml
reports/map-schools/automate.yml
reports/randomize-histograms/automate.yml
reports/run-onsset/automate.yml
reports/show-maps/automate.yml
+ tools/add-numbers/automate.yml
tools/ask-question/automate.yml
tools/find-places/automate.yml
tools/send-emails/automate.yml
+ widgets/paint-letters/automate.yml
widgets/watch-cpu-history/automate.yml
+ widgets/watch-cpu/automate.yml
widgets/watch-ram-history/automate.yml
widgets/watch-ram/automate.yml
```

- [Done] Identify past examples

## Monday 20220502-0815 - 20220502-0830: 15 minutes

- [Done] Restore and test watch cpu example
- [Done] Test environment.interval
- [Done] Implement environment.interval

## Friday 20220506-1245 - 20220506-1300: 15 minutes

## Thursday 20220512-1745 - 20220512-1800: 15 minutes

```
routines/batch.py:46:        variable_data = load_variable_data(path, variable_id)
routines/automation.py:151:  process_data=load_variable_data)
routines/automation.py:179:  process_data = load_variable_data
routines/automation.py:190:  process_data = load_variable_data
routines/variable.py:434:    variable_data = load_variable_data(folder / variable_path, variable_id)
routines/variable.py:517:    def load_variable_data(path, variable_id):
```

## Sunday 20220515-0030 - 20220515-0045: 15 minutes

- It looks like `load_file_data` loads the value only if the suffix is a dictionary and it can be loaded as a dictionary. I found a possible error.
- It looks like `get_value` is used in StringView and all StringView derivatives.
- Let's map the current flows of what happens when the path suffix is txt and how returning {value} instead of {path} will affect each flow.

    batch.get_data
        see_automation_batch_mode_variable
            case 1: txt {path}
                return file response
            case 2: txt {value}
                return value response directly
        StringView.get_value
            StringView.render_input: NumberView, PasswordView, EmailView
                case 1: txt {path}
                    load from FILE_TEXT_CACHE
                case 2: txt {value}
                    return directly
            StringView.render_output: NumberView, PasswordView, EmailView
                case 1: txt {path}
                    load from FILE_TEXT_CACHE
                case 2: txt {value}
                    return directly
    _process_batch loads variables after running a batch
        _run_batch
            work
            _run_automation_single
            _run_automation_multiple
    get_data_by_id_from_folder loads data_by_id from a batch folder
        get_batch_definitions for reference_data_by_id
        get_data_by_id
            get_batch_dictionaries
                print_with

    Option 1: Return txt value if file is small or path if file is large
    _ Option 2: Return txt value always
    _ Option 3: Return txt path always (current)

- [Done] See where load_variable_data is used

## Monday 20220516-1200 - 20220516-1215: 15 minutes

    Option 3: Return txt path always (current)
        batch.get_data
            see_automation_batch_mode_variable will load from disk for both large and small
        StringView.get_value
            StringView will load from file text cache for both large and small
        _process_batch will load path for text but the values are not really used
        get_data_by_id_from_folder loads data_by_id from a batch folder and needs a patch to load text values, used when variables are used in folder names or file names

However, if we go by option 2 to return txt value always, then data_by_id could get very large for large text files.

I think that Option 1 could give a good mix between speed and memory optimization.

The maximum file name length on most operating systems is 255.

    _ Option 1: Stick to selective patch for get_data_by_id_from_folder
    Option 2: Modify load_file_data and remove FILE_TEXT_CACHE

Right now the issue is that StringView could still get very large and be stuffed in FILE_TEXT_CACHE. I think we should remove FILE_TEXT_CACHE and only cache small files.

Then the question is what defines a small file. The maximum size in bytes of a utf8 character is 4 bytes.

```
For example, UTF-8 is based on 8-bit code units. Therefore, each character can be 8 bits (1 byte), 16 bits (2 bytes), 24 bits (3 bytes), or 32 bits (4 bytes). Likewise, UTF-16 is based on 16-bit code units. Therefore, each character can be 16 bits (2 bytes) or 32 bits (4 bytes).

-- https://www.ibm.com/docs/en/db2-for-zos/12?topic=unicode-utfs
```

## Tuesday 20220517-1100 - 20220517-1115: 15 minutes

- [Done] Update load_file_data and remove FILE_TEXT_CACHE
- [Done] Write test for load_file_data

## Saturday 20220521-1030 - 20220521-1045: 15 minutes

## Monday 20220523-1500 - 20220523-1515: 15 minutes

## Wednesday 20220525-1130 - 20220525-1145: 15 minutes

- [Done] Implement authorizations.json

## Thursday 20220526-2100 - 20220526-2115: 15 minutes

- [Done] Restrict visible automations and batches and change script behavior using cookies

## Saturday 20220604-1830 - 20220604-1845: 15 minutes

- [Done] Consider renaming `base_path` to `root_path`

The question is whether we should use volumes or transfer the data in and out. I think we need to transfer the data because then we could potentially delegate to more workers. We would probably need to write to a kind of database if there are multiple workers though.

    Option 1: Run using the script command
    _ Option 2: Run using a copy of crosscompute inside the container

I don't think that crosscompute should be inside the container because its dependencies could potentially contaminate the runtime environment.

## Tuesday 20220607-1515 - 20220607-1530: 15 minutes

We'll put containers into the open source version, which will be more secure when running untrusted scripts.

    _ Option 1: Run on a timer
    _ Option 2: Run while a file exists
    _ Option 3: Run while a file does not exist
    Option 4: Loop forever using sleep infinity

I did a little research and it seems that `sleep infinity` will be better than `while true loop forever`. We can kill the container from the outside and in general I do not think we should rely on internal mechanisms to kill the container.

```
vim Containerfile
    FROM python
    RUN \
    useradd user
    USER user
    WORKDIR /home/user
    COPY --chown=user:user x.txt .
    CMD ["sleep", "infinity"]
vim Containerfile
    FROM python
    RUN \
    useradd user
    USER user
    WORKDIR /home/user
    COPY --chown=user:user a .
    CMD ["sleep", "infinity"]
```

Is it necessary to run the process as non root? It seems like it is. I'm not sure about how to handle cases where the user wants to install system packages that need sudo to run. And should we allow arbitrary images or whitelist those that are allowed?

- [Done] Check permissions created by COPY file after USER
- [Done] Check permissions created by COPY folder after USER

In both cases, the resulting file and folder are owned by user, which is what we want.

The question is, how often will the vendor need to install packages via sudo? Maybe the vendor can use pkcon?

[The following article seems to indicate that letting an unprivileged user install system packages is dangerous](https://sysdream.com/news/lab/2020-05-25-abusing-packagekit-on-fedora-centos-for-fun-profit-from-wheel-to-root/).

It seems like pkcon is not available by default from the default python container.

- [Done] Check whether pkcon is runnable from the default python container

```
FROM python
CMD ["sleep", "infinity"]
```

```
apt install -y packagekit-tools
```

Well, it seems like we cannot run systemd inside a container. Or running systemd is possible, but it seems to be complicated ([see link](https://zauner.nllk.net/post/0038-running-systemd-inside-a-docker-container)).

- [Done] Install pycon

[It seems like PackageKit was abandoned lately](https://blogs.gnome.org/hughsie/2019/02/14/packagekit-is-dead-long-live-well-something-else). Instead, people are using something called flatpak.

The flatpak command is installable. But flatpak seems to be a way to package desktop apps, which is not what we need. I think the majority of use cases can be covered by official images. We can prepare images with whitelisted system packages for the edge caseso

Or maybe we can allow dnf/apt installs if the software is in certain approved repositories like the default packages or rpmfusion.

```
environment:
  image: fedora
  packages:
    - id: pandas
      manager: pip
    - id: chromium
      manager: dnf
    - id: ffmpeg
      manager: dnf
      repository: rpmfusion-free
```

## Saturday 20220611-0945 - 20220611-1000: 15 minutes

Yesterday, we ran a number of experiments with podman containers. I think that today we can finish integrating containers with the public framework.

```
environment:
  image: fedora
  packages:
    - id: chromium
      manager: dnf
  container: podman
```

```
FROM fedora
RUN
dnf install chromium -y && \
dnf clean all && \
useradd user
USER user
WORKDIR /home/user
CMD ["sleep", "infinity"]
```

```
FROM python
RUN useradd user
USER user
WORKDIR /home/user
COPY --chown=user:user . .
CMD ["sleep", "infinity"]
```

```
IMAGE_TAG=add-numbers:0.1.0
podman build -t $IMAGE_TAG -f .containerfile
CONTAINER_ID=$(podman run -d $IMAGE_TAG)
podman cp runs/2-3 $CONTAINER_ID:runs/
```

Score!

- [Done] Copy custom batch into container
- [Done] Make a dummy container using the source image
- [Done] Have the container sleep infinity

## Sunday 20220612-0900 - 20220612-0915: 15 minutes

```
# .containerignore
.containerfile
.containerignore
.gitignore
batches/
runs/
tests/
```

```
podman exec $CONTAINER_ID ls --color -la
```

```
FROM python
RUN useradd user --create-home
USER user
WORKDIR /home/user
RUN mkdir runs
COPY --chown=user:user . .
CMD ["sleep", "infinity"]
```

- [Cancelled] Option 1: Use run.sh to make folders
- [Cancelled] Option 2: Use exec to make folders
- Option 3: Make folders in runs/2-3 ahead of time

```
podman build -t $IMAGE_TAG -f .containerfile
CONTAINER_ID=$(podman run -d $IMAGE_TAG)
# BEGIN prepare runs/2-3 
mkdir runs/2-3/input runs/2-3/output -p
# END prepare runs/2-3
podman cp runs/2-3 $CONTAINER_ID:runs/
podman exec $CONTAINER_ID python run.py runs/2-3/input runs/2-3/output
podman cp $CONTAINER_ID:runs/2-3 runs
```

- [Done] Run a custom batch
- [Done] Extract batch folder

## Sunday 20220612-0930 - 20220612-0945: 15 minutes

In this bit, we will integrate what we have learned into the framework. We will make it work with the add-numbers example first. Then I think we will have to add support for dependencies.

```
environment:
  image: fedora
  container: podman

container:
  image: fedora
  manager: podman
  packages:
    - id: chromium
      manager: dnf
```

I am debating whether container should be separate from environment. The reason is that certain options should be strictly relative to the container. I do agree that those options should be inside container. Now the question is whether container is inside environment. I think container should be inside environment. It is part of the execution environment.

I am thinking, however, that maybe manager is specified outside and not inside. The reason is that it would be hard to have different automations using different managers e.g. podman.

```
crosscompute --run --engine podman
_ crosscompute --run --container podman
_ crosscompute --run --container-manager podman
_ crosscompute --run --manager podman
```

To remove ambiguity between container managers and package managers, we'll just call it the container engine or engine for short. Other options could be subprocess or docker or kubectl/kubernetes.

- [Cancelled] Construct run.sh
- [Done] Define container configuration in add-numbers

## Friday 20220617-1200 - 20220617-1215: 15 minutes

- [Done] Prepare .containerfile and .containerignore
- [Done] Build container

I can make the folders on the local side, but then there might be files that get copied unnecessarily if the folders like output already exist and are uploaded into the container. I think it is better to make the folders in the container in advance.

- [Done] Put files into the container
- [Done] Run container

It seems like I can run the podman service via API if I don't want to use subprocess. I think subprocess is fine for now.

```
pip install podman
podman system service -t 120 &
ipython
    from podman import PodmanClient
    with PodmanClient() as client:
        print(client.version())
```

- [Done] Get files from the container
- [Done] Kill the container
- [Done] Prototype script that runs a batch using the container

## Friday 20220617-1500 - 20220617-1515: 15 minutes

- [Cancelled] Option 1 is to monkeypatch `_run_batch` based on engine
- [Cancelled] Option 2 is to set a constant based on engine
- Option 3 is to pass an argument based on engine

I added a command line option called engine.

- [Done] Add new command line option called engine
- [Done] Show warning if engine is raw

## Saturday 20220618-0930 - 20220618-0945: 15 minutes

- [Done] Parse container configuration to get container base image and command arguments

    run_batch_raw
    run_batch_podman
    run_batch_with_raw
    run_batch_with_podman

- Option 1: Pass `run_batch`
- [Cancelled] Option 1: Pass engine

I think passing the function will be easier to test. I think this design pattern is called dependency injection, to use an academic term.

I am revisiting the decision on whether to specify batch concurrency in the configuration file or command line. I think the batch concurrency configuration belongs in the configuration file because an automation might be designed to only run one at a time.

## Sunday 20220619-1130 - 20220619-1145: 15 minutes

- [Done] Define get_automation_engine

## Monday 20220620-1445 - 20220620-1500: 15 minutes

- [Done] Have the run command respect engine
- [Done] Define UnsafeEngine

```
environment:
  container:
    image: fedora
    packages:
      - id: chromium
        manager: dnf
```

## Friday 20220624-1045 - 20220624-1100: 15 minutes

```
environment:
  image: fedora
  packages:
    - id: chromium
      manager: dnf
```

```
map-mapbox-location
```

## Friday 20220624-1645 - 20220624-1700: 15 minutes

```
[Cancelled] Option 1: Store center in hidden input on map move
    ADV: Can be used by other widgets on page
    DIS: causes unnecessary slow down
Option 2: Get center on save by having look up dictionary by element id
```

## Monday 20220627-1100 - 20220627-1115: 15 minutes

Let's focus on finishing the podman integration first, implement `--no-patience`, update map-mapbox-location marker placement, implement the outage map, update all example forms.

    --no-patience
    --no-browser

- [Cancelled] Get container_definition
- [Done] Define PodmanEngine
- [Done] Integrate prototyped script
- [Cancelled] Introduce a new locking mechanism because flock won't work

```
Option 1: Use --env
Option 2: Use --env-file
```

I am thinking that using an env file might be safer.

- [Done] Finishing remaining TODO for podman integration
- [Done] Install container packages

## Monday 20220627-1645 - 20220627-1700: 15 minutes

    --no-restart
    --no-refresh
    --no-rebuild

## Tuesday 20220628-1545 - 20220628-1600: 15 minutes

- [Done] Rename --production to --no-restart
- [Done] Rename --static to --no-refresh
- [Done] Implement --no-rebuild

# Schedule

- [ ] Update map-mapbox-location marker placement
- [ ] Update example forms

# Tasks

- [30] Restore upload functionality
- [ ] Restore missing examples
- [ ] Consider making it possible to set break-inside auto for table label and table
- [ ] Implement qrcode view
- [ ] Implement checkbox view
- [ ] Implement ipynb forms
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
- [ ] Handle http errors using add_exception_view
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

# Milestones
# Lessons
