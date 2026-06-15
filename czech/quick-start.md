# Český rychlý start

Tento návod počítá s tím, že Paper server, MediaMTX a OBS běží na stejném počítači.

## Požadavky

```text
Paper 1.21.11
Java 21
MapEngine 1.8.12
LuigiScreen 1.1.0-alpha.10 nebo novější
```

Podporovaný systém:

```text
Windows x64
Linux x64
```

## 1. Vygeneruj MediaMTX config

Ve hře jako operátor spusť:

```text
/screen mediamtx same-pc
```

Vzniknou soubory:

```text
plugins/LuigiScreen/mediamtx/SAME_PC/mediamtx.yml
plugins/LuigiScreen/mediamtx/SAME_PC/setup.txt
```

Soubor `setup.txt` je soukromý, protože obsahuje heslo.

## 2. Nastav MediaMTX

Dej vygenerovaný `mediamtx.yml` vedle `mediamtx.exe` a MediaMTX restartuj.

Musí oznámit RTMP listener na portu:

```text
55556
```

## 3. Nastav OBS

Otevři:

```text
Nastavení -> Stream
```

Nastav:

```text
Služba: Vlastní
Server: celá URL z příkazu nebo setup.txt
Klíč streamu: prázdný
Použít ověření: vypnuto
```

Heslo už je součástí URL. Nedávej ho znovu do polí pro ověření.

Doporučený výstup pro plátno 7x4:

```text
Rozlišení: 896x512
FPS: 10
Bitrate: 1500 Kbps
Keyframe interval: 2 sekundy
B-frames: 0
```

Přidej v OBS zdroj obrazu a klikni na **Spustit vysílání**.

## 4. Vytvoř plátno

Postav rovnou svislou stěnu širokou 7 bloků a vysokou 4 bloky.

Dívej se na její levý horní blok a spusť:

```text
/screen create main 7 4
```

## 5. Kontrola

```text
/screen status main
/screen debug
```

## Server na hostingu

Pokud máš Minecraft server například na Minekeepu, nepoužívej `same-pc`, pokud tam MediaMTX opravdu neběží.

Použij:

```text
/screen mediamtx hosting
```

MediaMTX dej na externí VPS s veřejnou IP. Na Minekeep použij LuigiScreen `alpha.7` nebo novější, protože starší JAR neobsahoval Linux FFmpeg knihovny.

## Nemám veřejnou IP

Pro dočasné vysílání z domácího PC můžeš použít Playit, Tailscale nebo ZeroTier. Domácí PC ale musí zůstat zapnutý.

Pro skutečný provoz 24/7 použij VPS s MediaMTX a případně opakované video přes FFmpeg.
