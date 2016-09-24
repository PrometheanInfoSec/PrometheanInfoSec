#Phantom

###What is Phantom?
Phantom is a long arm module for Talos.  Specifically, it is a tool which seeks to extend the range at which one may deply the modules contained within the framework.  

In practice, Phantom is somewhat akin to a type of "malware" for the good guys.  We use Phantom to do some of the same things that an attacker might write up some malware for.  Except in the end we serve the purpose of network defense, rather than pillage.

Phantom can be deployed to remote machines on your network, then used to push scripts from the local framework through to those remote hosts.

Finally, let's cover the basic of Phantom.  What it is, and how to use it.  

Phantom is a "long arm" module for TALOS.  It is an agent, made to be deployed on your infrastructure, that calls back TALOS and accepts commands from TALOS.  You can push all sorts of commands to Phantom.  

In short, Phantom is to TALOS as Meterpreter is to Metasploit.  That while Metasploit is an offensive tool.  TALOS is a tool designed to assist computer network defenders in the protection of their own assets.

But you can't always expect a network defender to have TALOS installed on every single machine across his entire network.  That's where Phantom comes in.  You can install TALOS on your workstation.  Then use phantom to deploy modules to anywhere you have access.  

For this example, we're going to use one of Phantom's deploy modules.

From within the TALOS prompt, issue this command to load it:

`TALOS>>>` **`use deploy/phantom/ssh/multi`**

		loading new module in load_module

This module exists to seamlessly deploy one or more Phantom instances across your network via SSH.  
Next let's take a look at the options.

`deploy/phantom/ssh/multi>>>` **`show options`**

		Variables
		Name     Value  Required  Description
		------------------------------------------------------------------
		username        yes       Username to login with
		commands        no        Commands to send to deploys
		rhosts          yes       too long, to view type 'more <variable>'
		lhost           yes       The host to call back to
		custom          no        Custom script to use, blank for default
		lport    1226   yes       The port to call back to
		ex_dir   /tmp   yes       directory to execute from (think privileges)
		password        yes       password to login with
		rport    22     yes       Port to connect to
		listen   no     yes       Want to start listening?
		------------------------------------------------------------------

As you can see, there are a number of variables we will need to set here before we can deploy.  Lets go over what each of the ones we need to set accomplish.

		* username is the username to authenticate with on the remote host
		* password is the password to authenticate with
		* commands are optional commands to have phantom execute immediately
		* rhosts is a list of hosts to deploy to
		* lhost is the listening host Phantom should call back to (your box)
		* lport is the listening port Phantom should call back to
		* rport is the remote ssh port to connect to (default: 22)
		* listen tells TALOS whether or not to start a listener automatically

Let's start setting values.  I am going to deploy Phantom in this case to my local system.  Your local system may or may not have SSH installed.  Installing it is outside of the scope of this tutorial.  

You will likely need to set different values than I am setting.  But if you understand what each value does, that shouldn't be a problem.

`deploy/phantom/ssh/multi>>>` **`set username adhd`**
`deploy/phantom/ssh/multi>>>` **`set password adhd`**
`deploy/phantom/ssh/multi>>>` **`set rhosts 127.0.0.1`**
`deploy/phantom/ssh/multi>>>` **`set lhost 127.0.0.1`**
`deploy/phantom/ssh/multi>>>` **`set listen yes`**

You should now be good to launch.  Simply issue the run command, sit back, and relax.

		No custom script specified, building default..
		Attempting to push to: 127.0.0.1
		Command List: ['']
		Press Ctrl + C to exit...
		# Attempting to upload script..
		Script uploaded!
		Attempting to execute script..
		New session established


Now we can interact with the session we have established.

`#` **`interact 1`**

Let's push a module to it.  For this example we will use a basic honeyport.

Currently, this is a multi step process.

1) We load the module locally.
2) We push a copy of the module to phantom
3) We edit the variables locally
4) We push the variables to phantom which launches the module

First we will load the module locally
`S1>>` **`module local/honeyports/basic`**

Now we will push a copy to the Phantom instance
`S1>>` **`push`**

Now let's list the variables to see what we need to set
`S1>>` **`list variables`**

		---------------------------------------------------------------------------
		host                        no        Leave blank for 0.0.0.0 'all'
		whitelist 127.0.0.1,8.8.8.8 no        hosts to whitelist (cannot be blocked
		tripcode                    no        Tripcode to trigger script
		port                        yes       port to listen on
		-------------------------------------------------------------------------

We will set the port we want
`S1>>` **`set port 31337`**

And finally, launch the module
`S1>>` **`launch`**

In another terminal on your system you can confirm that the module was successfully launched by checking to see if something is listening on the port you specified.

`/opt/talos#` **`lsof -i -P | grep 31337`**

		python 21344 	adhd	10u   IPv4   1994461		0t0   TCP  *:31337	(LISTEN)

NOTE: if you run into any errors with the uploading or execution of your script, it is likely related to file permissions.  Try tweaking the permissions of your user, or changing the ex_dir value prior to launch.  (For example, changing ex_dir from `/tmp` to `/home/adhd`.)


There is a ton more to discover inside of TALOS.  So get busy and get exploring.  New content is constantly being added to this project.
