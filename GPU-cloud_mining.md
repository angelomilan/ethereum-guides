# Ethereum cloud mining for dummies

LET THE ETHEREUM FORUM GUYS NOW WHEN COMPLETE

_by A.Milan and M.Terzi with the precious support of @paul_bxd and @jesus666_

This step by step tutorial is easy to follow. It is as easy as copy / pasting.
Try to understand step by step what you are doing. If you do not, it does not really matter as long as you follow the instructions properly. If you have any questions, please contact @angelomilan or @terzim 

## Problem:
I want to mine Ether, but I do not want to use my machine and I do not want to invest on new hardware and pay thousand dollar electricity bills.

## Solution: **cloud mining** aka using Amazon’s cloud servers.

Since GPU mining is set to be 100x more efficient than CPU with Ethereum, we need to look for GPU power on the cloud.
The answer, apparently, is **Amazon Web services EC2**. 

On [Ethereum forum](https://forum.ethereum.org/discussion/2134/gpu-mining-is-out-come-and-let-us-know-of-your-bench-scores) @paul_bxd revealed an inner mean (hashrate?) of 24 MH/s using an AWS g2.8xlarge instance 
comparable to the benchmark of an AMD Radeon R9 280x : 23.2 MH/S which is the best in class for ethereum mining (Nvidia Geforce is far less efficient)

The tutorial is divided in two parts. In the first, we are going to create an Ubuntu Linux virtual machine on Amazon Web Services (AWS) EC2 (Elastic Compute Cloud); in the second part, we are going to install Ethereum C++ miner on Ubuntu. 

##Part 1 - Creating an Ubuntu Linux virtual machine on Amazon Web Services (AWS) EC2 (Elastic Compute Cloud)

###Step 1 - Get an AWS account

First things first: get an AWS account [here](http://aws.amazon.com):

Amazon Web Services (AWS) is a cloud computing service provided by Amazon, the well known e-commerce giant. 

As you can see, the registration process is very handy, since you can sign-in with your existing Amazon account.
You may notice that AWS offers the EC2 service free for 750 hrs/month, for 12 months. However, that is for the Linux _t2.micro instance_. That is good for testing, but not for mining Ethereum. I will tell you later what instance to select to maximize the GPU power.

Once you have registered on [AWS](http://aws.amazon.com), you will be presented with a big list of the services offered by Amazon. Click on **EC2** (stands for, Elastic Compute Cloud), that will give you GPU horsepower for mining the Ethereum blockchain.

###Step 2 - Setup the pre-built Amazon Machine Image (AMI) on Amazon AWS EC2

An **Amazon Machine Image (AMI)** "provides the information required to launch an instance, which is a virtual server in the cloud."

For our purposes, we need to use the following AMI: 

* Image: **ami-2cbf3e44** for US-East or **ami-c38babf3** for US-West (Ubuntu Server 14.04 LTS (HVM) – CUDA 6.5)
* Instance type: **g2.2xlarge** (if you skip this step, you won’t have an nvidia device)
* Storage: Use at least 8 GB, 20+ GB recommended

How can we find it? To find a Linux AMI using the Images page

* Open the Amazon EC2 console.
* From the navigation bar, select **US East (N.Virginia)**;  
* In the navigation pane, click **Images -> AMIs**; 
* Switch to **Public Images** next to the search filter (the default is "Owned by Me" which will be at first empty, since you do not yet own any AMI)
* Click on the search filter and then (search by) _AMI ID_ -> **ami-2cbf3e44**

_Note: make always sure you are in the correct region (US East, N.Virginia as we said) otherwise you will not see the AMI we are insterested in on the list._

The ami-2cbf3e44, like all the ubuntu 14.04 images, is supported by Ethereum Frontier, but in addition this pre-built AMI has all the NVIDIA GPU drivers, OpenCL, etc... all pre-installed. 

###Step 3 - Customize and review your instance

Now we need to customize the instance to make sure we are doing things right. 

* You will be redirected to “Step 2: Choose an Instance Type”

As we said in the intro, we need a GPU instance to mine Ethereum. If you scroll down the list you will see 2 GPU instances.

* We will go for the **g2.2xlarge** (if you skip this step, you won’t have an nvidia device)
* Just click on the empty box on the left to choose the instance

_Note: At this point, if you want you can play with the **t2.micro free** instance before proceeding spending money._

* We are ready, just click “Review and launch” at the bottom

###Step 4 - Launch

You will now be prompted to create your access key aka “Key pair”. To use a virtual machine we first need an access key (keep it private!). Amazon AWS access keys consist of a pubic key and a private key.

* Click on “Create new key pair”:
* Type a name for the access key
* A **_.pem_** file will be automatically downloaded to your local machine, this is your private key.
* Backup this file since you will need this for remote access to your virtual machine on the AWS cloud
* Then click “Launch Instances”
* All right, now on “View instances”

Your instance should be pre-selected. Click connect. Connecting to your Linux instance on AWS

###Step 5 - Connect your machine

On your Mac: 

* Put your **_.pem_** file in the folder _Applications > Utilities_ 
* Launch Terminal
* Type or copy/paste ```chmod 400 /Applications/Utilities/youraccesskeyname.pem```
```ssh -i /Applications/Utilities/youraccesskeyname.pem ubuntu@YO.UR.PUBILICIP```
Note: you will need to use this line everytime you close Terminal and want to start again
* Type yes
* You should get a confirmation message ```Welcome to Ubuntu 14.04.1 LTS (GNU/Linux 3.13.0–37-generic x86_64)```
 
Note: if these steps don’t work the first time, quit Terminal and do it again, you will eventually succeed (this happened to me)



##Part 2 - Installing Ethereum on your instance and start mining

Ethereum client comes in 3 implementations. One written in [Go language](https://github.com/ethereum/go-ethereum), another written in [C++](https://github.com/ethereum/cpp-ethereum), and the third written in [Phyton](https://github.com/ethereum/pyethereum). 

At the moment, the only implementation supporting GPU mining is the [C++ implementation](https://github.com/ethereum/cpp-ethereum). However, the live testnet is running on the Go implementation (oh dear...). So you will need the Go implementation to read, synchronize the chain and credit Ether on your account, but the C++ miner to have GPU support. 
Anyway...


###Step 1 - Build Go-Ethereum client from source 

* According to the guide [here](https://github.com/ethereum/go-ethereum/wiki/Installation-Instructions-for-Ubuntu#building-from-source) we need to copy these lines to build the client:
```sudo apt-get install software-properties-common```
```sudo add-apt-repository -y ppa:ethereum/ethereum-qt```
```sudo add-apt-repository -y ppa:ethereum/ethereum```
```sudo add-apt-repository -y ppa:ethereum/ethereum-dev```
```sudo apt-get update```
```sudo apt-get install ethereum```
Copy/ paste and hit ENTER on your keyboard, one line at a time
Next time, to run the client and let it catch up with the "test-chain". 
Type:
 ```geth``` 
 or
  ```~/go-ethereum/build/bin/geth```  and hit ENTER.

But, wait don't do it! We still need to go through Step 2
or you will see the message "Block syncronization started" and terminal won't respond to your commands.

###Step 2 - Install the C++ miner (ethminer)

* Install ethminer. Again, following the [cpp-ethereum dev PPAs] guide ( https://github.com/ethereum/cpp-ethereum/wiki/Installing-clients#installing-cpp-ethereum-on-ubuntu-1404-64-bit) here is the important part. Ready to copy & paste? This time, you will need to hit ENTER 2 times after every line you paste. In fact, the program will ask you to confirm with one more ENTER
Let's do this:
 ```sudo add-apt-repository ppa:ethereum/ethereum-qt```
```sudo add-apt-repository ppa:ethereum/ethereum```
```sudo apt-get update```
```sudo apt-get install cpp-ethereum ```


_Note: do not confuse Eth (the Command Line Interface client of C++ Ethereum) and EthMiner (which is the standalone miner). Both come with the installation package but they are two different things_

* Once installed, benchmark ethminer to check that your system is in order: ```ethminer -G -M # (should give you your current hashrate, roughly 6MH/s)```

_Note: if you were just testing this guide with the free micro instance you 've now reached a dead end, in fact you will read this message "modprobe: ERROR: could not insert '**nvidia**': No such device"
The system is telling you that the gpu, an nvidia graphic card, is missing_ So, start over the guide and get the g2.8xlarge instance before proceeding any further.

You can now type
 ```geth``` 
 and wait until the client finishes catching up on the blockchain. 
 You will know that it's happened when you see lines like this coming in automatically:
 ``` I0607 21:55:54.122065    1392 chain_manager.go:662] imported 256 block(s) (0 queued 0 ignored) in 1.388846498s. #175604 [9e9b2828 / 58bacf39]``` 
 
``` I0607 21:55:56.828199    1392 chain_manager.go:662] imported 256 block(s) (0 queued 0 ignored) in 2.705918036s. #175860 [36775f4a / cb47a14d]``` 

like... every second.

No... maybe those lines are not "the client catching up on the blockchain" 
....like before if I type geth those lines coming in won't allow me to go any further... 
I type the command "geth account new" but the system ignores it

###Step 3 - Create a new account and run the syncro between the Go and C++ clients

What's this account, and why you need it? [Is this the "wallet"?]
[do I need this to put "inside" it the ether I mine?]


* So, once **geth** has finished catching up on the blockchain, generate a new account: ```~/go-ethereum/build/bin/geth account new``` 
* or simply ```geth account new``` (you can view if that was successful with ```geth account list```). The system will ask for a 'Passphrase" aka a password. To generate a complex password use thise tool called Last Pass http://lastpass.com and save it to your notepad
* You will be given an Address. Back it up in a notepad 
* Start again **geth** with RPC (remote procedure call) enabled: ```~/go-ethereum/build/bin/geth --rpc console``` or simply ```geth --rpc console```
* start ethminer: ```ethminer -M -G --opencl-device 0```

_Note: if you're using the larger g2 instance with 4 GPUs [which is the 2.8?] you may need to start ethminer 4 times, each time adding a --opencl-device <0..3> argument_ [what the f**ck? explain, step by step]

```ethminer -M -G --opencl-device 1```
```ethminer -M -G --opencl-device 2```
```ethminer -M -G --opencl-device 3```
[correct?]

* Now you should be able to see ethminer getting work packages from geth and hopefully even "mined a block" logs in geth.

_Note, if you encounter any issue or bug on this part 2 of the guide, please see the notes and comments at [Stephan Tual's GPU mining post](http://forum.ethereum.org/discussion/197/mining-faq-live-updates#latest)_


Next time you want to connect to your instance and check things,
you just need to type these lines:
```ssh -i /Applications/Utilities/youraccesskeyname.pem ubuntu@YO.UR.PUBILICIP```
(You don't need to re-install the client and the miner every time.)
You don't even need to login, as you may expect. You must remember that your new cloud machine is always working and was already "logged in".
* Start again **geth** with RPC (remote procedure call) enabled: ```~/go-ethereum/build/bin/geth --rpc console``` or simply ```geth --rpc console```
* start ethminer: ```ethminer -M -G --opencl-device 0```

[ERROR, I get "> ethminer -M -G --opencl-device 0
(anonymous): Line 1:18 Unexpected identifier (and 1 more errors)
" and the "automatic thing" starts again

## Q&A
**What if I quit Terminal and turn off my local computer?**
Does the instance stop to work?




 
**_Thanks to paul_bxd of the ethereum forum who initiated me to cloud mining with Ethereum and AWS EC2. Without his help and resources a wouldn’t be able to put this guide together._**

## Special announcement

A special announcement by @paul_bxd
```Now we are offering free space to host a server you buy. We can provide free power, internet and cooling. We ask for a % of the Ether you successfully mine. Is this of interest to you?```
 If you liked this post and want to see the next one reach me on @angelomilan on twitter and tell me you want it.
 
Upcoming tutorials:
How to cloud mine from your android device
What the f**k is ethereum?



### References:
http://ethereum.gitbooks.io/frontier-guide/content/gpu.html
https://forum.ethereum.org/discussion/2129/need-a-how-to-for-digitalocean-vps
https://forum.ethereum.org/discussion/296/cloud-mining
http://cryptorials.io/how-to-install-ethereum-frontier-video-guide/
https://aws.amazon.com/marketplace/pp/B00JV9JBDS/ref=srh_res_product_title?ie=UTF8&sr=0-2&qid=1432036933065
https://aws.amazon.com/marketplace/pp/B00JV9TBA6/ref=srh_res_product_title?ie=UTF8&sr=0-3&qid=1432036933065
http://forum.ethereum.org/discussion/2165/cloud-gpu-mining-with-amazon-aws-ec2#latest
