![tests](https://github.com/deviljin112/Pytest-File-Watcher/actions/workflows/python-app.yml/badge.svg)
![publish](https://github.com/deviljin112/Pytest-File-Watcher/actions/workflows/python-publish.yml/badge.svg)
![version)](https://img.shields.io/github/v/tag/deviljin112/Pytest-File-Watcher?label=Version)
![license](https://img.shields.io/github/license/deviljin112/pytest-file-watcher?color=yellow)
![downloads](https://img.shields.io/github/downloads/deviljin112/Pytest-File-Watcher/total)
![issues](https://img.shields.io/github/issues-raw/deviljin112/pytest-file-watcher)

# Pytest File Watcher

Pytest-File-Watcher is a tool that watches your files and runs your pytest tests automatically when it detects changes.

## Why?

I was tired of running `pytest` manually every time I made a change to my code. I wanted a tool that would watch my files and run my tests automatically when it detected changes.
\
While [pytest-watch](https://github.com/joeyespo/pytest-watch) is a great tool, it is simply not maintained anymore. It works for most things, but I wanted something simpler. I wanted to be able to create config files within my projects so I dont need to remember all the flags and commands I need to pass to pytest. That way I just need to run `pytest-w test` and it will do all the work for me.
\
Pytest-File-Watcher may not have all the features of pytest-watch (**yet**), but I plan to expand the functionality of this tool as needed.
\
You will find that Pytest-File-Watcher is quite opinionated. It is designed to work with my workflow. If you have any suggestions, please feel free to open an issue or a pull request. I am always open to suggestions.
\
I hope it helps someone else with managing a multi-project testing workflow.

## Disclaimer

This tool is still in development. It's not perfect by any means. It does what I need it to do, but it may not work for your use case. Please feel free to open an issue or a pull request if you find any bugs or have any suggestions. I only tested so many use cases, so I am sure there are many more that I have not thought of.

## Installation

```bash
pip install Pytest-File-Watcher
```

## Usage

Main entrypoint

```bash
pytest-w
```

You can run the `--help` command to see all available options.

```bash
pytest-w --help
```

or on any command.

```bash
pytest-w test --help
```

You can also view the `--version` of the tool.

```bash
pytest-w --version
```

### Test Watching

I strongly recommend using the `config.yaml` file to configure Pytest-File-Watcher. You can use the `configure` command to generate a `config.yaml` (see [#Config](#config)) file in the root of your project or if you prefer writing it yourself see [#Config (Advanced)](#config-advanced) section. As mentioned before this is quite opinionated. I personally do not like to pass many flags and commands when testing. Especially when those flags don't change, but it can be problematic to navigate a long line of shell command to edit something. Hence **use the config**.
\
\
Anyway... The `test` command is the real reason you are here. It will watch your files and run your tests automatically when it detects changes. You need to pass the path to the folder you want to watch. You can just pass `./` to watch all files in the current directory.

```bash
pytest-w test ./path/to/folder
```

### Test Flags

There are many flags supported by Pytest-File-Watcher. You can pass them when using the `test` command. These flags take priority over the `config.yaml` file if used.
\
The flags are quite self-explanatory. I will list them here for convenience.
\
\
Verbose: `-v` or `--verbose`. Used when you want to see the full pytest output.
\
**Note:** If you want a more verbose pytest output use the `-p` flag instead.

```bash
pytest-w test ./path/to/folder -v
```

Auto Clear: `-a` or `--auto-clear`. Used when you want to clear the terminal after each test run.

```bash
pytest-w test ./path/to/folder -a
```

Config: `-c` or `--config`. Used when you want to pass a custom config file.

```bash
pytest-w test ./path/to/folder -c "path/to/config.yaml"
```

Ignore: `-i` or `--ignore`. Used when you want to ignore certain folders.

```bash
pytest-w test ./path/to/folder -i "venv" -i "node_modules"
```

Extensions: `-e` or `--extensions`. Used when you want to watch for extra file extensions.

```bash
pytest-w test ./path/to/folder -e ".py" -e ".txt"
```

Passthrough: `-p` or `--passthrough`. Used when you want to pass extra flags to pytest.

```bash
pytest-w test ./path/to/folder -p "-k" -p "test_something"
```

On Pass: `--on-pass`. Used when you want to run a shell command when all tests pass.

```bash
pytest-w test ./path/to/folder --on-pass "echo 'All tests passed'"
```

On Fail: `--on-fail`. Used when you want to run a shell command when a test fails.

```bash
pytest-w test ./path/to/folder --on-fail "echo 'A test has failed'"
```

### Config

If you are not familiar with yaml files, you can use this option to generate a `config.yaml` file in the root of your project.

```bash
pytest-w configure create
```

You can also edit the `config.yaml` file.

```bash
pytest-w configure edit
```

And view the current configuration.

```bash
pytest-w configure view
```

All above commands accept the `--config` flag to specify a custom config file. The default is the current working path.

```bash
pytest-w configure create --config "path/to/config.yaml"
```

### Config (Advanced)

Pytest-File-Watcher can be configured using a `config.yaml` file in the root of your project. Example of currently supported options:

```yaml
verbose: bool
autoClear: bool
onPass: shell-command
onFail: shell-command
extensions:
  - string-item
ignore:
  - string-item
passthrough:
  - string-item
  - string-item
```

See [example config](./example_config.yaml).
\
This will likely change in the future, and be expanded further. I will do my best to keep the documentation up-to-date.

### Recommendation

My daily driver is WSL2 within Windows 10. I have found myself running Pytest-File-Watcher in the background while doing other operations in another terminal tab and often forgetting to check the status of tests. I know that on linux you can use [notify-send](https://vaskovsky.net/notify-send/linux.html) to send notifications to your desktop. Well, on WSL you can use [wsl-notify-send](https://github.com/stuartleeks/wsl-notify-send) which essentially sends a notification to the Windows notification center. I have found this to be very useful and I highly recommend checking it out and using it in your own projects.
\
I have added the following to my `config.yaml` file:

```yaml
onPass: wsl-notify-send.exe --category "Pytest-File-Watcher" "All tests passed"
onFail: wsl-notify-send.exe --category "Pytest-File-Watcher" "A test has failed"
```

Now I can forget about Pytest-File-Watcher and it will notify me when it is done running tests. Not sure how this will work on other platforms, but I am sure there is a similar tool for MacOS.

## Manual Build

If you want to build the project manually, you can do so by cloning the repository. I recommend using a virtual environment.

```bash
python -m venv venv
source venv/bin/activate
```

Then install the dependencies.

```bash
pip install -r requirements.txt
```

And finally build the project.

```bash
pip install -e .
```

Then you can run the tool whenever you are in that virtual environment.

## Contributing

Contributions are welcome! Please feel free to open an issue or a pull request.

## Acknowledgements

- [pytest-watch](https://github.com/joeyespo/pytest-watch)
- [watchfiles](https://github.com/samuelcolvin/watchfiles)
- [typer](https://github.com/tiangolo/typer)

## Author

[Deviljin112](https://github.com/Deviljin112)

## License

MIT
