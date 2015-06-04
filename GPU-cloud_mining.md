# Ethereum cloud mining for dummies

This step by step tutorial is easy to follow. It’s as easy as copy / pasting
If you don’t understand what you are doing, it doesn’t matter.

## Problem:
I want to mine Ether, but I do not want to use my machine and I do not want to invest on new hardware and pay thousand dollar electricity bills.

## Solution: **cloud mining** aka using Amazon’s cloud servers.
Since GPU mining is set to be 100x more efficient than CPU with Ethereum, we need to look for GPU power on the cloud.
The answer, apparently, is **Amazon Web services EC2**

On ethereum forum
https://forum.ethereum.org/discussion/2134/gpu-mining-is-out-come-and-let-us-know-of-your-bench-scores
paul_bxd revealed an inner mean (hashrate?) of 24 MH/s using an AWS g2.8xlarge instance 
comparable to the benchmark of an AMD Radeon R9 280x : 23.2 MH/S which is the best in class for ethereum mining (Nvidia Geforce is far less efficient)

* We are going to create an ubuntu linux virtual machine on amazon web services (AWS) EC2
**THEN**
* we are going to install ethereum miner on ubuntu. 

Step 1.
a) First thing first… Get an AWS account [here](aws.amazon.com):
Amazon Web Services (AWS) - Cloud Computing Services
Amazon Web Services offers reliable, scalable, and inexpensive cloud computing services. Free to join, pay only for…aws.amazon.com
 
AWS stands for Amazon Web Services. Amazon, the well known ecommerce giant is also big in Hosting for developers and more.
If you trust Amazon, you can trust their web services.
Click on top right the button
 

As you can see, the registration process is very handy, since you can sign-in with your existing Amazon account.
You may notice that AWS offers the EC2 service free for 750 hrs/month, for 12 months
But that’s for the Linux t2.micro instance. Which is good for testing, but not for mining Ethereum. I will tell you later what instance to select to maximize the GPU power.

Once you have registered on AWS, you will be presented with a big list of the services offered by Amazon:

Click on EC2, which will give us GPU power-horses for mining.

It’s time to create the Linux virtual machine
click on instances from the menu “Instances”. then click:


allright, now you have to choose the right “OS image” amazon call them AMI or Amazon Machine image.
First off, make sure you are in the correct region (US East) otherwise you will not see it in the list
 
 


once you selected N. Virginia, it’s time to go to “community AMIs” and to search for this image:
“ami-2cbf3e44”
This one, like all the ubuntu 14.04 images, is supported by Ethereum Frontier, but in addition The pre-built AMI has all the NVIDIA GPU drivers, OpenCL, etc all pre-installed. 
You will be redirected to “Step 2: Choose an Instance Type”
As we said in the intro, we need a GPU instance to mine Ethereum.
If you scroll down the list you will see 2 GPU instances.
we will go for g2.8xlarge instance
just click on the empty box on the left to choose the instance
At this point, if you want you can play with the t2.micro free instance before proceeding spending money.
We are ready, just click “Review and launch” at the bottom


 




THEN,
Launch
You will now be prompted to create your access key aka “Key pair”
To use a virtual machine we first need an access key (keep it private!),
Amazon AWS access keys consist of a pubic key and a private key.
click on “Create new key pair”:
 
 
type a name for the access key
a .pem file will be automatically downloaded to your local machine, this is your private key.
Backup this file since you will need this for remote access to your virtual machine on the AWS cloud
 


Then click “Launch Instances”
All right, now on “View instances”
your instance should be pre-selected
Click connect
Connecting to your Linux instance on AWS
 
 


 
On your Mac
Put your .pem File in the folder Applications > Utilities. 
Launch Terminal
type or copy/paste
chmod 400 /Applications/Utilities/youraccesskeyname.pem
 
ssh -i /Applications/Utilities/youraccesskeyname.pem ubuntu@YO.UR.PUBILICIP
you will need to use this line everytime you close Terminal and want to start again
 
yes
 
you should get a confirmation message
“
Welcome to Ubuntu 14.04.1 LTS (GNU/Linux 3.13.0–37-generic x86_64)
“
If these steps don’t work the first time, quit Terminal and do it again, you will eventuallY succeed (this happened to me)
On your Android device
copy your .pem File into your sd card. If there is no sd card in your android, transfer it via usb
Download and launch Juice SSH app

LE MIE STUPIDE DOMANDE

E’ vero, ci sono molte guide valide… il “difficile” è mettere tutto insieme quando hai una necessità specifica.

Fino al lancio dell’ istanza ho chiarezza completa. 
Poi ho problemi nel definire la giusta sequenza di azioni / installazioni da fare.

Di quali “componenti” ho bisogno? Miner?
Client go o CPP?
Devo creare un wallet?
Coinbase (?!!)

Ho notato che in questa fase esistono client in linguaggi diversi, diverse denominazioni per una certa cosa, diversi comandi per ottenere una stessa cosa… (geth..)

L’ idea è di incanalare l’ utente in un percorso obbligato, senza dargli opzioni che possono confondere i newbie… come me
 
*************FROM HERE, WORK IN PROGRESS **************
	•	 
	•	make sure to open port 30303 for both TCP and UDP connections from `anywhere` in your security group settings
	•	 
Creating an ethereum account (aka a eth.coinbase? aka a wallet with an address)
 
 
 
 
# INSTALLING GETh
 
	•	build geth from source: https://github.com/ethereum/go-ethereum/wiki/Installation-Instructions-for-Ubuntu#building-from-source
	•	run geth and leave it to catch up on the chain: ~/go-ethereum/build/bin/geth
	•	install ethminer from cpp-ethereum dev PPAs: https://github.com/ethereum/cpp-ethereum/wiki/Installing-clients#installing-cpp-ethereum-on-ubuntu-1404-64-bit
Building Geth (command line client)
 
 
First, install git
 
```sudo apt-get install git```
 
Clone the repository to a directory of your choosing:

```git clone https://github.com/ethereum/go-ethereum```

 
Building geth requires some external libraries to be installed:
```sudo apt-get install -y build-essential libgmp3-dev golang```

 
Finally, build the geth program using the following command.
```cd go-ethereum
make geth```

 
You can now type:
```build/bin/geth``` to start your node.
 
0x676f
 
1)INSTALLING GO-ETHEREUM
 
Installing from PPA
For the latest development snapshot, both ppa:ethereum/ethereum andppa:ethereum/ethereum-dev are needed. If you want the stable version from the last PoC release, simply omit the -dev one.
Warning: The ethereum-qt PPA will upgrade your system-wide Qt5 installation, from 5.2 on Trusty and 5.3 on Utopic, to 5.4.
sudo apt-get install software-properties-common
sudo add-apt-repository -y ppa:ethereum/ethereum-qt
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo add-apt-repository -y ppa:ethereum/ethereum-dev
sudo apt-get update
sudo apt-get install ethereum

 
Run mist for the GUI or geth for the CLI.
You can alternatively install only the CLI or GUI, with apt-get install geth or apt-get install mist respectively.
Building from source
Building Geth (command line client)
Clone the repository to a directory of your choosing:
git clone https://github.com/ethereum/go-ethereum

 
Building geth requires some external libraries to be installed:
sudo apt-get install -y build-essential libgmp3-dev golang

 
Finally, build the geth program using the following command.
cd go-ethereum
make geth

 
You can now run build/bin/geth to start your node.
 
2) Installing cpp-ethereum client (aka C++)(ETHEREUM MINER — FRONTIER) aka ethminer on Ubuntu 14.04, 64 bit
For the latest stable version (copy, paste and hit enter, one line at a time):
sudo add-apt-repository ppa:ethereum/ethereum-qt
sudo add-apt-repository ppa:ethereum/ethereum
sudo apt-get update
sudo apt-get install cpp-ethereum
 
for the last line, type Y, than hit ENTER (it’s the longer process)
 
 
Then execute on GPU:
https://forum.ethereum.org/discussion/2134/gpu-mining-is-out-come-and-let-us-know-of-your-bench-scores
the important part:
run
eth -M -G
 
You will get a confirmation message
“
Ethereum (++) 0.8.2
 
Code by Gav Wood, (c) 2013, 2014.
 
Based on a design by Vitalik Buterin.
“
Of course, you’ll need a GPU for this to work.
Use
— opencl-device 0
if Eth is not identifying your OCL device properly
 
	•	benchmark ethminer to check that your system is in order: ethminer -G -M (should give you your current hashrate, roughly 6MH/s)
	•	You’re almost done! Once geth has finished catching up on the blockchain, generate a new account: ~/go-ethereum/build/bin/geth account new
	•	start it again with RPC enabled: ~/go-ethereum/build/bin/geth — rpc
	•	start ethminer: ethminer -G
	•	if you’re using the larger g2 instance with 4 GPUs you many need to start ethminer 4 times, each time adding a — opencl-device <0..3>argument
	•	now you should be able to see ethminer getting work packages from geth and hopefully even “mined a block” logs in geth.
 
GPU mining with ethminer
To mine with eth:
eth -m on -G -a <coinbase> -i -v 8 //
To install ethminer from source:
cd cpp-ethereum
cmake -DETHASHCL=1 -DGUI=0
make -j4
make install
To set up GPU mining you need a coinbase account. It can be an account created locally or remotely.
 
 
********END*********
How can I check my balance?
 
eth.getBalance(“”)
ethereum -mine
 
 
 
 
We need to change permissions on the file so that Amazon’s servers can reading
Are you sure you want to continue connecting?
type yes and hit enter
type sudo apt-get update
then hit enter
Now is the turn of the ethereum miner.
We need to install it on our linux virtual machine
sudo wget http://
As soon as I see that my worker login is accepted,,
I usually quit (ctrl+c) and start up a tmux session.
Otherwise the mining process will stop the moment I close down the ssh sesssion.
With tmux, next time you login, make yourself root and just type:
tmux attach
and you will be back at where you left off :-)
 
You need an ssh client SSH CLIENT ON WINDOWS putty, putty gen On your local Mac machine: launch terminal (pre-installed with every mac)
You need a COIN BASE ADDRESS, NOT coinbase.com account!
https://www.youtube.com/watch?v=CnKnclkkbKg 3:30
type:
ethereum accounts new
 
$ ethereum — unlock=0b16XXXXXXX:mypassphrase-from-the-first-command $ ethereum — mine
 
Type putting the account number in quotes (single or double). For example:
> eth.getBalance("0x13bf27ad7781e484a0c68752f3bc949cf4262158")
You’ll notice that the addresses are printed in a collection as quoted strings:
> eth.accounts
['0x13bf27ad7781e484a0c68752f3bc949cf4262158']
This is the same when checking your coinbase:
> eth.coinbase
'0x13bf27ad7781e484a0c68752f3bc949cf4262158'
Which allows you to chain commands like so:
 
How much will you pay to run AWS GPU instance?
A note on spot instances
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

 
 
GO-ETHEREUM INSTALLATION
ethereum/go-ethereum
go-ethereum - Jeffrey Wilcke's Go implementation of the Ethereum y/w papergithub.com
INSTALLING FROM PPA
Specificly, we are going to install ethereum-cli (aka ethereum comment line( apt-get install geth
Terminal, copy and paste the instructions on à Install from PPA, one line at a time
sudo apt-get install software-properties-common
sudo add-apt-repository -y ppa:ethereum/ethereum-qt
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo add-apt-repository -y ppa:ethereum/ethereum-dev
sudo apt-get update
sudo apt-get install ethereum
now ethereum is available.
type ethereum version 
just to check
To start the ethereum client just type 
ethereum
then
ethereum -miner
 
then 
geth account list
geth — mine — logilevel “3” — minerthreads “1” console
https://www.youtube.com/watch?v=XMM47nQJUVA
https://www.youtube.com/watch?v=CnKnclkkbKg
Results: 24 MH/S ( all 4 GPUs)
two extra steps for your security.
I highly recommend you to activate Multi factor authentication for AWS
and Enable billing alerts every 10 or 50$
AWS Identity and Access Management (IAM):
The first tool in our arsenal is the AWS Identity and Access Management service (IAM). This allows you to create access keys with specific restrictions to ensure users of these keys only have access to certain account privileges and regions. Using highly specific keys for your application can prevent any unauthorized intruders from total account control. For example, I needed the AWS key that was used to compromise my account solely for file upload and download from an already existing bucket in the east region of the US. Had I used an IAM key with only these priveleges, the hackers wouldn’t have been able to do anything but download a few images I was playing around with.
 
WORK IN PROGRESS
Resources:
http://cryptorials.io/how-to-install-ethereum-frontier-video-guide/
https://aws.amazon.com/marketplace/pp/B00JV9JBDS/ref=srh_res_product_title?ie=UTF8&sr=0-2&qid=1432036933065
 
https://aws.amazon.com/marketplace/pp/B00JV9TBA6/ref=srh_res_product_title?ie=UTF8&sr=0-3&qid=1432036933065
Using Amazon EC2 to mine Dogecoin
Over the holidays I mined dogecoin using an Amazon AWS cluster compute box. My goals were as follows: to learn about…grantammons.me

How to mine Cryptocurrency with Amazon EC2
One of the amazing things about Crypto Currency is that you can create it yourself, with the proper computing power and…www.usacryptocoins.com
http://doges.org/digging-coins/mining-in-the-cloud-using-aws-gpu-instances/
Comment Redirect - Easily Increase your Following - Yoast
Redirect your first time commenters to a special page!yoast.com
https://forum.ethereum.org/discussion/2129/need-a-how-to-for-digitalocean-vps
Ubuntu 14.04 Amazon EC2 Cloud Desktop using LXQT
Using Amazon EC2's free usage tier to host your own cloud desktop is a very economical way to to have a desktop at hand…www.gaggl.com
https://github.com/ethereum/go-ethereum/wiki/mining
https://www.youtube.com/watch?v=JPw9XflZGX8
LET THE FORUM KNOW AFTER THE TUTORIAL IS COMPLETE:
https://forum.ethereum.org/discussion/2129/need-a-how-to-for-digitalocean-vps
https://forum.ethereum.org/discussion/296/cloud-mining
 
**_Thanks to paul_bxd of the ethereum forum who initiated me to cloud mining with Ethereum and AWS EC2. Without his help and resources a wouldn’t be able to put this guide together._**

A special announcement by paul_bxd
now we are offering free space to host a server you buy. We can provide free power, internet and cooling. We ask for a % of the Ether you successfully mine. Is this of interest to you?
reach me on twitter @angelomilan
If you liked this post and want to see the next one “ “ tip me some ether love

References:
http://ethereum.gitbooks.io/frontier-guide/content/gpu.html
