# Windows: NSIS /S silent install leaves incomplete payload

**Affected:** SoduWork installer v1.13.19, Windows 11 x64

## Symptom

Installing with `Setup.exe /S` finishes in ~3.5s with no error, but the client
fails to start with "installation incomplete". The installed `SoduWork.exe` is
only the ~147 KB launcher; the real payload was not extracted.

## Evidence

`.soduwork-update.log`:

```
1.13.19 payload-incomplete elapsedMs=3531
```

The graphical installer afterwards reports:

```
Cannot open the file for writing ...\d3dcompiler_47.dll
(if ignored) The installation files are not complete. This installation did not finish. Please try again.
```

## Workaround

Clean up the leftover directory (`.soduwork-versions\*.tmp`) and reinstall using
the graphical wizard with "Install for all users".

## Suggested fix

- On payload validation failure, roll back `.soduwork-versions\*.tmp` and exit
  with a non-zero code instead of leaving a half-installed launcher.
- Document whether `/S` silent install is officially supported.
