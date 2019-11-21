---
layout: post
title:  "Terraform starter 101"
date:   2019-11-21 19:00:00 +0000
categories: [terraform]
---
If you're starting out in the DevOps culture space it will be hard not to hear [`Terraform`](https://terraform.io) in conversations. So what is terraform? And why should I learn it?

Terraform - Use Infrastructure as Code to provision and manage any cloud, infrastructure, or service [1].
So this is taken directly from the terraform website, but as I've readed in articles and books on the topic it can do more than that.

Why learn terraform? Well for me, it's to reproduce the same thing multiple times exactly as I declare it. UI interfaces change quite quickly as cloud providers release new items, and I just what to declare create me a server that must have x. 


Over the coming months I will pull together information and examples on how and why I've used terraform. A snippet is below of a module to create a VPC in an AWS account:

<br>
<i>main.tf</i>

{% highlight hcl %}

resource "aws_vpc" "my_vpc" {
  cidr_block = "${var.vpc_cidr}"
  instance_tenancy = "default"
  enable_dns_support = true
  enable_dns_hostnames = true
  assign_generated_ipv6_cidr_block = true

  tags = {
    Name      = "${var.vpc_name}",
    DeployedBy = "${var.deployed_by}"
  }
}

resource "aws_internet_gateway" "my_igw" {
  vpc_id = "${aws_vpc.my_vpc.id}"
  
  tags = {
    Name = "${var.vpc_name}-${var.igw_name}",
    DeployedBy = "${var.deployed_by}"
  }
}

resource "aws_subnet" "my_public_subnet" {
  count = "${length(var.public_subnets_cidr)}"
  vpc_id = "${aws_vpc.my_vpc.id}"
  availability_zone = "${element(var.availability_zones, count.index)}"
  cidr_block = "${element(var.public_subnets_cidr, count.index)}"
  map_public_ip_on_launch = true
  ipv6_cidr_block = "${cidrsubnet(
    aws_vpc.my_vpc.ipv6_cidr_block, 8, count.index+1)}"
  assign_ipv6_address_on_creation = false

  tags = {
    Name      = "${var.vpc_name}-${var.public_subnet_name}-${count.index + 1}",
    DeployedBy = "${var.deployed_by}"
  }
}

{% endhighlight %}


<br>

If you have time please join me in the next post.
<br>
<hr>
<br>
Reference:
[1] [terraform](https://terraform.io)