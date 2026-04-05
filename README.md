# AnselOS
A domain-based microkernel-inspired operating system, built around the core principle:
> **Everything is a domain.**

---

## 📌 Overview
AnselOS is a research operating system with a unique **3-domain privilege model** and a split-kernel design:
- **Domain 0 (Kernel Domain)**: The closed-source, highest-privilege core, responsible for hardware control, memory management, and domain arbitration.
- **Domain S (Session/Temporary Domain)**: Open-source, stateless, zero-residue transition layer for secure cross-domain calls.
- **Domain N (User Normal Domain)**: Open-source, isolated execution environment for user applications and services.

The design combines the performance of a monolithic kernel with the security and fault isolation of a microkernel.

---

## 🏗️ Architecture
### 1. Three Privilege Domains
| Domain | Type | Openness | Purpose |
|--------|------|----------|---------|
| **0 (Kernel)** | Highest privilege | Closed-source | Hardware control, memory/CPU scheduling, domain lifecycle management. |
| **S (Session)** | Temporary transition | Open-source | Stateless, zero-residue cross-domain calls, interrupt handling, and atomic operations. |
| **N (User)** | Isolated execution | Open-source | Applications, system services, and user-level system call handling. |

### 2. Split-Kernel Runtime
After boot, the initial monolithic kernel splits into two parts:
- **HardwareKernel (Domain 0)**: Low-level hardware management, interrupts, DMA, and physical memory.
- **UserKernel (Domain N, privileged)**: System call front-end, permission checking, policy enforcement, and request forwarding.

### 3. Hot-Plug Mechanism
`HotHardwareKernel` is a temporary, domain-0-only instance created during hardware events (plug/unplug). It handles enumeration, driver loading/unloading, and resource management, then merges cleanly into the main `HardwareKernel`.

---

## 📂 Repository Structure
```text
AnselOS/
├── LICENSE                 # GPLv3 with Domain 0 and OSBOOT.efi closed-source exception
├── .gitignore              # Ignores build artifacts and closed-source domain 0 code
├── README.md               # This file
├── src/
│   ├── domain0/           # Closed-source: only public headers are committed
│   │   └── include/       # Public API headers
│   ├── domainS/           # Open-source: temporary transition domain
│   └── domainN/           # Open-source: user domain, UserKernel and applications
└── docs/                   # Architecture and design documentation
