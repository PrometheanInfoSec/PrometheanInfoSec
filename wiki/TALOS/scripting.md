#How to script TALOS

### Overview
The default method by which it is expected TALOS will usually be used, is via the TALOS shell (execute talos.py).  That being said.  When you need to run tasks in a more automated fashion.  We have a solution for that.  TALOS includes the ability to run scripted operations in a very simple and convenient way.

### The Process
1. Write up your script
2. Execute talos.py with the script specified

### Writing up your script
Now, we have tried to make this process as simple as can be.  Here's the deal.  All you need to do, it write up a file containing the commands you want to run.  In order, seperated by newlines.
So, every line should contain one command, and one command alone.
Here is an actual example of how you would code up the commands to run the module local/honeyports/basic on port 445
```
load local/honeyports/basic
set port 445
run
```
Save this to a file.  With an appropriate name.

### Executing talos.py with your script
The way in which one would execute the script is as follows.  Run talos.py with the *-s* or *--script* option.  Specifying the path to your saved script file.
Here is an example running one of the built in scripts.
```
python talos.py --script scripts/honeyport_basic_445
```

##Basic Scripting
TALOS is at its most basic level, simply an interpreter.  It takes in commands from you the user via the prompt, and converts those commands into some sort of output based on the rules specified within the framework.  Ex.  If you ask TALOS for help, you will get this response:

`TALOS>>>` **`help`**

		# Available commands
		#  1) help
		#     A) help <module>
		#     B) help <command>
		#  2) list
		#     A) list modules
		#     B) list variables
		#     C) list commands
		#     D) list jobs
		#     E) list inst_vars
		#  3) module
		#     A) module <module>
		#  4) set
		#     A) set <variable> <value>
		#  5) home
		#  6) query
		#     A) query <sqlite query>
		#  7) read
		#     A) read notifications
		#     B) read old
		#  8) purge
		#     A) purge log
		#  9) invoke
		#     A) invoke <filename>
		#  10) update
		#  99) exit


One thing that you can do with TALOS to make your life even easier, is to script up certain functions.  

For example, if you find yourself constantly needing to launch a honeyport (simple example) you can write all the commands out to a script, and then simply call that script to perform the task.  

To launch a honeyport on port 445 we would write out a script that looks like this:

		load local/honeyports/basic
		set port 445
		run -j

We then have two choices for launching this script.

First, we can launch this script when we launch TALOS by specifying the `--script` option.  Like so:

`/opt/talos#` **`./talos.py --script=/path/to/my/script`**

Or, if we're already inside the TALOS interpreter, we can launch the script using the invoke command.

`TALOS>>>` **`invoke /path/to/my/script`**
