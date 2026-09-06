# RedData C++ API adapter

These unmodified headers and LICENSE come from the RedData `api` branch,
commit `26b9608da49be546631142d56eff680b43c3421a`, previously used by
RedFileSystem: https://github.com/Cyberpunk2077-Mods/RedData/tree/26b9608da49be546631142d56eff680b43c3421a

RedData v0.10.3 contains the runtime plugin sources but does not ship these
headers. Keep the adapter separate from the versioned `deps/RedData` submodule.
It calls RedData through RedLib RTTI (`RedData.Json.ParseJson` and `ToString`)
and does not link to or build RedData.dll. Those entry points are present in
v0.10.3. The RedData plugin must be installed in the game for JSON operations.

When updating RedData, check its RTTI entry points for API compatibility.
