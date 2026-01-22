# Bblwrp

## BBLWRP Technical Roadmap: The 10-Year Vision

### Enterprise Sandboxing & Deployment Framework (2026–2036)

---

<sub>This is a formal draft outlining BBLWRP’s technical trajectory. Over the next decade, BBLWRP will act as a primary driver for the **Rust-for-Windows** and **Swift-to-Wasm** ecosystems.</sub>

---

## Overview

**BBLWRP** (pronounced *“bubble wrap”*) is a next-generation isolation platform. Built on a re-architected foundation derived from **Sandboxie Plus**, it replaces legacy C/C++ with a tiered, memory-safe architecture designed for high-stakes enterprise environments.

* **Enforcement Layer (Rust):** High-performance kernel-mode drivers providing the "oxidized" foundation for Windows.
* **Orchestration Layer (Swift):** High-level policy logic and fleet management.
* **Universal Execution (Wasm):** On non-Apple platforms, Swift is compiled into **WebAssembly Components**, executing within a Rust-hosted runtime for hardware-agnostic isolation.

---

## Strategic Core: Construction over Retrofitting

BBLWRP operates on the principle that **isolation must be enforced by construction**. We move away from reactive "patch-and-pray" security toward a formal proof of safety.

* **ABI Parity:** BBLWRP is committed to contributing to `windows-rs` to ensure Rust kernel abstractions have 1:1 parity with the Microsoft C++ WDK.
* **Full Swift Support:** Over 10 years, BBLWRP will drive the maturity of "Full Swift" (beyond the Embedded subset) on Wasm, enabling rich, concurrent policy engines that leverage the complete Swift standard library.
* **Idempotent State:** All sandbox deployments are declarative. The engine reconciles the system state against the Swift-defined policy module, ensuring absolute consistency across enterprise fleets.

---

## Language Strategy: The Swift-to-Wasm Pipeline

BBLWRP standardizes on a **Swift-to-Wasm** pipeline that evolves in two stages to meet both immediate performance needs and long-term scalability.

### 1. Short-term (Embedded Swift): The Transitional Bridge

Using the **Swift 6.2+ toolchain**, we utilize the "Embedded" subset of the language. This removes the overhead of a heavy runtime and garbage collection, allowing for:

* **Minimal Binaries:** Tiny Wasm modules that load with near-zero latency.
* **Determinism:** Predictable performance critical for kernel-level policy arbitration.
* **Zero-Runtime Interop:** Clean communication with the Rust host via Wasm Interface Types (WIT).

### 2. Long-term (Full Runtime): The Final Logic Plane

Utilizing the **WASI 0.3+ Component Model**, BBLWRP will host a shared Swift runtime as a Wasm component. This enables the **full language** (Reflection, Concurrency, Foundation) with minimal per-instance overhead, allowing for highly complex, identity-aware policy engines.

---

## Roadmap: 10-Year Phased Evolution

### Phase 0: The Oxidized Foundation (Years 1–4)

**Objective:** Establish a kernel-mode enforcement layer with native C++ parity.

* **Upstream Parity:** Contributions to `windows-rs` for undocumented NT-kernel APIs.
* **Verified Drivers:** Reimplementation of Sandboxie core filtering in Rust.
* **Native Hooks:** Leveraging Windows Kernel Callbacks (WDM/KTM) for object-level isolation.

### Phase 1: The Swift Transition (Years 3–7)

**Objective:** Transition the control plane to a high-concurrency Swift-Wasm engine.

* **Swift-Wasm Maturity:** Contributing to `swift-wasm` to stabilize the WASI 0.3 concurrency model.
* **Embedded-First Hot-Path:** Deploying low-latency policy engines in Embedded Swift for real-time I/O arbitration.
* **Component Orchestrator:** Rust-hosted **Wasmtime** engine to execute multi-threaded Full Swift policies as the spec matures.

### Phase 2: Global Policy Mesh (Years 6–10)

**Objective:** Cross-platform federation and idempotent deployment at scale.

* **Unified Semantics:** Applying the same Swift-Wasm policies across Windows (Rust-driver), macOS (`Virtualization.framework`), and Linux (eBPF).
* **Identity-Aware Isolation:** Native integration with Entra ID and SPIFFE for zero-trust policy assignment.

---

## BBLWRP Enterprise SDK Architecture

```mermaid
flowchart LR
    %% Main Phases Colors
    classDef upstream fill:#f9f,stroke:#333,stroke-width:2px,color:#000;
    classDef phase0 fill:#ffeb99,stroke:#333,stroke-width:2px,color:#000;
    classDef phase1 fill:#99ccff,stroke:#333,stroke-width:2px,color:#000;
    classDef phase2 fill:#b3ffb3,stroke:#333,stroke-width:2px,color:#000;

    %% UPSTREAM
    subgraph UPSTREAM["YEARS 1-10: UPSTREAM CONTRIBUTIONS"]

    __spacer01@{ shape: text, label: "
\n

\n
" }

    __spacer02@{ shape: text, label: "
\n

\n
" }

        direction BT
        U1["windows-rs: Kernel/WDK Parity"]:::upstream
        U2["Swift-Wasm: Embedded & Full Toolchains"]:::upstream
        U3["WASI 0.3+ Component Model Specs"]:::upstream

        U1 === U2 === U3
    end

    %% PHASE 0
    subgraph P0["PHASE 0: OXIDIZED ENFORCEMENT"]
        direction BT
            __spacer001@{ shape: text, label: "
\n

\n
" }

    __spacer002@{ shape: text, label: "
\n

\n
" }
        A1["Legacy Sandboxie C++ Core"]:::phase0
        A2["Verified Rust Driver (BblwrpDrv)"]:::phase0
        A3["Rust-Native FltMgr/WDK Bindings"]:::phase0

        A1 --> A2 --> A3
    end

    %% PHASE 1
    subgraph P1["PHASE 1: THE SWIFT TRANSITION"]
        direction BT
                    __spacer0001@{ shape: text, label: "
\n

\n
" }

    __spacer0002@{ shape: text, label: "
\n

\n
" }
        B1["Embedded Swift 6.x (Wasm Guest)"]:::phase1
        B2["Low-Latency Policy Hot-Path"]:::phase1
        B3["Wasmtime-Rust Orchestrator"]:::phase1
        B4["Full Swift Runtime (WASI 0.3)"]:::phase1

        B1 --> B2 --> B3 --> B4
    end

    %% PHASE 2
    subgraph P2["PHASE 2: GLOBAL POLICY MESH"]
        direction BT
                    __spacer00001@{ shape: text, label: "
\n

\n
" }

    __spacer00002@{ shape: text, label: "
\n

\n
" }
        C1["Swift-Native Cloud Orchestration"]:::phase2
        C2["Idempotent Fleet Reconciler"]:::phase2
        C3["Cross-Platform Policy (macOS/Linux)"]:::phase2

        C1 --> C2 --> C3
    end

    %% Main Flow
    UPSTREAM --> P0 --> P1 --> P2

```

---

## Rationale for the Phased Approach

* **Security by Construction:** BBLWRP eliminates the "70% problem"—memory-safety vulnerabilities. By using Rust for kernel enforcement, we achieve **deterministic safety**.
* **The "Embedded" Advantage:** Starting with Embedded Swift ensures that we don't trade security for performance. We provide the speed of C with the safety of Swift from Day 1.
* **Idempotent Fleet Management:** Our **Declarative Engine** ensures that if a policy is defined in Swift, the Rust enforcer reconciles the system state to match it—automatically fixing "broken" sandboxes.
* **Hardware-Level Integrity:** We align with **Apple’s Memory Integrity Enforcement (MIE)** and Windows' **Rust-in-Kernel** milestones. By 2026, silicon-level memory tagging (like ARM MTE or Intel LAM) is becoming standard; BBLWRP’s architecture is designed to offload safety checks directly to this hardware.

---

## Final Clarification

> BBLWRP contains **no maintained C/C++ code**. Any C/C++ in the environment exists strictly within the legacy OS boundary.
> On non-Apple platforms, Swift orchestration logic is compiled into **WebAssembly Components** and executed within a **Rust-hosted Wasmtime instance**. This ensures that even a "compromised" policy engine cannot escape its memory-isolated execution stack.

---

## Additional Resources

- **[Rust for Windows](https://github.com/microsoft/windows-rs)** – The official Microsoft projection for calling Windows APIs natively in Rust.
- **[Swift for WebAssembly (SwiftWasm)](https://book.swiftwasm.org/)** – Documentation for the Swift toolchain targeting the Wasm Component Model.
- **[Wasmtime Component Model](https://www.google.com/search?q=https://docs.wasmtime.dev/component-model/intro.html)** – Guidance on building interoperable "Lego-brick" components with WIT.
- **[Microsoft: The Case for Memory Safety](https://www.google.com/search?q=https://aka.ms/memory-safety)** – Microsoft's 2025/2026 whitepaper on the transition from C++ to Rust in the Windows Kernel.
- **[Apple Security: Memory Integrity Enforcement](https://security.apple.com/blog/memory-integrity-enforcement/)** – Deep dive into hardware-level memory safety on Apple Silicon.
- **[WasmKit](https://github.com/swiftwasm/WasmKit)** – A lightweight, embeddable Wasm runtime written in Swift, often used for local policy testing.



Looking to get familiar with the technologies that will be used in this fork? I'm working on some separate supplementary material [here](https://github.com/DesignsbyBlanc/ETL_pipeline_sampler).

---

#### Original project ReadME starts here:

The below information is from the original project at the time the project was forked.

---
# Sandboxie Plus / Classic

<p align='center'>
EN | <a href='./README_zh_CN.md'>中文</a>
</p>

[![Plus license](https://img.shields.io/badge/Plus%20license-Custom%20-blue.svg)](./LICENSE.Plus) [![Classic license](https://img.shields.io/github/license/Sandboxie-Plus/Sandboxie?label=Classic%20license&color=blue)](./LICENSE.Classic) [![GitHub Release](https://img.shields.io/github/release/sandboxie-plus/Sandboxie.svg)](https://github.com/sandboxie-plus/Sandboxie/releases/latest) [![GitHub Pre-Release](https://img.shields.io/github/release/sandboxie-plus/Sandboxie/all.svg?label=pre-release)](https://github.com/sandboxie-plus/Sandboxie/releases) [![GitHub Build Status](https://github.com/sandboxie-plus/Sandboxie/actions/workflows/main.yml/badge.svg)](https://github.com/sandboxie-plus/Sandboxie/actions) [![GitHub Codespell Status](https://github.com/sandboxie-plus/Sandboxie/actions/workflows/codespell.yml/badge.svg)](https://github.com/sandboxie-plus/Sandboxie/actions/workflows/codespell.yml) [![WinGet Build Status](https://github.com/sandboxie-plus/Sandboxie/actions/workflows/winget.yml/badge.svg)](https://github.com/sandboxie-plus/Sandboxie/actions/workflows/winget.yml) [![Gurubase](https://img.shields.io/badge/Gurubase-Ask%20Sandboxie%20Guru-006BFF)](https://gurubase.io/g/sandboxie)

[![Roadmap](https://img.shields.io/badge/Roadmap-Link%20-blue?style=for-the-badge)](https://www.wilderssecurity.com/threads/updated-sandboxie-plus-roadmap.456886/) [![Join our Discord Server](https://img.shields.io/badge/Join-Our%20Discord%20Server%20for%20bugs,%20feedback%20and%20more!-blue?style=for-the-badge&logo=discord)](https://discord.gg/S4tFu6Enne)

|  System requirements  |      Release notes     |     Contribution guidelines   |      Security policy      |      Code of Conduct      |
|         :---:         |          :---:         |          :---:                |          :---:            |          :---:            |
| Windows 7 or higher, 32-bit or 64-bit. |  [CHANGELOG.md](./CHANGELOG.md)  |  [CONTRIBUTING.md](./CONTRIBUTING.md)  |   [SECURITY.md](./SECURITY.md)  |  [CODE_OF_CONDUCT.md](./CODE_OF_CONDUCT.md)  |

Sandboxie is a sandbox-based isolation software for 32-bit and 64-bit Windows NT-based operating systems. It creates a sandbox-like isolated operating environment in which applications can be run or installed without permanently modifying local & mapped drives or the Windows registry. An isolated virtual environment allows controlled testing of untrusted programs and web surfing.<br>

Sandboxie allows you to create virtually unlimited sandboxes and run them alone or simultaneously to isolate programs from the host and each other, while also allowing you to run as many programs simultaneously in a single box as you wish.

**Note: This is a community fork that took place after the release of the Sandboxie source code and not the official continuation of the previous development (see the [project history](./README.md#-project-history) and [#2926](https://github.com/sandboxie-plus/Sandboxie/issues/2926)).**

## ⏬ Download

[Latest Release](https://github.com/sandboxie-plus/Sandboxie/releases/latest)

## ✨ Changelog

<a href='./CHANGELOG.md'>EN</a>

## 🚀 Features

Sandboxie is available in two editions, Plus and Classic. They both share the same core components, this means they have the same level of security and compatibility.
What's different is the availability of features in the user interface.

Sandboxie Plus has a modern Qt-based UI, which supports all new features that have been added since the project went open source:

  * Snapshot Manager - takes a copy of any box in order to be restored when needed
  * Maintenance menu - allows to uninstall/install/start/stop Sandboxie driver and service when needed
  * Portable mode - you can run the installer and choose to extract all files to a directory
  * Additional UI options to block access to Windows components like printer spooler and clipboard
  * More customization options for Start/Run and Internet access restrictions
  * Privacy mode sandboxes that protect user data from illegitimate access
  * Security enhanced sandboxes that restrict the availability of syscalls and endpoints
  * Global hotkeys to suspend or terminate all boxed processes
  * A network firewall per sandbox which supports Windows Filtering Platform (WFP)
  * The list of sandboxes can be searched with the shortcut key Ctrl+F
  * A search function for Global Settings and Sandbox Options
  * Ability to import/export sandboxes to and from 7z files
  * Integration of sandboxes into the Windows Start menu
  * A browser compatibility wizard to create templates for unsupported browsers
  * Vintage View mode to reproduce the graphical appearance of Sandboxie Control
  * A troubleshooting wizard to assist users with their problems
  * An Add-on manager to extend or add functionality via additional components
  * Protections of sandboxes against the host, including the prevention of taking screenshots
  * A trigger system to perform actions, when a sandbox goes through different stages, like initialization, box start, termination or file recovery
  * Make a process not sandboxed, but its child processes sandboxed
  * Sandboxing as a unit of control to force programs to automatically use the SOCKS5 proxy
  * DNS resolution control with sandboxing as control granularity
  * Limit the number of processes in the sandbox and the total amount of memory space they can occupy, and You can limit the total number of sandboxed processes per box
  * A completely different token creation mechanism from Sandboxie's pre-open-source version makes sandboxes more independent in the system
  * Encrypted Sandbox - an AES-based reliable data storage solution
  * Prevent sandboxed programs from generating unnecessary unique identifier in the normal way

More features can be spotted by finding the sign `=` through the shortcut key Ctrl+F in the [CHANGELOG.md](./CHANGELOG.md) file.

Sandboxie Classic has the old no longer developed MFC-based UI, hence it lacks native interface support for Plus features. Although some of the missing features can be configured manually in the Sandboxie.ini configuration file or even replaced with [custom scripts](https://sandboxie-website-archive.github.io/www.sandboxie.com/old-forums/viewforum1a2d1a2d.html?f=22), the Classic edition is not recommended for users who want to explore the latest security options.

## 📚 Documentation

A GitHub copy of the [Sandboxie documentation](https://sandboxie-plus.github.io/sandboxie-docs) is currently maintained, although more volunteers are needed to keep it updated with the new changes. It is recommended to also check the following labels to track current issues: [Labels · sandboxie-plus/Sandboxie](https://github.com/sandboxie-plus/Sandboxie/labels).

A partial archive of the [old Sandboxie forum](https://sandboxie-website-archive.github.io/www.sandboxie.com/old-forums) that was previously maintained by Invincea is still available. If you need to find something specific, it is possible to use the following search query: `site:https://sandboxie-website-archive.github.io/www.sandboxie.com/old-forums/`.


## 🚀 Useful tools for Sandboxie

Sandboxie's functionality can be enhanced with specialized tools like the following:

  * [LogApiDll](https://github.com/sandboxie-plus/LogApiDll) - adds a verbose output to Sandboxie's trace log, listing invocations of relevant Windows API functions
  * [SbieHide](https://github.com/VeroFess/SbieHide) - attempts to hide the presence of SbieDll.dll from the application being sandboxed
  * [SandboxToys2](https://github.com/blap/SandboxToys2) - allows to monitor files and registry changes in a sandbox
  * [Sbiextra](https://github.com/sandboxie-plus/sbiextra) - adds additional user mode restrictions to sandboxed processes
  * [WrapLocale](https://github.com/UserUnknownFactor/WrapLocale) - provide more flexible locale pretending options than native LangId feature


## 📌 Project history

|      Timeline       |    Maintainer    |
|        :---         |       :---       |
| 2004 - 2013         | Ronen Tzur       |
| 2013 - 2017         | Invincea Inc.    |
| 2017 - 2020         | Sophos Group plc |
| 8 April 2020 - [open-source code](https://community.sophos.com/sandboxie/f/forum/119641/important-sandboxie-open-source-code-is-available-for-download) | Sophos Ltd. |
| 9 April 2020 onwards - project fork | David Xanatos |

Looking for older Sandboxie versions? Check the [version history](https://github.com/sandboxie-plus/sandboxie-old).

See the current [roadmap](https://www.wilderssecurity.com/threads/updated-sandboxie-plus-roadmap.456886/).

## 📌 Project support / sponsorship

[<img align="left" height="64" width="64" src="./.github/images/binja-love.png">](https://binary.ninja/)
Thank you [Vector 35](https://vector35.com/) for providing a [Binary Ninja](https://binary.ninja/) license to help with reverse engineering.
<br>
Binary Ninja is a multi-platform interactive disassembler, decompiler, and binary analysis tool for reverse engineers, malware analysts, vulnerability researchers, and software developers.<br>
<br>
[<img align="left" height="64" width="64" src="./.github/images/Icons8_logo.png">](https://icons8.de/)Thank you [Icons8](https://icons8.de/) for providing icons for the project.
<br>
<br>
<br>

## 🤝 Support the project

If you find Sandboxie useful, then feel free to contribute through our [Contribution guidelines](./CONTRIBUTING.md).

## 📑 Helpful Contributors

- DavidBerdik - Maintainer of [Sandboxie Website Archive](https://github.com/Sandboxie-Website-Archive/sandboxie-website-archive.github.io)
- Jackenmen - Maintainer of Chocolatey packages for Sandboxie ([support](https://github.com/Jackenmen/choco-auto/issues?q=is%3Aissue+Sandboxie))
- vedantmgoyal9 - Maintainer of Winget Releaser for Sandboxie ([support](https://github.com/vedantmgoyal9/winget-releaser/issues?q=is%3Aissue+Sandboxie))
- blap - Maintainer of [SandboxToys2](https://github.com/blap/SandboxToys2) addon
- diversenok - Security analysis & PoCs / Security fixes
- TechLord - Team-IRA / Reversing
- hg421 - Security analysis & PoCs / Code reviews
- hx1997 - Security analysis & PoC
- mpheath - Author of Plus installer / Code fixes / Collaborator
- offhub - Documentation additions / Code fixes / Qt5 patch and build script / Collaborator
- LumitoLuma - Qt5 patch and build script
- QZLin - Author of [sandboxie-docs](https://sandboxie-plus.github.io/sandboxie-docs/) theme
- isaak654 - Templates / Documentation / Code fixes / Collaborator
- typpos - UI additions / Documentation / Code fixes
- Yeyixiao - Feature additions
- Deezzir - Feature additions
- wzxjohn - Code fixes, Documentation additions
- okrc - Code fixes
- Sapour - Code fixes
- lmou523 - Code fixes
- sredna - Code fixes for Classic installer
- weihongx9315 - Code fix
- marti4d - Code fix
- jorgectf - CodeQL workflow
- stephtr - CI / Certification
- yfdyh000 - Localization support for Plus installer
- Dyras - Templates additions
- cricri-pingouin - UI fixes
- Valinwolf - UI / Icons
- daveout - UI / Icons
- kokofixcomputers - Support member of the [Discord](https://discord.gg/S4tFu6Enne) channel
- NewKidOnTheBlock - Changelog fixes
- Naeemh1 - Documentation additions
- APMichael - Templates additions
- 1mm0rt41PC - Documentation additions
- Luro223 - Documentation additions
- lwcorp - Documentation additions
- wilders-soccerfan - Documentation additions
- LepordCat - Documentation additions
- stdedos - Documentation additions

## 🌏 Translators

- czoins - Arabic
- yuhao2348732, 0x391F, nkh0472, yfdyh000, gexgd0419, Zerorigin, UnnamedOrange, DevSplash, Becods, okrc, 4rt3mi5, sepcnt, fzxx, Vstory, GT-Stardust - Simplified Chinese
- TragicLifeHu, Hulen, xiongsp - Traditional Chinese
- RockyTDR - Dutch
- clexanis, Mmoi-Fr, hippalectryon-0, Monsieur Pissou - French (provided by email)
- bastik-1001, APMichael - German
- timinoun - Hungarian (provided by email)
- isaak654, DerivativeOfLog7 - Italian
- takahiro-itou, lllIIIlll - Japanese
- VenusGirl - Korean
- 7zip, AndrzejRafalowski - Polish ([provided separately](https://forum.xanasoft.com/threads/polish-translation.4/page-2))
- JNylson - Portuguese and Brazilian Portuguese
- lufog, marat2509 - Russian
- LumitoLuma, sebadamus - Spanish
- 1FF, Thatagata - Swedish (provided by email or pull request)
- xorcan, fmbxnary, offhub - Turkish
- SuperMaxusa, lufog - Ukrainian
- GunGunGun - Vietnamese

All translators are encouraged to look at the [Localization notes and tips](https://git.io/J9G19) before sending a translation.

## 📚 Documentation Translators

- Vstory, GT-Stardust, wzxjohn - Simplified Chinese

All documentation translators are encouraged to look at the [Multilingual Translation Contribution Guide](https://github.com/sandboxie-plus/sandboxie-docs/issues/175#issuecomment-2840258519) before sending a translation.
