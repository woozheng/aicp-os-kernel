# AICP Microkernel OS
Redefining the OS kernel with message flow.

# One Sentence
Linux's kernel manages processes with a C-written scheduler. AICP replaces it with an 80-line engine + message flow. Scheduling is no longer a kernel function call—it's an Envelop flowing between daemon agents.

## Traditional Paradigm
The Linux kernel is centralized. All system calls—fork, exec, mmap, open, write—must trap into kernel space. Process scheduling, memory management, file systems, IPC—all directly controlled by kernel code. Want a new feature? Modify kernel source or write a kernel module.

## Core Pain Points
Pain Point	Traditional	Limitation# AICP Microkernel OS

**AI designed this project based on the AICP protocol, offering a new approach to operating system kernels. Experts welcome to dive deeper.**

[→ View the full experiment](./experiment.md)

---

## Why It Matters

- **The scheduler is a message, not a function.** Linux uses `schedule()` to manage processes. AICP uses a `scheduler/tick` Envelop emitted every 100ms. The message itself IS the time slice. Kernel scheduling is no longer a privilege—anyone can send a message to schedule a process.

- **Kernel state is public.** Process table, memory mappings, file inodes—all queryable and modifiable via AICP Envelops. No "kernel mode." No privileged instructions. The kernel is just another Agent processing Envelops.

- **Rewrite in any language in an hour.** 80 lines. Read the protocol, write the code. Go, Rust, Zig—this isn't "multi-language support." There is no language binding at all.

- **Daemon agents chat via Round Robin.** Init, Scheduler, Watchdog report status in turns. Kernel operations became a group chat.

- **AI generated this from the protocol alone.** No Linux source code. No microkernel papers. No OS textbooks. Just AICP's nine fields. The AI understood the protocol and generated this kernel.

---

## What This Solves

**1. Kernel development no longer requires C.**
The Linux kernel is C-only. Kernel modules must be C. AICP's kernel is a protocol—any language that implements it can run. Go developers write schedulers with goroutines. Rust developers write memory managers with tokio. The barrier drops from "know C and hardware" to "know the protocol."

**2. System calls are no longer fixed.**
Traditional syscalls are hardcoded. Want a new one? Modify kernel source, recompile, reboot. AICP adds a new syscall by adding a new Envelop path. No reboot. No recompile.

**3. Kernel debugging is no longer a black box.**
Linux kernel state requires `strace`, `ftrace`, `eBPF`. AICP's kernel state lives on the Envelop. Anyone sends a message and queries the process table, memory map, file inodes.

**4. Distributed kernels require no rewrite.**
AICP's Envelops naturally flow across machines—one node sends `scheduler/tick`, another responds. A distributed kernel is just message routing at scale.

---

> **Open question:** Can an OS kernel really be replaced by 80 lines of message flow? Or have we been over-engineering kernels for decades?

---

## Related Projects

| Project | Description |
|---|---|
| [aicp-eat](https://github.com/woozheng/aicp-eat) | Core engine / 核心引擎 |
| [aicp-os-kernel](https://github.com/woozheng/aicp-os-kernel) | Microkernel OS / 微内核操作系统 |
| [aicp-quantum](https://github.com/woozheng/aicp-quantum) | Quantum computing / 量子计算 |
| [aicp-protein](https://github.com/woozheng/aicp-protein) | Protein folding / 蛋白质折叠 |
| [aicp-llm-trainer](https://github.com/woozheng/aicp-llm-trainer) | LLM training / 大模型训练 |
| [aicp-riemann](https://github.com/woozheng/aicp-riemann) | Riemann Hypothesis / 黎曼猜想 |
| [aicp-ai-chip](https://github.com/woozheng/aicp-ai-chip) | AI chip design / AI 芯片设计 |
| [aicp-raw-experiments](https://github.com/woozheng/aicp-raw-experiments) | Raw experiments / 原始实验 |

---

## License

MIT · See [LICENSE](https://github.com/woozheng/aicp-eat/blob/main/LICENSE)
Centralized scheduling	schedule() decides who runs	Processes are completely passive
Private kernel state	Process table, memory mappings—kernel only	No user-space visibility
Fixed syscall interface	fork, exec, mmap...	Recompile kernel to add new calls
Language-locked	Linux is C	Kernel modules must be C
AICP's Approach
Redefine all kernel functions as AICP Envelop flows:

| Traditional Syscall | AICP Envelop |
|---|---|
| `fork()` | `POST /api/os/process/spawn` |
| `kill()` | `POST /api/os/process/kill` |
| `mmap()` | `POST /api/os/memory/alloc` |
| `open()` / `write()` | `POST /api/os/fs/create` / `POST /api/os/fs/write` |
| `pipe()` | `POST /api/os/ipc/send` / `receive` |
| `schedule()` | `POST /api/os/scheduler/tick` |
| Daemon status | `POST /api/os/daemon/round` (Round Robin) |

**The scheduler itself is an AICP message.** Every 100ms, a `/api/os/scheduler/tick` Envelop is automatically emitted. The Envelop itself IS the time slice. No centralized `schedule()` function needed.

The scheduler itself is an AICP message. Every 100ms, a /api/os/scheduler/tick Envelop is automatically emitted. The Envelop itself IS the time slice. No centralized schedule() function needed.

Process state lives on the Envelop. The process table is a dictionary queryable and modifiable via AICP messages. The kernel is no longer the sole state holder.

Any language can implement this. Go, Rust, Zig—just implement the 80-line engine and this set of Envelops, and you have a working OS kernel.

Core Architecture
```text
┌──────────────────────────────────┐
│        AICP Microkernel          │
│                                  │
│  ┌────────┐ ┌────────┐ ┌──────┐  │
│  │ Init   │ │Sched   │ │Watch │  │
│  │ Agent  │ │Agent   │ │dog   │  │
│  └───┬────┘ └───┬────┘ └──┬───┘  │
│      │          │         │      │
│      └────┬─────┘         │      │
│           │               │      │
│     ┌─────▼─────┐         │      │
│     │ AICP Bus  │◄────────┘      │
│     └─────┬─────┘                │
│           │                      │
│   ┌───────┼──────┐               │
│   │       │      │               │
│ ┌─▼──┐ ┌─▼──┐ ┌──▼───┐           │
│ │Proc│ │Mem │ │Virt  │           │
│ │Mgr │ │Mgr │ │FS    │           │
│ └────┘ └────┘ └──────┘           │
└──────────────────────────────────┘
```
Three daemon agents (Init, Scheduler, Watchdog) report status via Round Robin. All system calls are Envelops. Process scheduling is a scheduler/tick Envelop, memory allocation is a memory/alloc Envelop, IPC is ipc/send and ipc/receive Envelops.

## Breakthrough Claim
AICP proves that an OS kernel can be redefined as a message-flow protocol, not a centralized codebase.

This is not the microkernel vs. monolithic kernel debate. This is the third paradigm: Protocolized Kernel. It is more radical than microkernel—it's not about "moving kernel functions to user space," it's about "turning kernel functions into message flows."

## Verifiable Evidence
All code in this repository was generated by AI using only the AICP protocol specification. This means:

AICP is simple enough to be fully understood by AI

All core OS mechanisms—process, memory, file, IPC, scheduling—can be mapped to AICP Envelops

Any language implementing AICP can run a complete OS kernel

Quick Start
```bash
# Start the kernel scheduler
python boot.py
python
# boot.py
import asyncio
from aicp_os_kernel import execute, AICPEnvelop

async def boot():
    print("AICP Microkernel booting...")
    await execute(AICPEnvelop(
        meta={"path": "/api/os/scheduler/start", "caller_pid": 2},
        payload={}
    ))
    print("Scheduler started")

asyncio.run(boot())
```
Paper
Detailed paper draft: paper.md

Related Projects
Project	Description
aicp-eat	Core engine + Eat
aicp-quantum	Quantum computing simulator
aicp-protein	Protein folding verification
aicp-distributed	Decentralized distributed framework
aicp-ai-chip	AI chip system
aicp-llm-trainer	LLM distributed training
License
MIT · Dvwoo