##Work in progress##
On your android device:
* Copy your **_.pem_** File into your sd card. If there is no sd card in your android, transfer it via usb
* Download and launch Juice SSH app
*On Windows
download putty


### Billing and cost management
How much will you pay to run AWS GPU instance?



A note on spot instances (this was entirely and temporanely (?) copied... I need to go deeper and rewrite it

“Spot instances” are instances that potentially are much cheaper to to run than the standard sticker price. A normal reservation of a g2.2xlarge instance is $0.65/hr. However, you can bid on a “spot” instance, where your instance will continue to run as long as spot price is under your bid price.
Sometimes this makes a lot of sense. Spot instance pricing, however, has a tendency to wildly fluctuate all over the place, as demonstrated in the picture below:

As you can see, the price was around $6/hr (!!!) for Jan 17–19, and then came down significantly after that. Furthermore the trend line does not really follow any sort of pattern whatsoever. There are many theoriesabout why spot instance pricing is so volatile.
At the time of this writing, spot instances in the us-east-1d region are trending at 0.23/hr for a g2.2xlarge instance. This is much cheaper than the normal running rate of 0.65/hr.
 
Instances > Spot request
 
Go to the next step, “configure instance details”, and request spot instances, if they are below the normal running price ($0.65/hr at time of writing)


In the screenshot above, you can see that the current price in us-east-1d is $0.26/hr, so I set my maximum price to 0.27. Granted, if the price moves upwards at all, my instance will be terminated, but I can always fire up another once the price comes down.
Now you are ready to launch your instance. Click “Review and Launch”, then “Launch”.
Let the mining begin
Once the instance is up, ssh into the machine and configure your mining details.
At your mining website, you should notice that your miners are registered and working:
