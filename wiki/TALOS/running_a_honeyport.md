Let's take a look at how easy it is to run a honeyport from within TALOS.
We'll go with a basic honeyport.

From the TALOS prompt...
**`TALOS>>> use local/honeyports/basic`**

Next we can view all the items we need to configure before launching 
like so.
**`local/honeyports/basic>>> show options`**

		Variables
		Name		Value		Required	Description
		-------------------------------------------------------------------------
		host					no			Leave blank for 0.0.0.0 'all'
		whitelist	127.0.0.1	no			hosts to whitelist (cannot be blocked)
		port					yes			port to listen on
		tripcode			no			tripcode trigger for automation
		-------------------------------------------------------------------------

Note: that the prompt has changed from "TALOS" to "local/honeyports/basic"
this lets us know that we have loaded the honeyports module.


Looks like the only thing we need to set is the default port.

**`local/honeyports/basic>>> set port 4444`**

**`local/honeyports/basic>>> run`**

		Listening...

That's it.

