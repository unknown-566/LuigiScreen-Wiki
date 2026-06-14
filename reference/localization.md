# Localization

Bundled languages:

```text
cs
en
```

Files:

```text
plugins/LuigiScreen/messages_cs.yml
plugins/LuigiScreen/messages_en.yml
```

## Change language

Edit:

```yaml
language: en
```

Then run:

```text
/screen reload
```

## Edit messages

Message files use MiniMessage formatting:

```yaml
prefix: "<dark_gray>[<green>LuigiScreen</green>]</dark_gray> "
```

Keep placeholders such as `<width>`, `<error>` and `<url>` intact.

## Add a language

1. Copy `messages_en.yml`.
2. Rename it, for example:

```text
messages_de.yml
```

3. Translate values without changing keys.
4. Set:

```yaml
language: de
```

5. Run `/screen reload`.

Missing custom-language keys fall back to bundled English.

Bundled Czech installations also use bundled Czech defaults for newly introduced keys, so an older customized Czech file does not suddenly display new labels in English.
