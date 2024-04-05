# Our Fork!

This is our fork of the theia-trace-extension repo.

At this time it provides a template for creating both electron and web-based Theia Trace Compass applications

Changes we've made:

- This readme is entirely different, and focussed for internal MIPS developers (there is useful info in the upstream README though, so feel free to go read it)
- Almost all the "IDE" features have been removed (for now...)
- The application name has been set to "MIPS Shinro IPE"

## Other related links

- [Shinro App Block Diagram](https://wavesemi-my.sharepoint.com/:u:/g/personal/kmills_mips_com/EfAETD64JLZEmRujV3HFVkMBJw6JBPAuk87s2Yfwo-Z21g?e=dDO7v2)
- [Upstream Repo](https://github.com/eclipse-cdt-cloud/theia-trace-extension)
- [Tracecompass-server source code](https://github.com/eclipse-tracecompass-incubator/org.eclipse.tracecompass.incubator/tree/master/trace-server)
- [Common Trace Format](https://diamon.org/ctf/)
- [barectf](https://barectf.org/docs/barectf/3.1/index.html)
- [List of @theia plugins](https://www.npmjs.com/search?q=%40theia)
- [open-vsx registry](https://open-vsx.org/) (vscode plugins)
  
# Setting Up The Development Environment (on Linux)

## Linux packages:

```bash
sudo apt install build-essential libx11-dev libxkbfile-dev libsecret-1-dev
```

### Install nvm (if you haven't already): 

[Install nvm instruction](https://github.com/nvm-sh/nvm?tab=readme-ov-file#installing-and-updating)

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

After installing it, verify that it is installed correctly by checking the version

```
nvm --version
```

## Install node.js version 18 and yarn

Install node version 18
```
nvm install 18
```
Check the node version
```
node --version
```
You should see: `Now using node v18.20.1 (npm v10.5.0)`
If you see a different version, do this
```
nvm use 18
```

Install yarn (globally)
```
npm i -g yarn
```

## Build stuff
This will do lots of stuff that is mostly magic at this point.
```
yarn
```

## Start the electron app
The first time you do this, more magical things happen to complete the build.  Subsequent invocations are quicker.
```
yarn start:electron
```
You'll see the application open, but you cannot view traces yet.  You can open a folder, open files in the editor, do git stuff.

## Install Trace-Server and Example Traces
In order to view traces you need to download and start the trace server, and download
the example traces (at least until we have our own sample traces)

The sample traces will be in the TraceCompassTutorialTraces folder in to repo root
```
$ yarn download:sample-traces
$ yarn download:server
```

You may want to do this in a separate shell so it is easy to kill later (via ctrl-c)
```
$ yarn start:server
```

## Open and view an example trace
If the Shinro electron app is still running you can use it, if not, start it again.

- File --> Open Folder (browse to and select the TraceCompassTutorialTraces folder)
- Activate the File Browser (if not already active)
- Right-click on one of the trace folders (I suggest the LTTng one) --> Open with --> Trace Viewer

## Yarn Commands
All the `yarn <commands>` referenced above, and many more, are defined in the scripts section of the package.json file.  They are just shortcuts to running shell commands.

## If something goes horribly wrong
Before you use this command:
   - close any running electron instances
   - stop the tracecompass-server (ctrl-c if in the foreground, kill -9 <pid> if not.)

I added `yarn reset` (because things went horribly wrong several times for me and I could not figure out a less nuclear solution).  This command deletes all untracked files and ignored files from the clone.

It DOES NOT undo any changes you have to tracked files.  So far this has been the easiest way to get things working after the build starts failing (likely from me doing something silly).

For normal development this is probably never necessary.  I was messing around with adding and removing plugins from the product, and those actions did tend to cause state and cache problems that resulted in failed builds for no obvious reason and with poor error messages.
