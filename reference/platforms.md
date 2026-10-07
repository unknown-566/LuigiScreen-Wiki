# Supported Platforms

## Supported

| Platform | Status |
| --- | --- |
| Paper 1.21.11 | Supported (Java 21) |
| Paper 26.1.x, 26.2, 26.3 | Supported (Java 25, as Paper requires) |
| Windows x86_64 | Bundled native libraries |
| Linux x86_64 / amd64 | Bundled native libraries |

No other plugin is required. PacketEvents is bundled and relocated, so a
separately installed PacketEvents plugin does not conflict.

## Not currently supported

| Platform | Reason |
| --- | --- |
| Linux ARM64 | FFmpeg ARM natives are not bundled |
| Windows ARM64 | FFmpeg ARM natives are not bundled |
| macOS | macOS natives are not bundled or tested |
| Folia | Threading model is not supported |
| Spigot/CraftBukkit server | Unsupported; the plugin targets Paper APIs |
| Minecraft versions older than 1.21.11 | Untested |

## Hosting compatibility

The Minecraft host must permit:

- Java 21 or newer
- Plugin JARs around 55 MB
- Native library extraction to a temporary directory
- Outbound TCP to MediaMTX (only for OBS streams)
- Outbound HTTPS to YouTube and GitHub (only for online video and the yt-dlp install)
- Running a downloaded executable from `plugins/LuigiScreen/bin/` (only for online video; otherwise set `online-video.yt-dlp-path`)

MediaMTX does not need to run on the Minecraft host. External MediaMTX is recommended when the panel cannot run additional executables.
