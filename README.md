# TermTrax

TermTrax is an ultra lightweight terminal-only SSH and serial connection manager for Linux. It is a single Bash script built for small systems like Raspberry Pi or the uConsole CM4, and it keeps everything in a simple text UI.

## What it does

- Store and organize SSH and serial connections in a local config file.
- Launch SSH sessions with optional key, agent, or password authentication.
- Launch serial console sessions using common tools (screen, tio, picocom, minicom).
- Detach and reattach sessions when GNU screen is available.
- Optionally encrypt stored SSH passwords in a local vault using OpenSSL.

## Requirements

- screen (detach/reattach)
- openssl (password vault)
- sshpass (non-interactive SSH password)
- tio, picocom, or minicom (serial fallback if screen is not installed)

## Install

```bash
chmod +x termtrax
mv termtrax ~/bin/termtrax
```

Adjust the destination to a directory on your PATH, such as `/usr/local/bin`.

## Run

```bash
termtrax
```

Use the menu to add, edit, delete, and connect to sessions.

## SSH usage notes

TermTrax builds a standard SSH command and prints it before connecting. You can leave the session normally:

- Exit: type `exit` or press Ctrl+D
- SSH escape: `~.` to terminate a stuck session
- With screen: Ctrl+A then D to detach and keep the session running

## Serial usage notes

TermTrax prefers GNU screen for serial because it supports detach. If screen is unavailable, it falls back to other tools in this order: tio, picocom, minicom.

Exit keys by tool:
- screen: Ctrl+A then K to kill, or Ctrl+A then D to detach
- tio: Ctrl+T then Q
- picocom: Ctrl+A then Ctrl+X
- minicom: Ctrl+A then Z then X

## Password vault

If OpenSSL is installed, TermTrax can store SSH passwords in `~/.config/termtrax/vault.enc`. The vault is encrypted with:

```
openssl enc -aes-256-cbc -md sha512 -a -pbkdf2 -iter 100000 -salt
```

The master password is requested on first access and kept only in memory. You can reset the vault from the Settings menu, which will delete the encrypted file.

If OpenSSL is not available, TermTrax runs in degraded mode and does not store passwords.

## Files and locations

- Connections: `~/.config/termtrax/connections.db`
- Settings: `~/.config/termtrax/settings.conf`
- Vault: `~/.config/termtrax/vault.enc`

## Examples

- Add an SSH host and connect:
  - Name: `web01`
  - Type: `ssh`
  - Host: `10.0.0.5`
  - User: `pi`
  - Port: `22`
  - Auth: `key`

- Add a serial console:
  - Name: `router-console`
  - Type: `serial`
  - Device: `/dev/ttyUSB0`
  - Baud: `115200`

- Detach and reattach:
  - Detach from a screen session with Ctrl+A then D
  - Reattach via Settings -> Reattach sessions
