
# Fivem Version Check

FiveM Simple version control system



## Support

Join Discord for support.

  
## Installation Instructions

* Step 1: Create your own version file on github and make a table and save it.
**versions.json**

```bash
[   
    {
        "script":"doxria-motel",
        "version":"1.5.2"
    }
]
```

* Step 2: Paste the following code on the server side and create a checkpoint on github.

**server.lua**
```bash 
local ScriptName = GetInvokingResource() or GetCurrentResourceName()

Version = {
    DB = "https://raw.githubusercontent.com/doxria/fivem-version-check/main/versions.json",
    Loop = false,
    LoopTime = 1000
}

SetTimeout(1000, function()
    VersionCheck()
end)

NeedUpdate = "^2[Script Name]^1 A new update is available for this script."

VersionCheck = function()
    ver = GetResourceMetadata(ScriptName, "version")
    PerformHttpRequest(Version.DB, function (err, data, headers)
        local versions = json.decode(data)
        for k,v in pairs(versions) do
            if v.script == ScriptName and ver ~= v.version then 
                print(NeedUpdate)
                while Version.Loop do 
                    print(NeedUpdate)
                    Citizen.Wait(Version.LoopTime)
                end
               break
            end
        end
    end)
end
```
