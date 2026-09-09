# UniversalSynSaveInstance

> [!IMPORTANT]
> Independent, unofficial project. Not affiliated with, endorsed by, or officially connected to Roblox Corporation. "Luau" is a trademark of Roblox Corporation.

Save the current Roblox data model as a place or model file.

Binary output is enabled by default:

- Places: `.rbxl`
- Models: `.rbxm`

Use `Binary = false` only when you specifically need XML output (`.rbxlx` or `.rbxmx`). Binary output supports Zstandard compression by default; set `CompressionMode = false` when an uncompressed file is needed.

## Run

```luau
local saveinstance = loadstring(game:HttpGet(
	"https://raw.githubusercontent.com/xeonise/UniversalSynSaveInstance/main/saveinstance.luau",
	true
), "saveinstance")()

saveinstance({
	SafeMode = false,
	Binary = true,
})
```

Add a revision query when testing a newly pushed version:

```luau
"https://raw.githubusercontent.com/xeonise/UniversalSynSaveInstance/main/saveinstance.luau?rev=main"
```

## Common options

```luau
saveinstance({
	Binary = true,          -- `.rbxl` / `.rbxm`; default
	SafeMode = false,       -- avoid automatic kick/disconnect handling
	IsModel = false,        -- save a place; set true for an `.rbxm`
	FilePath = "my-place", -- optional output name/path
	Object = workspace,     -- optional instance to save as a model
})
```

`Callback` can be supplied instead of writing a file:

```luau
saveinstance({
	Callback = function(contents)
		print("Generated", #contents, "bytes")
	end,
})
```

## Current binary support

The RBXL writer supports Zstandard or LZ4 compression, modern `Content`, legacy `ContentId`, `SecurityCapabilities`, shared strings, references, color sequences, and common Roblox value types.

## Limitations

- Terrain voxel data is executor-dependent. If the log says Terrain was skipped because its hidden data could not be read, the output will not include Terrain.
- Opaque/external `Content` sources cannot be reconstructed through public reflection and are omitted.
- `Instance.HistoryId` is intentionally skipped to avoid save-to-save diff noise.

## Credits and license

This fork builds on the work of [luau/UniversalSynSaveInstance](https://github.com/luau/UniversalSynSaveInstance), Moon/LorekeeperZinnia, Anaminus, Dekkonot, Synapse X contributors, Rojo, and the Roblox Client Tracker. See [LICENSE](LICENSE) for terms.
