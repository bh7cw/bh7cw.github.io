---
title: Hack on Fedora CoreOS on AWS EC2
date: 2020-07-13 18:33:49
author: bh7cw
top: true
cover: true
coverImg: /images/2.jpg
toc: true
summary: This post concludes building Fedora CoreOS image with COSA on AWS EC2.
categories: Markdown
tags:
  - Fedora CoreOS
  - CoreOS Assembler
  - AWS
---
## Build image with COSA
```
chmod +x ./cosa && ./cosa
cosa init https://github.com/coreos/fedora-coreos-config.git
cosa fetch
cosa build
cosa buildextend-aws --upload
```
-------------------------------------------
## Upload vmdk file to s3
upload built latest fedora coreos image to S3
create a s3 bucket name “images” manually
`aws s3 cp rhcos.raw s3://images`
## Import Snapshot
`aws ec2 import-snapshot --description "My dev fcos" --disk-container="file://{fcosdir}/buildfcos/fcos.json"`
## Check Status
`aws ec2 describe-import-snapshot-tasks --import-task-ids import-snap-1234567890abcdef0`
## Create Image
Create Snapshot
Ref:
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/creating-an-ami-ebs.html#creating-launching-ami-from-snapshot

## Register Image
AMIs: Register new AMI
~~(commands tbc)~~
aws ec2 register-image --image-location fcos20200610/fedora-coreos-32.20200611.dev.0-aws.x86_64.vmdk --name "MyFCOS"

## launch ec2 instance
Chose the created image to build ec2 instance.
