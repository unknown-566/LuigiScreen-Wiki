# Český rychlý start

## Požadavky

```text
Paper 1.21.11 (Java 21) nebo Paper 26.x (Java 25)
Windows x64 nebo Linux x64
```

Žádný další plugin není potřeba. MapEngine už LuigiScreen nepotřebuje.

## 1. Instalace

Dej `LuigiScreen.jar` do složky `plugins/` a restartuj server.
Česky přepneš v `plugins/LuigiScreen/config.yml`:

```yaml
language: cs
```

## 2. Vytvoř obrazovku

Dívej se na levý horní blok rovné svislé stěny a napiš:

```text
/screen create main 4 3
```

Čísla jsou šířka a výška v blocích. Jeden blok = 128×128 pixelů.

## 3. Pusť něco

Soubory dej do `plugins/LuigiScreen/media/` (MP4, PNG, JPG, GIF…). Pak:

```text
/screen play main intro.mp4
/screen play main https://www.youtube.com/watch?v=aqz-KE-bpKQ
```

Když máš jen jednu obrazovku, název můžeš vynechat: `/screen play intro.mp4`.

YouTube potřebuje yt-dlp. Nainstaluješ ho jedním klikem ve Web Studiu →
**Systém → YouTube a odkazy na videa**. `yt-dlp.exe` sám nespouštěj, plugin ho volá sám.

## 4. Ovládání

```text
/screen queue main plakat.png 20s   – pustit po aktuální položce
/screen pause main                  – pozastavit / pokračovat
/screen skip main                   – další položka
/screen return main                 – zpět na normální program
/screen source main uvod.mp4        – co obrazovka ukazuje normálně
/screen info main                   – stav obrazovky
```

Všechny příkazy najdeš v [Příkazech](../reference/commands.md) nebo napiš jen `/screen`.

## 5. Web Studio

Nejjednodušší ovládání je v prohlížeči:

```text
/screen web
```

Klikni na odkaz v chatu. Funguje i na mobilu ve stejné síti. Když se odkaz
z jiného PC neotevře, povol ve firewallu serveru TCP port `8765` pro Javu.

## Vysílání z OBS

Pro živý obraz z OBS spusť `/screen obs same-pc` (vše na jednom PC) a postupuj
podle [Výběru síťového nastavení](../streaming/overview.md). Na hostingu typu
Minekeep použij `/screen obs hosting` a MediaMTX na externí VPS.

Bez veřejné IP pomůže Playit, Tailscale nebo ZeroTier, ale domácí PC musí
zůstat zapnuté. Pro 24/7 bez PC je jednodušší pustit lokální video nebo
YouTube odkaz.
