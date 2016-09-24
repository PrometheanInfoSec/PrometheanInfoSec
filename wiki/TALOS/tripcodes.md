#Tripcodes

TALOS comes with a useful automation feature that allows you to launch scripts in response to the triggering of modules on your network.  

This functions using something called "tripcodes".  Certain modules can accept a tripcode as a variable before they're launched.  An example of a module with such a capability is local/honeyports/basic.

If let's take a look at this module.  From within the TALOS prompt issue these commands.

`TALOS>>>` **`use local/honeyports/basic`**

`local/honeyports/basic>>>` **`list variables`**

		Name      Value             Required  Description
		----------------------------------------------------------------------------
		host                        no        Leave blank for 0.0.0.0 'all'
		whitelist 127.0.0.1,8.8.8.8 no        hosts to whitelist (cannot be blocked)
		port                        yes       port to listen on
		tripcode                    no        tripcode trigger for automation
		----------------------------------------------------------------------------


We can see that there is an option here for a "tripcode".  

Here is how that works.  The module will take anything you write into that box, and store it while it runs.  In if someone triggers the honeyport (by visiting it) the module will "phone home" back to TALOS with the tripcode specified.  TALOS will then check to see if there is a script mapped to the tripcode it just received.  If there is, TALOS will launch it.  

So let's add a tripcode.

`local/honeyports/basic>>>` **`set tripcode testinginprogress`**

Before we can launch we also need to set a port.

`local/honeyports/basic>>>` **`set port 1337`**

Everything should be good now.  Let's launch our module in the background.

`local/honeyports/basic>>>` **`run -j`**

In another terminal now, let's explore what we did when we set that specific tripcode.
Navigate to the TALOS directory and open up the file `mapping`.

`#` **`cd /opt/talos`**

`/opt/talos#` **`cat mapping`**

		###The format for this file is simple
		# It goes: <tripcode>,<script>
		# So for example, with tripcode: aaaa
		# and script talos/fightback
		# You would write: aaaa,talos/fightback
		# NOTE: script paths should be relative from the scripts folder
		###

		testinginprogress,talos/honeyport_basic_445

It looks like the tripcode "testinginprogress" is the default tripcode specified in the mapping file.  The mapping file is the place TALOS looks to map tripcodes to scripts.  

In this case, the tripcode testinginprogress is mapped to a template script located in the directory `scripts/talos`.

We can guess what this script does based on its name.  

You could edit this file to add your own tripcodes, and map them to scripts that you create.  

Let's trigger our tripcode.

Firstly we want to see that there's nothing listening on port 445 (You will need to be root for this, or use sudo).

`/opt/talos#` **`lsof -i -P | grep 445`**

You shouldn't see anything.

Now, attempt to connect to your honeyport

`/opt/talos#` **`nc localhost 1337`**

If the connection hangs simply hit ctrl+C.

If we run lsof again...

`/opt/talos#` **`lsof -i -P | grep 445`**

		python  17565     root    3u  IPv4 19923014      0t0  TCP *:445 (LISTEN)

