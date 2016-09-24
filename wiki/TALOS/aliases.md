*Aliases**

TALOS comes with many useful features to assist you.  One of those features that is included to make your life easier and your network operations faster is the combination of aliases and autocomplete.

It is quite hard to constantly be learning a whole new collection of commands for each and every framework/tool that you need to use.  As such, TALOS has a robust alias system baked into the interpreter.  

You can learn the TALOS commands.  Or you can use the alias that allow you to speak to the interpreter in different ways.  

For example, the TALOS command to load a new module is `module`.  But if you want to, there are aliases you can use instead of this command.  For example you could load a module with the commands `load`, `use`, or even (to emphasise the file system like nature of the modules) `cd`.  

The command to show what variables can be modified for a module is `list variables`.  But you can use some other aliases such as `show options`, `show variables`, `list options` or even `ls`.  

You can add your own aliases too.  That way if you have a framework you're more comfortable with, and want to speak to TALOS in the same way you speak to it, you can.  Or perhaps you want to build your own list of single character shortcuts to make your hacking even faster.  You can do that.

Simple edit the `aliases` file located in the `conf` directory.  You can append your new alias like so: 
		
**`myalias, command`**

For example:

**`open, module`**

It's that easy.

**Autocomplete**

Now, let's briefly talk about the autocomplete, and the way it works with the aliases feature.  At the time of the creation of this document, the autocomplete has three tiers of commands.  It will likely be far more fine grained in the future.  Those tiers are "loaders", "commands", and "seconds".  Don't worry too much about this.  What's important for you to know is that any aliases you add will automatically be added to the autocomplete system in the same tier as the command they alias.  

To try out the autocomplete system, simply go into the TALOS prompt and hit **TAB**.  The autocomplete is intelligent, based on the tiers mentioned above it can guess what you're trying to write next, and supply you with a list of commands to choose from.  

For example, if you go to the prompt and type `load ` then hit **TAB** twice the autocomplete will spit out a list of modules avaiable to **load** since it assumes that's what you intend to type next.  

`TALOS>>>` **`load `**

		deploy/phantom/ssh/basic             local/honeyports/basic_multiple
		deploy/phantom/ssh/basic+            local/honeyports/invisiports
		deploy/phantom/ssh/multi             local/honeyports/rubberglue
		generate/phantom/basic               local/listener/phantom/basic
		generate/wordbug/doc                 local/listener/phantom/basic_bak
		generate/wordbug/docz                local/listener/phantom/multi_auto
		local/detection/human_py             local/listener/webbug/local_save
		local/detection/simple-pivot-detect  local/listener/webbug/no_save
		local/honeyports/basic               local/spidertrap/basic

That is a basic rundown of the autocomplete and alias system within TALOS.
