
---
title: "Configuration"
linkTitle: "Configuration"
weight: 8

---

{{% pageinfo %}}
For basic usage, please refer to 'start.sh'. More configuration methods can be viewed by executing the -help parameter on the program
{{% /pageinfo %}}

## 0. Prepare a linux system
lockval only supports running in a Linux 64bit(arm/amd) environment, so you need to prepare a Linux operating environment. Then continue the following steps in the Linux environment

## 1. start the Lockval Engine
- unzip the zip file you downloaded
- run 'start.sh'

## about 'config.yaml'
config.yaml is the trigger configuration file. It can configure cron timer to call api and call api through http server, as well as debug script and other functions
- key: 1111 # update this file password
- pwd: 2222 # debug script password. like playground
- chk: 3333 # call api password
