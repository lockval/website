---
title: "Main API Reference"
linkTitle: "Main API Reference"
---

In the call main function, there is a parameter, which we usually define as the variable "input", which provides the following members and methods. Because the bottom layer of Lockval Engine is implemented in Go, we directly show the Go code

```go
UID      string         // Which user called (call,login,watch)
KSUID    string         // system call ID (system call)
Info     string         // login Info(login)
WATCHUID string         // Which ID to observe (watch)
Requ     map[string]any // User Request Parameters (call)

Json any // Json object

GetResp *GetAndLockResp // Data returned from GetAndLock

Throw            func(Code int, Error string)                    // Throw throw an error to the client
Log              func(v any)                                     // Log Output an arbitrary data to the console
Sleep            func(ms int64, ksuid, cmd string, obj any)      // Sleep Call cmd("xxx/xxx", obj) after timing ms, ksuid:must have value
GetSubValAll     func(id, key string) *GetOpt                    // GetSubValAll Get all the data of the key
GetAndLock       func()                                          // GetAndLock Acquire and lock (can only be called once)
DiscardAndUnlock func() (resp *PutAndUnlockResp)                 // DiscardAndUnlock discard all edits (can only be called once)
PutAndUnlock     func() (resp *PutAndUnlockResp)                 // PutAndUnlock change and unlock (can only be called once)
GetSubVal        func(id, key string, subkeys ...string) *GetOpt // GetSubVal Get some data, when the subkeys are empty, it is to get all
PutSubVal        func(id, key string, kvs ...string) *PutOpt     // PutSubVal set key value
```

The code can be viewed here:

https://github.com/lockval/go2plugin/blob/main/input.go

How to use, let's give a simple example:

{{< tabpane persistLang=false >}}

{{< tab header="Language：" disabled=true />}}

{{< tab header="JavaScript" lang="typescript" >}}
import { Dict, DBOperate } from "../libs/lockvalserver"

export function main(input: DBOperate<any>) {
  input.GetSubVal(input.UID, "mBase", "Count")
  input.GetAndLock()

  let c = input.GetResp.IDKey[input.UID]?.KeySub["mBase"]?.SubVal["Count"]

  let ci = Number(c)
  ci++
  c = ci.toString()

  input.PutSubVal(input.UID, "mBase", "Count", c)
  input.PutAndUnlock()

  return { Hello: "JS" }
}
{{< /tab >}}

{{< tab header="Lua" lang="Lua" >}}
return require("umd").define({
    "exports",
    "other/helper",
}, function(exports, helper)

    function exports.main(input)

        input.GetSubVal(input.UID, "mBase", "Count")
        input.GetAndLock()

        local c = helper.GetResp(input, input.UID, "mBase", "Count")
        if c == "" then
            c = "0"
        end
        local ci = tonumber(c)
        ci = ci + 1
        c = tostring(ci)

        input.PutSubVal(input.UID, "mBase", "Count", c)
        input.PutAndUnlock()

        local retval = {}
        retval.Hello = "Lua"
        return retval
    end

end)
{{< /tab >}}

{{< tab header="Python(Starlark)" lang="python" >}}
def main(input):
  input.GetSubVal(input.UID, "mBase", "Count")
  input.GetAndLock()

  c = input.GetResp.IDKey[input.UID].KeySub["mBase"].SubVal["Count"]

  if c=="":
    c="0"
  ci = int(c)
  ci+=1
  c = str(ci)

  input.PutSubVal(input.UID, "mBase", "Count", c)
  input.PutAndUnlock()

  return {"Hello":"Starlark"}
{{< /tab >}}

{{< tab header="Go" lang="go" >}}
package usr

import (
	"GoPluginMagicModule/src/helper"
	"strconv"

	"github.com/lockval/go2plugin"
)

func (e export) Export_testBaseCount(input *go2plugin.Input) map[string]any {
	input.GetSubVal(input.UID, "mBase", "Count")
	input.GetAndLock()

	c := helper.GetResp(input, input.UID, "mBase", "Count")

	ci, _ := strconv.Atoi(c)
	ci++
	c = strconv.Itoa(ci)

	input.PutSubVal(input.UID, "mBase", "Count", c)
	input.PutAndUnlock()

	return map[string]any{"Hello": "Go"}
}
{{< /tab >}}

{{< /tabpane >}}