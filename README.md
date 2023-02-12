# i3-projects
A simple script that allows workspaces to be saved and later recalled using `i3-save-tree` and `append_layout`.

## Usage
Run the script.
```
usage: project-save [-h] [-w WORKSPACE] [-v] [-o] name

i3 workspace project saver

positional arguments:
  name                  name of project

options:
  -h, --help            show this help message and exit
  -w WORKSPACE, --workspace WORKSPACE
                        workspace to initialize to
  -v, --verbose         verbose logging
  -o, --override        force override of files
```

From the output results, modify the script and json that represents your workspace.

The template included is a Jinja template, which can permit modification to what's generated, or you can modify one or both files afterward.

Generally, you'll need to modify at least the .json file to select the appropriate swallows for i3. More information can be found [here](https://i3wm.org/docs/layout-saving.html)

## Installation
Pull the repository and put `project-save` on path.

The project also requires installing `jq` and `Jinja` from path:
```
pip install jq
pip install Jinja2
```

## Examples
The following is an example setup using this automation to setup a chrome browser, a gnome-terminal, and VS code:

The main script, which swaps to workspace "3", clears it out, and then appends the configued windows:
```
#!/bin/sh

i3-msg "workspace 3; [workspace=3] kill; append_layout /home/mezner/bin/project-i3-projects.json"

# Add shell commands to open applications here!
/snap/bin/code -n $HOME/src/i3-projects/i3-projects.code-workspace
gnome-terminal
google-chrome https://github.com/Mezner/i3-projects
```

Note that you may want to choose to run these in parallel.

The JSON for i3 (note that the only things modified by hand were the `swallows`):
```
// For stacks and splits, see: https://i3wm.org/docs/layout-saving.html
// vim:ts=4:sw=4:et
{
    // splitv split container with 2 children
    "border": "normal",
    "floating": "auto_off",
    "layout": "splitv",
    "marks": [],
    "percent": 0.3,
    "type": "con",
    "nodes": [
        {
            "border": "normal",
            "current_border_width": 2,
            "floating": "auto_off",
            "geometry": {
               "height": 531,
               "width": 814,
               "x": 0,
               "y": 0
            },
            "marks": [],
            "name": "project-save -w 3 i3-projects",
            "percent": 0.5,
            "swallows": [
               {
                  "class": "^Gnome\\-terminal$"
               }
            ],
            "type": "con"
        },
        {
            "border": "normal",
            "current_border_width": 2,
            "floating": "auto_off",
            "geometry": {
               "height": 2078,
               "width": 1647,
               "x": 4741,
               "y": 26
            },
            "marks": [],
            "name": "New Tab - Google Chrome",
            "percent": 0.5,
            "swallows": [
               {
                  "class": "^Google\\-chrome$"
               }
            ],
            "type": "con"
        }
    ]
}

{
    "border": "normal",
    "current_border_width": 2,
    "floating": "auto_off",
    "geometry": {
       "height": 2078,
       "width": 1903,
       "x": 4485,
       "y": 26
    },
    "marks": [],
    "name": "Welcome - i3-projects - Visual Studio Code",
    "percent": 0.7,
    "swallows": [
      {
         "instance": "^code$"
      }
    ],
    "type": "con"
}
```

Note that sometimes editors like VSCode and IntelliJ will pop interstitial windows that unintentionally get swallowed. I've found in IntelliJ's case, you want to choose the *title* of the window to get the appropriate capture.