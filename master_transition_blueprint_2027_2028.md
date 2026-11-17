# The Master Transition Blueprint (Jan 2027 – Dec 2028)

**From Dual CCIE (SP & DC) Escalation Lead to CCDE & Hyperscaler Infrastructure / SRE / SWE**

## 1. Executive Strategy & The 24-Month Arc

Trying to study for the **CCDE** and **Go/Distributed Systems/LeetCode** in the same week destroys retention and guarantees burnout. Instead, this blueprint uses a **strictly sequenced two-stage arc** across 24 months (\~1,560 total hours at 15 hours/week):

1. **Stage 0: The Architecture Capstone (Jan – Jun 2027 | 6 Months)**

   * Close out your Cisco certification era completely by passing **CCDE v3.1 Written (400-007)** and the **June 2027 Practical** (Large-Scale Networks or AI Infrastructure elective).

   * *Escape Hatch (Option B):* If work escalations surge in Q1 2027, pass only the **CCDE Written (400-007)** by February 2027—securing your CCDE Expert Specialist badge and recertifying both CCIEs—and immediately launch Stage 1 four months early.

2. **Stage 1: The Systems & Software Engineering Sprint (Jul 2027 – Dec 2028 | 18 Months)**

   * Execute a disciplined, 3-phase transition from network infrastructure specialist to **Google SRE (SRE-SWE / Network SRE)**, **AWS Network Development Engineer (NDE)**, or **Meta Network Production Engineer (NetPE)**.

```
2027                                          2028
├─────────────────────┬───────────────────────┼───────────────────────┬───────────────────────┤
│   Q1–Q2 (Jan–Jun)   │    Q3–Q4 (Jul–Dec)    │   Q1–Q2 (Jan–Jun)     │    Q3–Q4 (Jul–Dec)    │
├─────────────────────┼───────────────────────┼───────────────────────┼───────────────────────┤
│   STAGE 0: CCDE     │   PHASE 1: GO & OS    │ PHASE 2: DISTRIBUTED  │ PHASE 3: K8S, GRPC &  │
│  • 400-007 Written  │  • Donovan + Harsanyi │  • Kleppmann (DDIA)   │   KERNEL PROFILING    │
│  • June Practical   │  • Cox-Buday + Ward   │  • Jeffery (Raft WAL) │  • Jean (gRPC) + OTel │
│  • Zero Go/LeetCode │  • Milestone 1 (Ship  │  • Rice (eBPF/XDP)    │  • Hausenblas (K8s)   │
│    during this box  │    internally at work)│  • Milestone 2 (Raft) │  • Gregg (bpftrace)   │
│                     │  • DSA: 1 hr/wk (0B)  │  • DSA: 3 hrs/wk      │  • Milestone 3 + Loops│
└─────────────────────┴───────────────────────┴───────────────────────┴───────────────────────┘


```

### Why AWS & GCP Certifications Are Intentionally Excluded

For **Google SRE** and **AWS Network Development Engineer (NDE)** loops, vendor cloud certifications (such as AWS Solutions Architect Pro or GCP Cloud Architect) carry **virtually zero weight** in engineering hiring committees:

* **Consumer APIs vs. Infrastructure Internals:** Cloud architect certifications test how enterprise customers stitch together managed services (IAM, Control Tower, billing). In hyperscaler SRE/NDE roles, you build and operate the underlying distributed fabric powering those services.

* **Opportunity Cost:** Skipping cloud badges saves **80–120 hours** of memorizing proprietary service limits—time reinvested directly into **NeetCode graph problems**, **eBPF/XDP**, and **Milestone 3 (Kubernetes Operator)**. *(Only add a cloud architect badge if you later pivot toward pre-sales Solutions Architecture or ProServe delivery).*

## 2. Target Roles & Interview Loop Alignment (Google SRE vs. AWS NDE)

This blueprint is engineered specifically for **Google SRE (SRE-SWE & Network SRE)** and **AWS Senior Network Development Engineer (NDE / NetDE)**, with direct crossover into **Meta Network Production Engineer (NetPE)**:

| Interview Loop Pillar | How **Google SRE** *(SRE-SWE / NetSRE)* Tests It | How **AWS Network Development Engineer (NDE)** Tests It | How Your Blueprint Covers It | 
| ----- | ----- | ----- | ----- | 
| **1. Coding & Scripting** | **1–2 full SWE-style coding rounds** (Go/Python) testing data structures, algorithms (graphs, trees, hash maps), and concurrency/log parsing. | **1–2 practical coding/automation rounds** (Go/Python) focused on network state parsing, API automation, and moderate algorithmic problem-solving. | **Donovan**, **Cox-Buday**, **Harsanyi**, and **120–150 NeetCode Go problems** in Year 2. | 
| **2. Linux & Kernel Internals** | **Heavy depth:** process states (`D-state`), memory/cgroups, `epoll`, socket buffers, `strace`, and eBPF/kernel packet drops. | **Moderate-to-high depth:** Linux routing tables, namespaces, socket behavior, TCP tuning, and host-level packet capture/diagnostics. | **Ward (*How Linux Works*)**, **Kerrisk (*TLPI*)**, **Liz Rice (*eBPF*)**, and **Brendan Gregg**. | 
| **3. Large-Scale System Design (NALSD)** | **Non-Abstract Large System Design (NALSD):** Designing distributed control planes, global load balancing, multi-region failover, backpressure, and SLO/error-budget trade-offs. | **Large-Scale Network Architecture:** Designing Clos data center fabrics, BGP/EVPN scale-out, automated traffic engineering, and blast-radius isolation. | **Kleppmann**, **Travis Jeffery (Raft WAL)**, **Milestone 2**, and your **Dual CCIE SP/DC** background. | 
| **4. Troubleshooting & Ops** | **Live simulated outage round:** Convergent debugging under ambiguity (mitigate first, hypothesize, inspect kernel/network signals). | **Deep protocol & outage troubleshooting:** BGP/MPLS/EVPN convergence bugs, microbursts, and automated remediation. | **16 years in Cisco Escalation** (your strongest natural advantage) + **Milestone 1**. | 
| **5. Behavioral / Leadership** | **Googleyness & Leadership:** Blameless postmortems, handling ambiguity, and cross-team technical influence. | **Amazon Leadership Principles (LPs):** Deep STAR stories around *Ownership*, *Dive Deep*, *Insist on the Highest Standards*, and *Invent and Simplify*. | **Cisco G12 Escalation Lead** stories and shipping **Milestone 1** in production. | 

### Calibration Notes by Target Role

* **AWS Senior NDE (L6):** Your **Dual CCIE (SP + DC)** puts you ahead of 90% of candidates on networking and architecture rounds immediately; your Go coding and Linux/automated control-plane milestones ensure you clear their software automation bar.

* **Google SRE:** Google separates SRE into **SRE-SWE** (heavier algorithmic coding) and **SRE-SE / Network SRE** (heavier Linux, networking, and systems troubleshooting). Completing the full Year 2 NeetCode ramp prepares you for **both**, with your highest natural leverage in **Google Network SRE / SRE-SE** and **AWS Senior NDE**.

## 3. Three Critical Pitfalls & How This Plan Fixes Them

Before writing a single line of code, this revised blueprint corrects three traps that routinely derail senior network engineers transitioning into hyperscaler loops:

### Pitfall 1: The 20-Book Trap (\~8,000 Pages vs. 1,560 Hours)

* **The Trap:** Reading 20 dense systems books sequentially (Kerrisk's *TLPI* alone is 1,550 pages; Brendan Gregg's two books total \~1,700 pages) will consume 12+ months in passive reading before you build real software.

* **The Correction (Spine vs. Desk Reference Split):**

  * **6 Spine Books (Read systematically):** Donovan & Kernighan, Teiva Harsanyi, Katherine Cox-Buday, Brian Ward, Martin Kleppmann, and Travis Jeffery (plus targeted chapters of Matthew Titmus and Hausenblas/Schimanski).

  * **On-Demand Desk References (Never read cover-to-cover):** Open Kerrisk (*TLPI*), Alex Rios, Richard Blum (*Bash Bible*), and Brendan Gregg (*BPF Performance Tools*) **only** when your weekend code explicitly calls a syscall (`fsync`, `epoll`, `SIGTERM`) or when you are profiling a running daemon on Sunday mornings.

  * **Streamlined Toolchain:** Replace dual gRPC books with **Erick Jean's *gRPC Go for Professionals*** paired directly with official OpenTelemetry Go docs. Pair Liz Rice's *Learning eBPF* with the pure-Go **`cilium/ebpf`** library so you stay inside the Go ecosystem rather than getting bogged down in C/libbpf build chains.

### Pitfall 2: The Friday LeetCode Volume Gap

* **The Trap:** Practicing 1 hour/week on Fridays with a strict `0 allocs/op` benchmark rule teaches great Go memory discipline, **but** hyperscaler interviewers do not run `go test -benchmem`. At 1–2 problems/week, you will only solve \~60–75 problems—too thin to build pattern-recognition speed for Google or Meta coding screens.

* **The Correction (Two-Stage DSA Ramp):**

  * **H2 2027 (Months 7–12):** Keep the **Friday 1-hour Zero-Heap Lab (`0 B/op`, `0 allocs/op`)** to master Go slices, maps, pointers, and memory alignment.

  * **All of 2028 (Months 13–24):** Expand DSA prep to **3 hours/week** (Friday 1 hr + two 1-hr weeknight blocks) and complete **120–150 curated problems (NeetCode 150)** in Go, prioritizing **Graphs (BFS/DFS/Dijkstra), Trees, Heaps, Hash Maps, and Sliding Windows**.

### Pitfall 3: GitHub Repos vs. Production Signal at Senior Level (L5/L6)

* **The Trap:** With 16+ years of experience, hyperscalers evaluate you at **Senior (L5/L6)**. At this level, standalone personal GitHub projects prove coding literacy, but hiring committees require **verified production impact**—software shipped to solve real operational pain at scale.

* **The Correction (The "Trojan Horse" Production Strategy):**

  * **Deploy Milestone 1 at Work (Q4 2027):** Build your concurrent Go ingestion/telemetry engine to solve a real Cisco escalation bottleneck (e.g., automated BGP/EVPN/SR-TE state diffing, gNMI collector health auditing, or multi-node syslog/telemetry triage). Quantify the operational impact (e.g., *"Built a concurrent Go diagnostic daemon reducing root-cause triage time by X% across Y customer nodes"*).

  * **Open-Source / Cross-Team Commit (Q2–Q3 2028):** Submit at least 1–2 merged pull requests to a cloud-native networking project (such as **Containerlab**, **Cilium**, **OpenConfig/gNMI**, or **SONiC**) so your Go code has been reviewed by external systems engineers prior to your interviews.

## 4. Hyperscaler Interview Evaluation Matrix

```
┌────────────────────────────────────┐     ┌─────────────────────────────────────────┐
│         THE INTERVIEW LOOP         │     │       YOUR EXACT CURRICULUM DRIVER      │
├────────────────────────────────────┤     ├─────────────────────────────────────────┤
│ • Systems Coding & Concurrency     │ ──► │ • Donovan, Harsanyi, Cox-Buday, Wiener  │
│ • Linux Kernel & OS Internals      │ ──► │ • Ward, Kerrisk (Ref), Rice, Gregg      │
│ • Distributed Systems Design       │ ──► │ • Kleppmann, Jeffery, Newman, Titmus    │
│ • Network Control Planes & Telemetry│ ──► │ • Woodbeck, Edelman, Jean, Hausenblas   │
└────────────────────────────────────┘     └─────────────────────────────────────────┘


```

### Role Specialization Matrix (Choose Your Final Loop Focus in Q2 2028)

| Target Role | Core Daily Focus | Primary Curriculum Anchor | Code vs. Infra Split | 
| ----- | ----- | ----- | ----- | 
| **Network Software Engineer** *(Google NetEng / AWS NetDE)* | SDN control planes, CNI plugins, eBPF data paths, fabric telemetry | Woodbeck, Rice (`cilium/ebpf`), Jean (gRPC) | **60% Code / 40% Network** | 
| **Network SRE / Production Engineer** *(Google SRE / Meta NetPE)* | Auto-healing, fleet scale, kernel bottlenecks, blast-radius defense | Gregg (*Systems & BPF*), Titmus, Jeffery | **50% Code / 50% Systems** | 
| **Platform / Cloud-Native Engineer** | Custom Kubernetes operators, service mesh control loops | Hausenblas (*Programming K8s*), Titmus | **80% Code / 20% Infra** | 
| **Distributed Systems Engineer** | Core state engines, consensus, high-throughput pipelines | Kleppmann (*DDIA*), Jeffery (*Raft*) | **100% Systems Code** | 

## 5. Stage 0: CCDE v3.1 Execution Sprint (Jan – Jun 2027)

*Rule: Zero Go coding, zero LeetCode, and zero distributed systems reading during Stage 0.*

* **Jan – Feb 2027: CCDE Written Exam (400-007)**

  * **Skip/Skim (60% of blueprint):** Core SP routing, MPLS, EVPN/VXLAN, DCI, and high availability—you already own this at Dual CCIE depth.

  * **Deep Focus (40% of blueprint):** Enterprise Campus/SD-WAN, Workforce Mobility, Zero Trust Security/Compliance, and **Business Strategy Design** (CAPEX vs. OPEX trade-offs, RPO/RTO alignment, Agile vs. Waterfall delivery, data sovereignty).

  * **Target:** Pass **400-007** by **February 28, 2027** (earns CCDE Expert Specialist badge and recertifies both CCIEs).

* **Mar – Jun 2027: CCDE Practical Exam (June 2027 Window)**

  * **Elective Selection:** Choose **Large-Scale Networks** or **AI Infrastructure** (plays directly to your SP/DC scale experience).

  * **Drill Cadence:** Complete 6–8 full-length timed scenario simulations (Cisco Live tectorials, Orhan Ergun, Martin J., or Zig Zsiga). Train yourself out of the "escalation over-engineering trap"—always choose the design constrained by the fictional customer's budget, staff skill level, and migration timeline.

  * **Target:** Sit and pass the 8-hour Practical in **June 2027**.

## 6. Stage 1: Visual Architecture Study Map (Jul 2027 – Dec 2028)

```
                   [ PHASE 1: Jul – Dec 2027 | Local Mastery & Go Mechanics ]
      Donovan + Cox-Buday (Go Sync) ──► Edelman & Oswalt (YANG/gNMI) ──► Woodbeck (Sockets)
              │                                                                │
              ├─[+] Harsanyi (100 Go Mistakes Code Review Focus)               ▼
              ▼                                                      [Project Milestone 1]
      Ward + Blum (Ref) (Linux OS & Shell)                        (Go Telemetry & Triage Engine
              │                                                    Deployed Internally at Cisco)
              └────────────────────────────────────────────────────────────────┘
                                                                               │
                                 ┌─────────────────────────────────────────────┘
                                 ▼
               [ PHASE 2: Jan – Jun 2028 | Distributed Core & Kernel Boundaries ]
      Kleppmann (DDIA Ch 1-12) ──► Jeffery (Raft & WAL) ──► Wiener Ch 16 (Graph Mechanics)
              │                                                        │
              ├─[+] Newman (Microservices Decoupling)                  ▼
              ▼                                              [Project Milestone 2]
      Kerrisk (Ref: fsync/epoll)                          (Distributed Raft Control Plane
              │                                            + XDP/eBPF Packet Filter)
              └─[+] Rice + cilium/ebpf (Kernel Data Plane)             │
                                                                       ▼
               [ PHASE 3: Jul – Dec 2028 | Scale Platforms, gRPC & Profiling ]
      Jean (gRPC) ──► Titmus (Resilience) ──► Rios (Ref: Syscalls) ──► Gregg (Systems Perf)
        │                                                                │
        └─[+] OpenTelemetry Go SDK                                       └─[+] Gregg (BPF Tools)
              │                                                                  │
              └──────────────────────────┬───────────────────────────────────────┘
                                         │
                                         ▼
                      [Project Milestone 3: K8s Custom Operator]
                   (CRD Controller + Streaming gRPC + OTel + bpftrace)


```

## 7. Detailed Phase-by-Phase Curriculum & Milestones

### Phase 1: Core Language Mechanics, Automation & OS Baselines (Jul – Dec 2027)

*Mindset Shift: Translate Cisco concepts to Linux—treat the Kernel Routing Table as your local RIB, Network Namespaces as VRFs, and Go slice headers as raw memory pointers.*

#### Spine Books (Read & Code Actively)

1. ***The Go Programming Language*** **(Donovan & Kernighan) — Chapters 1–11**

   * Master composite types (slices, maps, structs), interfaces (`io.Reader`/`io.Writer`), goroutines, buffered/unbuffered channels, `select` multiplexing, `sync.Mutex`/`sync.RWMutex`, and `go test -bench`. *(Skim Ch 12–13 reflection/unsafe; do not get stuck there).*

2. ***100 Go Mistakes and How to Avoid Them*** **(Teiva Harsanyi) — Chapters 2–6, 8–11**

   * Train your eye immediately to catch loop-variable pointer bugs, string-to-byte slice allocation spikes, goroutine leaks, context cancellation traps, and heap escape analysis using `pprof`.

3. ***Concurrency in Go*** **(Katherine Cox-Buday) — Chapters 1–6**

   * Master CSP modeling, worker pools, fan-out/fan-in pipelines, heartbeats, rate limiting, and the Go M:N work-stealing scheduler.

4. ***How Linux Works, 3rd Edition*** **(Brian Ward) — Chapters 1–11, 15, 17**

   * Solidify user-space vs. kernel-space boundaries, `/proc`, systemd, process scheduling, Linux kernel routing/netfilter, and cgroups/namespaces.

#### Targeted Chapters & On-Demand Desk References

* ***Network Programming with Go*** **(Adam Woodbeck) — Ch 3, 4, 7, 8–10:** TCP framing, deadlines, Unix Domain Sockets (IPC), and HTTP/2 multiplexing.

* ***Network Programmability and Automation, 2nd Ed.*** **(Edelman et al.) — Ch 2, 6, 8, 11, 14:** Idempotency, YANG/JSON serialization, NETCONF/RESTCONF/gNMI workflows.

* ***Generic Data Structures and Algorithms in Go*** **(Richard Wiener) — Ch 1, 5–9, 16:** Stacks, queues, hash tables, binary trees, and adjacency-list graphs (BFS/DFS).

* **Desk Reference Only:** *Linux Command Line and Shell Scripting Bible* (Blum) — Keep at your desk for `awk`, `sed`, `grep`, file descriptors (`stdin`/`stdout`/`stderr`), and `cron` automation.

#### Hands-On Execution (Jul – Dec 2027)

* **Algorithmic Lab (Fridays, 1 hr/wk — Starting Sep 2027):**

  * Focus: Arrays, Hashing, Two Pointers (`LC 217`, `LC 242`, `LC 1`, `LC 125`, `LC 167`).

  * **Year 1 Rule:** Enforce `0 B/op` and `0 allocs/op` via `go test -benchmem` on array/pointer solutions to master Go memory layout.

* **Project Milestone 1: Concurrent Telemetry & State-Diff Ingestion Engine (10 hrs/wk)**

  * **Build:** A daemonized Go CLI tool that opens concurrent non-blocking sessions to enterprise routers/controllers (NSO/CNC/gNMI/RESTCONF), parses deeply nested YANG/JSON state trees via worker-pool channel pipelines, and detects routing/EVPN state drift using `sync.RWMutex`-protected in-memory maps.

  * **Trojan Horse Deliverable:** Deploy this tool on real escalation workflows at Cisco to automate multi-device health checks and capture measurable triage time savings for your resume.

  * **Verification:** Pass `go run -race` under synthetic load with zero goroutine leaks (`pprof`) and trace `openat()`, `socket()`, and `read()` syscalls via `strace`.

### Phase 2: Distributed Core, Systems Architecture & Kernel Data Planes (Jan – Jun 2028)

#### Spine Books (Read & Code Actively)

1. ***Designing Data-Intensive Applications*** **(Martin Kleppmann) — Chapters 1–12**

   * Master storage engines (LSM-trees vs. B-trees, WAL), Protobuf schema evolution, single/multi-leader replication, consistent hashing/partitioning (map directly to ECMP/Maglev), clock drift, network partitions, and linearizable consensus (Raft/Paxos).

2. ***Distributed Services with Go*** **(Travis Jeffery) — Chapters 1–4, 7–9**

   * Build a binary Protobuf log, an on-disk append-only Write-Ahead Log (WAL) with memory-mapped index files, a gRPC server interface, Serf gossip service discovery, and **HashiCorp Raft** consensus replication.

#### Targeted Chapters & On-Demand Desk References

* ***Building Microservices, 2nd Ed.*** **(Sam Newman) — Ch 3–6, 11–13:** Bounded contexts, API versioning, bulkheads, backpressure, and service-to-service mTLS identity.

* ***Learning eBPF*** **(Liz Rice) — Ch 1–8, 10:** eBPF verifier rules, eBPF Maps, XDP/TC packet-path hooks, and Go user-space integration using **`cilium/ebpf`**.

* **Desk Reference Only:** *The Linux Programming Interface* (Michael Kerrisk) — Open strictly for **Ch 4 & 13** (`fsync()` page-cache flushing), **Ch 6 & 24–27** (`fork`/`execve`/signals), **Ch 38** (`CAP_NET_ADMIN`), and **Ch 63** (`epoll()` event loops).

#### Hands-On Execution (Jan – Jun 2028)

* **Algorithmic Lab (Ramps to 3 hrs/wk — 50–60 Problems Completed):**

  * Focus: Sliding Windows (`LC 121`, `LC 3`), Stacks (`LC 20`), Trees, and **Graph Traversals / Shortest Path** (`LC 200` Number of Islands, `LC 743` Network Delay Time, `LC 207` Course Schedule) anchored in Wiener Ch 16.

* **Project Milestone 2 (Capstone): Distributed Raft Network Control Plane + eBPF/XDP Filter**

  * **Build:** A 3-node distributed network controller in Go backed by your custom WAL (`fsync()` durability) and **HashiCorp Raft** consensus to manage shared IP pools and route intent across nodes.

  * **Kernel Upgrade:** Attach an eBPF/XDP program (via `cilium/ebpf`) to a local Linux interface to drop unauthorized ingress packets in kernel space and stream live drop/pass counters into your Go user-space control plane via eBPF Maps.

  * **Verification:** Execute `kill -9` on the active Raft leader under continuous write load; verify sub-second leader re-election, zero split-brain corruption, and clean `SIGTERM` graceful shutdown.

### Phase 3: Cloud-Native Resiliency, Custom Operators & Telemetry (Jul – Dec 2028)

#### Spine & Targeted Books

1. ***gRPC Go for Professionals*** **(Erick Jean) — Chapters 2–9**

   * Protobuf wire encoding, unary & bidirectional streaming RPCs, `FieldMasks`, rich error models, interceptors (auth, logging, validation), deadlines, and jittered exponential backoff. *(Pair with official OpenTelemetry Go SDK docs for tracing/metrics).*

2. ***Cloud Native Go*** **(Matthew Titmus) — Chapters 4–6, 9–11**

   * Context propagation, circuit breakers, Token Bucket rate limiters, load shedding, and `slog` + Prometheus + OpenTelemetry instrumentation.

3. ***Programming Kubernetes*** **(Hausenblas & Schimanski) — Chapters 3–7**

   * `client-go`, Informers/Listers/workqueues, Custom Resource Definitions (CRDs), and building reconciliation loops with **Kubebuilder**.

4. ***Systems Performance, 2nd Ed.*** **(Brendan Gregg) — Chapters 2–5, 10–11**

   * The **USE Method** (Utilization, Saturation, Errors), Linux TCP stack tuning, packet drop diagnostics, and cgroup container throttling.

5. **Desk References Only:**

   * *System Programming Essentials with Go* (Alex Rios) — Reference Ch 2, 5, 6, 7 for `SO_REUSEPORT`, `GOGC` pacer tuning, and `golang.org/x/sys/unix`.

   * *BPF Performance Tools* (Brendan Gregg) — Cookbook reference for `bpftrace` one-liners, `tcplife`, `vfs_write` latency, and container namespace tracing.

#### Hands-On Execution (Jul – Dec 2028)

* **Algorithmic Lab (3 hrs/wk — Reach 120–150 Total NeetCode Problems):**

  * Focus: Heaps/Priority Queues (`LC 215`, `LC 23`), Modified Binary Search (`LC 33`), Intervals, and timed 45-minute mock coding screens (2 problems in 45 minutes, narrating time/space complexity aloud).

* **Project Milestone 3: Resilient Kubernetes Custom Network Operator**

  * **Build:** Refactor Milestone 2 into a **Kubebuilder Kubernetes Operator** managing custom network CRDs (`NetworkPolicyIntent`, `RoutePool`). Replace legacy APIs with **bidirectional streaming gRPC** connecting the controller to host agents that dynamically program Linux `nftables`/routing tables.

  * **Defense & Observability:** Inject Token Bucket rate limiters, circuit breakers, memory-pressure load shedding, and end-to-end **OpenTelemetry** spans (CLI → gRPC stream → Operator reconciliation → host agent).

  * **Final Systems Audit:** Blast the cluster with synthetic load and diagnose bottlenecks live using `strace`, `iostat`, `tcplife`, and custom `bpftrace` scripts.

* **Q4 2028 Interview Launch:** Begin mock system design loops (NALSD / global VPC & backbone control planes, multi-region failover, and telemetry pipelines) and submit applications to Google SRE and AWS NDE.

## 8. Weekly 15-Hour Execution Calendar & Guardrails

### Year 1 Cadence (Jan – Dec 2027)

```
┌────────────────────────────────────────────────────────────────────────────┐
│                           WEEKDAY SESSIONS (7 Hours)                       │
├────────────────────────────────┬───────────────────────────────────────────┤
│ Monday – Thursday (1.5h / day) │ Friday (1 Hour)                           │
│ 🛠️ Micro-Labs / CCDE Drills    │ 💻 Go Memory & Zero-Heap Lab              │
│ • Q1-Q2 27: CCDE case drills   │ • Q1-Q2 27: CCDE weak-domain review       │
│ • Q3-Q4 27: 20m read + 70m CLI │ • Q3-Q4 27: Arrays/Maps (0 allocs/op)     │
└────────────────────────────────┴───────────────────────────────────────────┘
┌────────────────────────────────────────────────────────────────────────────┐
│                           WEEKEND SESSIONS (8 Hours)                       │
├────────────────────────────────────────────────────────────────────────────┤
│ Saturday & Sunday (4 Hours / day)                                          │
│ 🏗️ The Long-Form Runway                                                    │
│ • Q1-Q2 27: Full 8-hour CCDE Practical Simulations (Split Sat/Sun)         │
│ • Q3-Q4 27: Sat = Core Go Engineering | Sun = Integration, strace & pprof  │
└────────────────────────────────────────────────────────────────────────────┘


```

### Year 2 Cadence (Jan – Dec 2028 — Ramping DSA to 3 Hrs/Wk)

* **Mon & Wed (1.5h/day):** Systems Reading (20m) + Micro-Labs / Prototypes (70m) = **3 hrs**

* **Tue & Thu (1.5h/day):** 30m Micro-Lab + **1 hr NeetCode Go DSA** = **3 hrs**

* **Friday (1h):** **1 hr Timed Go DSA** (Total DSA = **3 hrs/week**) = **1 hr**

* **Saturday (4h):** Core Milestone Engineering (Raft, eBPF, gRPC, Kubebuilder) = **4 hrs**

* **Sunday (4h):** Integration, `kill -9` Chaos Testing & `bpftrace`/Kernel Auditing = **4 hrs**

### Non-Negotiable Execution Rules

1. **The 45-Minute Weeknight Compilation Rule:** On weeknights, never debug a compiler error, pointer bug, or eBPF verifier rejection past 45 minutes. Write a failing unit test, document the blocker in a `TODO`, and solve it fresh on Saturday morning.

2. **The "One-In, One-Out" Escalation Valve:** When a high-severity customer incident hits your Cisco queue and consumes extra evening hours, **do not cut sleep to catch up**. Drop that week's weekday micro-labs cleanly and resume on the weekend. Consistency across 104 weeks beats intensity across 3 weeks.

3. **Strict Workspace Separation:** Keep your coding desk strictly for terminal execution and compiling. Read physical/digital books away from your desk.


