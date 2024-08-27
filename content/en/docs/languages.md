---
title: "Multi-language Support"
linkTitle: "Multi-language Support"
weight: 7
description: >
  Support multiple languages in your server.
---

For a wider range of use, Lockval Engine supports multiple languages. You can use one of the languages to develop your background program according to your own habits

## Support languages

{{< tabpane persist=false >}}

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

## Demo

You can view the demo here or build your own environment to test:

https://apidemo.lockval.com


