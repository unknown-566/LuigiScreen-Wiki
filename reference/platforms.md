# Supported Platforms

## Supported

| Platform | Status |
| --- | --- |
| Paper 1.21.11 | Supported |
| Java 21 | Supported |
| Windows x86_64 | Bundled native libraries |
| Linux x86_64 / amd64 | Bundled native libraries since alpha.7 |
| MapEngine 1.8.12 | Required |

## Not currently supported

| Platform | Reason |
| --- | --- |
| Linux ARM64 | FFmpeg ARM natives are not bundled |
| Windows ARM64 | FFmpeg ARM natives are not bundled |
| macOS | macOS natives are not bundled or tested |
| Folia | Threading model is not supported |
| Spigot/CraftBukkit server | Unsupported; the Paper/Bukkit plugin targets Paper APIs |
| Minecraft versions other than 1.21.11 | API and MapEngine compatibility are unverified |

## Hosting compatibility

The Minecraft host must permit:

- Java 21
- Plugin JARs around 55 MB
- MapEngine
- Native library extraction to a temporary directory
- Outbound TCP to MediaMTX

MediaMTX does not need to run on the Minecraft host. External MediaMTX is recommended when the panel cannot run additional executables.
