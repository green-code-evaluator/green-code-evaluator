# Creating a CLI app with Python

This page is intended to showcase how to create a simple CLI app with Python taking as example the one we built for HerHackathon. 

## Where to start
Think of what capabilities you want your app to cover. In this case our app will start by having only one command but we'll build it with the future in mind and in a way that is easy to extend to other commands if need be.  

```commandline
.
├── green_code_evaluator
│   ├── commands
│   │   ├── analyze.py
│   │   └── __init__.py
│   ├── __init__.py
│   ├── main.py
│   └── util
│       ├── cli.py
│       ├── __init__.py
│       ├── json_utils.py
│       └── text.py
├── LICENSE
├── Makefile
├── poetry.lock
├── pyproject.toml
└── README.md
```

## Creating a command
```python
@click.command(name="analyze", help="Analyzes some python code")  # decorator to state that this function is a command
@click.argument("input_path", nargs=1)  # mandatory argument - needs to be passed
@click.argument("results_directory_path", nargs=1)  # mandatory argument - needs to be passed
def command(input_path: str, results_directory_path: str):
    """

    Args:
        input_path: a folder or file path to be analyzed
        results_directory_path: directory where results are to be saved
    """
    if not os.path.exists(results_directory_path):
        os.makedirs(results_directory_path)

    if os.path.isfile(input_path):
        _analyze(input_path, results_directory_path)
    else:
        raise NotImplementedError("The capability you're trying to use is not implemented.")
```

Make sure you add the command name to the `__init__.py` file inside the commands folder. 

```python
__all__ = [
    "analyze"
]
```
This will enable you to add the command to the CLI app later on.

## Building the app
In the root folder create `main.py` and `__init__.py` files. 

In the `main.py` file you should have something like this:
```python
from green_code_evaluator.commands import analyze


@click.group()
def cli():
    pass


cli.add_command(analyze.command)

def run():
    cli()


if __name__ == "__main__":
    run()
```

In the `__init__.py` file something like the following should exist:

```python
from green_code_evaluator import main

__version__ = "1.0.0"

run = main.run
```

This is what will enable you to package the CLI app.

## Packaging the app
In this case we're using poetry to package our app so in the `pyproject.toml` file we need to add the following lines:

```toml
[tool.poetry.scripts]
gcode = "green_code_evaluator:run"
```
