
---
title: "trigger"
linkTitle: "trigger"
weight: 8

---
Triggers provide the following functionality:

(cron) cron periodically executes api scripts
(debug) Upload the script directly via http and execute it
(timer) Timers provided to api scripts
(caller) Execute the api script through the http interface

## about 'config.yaml'
config.yaml is the trigger configuration file.
```yaml
key: 1111 # update this file and (cron) password
pwd: 2222 # (debug) password
chk: 3333 # (caller) password

task:
  life:
    sayHi: usr/testChat 0 */1 * * * * {"text":"I am TriggerCron. Hi"}
    SayBye: usr/testChat 30 */1 * * * * {"text":"I am TriggerCron. Bye"}

inst:
  life:
    - worldBoss:1
    - worldBoss:2

```
- key: 1111 # update this file and (cron) password
- pwd: 2222 # (debug) password
- chk: 3333 # (caller) password


## trigger http api
```console
# update config.yaml
curl --insecure "https://localhost:59102/upd?key=admin" -X POST --data-binary @config_new.yaml
# cat config.yaml
curl --insecure "https://localhost:59102/cat?key=admin"
# execute a task of an instance
curl --insecure "https://localhost:59102/cmd?key=admin&inst=life&task=sayHi"

# update and debug script
curl --insecure "https://localhost:59102/js?pwd=admin" -X POST --data-binary @main.js
curl --insecure "https://localhost:59102/lua?pwd=admin" -X POST --data-binary @main.lua
curl --insecure "https://localhost:59102/star?pwd=admin" -X POST --data-binary @main.star

# call a script function
curl --insecure "http://localhost:59102/call?chk=3333&uid=???&cmd=sys/testItsTime" -X POST -d '{"n":1}'
```