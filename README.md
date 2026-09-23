# AccentLogic HP Debloat

Silent OEM cleanup for new HP Windows 10/11 PCs used by AccentLogic.

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force
irm https://raw.githubusercontent.com/accentdavid/hp-debloat/main/hp-debloat.ps1 | iex
```

Run **elevated**. The script refuses non-HP hardware unless you pass `-Force`.

The branded landing page ships on a `*.grok.me` host via Grok Build → Publish (same path as Dell Strip and Lenovo ThinkClean). The script source of truth stays on this repo so NinjaOne and `irm | iex` keep a stable URL.

## What it removes

- HP consumer AppX: myHP, JumpStarts, QuickDrop, WorkWell, EasyClean, Privacy Settings, Power Manager, Welcome, Connected Music, etc.
- HP Wolf / Sure Click / Sure Sense / Sure Run / Sure Recover (correct uninstall order)
- HP Client Security Manager, Security Update Service (last), Notifications, Connection Optimizer, Documentation
- HP Insights / Touchpoint Analytics / Poly Lens
- Trial AV that ships on consumer HP images (McAfee LiveSafe, WebAdvisor, Security Scan Plus)
- HP Support Assistant (Ninja + Windows Update cover drivers)

## What it keeps

- OneDrive
- HP Audio Control / Realtek audio UWP
- Function-key / hotkey UWP services
- Chipset, GPU, NIC, touchpad, printer drivers
- HP PC Hardware Diagnostics (useful on warranty tickets)
- HP Smart when an HP printer is present

## NinjaOne

Paste `hp-debloat.ps1` into a Windows PowerShell automation script. Run as **SYSTEM**. No GUI prompts. Log:

`C:\\ProgramData\\AccentLogic\\HP-Debloat.log`

Detection tag:

`C:\\ProgramData\\AccentLogic\\HP-Debloat.tag`

## Switches

`irm | iex` cannot pass parameters. Download then run:

```powershell
irm https://raw.githubusercontent.com/accentdavid/hp-debloat/main/hp-debloat.ps1 -OutFile $env:TEMP\\hp-debloat.ps1
& $env:TEMP\\hp-debloat.ps1 -WhatIf
```

| Switch | Meaning |
| --- | --- |
| `-WhatIf` | Report only |
| `-Force` | Skip HP manufacturer check |
| `-KeepSupportAssistant` | Leave HPSA installed |
| `-KeepOmen` | Leave Omen Gaming Hub / Command Center |
| `-RemoveDiagnostics` | Also remove HP PC Hardware Diagnostics |

## Caveats

- HP Sure Run persistence in BIOS can reinstall Wolf after reboot. Disable Sure Run in BIOS if it comes back.
- Re-run after the first reboot if Wolf or Connection Optimizer is still present.
- This does not remove Microsoft Store consumer apps (Xbox, Solitaire, etc.). Use Ninja script **Remove Microsoft Bloatware** (id 88) for that.
