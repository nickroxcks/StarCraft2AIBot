# Sc2LadderServer
Fork of Cryptyc's ladder server for SC2 API. Used for the CMPUT 350 SC2 tournament at the University of Alberta.


# Developer Install / Compile Instructions
## Requirements
* [CMake](https://cmake.org/download/)
* Starcraft 2 ([Windows](https://starcraft2.com/en-us/)) ([Linux](https://github.com/Blizzard/s2client-proto#linux-packages)) 
* [Starcraft 2 Map Packs](https://github.com/Blizzard/s2client-proto#map-packs) 

## Windows

Download and install [Visual Studio 2017](https://www.visualstudio.com/downloads/) if you need it.

```bat
:: Clone the project
$ git clone --recursive https://github.com/solinas/Sc2LadderServer.git
$ cd Sc2LadderServer

:: Create build directory.
$ mkdir build
$ cd build

:: Generate VS solution.
$ cmake ../ -G "Visual Studio 15 2017 Win64"

:: Build the project using Visual Studio.
$ start Sc2LadderServer.sln
```

Optional: If you want to use Visual Studio 2019 instead, you'll need to replace the `cmake` command above with:
```bat
$ cmake ../ -G "Visual Studio 16 2019"
```
and then make the code change described [here](https://github.com/Blizzard/s2client-api/issues/319).

### Linux
```bash
# Get the project.
$ git clone --recursive https://github.com/solinas/Sc2LadderServer.git
$ cd Sc2LadderServer

# Create build directory.
$ mkdir build && cd build

# Generate a Makefile.
# Use 'cmake -DCMAKE_BUILD_TYPE=Debug ../' if debuginfo is needed
$ cmake ../

# Build.
$ make
```

### OS X
```bash
# Get the project.
$ git clone --recursive https://github.com/solinas/Sc2LadderServer.git
$ cd Sc2LadderServer

# Apply compilation fixes for OS X.
$ git apply hacks/civetweb_compilation_fix.patch

# Create build directory.
$ mkdir build && cd build

# Generate a Makefile.
# Use 'cmake -DCMAKE_BUILD_TYPE=Debug ../' if debuginfo is needed
$ cmake ../

# Build.
$ make
```

### Submodules
If you don't initially do a `--recursive` clone (in which case, submodule folders will be left empty), you can download any submodules later like so:
```
git submodule update --init --recursive
```
Alternatively, you could opt to symlink the folder of the submodule in question to an existing copy already on your computer. However, note that you will very likely be using a different version of the submodule to that which would otherwise be downloaded in this repository, which could cause issues (but it's probably not too likely). 
 
## Configuration

### Maps
Make sure you've installed the maps, as per the directions from [Blizzard's SC2Client-Proto project](https://github.com/Blizzard/s2client-proto#map-packs).
   
### Configuration files
All configuration files should be placed in the project root directory.

##### LadderManager.json
Create a file called `LadderManager.json` It should be in json format, with entries as described in the table below.
 
| Config Entry Name | Description |
|---|---|
| `ErrorListFile`           | Place to store games where errors have occured |
| `BotConfigFile`           | Location of the json file defining the bots |
| `MaxGameTime`             | Maximum length of game |
| `CommandCenterDirectory`  | Directory to read .ccbot command center config files |
| `LocalReplayDirectory`    | Directory to store local replays |
| `EnableReplayUpload`      | True/False if replays and results should be uploaded |
| `UploadResultLocation`    | Location of remote server to store results |
| `ResultsLogFile`          | Local file to store results in json format |
| `PlayerIdFile`            | Location of file to store player IDs.  |

##### BotConfigFile.json
Create a `BotConfigFile.json`  file that will describe the roster of bots and their required attributes.  It should also contain an array of maps to be used.  For each map you want the bots to play on, add its name into this array, **including** the `.SC2Map` file ending.

## Building your own bot
In order to work with the ladder manager, your bot's `main()` should call `RunBot()` from LadderInterface.h. [DebugBot](https://github.com/solinas/Sc2LadderServer/tree/master/tests/debugbot) can be used as an example for how to do this. However, do not submit a copy of this entire repository as your final project. If you're unsure how to include the SC2 API headers and libraries, please take a look at these [instructions](https://github.com/davechurchill/commandcenter#developer-install--compile-instructions-windows).

[SC2 API Reference and Tutorials](https://blizzard.github.io/s2client-api/)

## Ladder server architecture
The ladder server establishes a facade in between the bots and the SC2 instances, as is illustrated below.

![Ladder Server Architecture](docs/LadderServerArchitecture.png)
