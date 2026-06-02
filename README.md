# aws-vpc-from-scratch
Built a VPC on AWS from the ground up : subnets, internet gateway, and CLI. First project in a 9-part networking series.
<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Build a Virtual Private Cloud

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-vpc)

**Author:** Victor Njogu  
**Email:** victornjogu24@gmail.com

---

## Build a Virtual Private Cloud (VPC)

![Image](http://learn.nextwork.org/sincere_maroon_witty_monstera_deliciosa/uploads/aws-networks-vpc_2facf927)

---

## Introducing Today's Project!

So this project was basically my first real dive into AWS networking. I set up a VPC from scratch created the network, split it into a subnet, and hooked it up to the internet with an internet gateway. Sounds simple, but there's actually a lot going on under the hood that I didn't fully appreciate before doing this hands-on.
I also did the bonus CLI mission at the end, which honestly ended up being my favourite part. More on that later.

### What is Amazon VPC?

Before this project, I knew what a VPC was in theory an isolated chunk of the AWS cloud that's yours. But I didn't really get why it matters until I actually built one.
The way I now think about it: AWS is like a massive shared building, and your VPC is your own locked office inside it. Nobody else can walk in unless you specifically let them. You decide the layout, the access rules, everything.
Without VPCs, all your resources would just be floating in the same open space as everyone else's. That's a security nightmare. VPCs are what make AWS actually usable for real workloads.
In this project I created a VPC, carved a subnet into it, attached an internet gateway, and figured out how CIDR blocks work along the way. The CLI bonus at the end caught me off guard wasn't expecting it but I'm glad it was there.

In today's project, I used Amazon VPC to create a small isolated cloud creating a vpc creating a subnet, setup an internet gateway and even confiwgured ip addresses and CIDR vlocks 

### Personal reflection

This project took me about 4 hours 

One thing I didn't expect in this project was going to dive into the aws cli 

---

## Virtual Private Clouds (VPCs)

### What I did in this step

Went into the VPC console and created NextWork VPC with the CIDR block 10.0.0.0/16.

### How VPCs work

A VPC gives your resources a private space to live in. By default nothing from outside can reach them, and you control everything who talks to who, what's public, what's not.

### Why there is a default VPC in AWS accounts

I was surprised to find a VPC already sitting there when I opened the console. Turns out AWS creates one automatically when you set up an account, mostly so services like EC2 have somewhere to launch into right away. It's fine for experimenting, but for anything structured you'd want your own.

![Image](http://learn.nextwork.org/sincere_maroon_witty_monstera_deliciosa/uploads/aws-networks-vpc_2facf927)

### Defining IPv4 CIDR blocks

This took me a minute to wrap my head around. Basically 10.0.0.0/16 means I'm reserving a block of 65,536 IP addresses for this VPC — from 10.0.0.0 all the way to 10.0.255.255. The /16 tells you how many bits are locked in. Subnets I create later will each take a smaller chunk of this range.

---

## Subnets

### What I did in this step

Created Public 1 inside NextWork VPC, gave it the CIDR block 10.0.0.0/24, dropped it in the first available Availability Zone, and turned on auto-assign public IPv4.

### Creating and configuring subnets

Basically neighbourhoods inside your VPC. Each one gets its own IP range and sits in a specific Availability Zone. You use them to separate resources that need different levels of access public-facing stuff in one, sensitive internal stuff in another.

### Public vs private subnets

A public subnet has a path to the internet. A private one doesn't it's only reachable from within the VPC. Classic example: your web server goes in the public subnet, your database goes in the private one. Nobody should be hitting your database directly from the internet.
Worth noting even though I named it Public 1, it wasn't actually public yet at this point. That only happens once you wire up a route table to point traffic toward the internet gateway. That comes in the next project.

![Image](http://learn.nextwork.org/sincere_maroon_witty_monstera_deliciosa/uploads/aws-networks-vpc_157c4219)

### Auto-assigning public IPv4 addresses

Without this, instances launched here would only have private IPs and couldn't be reached from outside. Flipping this on means every new EC2 instance in this subnet automatically gets a public IP. Saves a step every single time.

---

## Internet gateways

### What I did in this step

Created NextWork IG and attached it to NextWork VPC.

### Setting up internet gateways

The internet gateway is literally the door between your VPC and the internet. Without it, everything inside the VPC is cut off can't reach out, can't be reached. Attaching it is what makes public resources actually public.



![Image](http://learn.nextwork.org/sincere_maroon_witty_monstera_deliciosa/uploads/aws-networks-vpc_4ae90410)

---

## Using the AWS CLI

### What I'm doing in this extension

Used CloudShell to rebuild all three resources VPC, subnet, internet gateway — using CLI commands instead of clicking through the console.

### Exploring CloudShell and CLI

CLI lets you manage AWS resources through typed commands instead of navigating menus. CloudShell is a terminal built right into AWS, so there's nothing to install just open it and start typing.

### Debugging my setup

Ran into an error when I forgot --cidr-block on the subnet command got a MissingParameters error. Pretty self-explanatory once you read it. The correct command is:
bashaws ec2 create-subnet --vpc-id [VPC-ID] --cidr-block 10.0.100.0/25
Always make sure your subnet CIDR falls within the VPC's range, otherwise you'll hit another error.

![Image](http://learn.nextwork.org/sincere_maroon_witty_monstera_deliciosa/uploads/aws-networks-vpc_9b2465411)

### Comparing CloudShell vs AWS Console

CLI is just faster. No page loads, no navigating five menus to find one setting. Once you know the commands, there's really no going back. I get now why engineers lean on this heavily for automation and scripting the console is great for learning, but CLI is where the real efficiency is.

---

---

