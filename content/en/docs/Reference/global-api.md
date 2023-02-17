---
title: "Global API Reference"
linkTitle: "Global API Reference"
---

We provide a global set of Builtin functions

```go
// MakeKSUID make a ID
func MakeKSUID() string 

// MapKeys Traverse m callback key to f
func MapKeys(m any, f func(k any))

// WatchKick kick off uid
func WatchKick(watchuid string, uid string)

// WatchClear clear all watch
func WatchClear(watchuid string)
```

And we also provide the Go plugin method to support more functions.

Generate api.\*.so files by writing plug-ins to support more functions for server-side script codes
When the api starts, it will search for api.\*.so and add all of them to become available functions of the script
This "lockval" plugin can click [here](https://github.com/lockval/api.so) to view the source code

After loading some plugins. Your plugin will be in G["YourPluginName"]

---

How to use, let's give a simple example:

{{< tabpane persistLang=false >}}

{{< tab header="Language：" disabled=true />}}

{{< tab header="JavaScript" lang="typescript" >}}
import { Dict, DBOperate } from "../libs/lockvalserver"

export function main(input: DBOperate<any>) {
  let c=globalThis.G["lockval"].Sum("a","b")
  return { Hello: globalThis.Builtin.MakeKSUID() }
}
{{< /tab >}}

{{< tab header="Lua" lang="Lua" >}}
return require("umd").define({
    "exports",
    "other/helper",
}, function(exports, helper)

    function exports.main(input)
        local c = _G.G["lockval"].Sum("a","b")
        return {Hello = _G.Builtin.MakeKSUID()}
    end

end)
{{< /tab >}}

{{< tab header="Python(Starlark)" lang="python" >}}
def main(input):
  c=globalThis.G["lockval"].Sum("a","b")
  return {"Hello":globalThis.Builtin.MakeKSUID()}
{{< /tab >}}


{{< /tabpane >}}