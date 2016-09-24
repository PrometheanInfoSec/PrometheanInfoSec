S as a project is still in its infancy.  As such, there isn't a ton of documentation to cover the new features constantly being added to the framework.  In this section we will briefly touch on some of the features that were just added at the time of this document's publication.

**Variables**

Obviously, TALOS has built in support for variables.  Earlier in this walkthrough, we learned how to set variables for a specific module.  But did you know you can also set global variables?  

You can set global variables by setting them without a module loaded.  (From the TALOS prompt.)

Either start TALOS fresh, or if you are inside of TALOS and have a module loaded you can use the `unload` command to go back to the TALOS prompt.

Once your prompt looks like this:
`TALOS>>>` you are good to go.  (This means you do not currently have a module loaded.)

Issue the `set` command, and any variables you set will be added to the global context.

`TALOS>>>` **`set test testy`**

`TALOS>>>` **`list variables`**

		Name  Value  Required  Description
		----------------------------
		test  testy  no        Empty
		----------------------------

There are three variable contexts inside of TALOS.  Global, local, and remote.  

Each is accessible by a different variable preface.  Global variables are prefaced with a `%` (percent sign).  Local variables are prefaced with a `$` (dollar sign).  Remote variables are prefaced with an `@` at symbol.

Let's load up a module, and watch this context difference in action.

`TALOS>>>` **`use local/honeyports/basic`**

		loading new module in load_module

We can use the echo command to see the contents of our variables.

First let's echo a local (module specific) variable.

`local/honeyports/basic>>>` **`echo $whitelist`**

		127.0.0.1,8.8.8.8

Now let's echo that global variable we set earlier.

`local/honeyports/basic>>>` **`echo %test`**

		testy

Make sense?  Whatever variables are needed by the currently loaded module can be accessed in the local context.  The other variables are stored in the background.

But wait! What about the remote context?  I'm glad you asked.  The remote context is an append only context used by query modules launched from phantom.  We haven't covered phantom just yet.  But let's talk about it briefly.

Phantom as a tool is a part of TALOS.  It is an agent that can be deployed on remote systems.  You can then push scripts to Phantom to run them on the remote system.

For example, if you needed to deploy a honeyport on a remote system on the other side of your network, you could use phantom to do that without having to install TALOS there.

There are special modules in phantom called "query modules" that run a task, and return the result.  We're not going to cover their use here.  But what happens with them is they write into the remote context.  They can not read there, nor can they overwrite, they can only append.

You can then access what data is being returned by Phantom using the variable preface @.  

**Comments**

TALOS can accept comments in scripts.  Just prepend your line with a `#` (pound sign) and TALOS will ignore it.

**Conditionals**

TALOS can accept conditional statements in scripts in the form of **if**s

You can write these into your scripts like so:

		if 1 == 1
		echo 1
		echo 2
		echo 3
		fi

Don't be afraid to use variables inside these conditionals.

		if $count == 1
		exit
		

**Goto Statements**

TALOS accepts goto statements inside of scripts.  Place a marker (usually a line you have commented out).  Then jump to it.

		#gohere
		echo 1
		goto #gohere

**Loops**

You can increment and decrement variables in TALOS.  Combine this with ifs and gotos and you can create loops.

		set count 10
		#gohere
		if $count > 0
		dec count
		echo $count
		goto #gohere
		fi
		

**Helper Commands**

There are a selection of commands baked into the interpreter for the express purpose of assisting scripting.  A list of some of the newer commands is included here:

		del <var> --> Delete a variable
		copy <from> <to> --> Copy a var to another var
		cat <var0> <var1> --> concat two vars together
		dec <var> --> decrement the value of a var
		inc <var> --> increment the value of a var
		shell <command> --> execute a shell command
		wait <seconds> --> pause the script
		echo <value> --> echo a value
		echo <var> --> echo a var
		echo::vars_store --> echo global variable context
		echo::variables --> echo local variable context
		invoke <path/to/script> --> invoke a script (think functions)
		put <variable> <value> --> put a value into a variable (append)
		pop <variable> --> pop last value from variable
		isset <variable> -->  check if a variable is set
		length <variable> --> get length of variable
		set <variable> <value> --> set a variable
		query <your_query> --> query the database

