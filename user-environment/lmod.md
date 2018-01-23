# Lmod

## Pre-Requisites

```
lua-bitop
lua-filesystem
lua-posix
lua-term
```

## Initialization

1) Initialization Script must be sourced
```
source /opt/lmod/lmod/init/bash
```

2) MODULESHOME Environment Variable must be set to Root Directory of Modules Tree
Example:
```
export MODULESHOME=/opt/lmod/modulefiles/ola02
```

3) MODULEPATH Environment Variable must be set to First-Level Directory of Modules Tree
Example:
```
export MODULEPATH=${MODULESHOME}/level0
```

## Module Files

Lmod Modulefiles have two specificities compared to Environment Modules Modulefiles:
* Language is LUA and not TCL (but TCL is properly understood as well for easier compatibility)
* Modulefiles are organized as a hierarchy

Module Files Hierarchy:
1. Level #1: Modules for software components that do not depend on compiler environment
2. Level #2: Modules for software components that depend on compiler environment ONLY
3. Level #3: Modules for software components that depend on compiler environment AND MPI library

## User Commands

List Available Modules:
```
ml avail
```
Load / Unload Module:
```
ml <module>
ml unload <module>
```
List Loaded Modules:
```
ml list
```
Switch from <module 1> to <module 2> = Unload <module 1> and Load <module 2>:
```
ml switch <module 1> <module 2>
```
Unload All Loaded Modules (Purge Environment):
```
ml purge
```

## Default Module Version

1. Create a '.version' hidden file inside the directory of the module
2. Set default module version inside the '.version' file:
```
#%Module
set ModulesVersion "13.1.5"
```

## Save / Restore Current Modules

Save Currently Loaded Modules:
```
module save
```
Configuration File:
```
~/.lmod.d/default
```
Restore Default Modules:
```
module restore
```
