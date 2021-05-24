---
title: "How to import local modules with Python"
tags:
  - python
  - programmation
toc: true
toc_sticky: true
header:
  teaser: https://imgs.xkcd.com/comics/python.png
  og_image: https://imgs.xkcd.com/comics/python.png
---

Importing files for local development in Python can be cumbersome. In this article, I summarize some possibilities for the Python developer.   

![https://xkcd.com](https://imgs.xkcd.com/comics/python.png)
{: style="text-align: center"}

**TL; DR**: I recommend using `python -m` to run a Python file, in order to add the current working directory to `sys.path` and enable relative imports.
{:  .notice--success}

## Definitions

**Script**: Python file meant to be run with the command line interface.
{:  .notice--info}

**Module**: Python file meant to be imported.
{:  .notice--info}

**Package**: directory containing modules/packages.
{:  .notice--info}


## Example

Consider the following project structure:
~~~
project
├── e.py
└── src
    ├── a
    │   └── b.py
    └── c
        └── d.py
~~~

Say that in `b.py`, we want to use `d.py`:

<script src="https://gist.github.com/fortierq/8c95edc33c45a5d904d1a30d66b00d6f.js"></script>

Let's try to run `python` from the `project` directory:
~~~ shell
~/Documents/code/project$ python3 src/a/b.py

Traceback (most recent call last):
  File "/home/qfortier/Documents/code/project/src/a/b.py", line 4, in <module>
    import src.c.d
ModuleNotFoundError: No module named 'src'
~~~

What happened? When Python tries to import a module, it looks at the paths in `sys.path` (including, but not limited to, PYTHONPATH):
<script src="https://gist.github.com/fortierq/b329d5223604404b600289ddf2991f8c.js"></script>
~~~ shell
~/Documents/code/project$ python3 src/a/b.py

['/home/qfortier/Documents/code/project/src/a', '/usr/lib/python38.zip', '/usr/lib/python3.8',  ...]
~~~
We see that the directory containing b.py (the script we run) is added to `sys.path`. But `d.py` is not reachable from the directory `a`, hence the above error.  

## 1st solution: add root to sys.path

We can add the path to the root of the project:
<script src="https://gist.github.com/fortierq/d7388895aa6531a2a3c2d2eb6f89438f.js"></script>
~~~ shell
~/Documents/code/project$ python3 src/a/b.py

['/home/qfortier/Documents/code/project/src/a',  ..., '.']
~~~
The added path '.' refers to the current working directory (from which Python was run) ~/Documents/code/project. Python can indeed import `c.d` from this directory.  

This solution works regardless of the directory used to run Python:
~~~ shell
~/Documents/code/project$ cd ../..
~/Documents$ python3 code/project/src/a/b.py

['/home/qfortier/Documents/code/project/src/a',  ..., 'code/project']
~~~

It should also work on a different computer, with a different filesystem or OS, thanks to Pathlib.  
However, modifying `sys.path` at the beginning of every file is tedious and hard to maintain. A better solution would be to use a `context.py` file modifying `sys.path`and imported by every other file, but this is still unsatisfying.  
Another possibility is to add the root directory to PYTHONPATH (before running the script). This is done automatically by `poetry`, for example.

## Relative import

Relative imports were introduced in [PEP 328](https://www.python.org/dev/peps/pep-0328) as a way to improve maintainability and avoid very long import statements. They must use the `from .module import something` syntax:
<script src="https://gist.github.com/fortierq/55871b627e3929578a393acab4abe72d.js"></script>

~~~ shell
~/Documents/code/project$ python3 src/a/b.py
Traceback (most recent call last):
  File "src/a/b.py", line 4, in <module>
    from ..c import d
ImportError: attempted relative import with no known parent package
~~~
This is not working! Indeed, relative imports rely on the `__name__` or `__package__` variable (which is `__main__` and `None` for a script, respectively).  
If we import b.py from another file, everything is fine:

<script src="https://gist.github.com/fortierq/415e91b120b4fe30db8fc4140fae5139.js"></script>
<script src="https://gist.github.com/fortierq/1cc0edcdff4addd56af5b7d4439a28b8.js"></script>
~~~ shell
~/Documents/code/project$ python3 e.py

Name: src.a.b
Package: src.a
~~~

**Note**: to go up several directories in relative imports, use additional dots: `from ...c import d` would go 2 directories up, for example.
{:  .notice}

## 2nd solution: run as a module

It is unfortunate that scripts can't use relative imports. [PEP 338](https://www.python.org/dev/peps/pep-0338/) overcomes this limitation by adding the `-m` option. With this option, a script is run as if it was imported as a module.
<script src="https://gist.github.com/fortierq/415e91b120b4fe30db8fc4140fae5139.js"></script>
<script src="https://gist.github.com/fortierq/1cc0edcdff4addd56af5b7d4439a28b8.js"></script>
~~~ shell
~/Documents/code/project$ python3 -m src.a.b  # note that we use . and not / here

Name: __main__
Package: src.a
~~~
`python3 -m src.a.b` acts as if a Python file containing `import src.a.b` was run in the current directory, with two notable differences:  
- the `__package__` variable is filled, enabling relative imports (see [PEP 366](https://www.python.org/dev/peps/pep-0366/))
- the directory from which Python was run (here: ~/Documents/code/project) is added to `sys.path` 

Since I don't see any drawback to this, I recommend always using `python3 -m` to run a script.
## Run as a module on Visual Code

The default launch configuration in Visual Code runs Python files as scripts (without `-m`). This [GitHub issue](https://github.com/microsoft/vscode-python/issues/5184?fbclid=IwAR20Tk7RyK-2SoL4l-shGe-puCsxTpE4vR8RlpoACP-WK54sgKhwdF6xZH0) explains how to add `-m` automatically:
- add the ["Command Variable" extension](https://marketplace.visualstudio.com/items?itemName=rioj7.command-variable) to Visual Code
- set the following Python launch configuration in your settings.json:
<script src="https://gist.github.com/fortierq/53a9e73cee609c763966839ff4ca25e0.js"></script>

## 3rd solution (outdated): install in editable mode

`pip install -e ...` installs a package in editable mode, which can then be imported anywhere on the computer. In practice, this is essentially a symbolic link to your package. Therefore, any modification of the package is reflected on the code importing it. It requires a `setup.py` at the root of your package.  

**Note**: according to [PEP 517](https://www.python.org/dev/peps/pep-0517), this is no longer recommended: Python packages should rely on a toml file (see [Poetry](https://python-poetry.org/)) and not on `setup.py` anymore.  
{:  .notice--danger}

## References
- Topics on stackoverflow: <https://stackoverflow.com/questions/22241420/execution-of-python-code-with-m-option-or-not>, <https://stackoverflow.com/questions/16981921/relative-imports-in-python-3>
- Import system of Python: <https://docs.python.org/3/reference/import.html>, <https://docs.python.org/3/library/runpy.html>
- pip install: <https://pip.pypa.io/en/stable/cli/pip_install/>
