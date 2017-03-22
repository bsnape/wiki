---
layout: post
title:  "AWS Infrastructure"
date:   2016-07-06
categories: aws
---
## Components
- public subnet
- private subnet
- VPC
- IAM
- Route 53
- AZ
- ENI
- IGW
- CGW customer gateway http://docs.aws.amazon.com/AmazonVPC/latest/NetworkAdminGuide/Introduction.html
- VPG
- S3
- elasticache
-

### Public subnet


### Private subnet

### IAM (Identity and Access Management)

An IAM _role_ is similar to a user but can be assumed by anyone that requires it.
Ultimately, it is an AWS entity with permission policies that determines what the entity can and cannot do.

You can use roles to provide access to users, applications etc. to use your AWS resources without needing
to embed credentials within an application for example.

#### Usage

Roles are assigned to instances at boot time (e.g. Terraform).


### Customer Gateway (CGW)

This is when you use an Amazon VPC VPN connection to link your data centre (or network)
to your Amazon VPC.

A _customer gateway_ is the anchor on your side of the connection.
It can be a physical or software appliance.
The anchor on the AWS side is called a _virtual private gateway_.
