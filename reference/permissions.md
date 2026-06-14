# Permissions

LuigiScreen currently has one administration permission:

```text
luigiscreen.admin
```

Default:

```text
op
```

It grants access to all `/screen` commands, including:

- Creating and removing screens
- Starting and stopping streams
- Viewing status
- Reloading configuration
- Debug statistics
- Generating MediaMTX credentials and configuration

## LuckPerms example

Grant the permission:

```text
/lp user PLAYER permission set luigiscreen.admin true
```

Remove it:

```text
/lp user PLAYER permission unset luigiscreen.admin
```

Because the MediaMTX wizard generates private credentials, do not grant this permission to untrusted users.
