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

# Installation
Pull the repository and put `project-save` on path.

The project also requires installing `jq` and `Jinja` from path:
```
pip install jq
pip install Jinja2
```