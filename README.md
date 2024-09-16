# Advanced Quick Directory Aliases

Enables quick directory aliases and navigation. Allows for easy and consistent navigation between disparate directories.

## Table of Contents
**[Installation](#installation)**<br>
**[Usage](#usage)**<br>
**[Autocomplete](#autocomplete)**<br>
**[Supported Environments](#supported-environments)**<br>
**[Implementation Details](#implementation-details)**<br>
**[License](#license)**<br>


## Installation

1. Download the [script](https://github.com/dakoo/shell-directory-management/blob/master/quick-directory-aliases.sh)
1. Source the script into your current shell (add this to your shell startup script to always have the command available)
1. Done

#### Quick setup commands

> Note: Before running the following, change `RC_FILE` to match your preferred shell's rc file, or any other file that you source when a new shell/terminal is created.

```bash
RC_FILE=~/.zshrc
SCRIPT_DIRECTORY=~/scripts

mkdir -p $SCRIPT_DIRECTORY
curl -o $SCRIPT_DIRECTORY/quick-directory-aliases.sh https://raw.githubusercontent.com/dakoo/shell-directory-management/master/quick-directory-aliases.sh
printf "\n. $SCRIPT_DIRECTORY/quick-directory-aliases.sh\n" >> $RC_FILE
. $RC_FILE
```

> The default name for the command is "d". Edit the `quick-directory-aliases.sh` script and change the function name `d()` to a name of your choosing.

## Usage

#### Add an alias
```bash
% cd /any/really/long/or/short/directory/path/thats/hardoreasy/to/remember
% d + shortAliasName
```
> Note: changes take effect immediately across terminals/shells.

Also you can add a tag to the alias.

```bash
% cd /any/really/long/or/short/directory/path/thats/hardoreasy/to/remember
% d + shortAliasName "some tag or comment"
```

#### Navigate to an alias
```bash
% d shortAliasName

% pwd
/any/really/long/or/short/directory/path/thats/hardoreasy/to/remember
```

#### Remove an alias
```bash
% d - shortAliasName
```

#### Remove an alias by the current directory
```bash
% d -
```

#### See all aliases
```bash
% d
workspace = /home/mcwoodle/workspaces/someWorkspaceDirectory ->             workspace
bin = /usr/bin ->                                                           bin folder
nhl = /home/mcwoodle/go/leafs/go ->                                         .
```

#### Clear all aliases

```bash
% d c all
```

#### Help 

```bash
% d ?
```

#### Execute a command in the alias

##### Run intellij idea in the folder

```bash
% d e workspace ij
```

##### Run sourcetree in the folder

```bash
% d e workspace st
```

##### Run finder in the folder(mac)

```bash
% d e workspace op
```

##### Custom Command in the folder

```bash
% d e workspace pwd
```

If the command consists of multiple words, wrap it with double quotes.

```bash
% d e workspace "ls -al"
```

##### Customize the embedded commands

You could modify the following block of the script.

```
    if $_d_excuteCommand;
    then
        _d_cmd=`printf "$_d_aliasRow" | sed -e "s,.* = \(.*\) -> .*,\1,"`
        printf "cd $_d_cmd\n"
        case "$2" in
            ij) /usr/local/bin/idea $_d_cmd;;
            st) stree $_d_cmd;;
            op) open $_d_cmd;;          
            *) "$2" $_d_cmd;;
        esac
        return 1
    fi
```

#### Autocomplete

Autocomplete is currently only supported using bash and zsh (or any script supporting `complete\compgen` or `compctl` built-ins).

Installation is automatic and works like any other bash/zsh tab based autocomplete.

#### Using with ssh to have your list of aliases available anywhere (not yet implemented)

If you're like me and work with a lot of remote servers, you could use something along the lines of:
```bash
ssh -t <user>@<hostname> "someScriptOrCommands_ToLoadMapFileAndSourceScript; zsh"
```
to push the ~/.dmap file along with the above script in order to get all of your aliases available remotely no matter which host you sign in to.

I have yet to implement this solution yet, but it's on my todo list.  If you get to it first, please let me know your approach.  Thanks!

## Supported Environments

This script uses standard sh and will work with any POSIX compliant shell. It's been tested with sh, bash, ksh, zsh, dash and been used on macOS, RHEL5, Ubuntu, and Bash on Ubuntu on Windows.

> C shell, tcsh/csh, are not POSIX compliant and therefore not supported.

> ksh on Bash On Ubuntu On Windows, along with being a mouthful, does not work due to a "cannot create pipe [Operation not permitted]" error when piping output to sed. 

> Autocomplete only works on bash and zsh.

## Implementation Details
* The script is sourced into your current working shell, allowing it to issue change directory commands within your shell.
* A function will be defined named 'd' in the current shell's context. If there is an alias with this name it will be removed.
* It creates and uses a `~/.dmap` file to store a map of aliases to directories.
* This map file can easily be edited manually (e.g. bulk renaming).

## License
Licensed under the Apache License, Version 2.0.
