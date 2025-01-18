# My TODO CLI app

After trying many TODO apps, I always ended up using a plain and simple .txt file in my desktop. It was messy, but it was simple and fast.

Then, one day I found out the [Todo.txt project](http://todotxt.org/) which aims to provide a simple and extensible format for storing tasks in a simple text file. To manage this file, there are many apps available, some meant directly for the command line, others include a web interface, or even plugins for popular editors.

I wanted something simple, so I started from their [todo.txt-cli](https://github.com/todotxt/todo.txt-cli) implementation and customized it a bit to my needs.

Here you can find the instructions to install and use it as you wish.

## Installation

These are the default instructions that come with the original project. I haven't tried them myself in distributions other than Ubuntu, so I can't guarantee they will work.

### Download
Download the code in this repo.

### OS X / macOS

```shell
brew install todo-txt

cp -n $(brew --prefix)/opt/todo-txt/todo.cfg ~/.todo.cfg
```

**Note**: The `-n` flag for `cp` makes sure you do not overwrite an existing file.

### Linux

#### From command line

```shell
make
make install
make test
```

*NOTE:* Makefile defaults to several default paths for installed files. Adjust to your system:

- `INSTALL_DIR`: PATH for executables (default `/usr/local/bin`)
- `CONFIG_DIR`: PATH for the `todo/config` configuration template (default `/usr/local/etc`)
- `BASH_COMPLETION`: PATH for autocompletion scripts (default to `/usr/local/share/bash-completion/completions`)

```shell
# Note: This is how I configured my installation
make install CONFIG_DIR=~/.todo INSTALL_DIR=~/.local/bin
```

#### Arch Linux (AUR)

https://aur.archlinux.org/packages/todotxt/


## Configuration

I have adapted the original configuration file to my needs. You can find it in the `customization` directory in this repo. You should copy it to your configuration directory, which in my case is `~/.todo`.

```shell
cp customization/config ~/.todo/config
```

I have also created my own sorting function (see [Usage](#usage)). It should be copied into the `~/.todo/actions` directory.

```shell
cp customization/custom_sort_table ~/.todo/actions/custom_sort_table
```

Lastly, add this line to your `.bashrc`file,

```shell
export TODOTXT_DEFAULT_ACTION="custom_sort_table"
```

and this line to your `.bash_aliases` file.

```shell
alias todo='todo.sh'
```

## Usage

The default commands are listed in the [USAGE][USAGE] file. With my customizations the workflow is as follows:

1. Add a task with `todo.sh add "(X) TASK_DESCRIPTION +project @context due:YYYY-MM-DD"`. The order of the tags is not important, but the due date should be in the format `YYYY-MM-DD`.

2. List tasks simply with `todo`. This will list all tasks sorted using my customized priorty list:
- First, those tasks that are due in less than 2 days, marked in red.
- Second, tasks organized by priority, from A to Z, marked in yellow. The priority is simply a letter in parenthesis at the beginning of the task.
- Third, tasks with a due date, marked in green.
- Lastly, tasks without a due date, marked in white.

```ansi
# Example output from `todo`:
^[[1;34m ID   Pri.  Due Date    Project              Description                             ^[[0m
^[[1;34m ---- ----  ----------  -----------          ----------------------------------------^[[0m
^[[1;31m 03         2025-01-19                       This is a task due within 2 days        ^[[0m
^[[1;31m 05         2025-01-19  teaching             This task includes a +project tag       ^[[0m
^[[1;33m 01   (A)                                    This is the task with highest priority  ^[[0m
^[[1;33m 02   (Y)                                    This one has less priority              ^[[0m
^[[0;32m 04         2025-01-27  reading              Due date but not within 2 days          ^[[0m
^[[0;37m 06                                          Task without priority or date           ^[[0m
^[[0;37m 07                     another_project      This one includes a project             ^[[0m
```


3. To reprioritize a task, simply use `todo pri NR PRIORITY`. For instance, to set the priority of task 1 to `Z`, use `todo pri 1 Z`.

4. To mark a task as done, use `todo do NR`. This will mark the task as done and move it to the `done.txt` file. For instance, to mark task 1 as done, use `todo do 1`.

## License

GNU General Public License v3.0 © [todo.txt org][github]

[github]: https://github.com/todotxt
[USAGE]: ./USAGE.md
