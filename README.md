# Xibo Client x64 — Unofficial Community Build

> **Not affiliated with Xibo Signage Ltd.** This is an independent community build provided free of charge under the terms of the [AGPLv3 licence](LICENSE). The original source code is available at [github.com/xibosignage/xibo-dotnetclient](https://github.com/xibosignage/xibo-dotnetclient).

## What this is

The official Xibo Windows Player v4 R406.3 ships as a 32-bit (x86) application. On 64-bit Windows, that means a hard virtual memory ceiling of roughly 3–4 GB regardless of how much physical RAM the machine has.

For demanding signage layouts — multiple simultaneous video zones, CefSharp-powered HTML widgets, live web content — the player hits that ceiling and crashes. This build removes that ceiling by converting the player to run as a genuine 64-bit process.

No functionality is altered. No UI is changed. The player connects to the CMS, downloads layouts, and plays content exactly as before — it just no longer crashes when it needs more than 4 GB of RAM.

## Download

Go to the [Releases](../../releases) page and download `xibo-client-v4-R406.3-win64-x64-setup.exe`.

The setup.exe includes the VC++ 2015–2022 x64 redistributable and will install it silently if not already present. It automatically detects and replaces an existing x86 Xibo Player installation.

## System requirements

- Windows 10 or Windows 11 (64-bit)
- 4 GB RAM minimum; 16 GB recommended for heavy layouts
- Existing Xibo CMS subscription (this build does not include or replace the CMS)

## What was changed

| Component | Change |
|---|---|
| `XiboClient.exe` | CLR header flags patched: 32BIT_REQUIRED cleared, AnyCPU enabled |
| `Xibo.scr` | Same CLR flag patch |
| `CefSharp.Core.Runtime.dll` | Replaced with x64 build from CefSharp 141.0.110 NuGet |
| `CefSharp.BrowserSubprocess.Core.dll` | Replaced with x64 build from CefSharp 141.0.110 NuGet |
| CEF runtime binaries | Replaced with chromiumembeddedframework.runtime.win-x64 141.0.110 |
| Locale PAK files (220 files) | Replaced with x64 CEF locale set |
| `dxcompiler.dll` / `dxil.dll` | Added (present in CEF 141 x64, absent from original x86 build) |
| `e_sqlite3.dll` | Replaced with x64 build |
| `WebView2Loader.dll` | Replaced with x64 build |
| `msvcr120.dll` | Replaced with x64 build (sourced from Microsoft.SqlServer.Types NuGet) |
| `watchdog\x86\` | Removed |
| Watchdog config | Install path updated to `C:\Program Files\Xibo Player\` |
| Installer | Reauthored from scratch using WiX Toolset 3.14 — x64 MSI + Burn bootstrapper |

All pure .NET (MSIL) assemblies with no native dependencies run as AnyCPU and required only the CLR flag patch. Mixed-mode C++/CLI assemblies and native DLLs required full replacement with genuine x64 NuGet builds.

## WiX source files

The `wix-src/` folder in this repository contains the full WiX source used to build the installer:

- `Product.wxs` — MSI product definition, shortcuts, registry, prerequisites
- `Bundle.wxs` — Burn bootstrapper wrapping the MSI and VC++ redistributable
- `Files.wxs` — Auto-generated file component manifest (314 components)

## Full write-up

A detailed technical article covering every step of the conversion process is available at **https://viperadman.blogspot.com/2026/05/converting-xibo-windows-player-x86-to-x64.html**.

## Licence

This build is derived from [Xibo for Windows](https://github.com/xibosignage/xibo-dotnetclient), which is open-source software licensed under the GNU Affero General Public License v3.0. This repository and build are provided under the same licence. See [LICENSE](LICENSE) for the full text.

Xibo is a trademark of Xibo Signage Ltd. This project is not affiliated with, endorsed by, or supported by Xibo Signage Ltd.
