ome modules in TALOS are written to be able to send notifications back to the command console.  This might can be incredibly useful in detecting and thwarting an attack on your network.  

One of the modules capable of sending notifications back to the command console is the module used in the previous example [Example 1: Running a Honeypot] "local/honeyports/basic".

In this example we will initiate a connection to our honeyport and observer the incoming notification.

Please run the module as you did in the previous example [Example 1: Running a Honeypot] barring one minor difference!  When the module is ready to run (that is, you have set the options and are ready to type "run") instead of typing run, type run -j.  This will launch the module in the background, leaving your prompt inside the main TALOS console rather than migrating it to the module.  The module will execute in the background as before.  

Once you have the module running, open another terminal and connect to the honeyport using netcat, like so.

`$` **`nc localhost 4444`**

The attempted connection may hang (appear to freeze and do nothing).  You can terminate your attempt to connect by pressing Ctrl+C if it does this.


Back inside your TALOS console, your notification should have arrived.  If it has not, just wait a minute and it will.  Looking back at the TALOS prompt you should now see something along the lines of...

		You have received 1 new notification
		1 total unread notifications
		command is: read notifications

Let's read the notification.

`local/honeyports/basic>>>` **`read notifications`**

		2016-07-14 15:55:53.207390:honeyports/basic connection from 127.0.0.1:52079
		2016-07-14 16:04:22.465195:honeyports/basic connection from 127.0.0.1:52081


The command `read notifications` will show you all currently unread notifications.  If you need to see a notification you have read previously you can issue the command `read old`.

`local/honeyports/basic>>>` **`read old`**

		#2016-07-14 15:55:53.207390:honeyports/basic connection from 127.0.0.1:52079
		#2016-07-14 16:04:22.465195:honeyports/basic connection from 127.0.0.1:52081

You can also view the log file.  It is located (from the talos directory) in logs/notify.log

