# Local Network

Use this setup when Paper and MediaMTX run on one computer while OBS runs on another computer in the same home network.

## Generate configuration

Run:

```text
/screen mediamtx lan
```

If LuigiScreen detects multiple LAN addresses, enter the address belonging to the network used by the OBS computer.

Example LAN address:

```text
192.168.1.64
```

## MediaMTX computer

Copy the generated `mediamtx.yml` to MediaMTX and restart it.

Allow inbound TCP `55556` in the firewall of the MediaMTX computer.

## OBS computer

Use the complete generated URL. It will contain the LAN address of the MediaMTX computer.

## Router

No router port forwarding is needed because traffic remains inside the local network.

## Keep the LAN address stable

Reserve the MediaMTX computer address in your router's DHCP settings or assign a suitable static LAN address. Otherwise the OBS URL can stop working after the address changes.
