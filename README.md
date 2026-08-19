

# **A Curated Guide to Code Sandboxing Solutions**

This document provides a comprehensive, curated list and analysis of modern code sandboxing solutions. It is designed to help developers, architects, and engineering leaders navigate the complex landscape of secure code execution, from foundational technologies to full-fledged platforms.

## Table of Contents

- [1. Why Secure Code Execution Matters Now](#1-why-secure-code-execution-matters-now)
- [2. Sandboxing Technologies](#2-sandboxing-technologies)
  - [Sandboxing Technologies Feature Matrix](#sandboxing-technologies-feature-matrix)
  - [2.1. Micro-Virtual Machines (MicroVMs): Hardware-Level Isolation](#21-micro-virtual-machines-microvms-hardware-level-isolation)
    - [Firecracker](#firecracker)
    - [libkrun](#libkrun)
  - [2.2. Application Kernels: Intercepting the System Call](#22-application-kernels-intercepting-the-system-call)
    - [gVisor](#gvisor)
    - [nsjail](#nsjail)
  - [2.3. Language Runtimes: Lightweight, High-Speed Isolation](#23-language-runtimes-lightweight-high-speed-isolation)
    - [WebAssembly (WASM)](#webassembly-wasm)
    - [V8 Isolates](#v8-isolates)
  - [2.4. Containerization: Namespace-Based Isolation](#24-containerization-namespace-based-isolation)
    - [Docker/OCI Containers](#dockeroci-containers)
    - [Incus System Containers](#incus-system-containers)
- [3. Feature Matrix: At-a-Glance Comparison](#3-feature-matrix-at-a-glance-comparison)
- [4. In-Depth Platform Profiles](#4-in-depth-platform-profiles)
  - [4.1. e2b: The AI Agent Sandbox Runtime](#41-e2b-the-ai-agent-sandbox-runtime)
  - [4.2. Daytona: Secure & Elastic Infrastructure for AI Code](#42-daytona-secure--elastic-infrastructure-for-ai-code)
  - [4.3. microsandbox: Self-Hosted MicroVMs for Untrusted Code](#43-microsandbox-self-hosted-microvms-for-untrusted-code)
  - [4.4. WebContainers: Browser-Native Development Runtime](#44-webcontainers-browser-native-development-runtime)
  - [4.5. Replit: Collaborative Browser-Based Development](#45-replit-collaborative-browser-based-development)
  - [4.6. Cloudflare Workers: Edge Computing with V8 Isolates](#46-cloudflare-workers-edge-computing-with-v8-isolates)
  - [4.7. Fly.io: Modern Application Hosting with MicroVMs](#47-flyio-modern-application-hosting-with-microvms)
  - [4.8. Kata Containers: Secure Container Runtime](#48-kata-containers-secure-container-runtime)
  - [4.9. Other Notable Platforms & Cloud Development Environments (CDEs)](#49-other-notable-platforms--cloud-development-environments-cdes)
  - [4.10. llm-sandbox: A Self-Hosted Library for LLM-Generated Code](#410-llm-sandbox-a-self-hosted-library-for-llm-generated-code)
  - [4.11. Docker Sandboxes: Disposable MicroVMs for AI Coding Agents](#411-docker-sandboxes-disposable-microvms-for-ai-coding-agents)
  - [4.12. Vercel Sandbox: Firecracker MicroVMs for Agent Workloads](#412-vercel-sandbox-firecracker-microvms-for-agent-workloads)
  - [4.13. AWS Bedrock AgentCore: Managed Agent Runtime & Code Interpreter](#413-aws-bedrock-agentcore-managed-agent-runtime--code-interpreter)
  - [4.14. Apple Containerization: VM-Backed Containers for macOS](#414-apple-containerization-vm-backed-containers-for-macos)
  - [4.15. NVIDIA OpenShell: Policy-Governed Sandboxing for Autonomous Agents](#415-nvidia-openshell-policy-governed-sandboxing-for-autonomous-agents)
  - [4.16. Koyeb Sandboxes: Bare-Metal MicroVMs for AI Agents](#416-koyeb-sandboxes-bare-metal-microvms-for-ai-agents)
  - [4.17. Amazing Sandbox: A Local CLI Wrapper Over OS-Native Sandboxing](#417-amazing-sandbox-a-local-cli-wrapper-over-os-native-sandboxing)
  - [4.18. nono: Per-Tool-Call Brokered Sandboxing for Agents](#418-nono-per-tool-call-brokered-sandboxing-for-agents)
  - [4.19. Deno Sandbox: Firecracker MicroVMs on Deno Deploy](#419-deno-sandbox-firecracker-microvms-on-deno-deploy)
  - [4.20. SandBase Harness: Multi-Backend Runtime for AI Agents](#420-sandbase-harness-multi-backend-runtime-for-ai-agents)
- [5. Credential Injection Proxies: Keeping Secrets Out of Sandboxed Code](#5-credential-injection-proxies-keeping-secrets-out-of-sandboxed-code)
  - [5.1. iron-proxy: An Egress Firewall for Untrusted Workloads](#51-iron-proxy-an-egress-firewall-for-untrusted-workloads)
  - [5.2. Infisical Agent Vault: A Local TLS-Terminating Credential Proxy](#52-infisical-agent-vault-a-local-tls-terminating-credential-proxy)
- [6. Docker vs MicroVM for Sandboxing](#6-docker-vs-microvm-for-sandboxing)
- [7. Choosing Your Sandbox: A Decision Framework](#7-choosing-your-sandbox-a-decision-framework)
  - [Axis 1: Security vs. Performance vs. Compatibility](#axis-1-security-vs-performance-vs-compatibility)
  - [Axis 2: Stateless Functions vs. Stateful Workloads](#axis-2-stateless-functions-vs-stateful-workloads)
  - [Axis 3: SaaS Convenience vs. Self-Hosted Control](#axis-3-saas-convenience-vs-self-hosted-control)
  - [Axis 4: AI/Agent-Specific vs. General-Purpose](#axis-4-aiagent-specific-vs-general-purpose)
- [8. Contributing](#8-contributing)

## **1\. Why Secure Code Execution Matters Now**

Code sandboxing has moved from a niche security tool to essential infrastructure for modern applications. Originally, sandboxing was mainly used by security teams to safely analyze suspicious files without risking production systems. While this remains important, two major trends have made sandboxing essential for product development.

**AI and LLM Applications**: Large Language Models now generate code that needs to run safely. AI agents, data analysis tools, and UI frameworks must execute untrusted, dynamically generated code. This requires secure runtime environments. For example, [Hugging Face uses e2b's sandboxing](https://e2b.dev/blog/how-hugging-face-is-using-e2b-to-replicate-deepseek-r1) for reinforcement learning pipelines, and [Groq uses e2b for Compound AI systems](https://e2b.dev/blog/groqs-compound-ai-models-are-powered-by-e2b) that combine LLMs with live code execution.

**User-Programmable Platforms**: Many SaaS applications, data tools, and developer platforms now let users submit their own code through plugins, custom scripts, or data transformations. This requires secure isolation to prevent security vulnerabilities. The same need exists for [Cloud Development Environments (CDEs)](https://www.gitpod.io/cde) and online IDEs like GitHub Codespaces, Gitpod, and Coder, which must isolate each user's environment from the host infrastructure and other users.

Sandboxing has changed from a security-only tool to a platform feature that enables new capabilities. The focus has shifted from just preventing attacks to safely enabling powerful functionality. This means sandboxes must be fast, reliable, and easy to use - not just secure. Modern solutions are judged by their SDKs, execution speed, and integration ease, because sandboxes are now part of core product functionality.

## **2\. Sandboxing Technologies**

Different sandboxing technologies make different trade-offs between three key factors: **Security Isolation**, **Performance & Startup Speed**, and **Compatibility** (how closely the sandbox behaves like a real machine). No approach is perfect - each makes different compromises.

## **Sandboxing Technologies Feature Matrix**

The following table provides a comparative overview of the core sandboxing technologies, highlighting their key characteristics and trade-offs:

| Technology | Isolation Level | Startup Time | Resource Overhead | Hardware Requirements | Compatibility | Primary Use Cases |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| [**Firecracker** ↓](#firecracker) | Hardware-Level | ~125ms | Low (5MB per VM) | KVM/Hardware Virtualization | Full Linux | Serverless, AI agents, ephemeral workloads |
| [**libkrun** ↓](#libkrun) | Hardware-Level | ~Container-speed | Low | KVM/Hardware Virtualization | Full Linux | Embedded sandboxing, self-hosted platforms |
| [**gVisor** ↓](#gvisor) | Application Kernel | ~100ms | Medium | Any Linux host | High (Linux API subset) | Multi-tenant containers, cloud services |
| [**nsjail** ↓](#nsjail) | Process-Level | ~50ms | Very Low | Any Linux host | High (filtered syscalls) | Code execution, long-running processes |
| [**Docker/OCI** ↓](#dockeroci-containers) | Namespace-Level | ~10-50ms | Very Low | Any Linux host | Full Linux | Development, CI/CD, application deployment |
| [**Incus** ↓](#incus-system-containers) | System Container / QEMU VM | ~1-3s (containers), ~3-10s (VMs) | Low (containers), Medium (VMs) | Any Linux host (containers) or KVM/QEMU (VMs) | Full Linux (full init system, dedicated kernel in VM mode) | AI agent sandboxing, development environments, multi-tenant isolation |
| [**WebAssembly** ↓](#webassembly-wasm) | Runtime-Level | ~10ms | Very Low | Any platform | Limited (WASM modules) | Edge computing, plugin systems |
| [**V8 Isolates** ↓](#v8-isolates) | Runtime-Level | ~1ms | Very Low | Any platform | JavaScript only | Edge functions, serverless JavaScript |

### **Key Characteristics:**

- **Isolation Level**: The strength of security boundaries between sandboxed processes
- **Startup Time**: Time to initialize and start executing code in the sandbox
- **Resource Overhead**: Memory and CPU overhead per sandbox instance
- **Hardware Requirements**: Dependencies on specific hardware or virtualization features
- **Compatibility**: How closely the sandbox environment matches a full operating system
- **Primary Use Cases**: Most common scenarios where each technology excels

### **2.1. Micro-Virtual Machines (MicroVMs): Hardware-Level Isolation**

MicroVMs provide the strongest security isolation. They use hardware virtualization to give each sandbox its own kernel, memory space, and virtual devices. This creates a hardware-enforced boundary between guest code and the host operating system, avoiding the shared kernel vulnerabilities of containers. The key innovation is dramatically faster boot times, making VMs practical for short-lived workloads.

#### **Firecracker**

* **GitHub:** [firecracker-microvm/firecracker](https://github.com/firecracker-microvm/firecracker)
* **Website:** [firecracker-microvm.github.io](https://firecracker-microvm.github.io/)

Developed and open-sourced by Amazon Web Services (AWS), [Firecracker](https://firecracker-microvm.github.io/) is a virtual machine monitor (VMM) that uses the Linux Kernel Virtual Machine (KVM) to create and manage lightweight microVMs. Its design philosophy is minimalist. It intentionally excludes all non-essential devices like USB controllers, graphics cards, and sound cards, which drastically reduces the potential attack surface and lowers the memory overhead of each microVM to less than 5 MiB.

* **Mechanism:** Firecracker runs as a user-space process on a host machine and is controlled via a RESTful API. This API allows for the programmatic configuration of the microVM, including setting the number of vCPUs, memory size, and attaching network interfaces or block devices. This API-driven approach is crucial for automating the lifecycle of sandboxes in cloud-native applications.  
* **Performance:** Firecracker's defining feature is its startup speed. It can launch a microVM and initiate user-space code in as little as 125 milliseconds, with a creation rate of up to 150 microVMs per second on a single host. This performance bridges the gap between the slow boot times of traditional VMs (often 10+ seconds) and the fast startup of containers, making it suitable for high-throughput, on-demand workloads like serverless functions. This capability is what enables services like AWS Lambda and AWS Fargate to provide isolated execution environments at scale.  
* **Security Model:** For defense-in-depth, Firecracker employs a companion "jailer" process. The jailer sets up a secure environment using Linux cgroups and namespaces to isolate the Firecracker VMM process itself before dropping its privileges. This provides a second layer of containment in the unlikely event that the virtualization barrier is compromised.  
* **Adoption:** This technology is the foundation for many major platforms:
  - [**e2b** ↓](#41-e2b-the-ai-agent-sandbox-runtime) - leverages Firecracker for secure, fast-starting sandboxes for AI agents
  - [**Fly.io** ↓](#47-flyio-modern-application-hosting-with-microvms) - uses Firecracker microVMs for modern application hosting
  - [**Vercel Sandbox** ↓](#412-vercel-sandbox-firecracker-microvms-for-agent-workloads) - runs untrusted/AI-generated code in Firecracker microVMs across Vercel's regional infrastructure
  - [**AWS Bedrock AgentCore** ↓](#413-aws-bedrock-agentcore-managed-agent-runtime--code-interpreter) - provisions a dedicated Firecracker microVM per agent session for its Code Interpreter and Runtime tools
  - **AWS Lambda** - Amazon's serverless computing service runs on Firecracker
  - **AWS Fargate** - Amazon's container hosting service uses Firecracker for isolation

#### **libkrun**

* **GitHub:** [containers/libkrun](https://github.com/containers/libkrun)
* **Website:** [github.com/containers/libkrun/wiki](https://github.com/containers/libkrun/wiki)

Similar to Firecracker, libkrun is a library-based virtualization solution designed to create lightweight, KVM-based virtual machines with minimal overhead. It is the core technology powering [**microsandbox**](https://github.com/microsandbox/microsandbox). By providing virtualization as a library, it allows applications to embed high-security sandboxing directly, achieving genuine hardware-level isolation with its own kernel and memory space, while maintaining startup times competitive with containers.

* **Adoption:** This technology is primarily used by:
  - [**microsandbox** ↓](#43-microsandbox-self-hosted-microvms-for-untrusted-code) - uses libkrun as its core virtualization technology for self-hosted sandboxing
  - **Podman** - Red Hat's rootless container engine can use libkrun for VM-level isolation while maintaining container compatibility
  - **crun** - OCI runtime that can use libkrun for enhanced security

The emergence of microVM technologies like Firecracker and libkrun has rendered the old dichotomy of "slow, secure VMs versus fast, insecure containers" largely obsolete. They offer a compelling "best of both worlds" approach that has become the new standard for high-security, ephemeral code execution.

### **2.2. Application Kernels: Intercepting the System Call**

This approach offers an intermediate level of isolation between microVMs and standard containers. Instead of running a full guest kernel, an "application kernel" runs in user-space and intercepts all system calls made by the sandboxed process. It fulfills these requests itself, making only a limited and carefully vetted set of its own system calls to the true host kernel.

#### **gVisor**

* **GitHub:** [google/gvisor](https://github.com/google/gvisor)
* **Website:** [gvisor.dev](https://gvisor.dev/)

Developed and used extensively by Google, [gVisor](https://gvisor.dev/) is an open-source application kernel written in the memory-safe language Go.

* **Mechanism:** When an application inside a gVisor sandbox attempts to perform an operation like opening a file or sending a network packet, it issues a standard Linux system call. gVisor intercepts this call before it reaches the host kernel. gVisor's user-space kernel then processes the request according to its own implementation of the Linux API. This architecture dramatically reduces the host kernel's attack surface, as the sandboxed application can no longer directly exploit vulnerabilities in the host's syscall interface.  
* **Advantages:** gVisor provides significantly stronger isolation than standard containers and has the major advantage of not requiring hardware virtualization support, meaning it can run on any Linux host, whether bare-metal or a VM. It is used in production to secure Google services like Cloud Run, App Engine, and Cloud Functions. It also offers advanced features like the ability to checkpoint and restore a running container's state.  
* **Disadvantages:** This interception layer can introduce performance overhead compared to running directly on the host kernel. Furthermore, because gVisor re-implements the Linux API, there can be compatibility issues if an application uses obscure or newly introduced syscalls that gVisor does not yet support.
* **Adoption:** This technology is used by:
  - **Google Cloud Run** - Google's serverless container platform uses gVisor for isolation
  - **Google App Engine** - Google's platform-as-a-service uses gVisor for security
  - **Google Cloud Functions** - Google's serverless functions use gVisor
  - **Kata Containers** - can optionally use gVisor as a runtime
  - Various **Kubernetes** clusters for enhanced container security

#### **nsjail**

* **GitHub:** [google/nsjail](https://github.com/google/nsjail)
* **Website:** [github.com/google/nsjail](https://github.com/google/nsjail)

[nsjail](https://github.com/google/nsjail) is a process isolation tool that leverages Linux namespaces and seccomp-bpf filters to create secure sandboxed environments. Originally developed by Google and used in production environments, it provides a lightweight alternative to full virtualization when strong isolation is needed.

* **Mechanism:** nsjail creates isolated environments by combining multiple Linux security features: namespaces (PID, mount, network, user), seccomp-bpf system call filtering, and resource limits via cgroups. Unlike gVisor's approach of reimplementing syscalls, nsjail uses kernel-level filtering and namespace isolation to restrict what the sandboxed process can access.
* **Advantages:** Extremely lightweight with minimal overhead compared to VMs or application kernels. Provides fine-grained control over system resources, file access, and network capabilities. Well-suited for scenarios where you need to run potentially untrusted code with specific resource constraints. Used successfully in production by platforms like [Windmill](https://windmill.dev) for sandboxing Python and Go execution. Windmill are using [Deno for Javascript and Typescript though and claim it to be much more lightweight compared to nsjail](https://denoland.medium.com/how-immutable-scripts-in-deno-allow-windmill-dev-yc-s22-to-build-production-grade-ops-791c2a5786b6#:~:text=For%20Windmill%E2%80%99s%20scripts%20in%20other%20languages%2C%20the%20sandboxing%20goes%20through%20NSJail%2C%20which%20has%20a%20big%20performance%20overhead%20and%20is%20complex%20to%20use%20and%20configure.%20Deno%2C%20due%20to%20its%20secure%20by%20default%20nature%2C%20has%20an%20edge%20in%20performance%20for%20multi%2Dtenant%20environments.).
* **Use Cases:** Particularly effective for long-running processes that need computational isolation without the overhead of full virtualization. According to community discussions, it's recommended for scenarios involving WebSocket data processing and light computational tasks where container-level isolation might be insufficient but VM overhead is undesirable.
* **Configuration:** Supports extensive configuration options for runtime restrictions, including CPU limits, memory constraints, filesystem access controls, and network isolation policies. Can be integrated into worker processes via languages like Rust for programmatic control.
* **Adoption:** This technology is used by:
  - **Windmill** - uses nsjail for sandboxing Python and Golang execution
  - **Google** - uses nsjail internally for various sandboxing needs
  - **Security research** - widely used in malware analysis and security research
  - **CTF platforms** - used by competitive programming and security challenge platforms

### **2.3. Language Runtimes: Lightweight, High-Speed Isolation**

This is the most lightweight form of sandboxing, where isolation is enforced by the language runtime itself rather than the operating system or hardware. This approach offers the fastest startup times and lowest resource overhead but is the most restrictive in terms of compatibility.

#### **WebAssembly (WASM)**

* **GitHub:** [WebAssembly/spec](https://github.com/WebAssembly/spec)
* **Website:** [webassembly.org](https://webassembly.org/)

[WebAssembly](https://webassembly.org/) is a binary instruction format designed as a portable compilation target for a stack-based virtual machine. Its [security model](https://webassembly.org/docs/security/) is built on two fundamental principles:

* **Memory Safety:** WASM code executes in a linear memory space that is completely isolated from the host process's memory. Every memory access is automatically bounds-checked by the runtime, preventing buffer overflows from affecting the host or other WASM modules. The call stack is also managed by the runtime and is inaccessible to the WASM code, which neutralizes traditional stack-smashing attacks.  
* **Capability-Based Security:** A WASM module is inert by default. It has no intrinsic ability to access the file system, network, or any other external resource. To perform any I/O, the host environment must explicitly provide these capabilities by passing in functions (known as "imports") during instantiation. This "default-deny" posture ensures that a module can only do what it has been explicitly permitted to do.
* **Adoption:** This technology is used across many platforms:
  - [**WebContainers** ↓](#44-webcontainers-browser-native-development-runtime) - uses WASM for browser-based Node.js runtime
  - **Shopify Scripts** - uses WASM for safe execution of custom scripts
  - **Fastly Compute@Edge** - uses WASM for edge computing
  - **Wasmtime** - server-side WASM runtime
  - **Docker+WASM** - Docker Desktop now supports WASM containers
  - **Kubernetes** - supports WASM workloads through various runtimes

#### **V8 Isolates**

* **GitHub:** [v8/v8](https://github.com/v8/v8)
* **Website:** [v8.dev](https://v8.dev/)

V8 Isolates are a core feature of Google's V8 JavaScript engine. An Isolate represents a completely independent instance of the V8 engine, with its own memory heap, garbage collector, and execution state.

* **Mechanism:** When multiple scripts run in different V8 Isolates within the same process (e.g., multiple tabs in a web browser), their objects, variables, and code are completely separate. One isolate cannot directly access the memory or state of another. This is the fundamental technology that enables platforms like  
  **Cloudflare Workers** and [**Deno Deploy**](https://deno.com/blog/anatomy-isolate-cloud) to securely run code from thousands of different customers on the same physical servers with extremely low overhead.  
* **Language-Specific Limitations:** While V8 Isolates excel at JavaScript execution, they are not well-suited for Python workloads. The V8 engine is specifically designed and optimized for JavaScript's execution model, memory management, and runtime characteristics. Python applications require different runtime environments and cannot benefit from V8's isolation technology. For Python sandboxing, alternative approaches like nsjail, gVisor, or microVMs are more appropriate choices.
* **The V8 Sandbox:** It is important to distinguish V8 Isolates from the newer [V8 Sandbox](https://chromium.googlesource.com/v8/v8.git/+/refs/heads/main/src/sandbox/README.md). The V8 Sandbox is a further defense-in-depth measure that operates *within* an isolate. It reserves a large region of virtual address space and ensures that all V8 heap pointers are confined to that space. This is designed to mitigate the impact of potential vulnerabilities *within the V8 engine itself*, preventing an exploit from achieving arbitrary memory read/write capabilities outside the sandboxed region. This demonstrates a multi-layered approach to security, even within the runtime.
* **Adoption:** This technology powers major edge computing platforms:
  - [**Cloudflare Workers** ↓](#46-cloudflare-workers-edge-computing-with-v8-isolates) - uses V8 Isolates for edge computing
  - **Deno Deploy** - uses V8 Isolates for serverless JavaScript
  - **Shopify Scripts** - uses V8 Isolates for safe script execution
  - **Chrome Browser** - uses V8 Isolates for tab isolation
  - **Node.js Worker Threads** - uses V8 Isolates for thread isolation
  - **Vercel Edge Runtime** - uses V8 Isolates for edge functions

### **2.4. Containerization: Namespace-Based Isolation**

Containerization represents the most widely adopted approach to application isolation, using Linux namespaces and control groups (cgroups) to create isolated environments that share the host kernel. While not providing the strongest security boundaries, containers offer an excellent balance of compatibility, performance, and operational simplicity.

#### **Docker/OCI Containers**

* **GitHub:** [docker/docker](https://github.com/docker/docker), [opencontainers](https://github.com/opencontainers)
* **Website:** [docker.com](https://www.docker.com/), [opencontainers.org](https://opencontainers.org/)

[Docker](https://www.docker.com/) and the broader [Open Container Initiative (OCI)](https://opencontainers.org/) ecosystem represent the de facto standard for containerization. Containers package applications with their dependencies while providing process-level isolation through kernel features.

* **Mechanism:** Containers use multiple Linux kernel features for isolation: namespaces (PID, mount, network, user, UTS, IPC) separate process trees, filesystems, and network stacks; cgroups limit and monitor resource usage (CPU, memory, I/O); and security modules like AppArmor or SELinux provide additional access controls. Unlike VMs, containers share the host kernel, making them lightweight but potentially less secure.
* **Advantages:** Extremely fast startup times (10-50ms), minimal resource overhead, extensive ecosystem of tools and images, excellent compatibility with existing applications, and mature orchestration platforms like Kubernetes. The shared kernel model enables efficient resource utilization and makes containers ideal for microservices architectures.
* **Security Considerations:** The shared kernel creates potential attack vectors, as demonstrated by container escape vulnerabilities. However, most security incidents (69% according to industry reports) result from misconfigurations rather than kernel exploits. Proper configuration, avoiding privileged containers, and using minimal base images significantly reduce risks. As noted in security discussions, "the Linux kernel has far too large of an attack surface to be trusted as a hard security boundary" - this is why platforms running hostile code prefer microVMs or specialized runtimes.
* **Rootless vs Root-based Runtimes:** Container security varies significantly between implementations:
  - **Podman** - Rootless by design, no daemon, better user namespace support, fork/exec model provides stronger isolation
  - **Docker** - Default daemon runs as root, creating privileged attack surface; Docker socket access equals root access
  - **For hostile code:** Rootless runtimes like Podman offer significantly better security than daemon-based Docker setups
* **Use Cases:** Ideal for trusted application deployment, development environments, CI/CD pipelines, and microservices architectures. Less suitable for running untrusted code from external sources or scenarios requiring the strongest security isolation.
* **Enhanced Security Options:** Technologies like gVisor provide additional security layers for containers, while Kata Containers offer VM-level isolation with container compatibility.
* **Adoption:** This technology is ubiquitous across the industry:
  - [**Daytona** ↓](#42-daytona-secure--elastic-infrastructure-for-ai-code) - uses containers for development environments
  - [**Replit** ↓](#45-replit-collaborative-browser-based-development) - uses containers for coding environments
  - [**Gitpod** ↓](#49-other-notable-platforms--cloud-development-environments-cdes) - uses containers for development workspaces
  - [**Coder** ↓](#49-other-notable-platforms--cloud-development-environments-cdes) - uses containers for development environments
  - **Kubernetes** - the foundation of modern container orchestration
  - **Docker Hub** - the largest container registry with billions of downloads

#### **Incus System Containers**

* **GitHub:** [lxc/incus](https://github.com/lxc/incus)
* **Website:** [linuxcontainers.org/incus](https://linuxcontainers.org/incus/)

[Incus](https://linuxcontainers.org/incus/) is a system container and virtual machine manager, maintained as a community fork of Canonical's LXD by many of the same people who originally created LXD. Unlike Docker's application containers (which isolate single processes), Incus system containers run a complete operating system with a full init system, making them behave like lightweight virtual machines while sharing the host kernel.

* **Mechanism:** Incus operates in two distinct modes. System containers use Linux kernel primitives - namespaces, cgroups, AppArmor/SELinux - applied to an entire operating system rather than a single process. Each container runs its own init system (e.g., systemd), can manage services, run Docker inside itself (Docker-in-Container), and behave like a full machine. Full QEMU/KVM virtual machines provide hardware-level isolation with a dedicated guest kernel, comparable to Firecracker or libkrun but with the operational convenience of the Incus management layer. Containers run unprivileged by default, with automatic UID/GID mapping to prevent privilege escalation to the host.
* **Advantages:** System containers provide stronger isolation than application containers because they present a complete OS boundary rather than just process-level namespacing. The full init system enables running complex multi-service workloads, including Docker daemons, SSH servers, and development toolchains, within a single isolated container. Incus also offers an image-based workflow with daily-published distribution images, multiple storage backends, network management, and device passthrough (USB, GPU, NICs, disks).
* **Security Model:** Incus employs defense-in-depth: unprivileged containers by default prevent root-in-container from mapping to root-on-host; Seccomp filters restrict dangerous system calls; AppArmor profiles confine container access; and cgroup-based resource restrictions prevent denial-of-service. For workloads requiring the strongest possible isolation, Incus's QEMU VM mode provides a hardware-enforced boundary with a dedicated kernel, achieving microVM-grade security while retaining the same unified CLI and management API.
* **Use Cases:** Well-suited for development environments, AI coding agent sandboxes, CI/CD pipelines requiring full OS capabilities, and multi-tenant isolation scenarios where each tenant needs a complete, isolated operating system environment.
* **Adoption:** This technology is used by:
  - **Canonical** - Incus's predecessor LXD powers Ubuntu's container infrastructure
  - **Enterprise environments** - used for multi-tenant hosting and development environments

## **3\. Feature Matrix: At-a-Glance Comparison**

The following table provides a high-level, comparative overview of the leading code sandboxing solutions, allowing for a quick assessment of their core attributes and positioning.

| Solution | Primary Technology | Launch Date | GitHub Stars | License | Self-Hosted | SaaS Available | Filesystem Access | Network Access | Workload Suitability |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| [**e2b** ↓](#41-e2b-the-ai-agent-sandbox-runtime) | Firecracker (MicroVM) | Nov 2023 | 8.9k+ | Apache-2.0 | Yes | Yes | Persistent | Full | Short & Long-Running |
| [**Daytona** ↓](#42-daytona-secure--elastic-infrastructure-for-ai-code) | Containers (OCI/Docker) | 2023 | 21k+ (frozen) | Proprietary (was AGPL-3.0) | No (since Jun 2026) | Yes | Persistent, Archivable | Full | Long-Running & Stateful |
| [**microsandbox** ↓](#43-microsandbox-self-hosted-microvms-for-untrusted-code) | libkrun (MicroVM) | May 2025 | 3.3k+ | Apache-2.0 | Yes (Primary) | No | Persistent & Ephemeral | Full | Short & Long-Running |
| [**WebContainers** ↓](#44-webcontainers-browser-native-development-runtime) | Browser-based Node.js/Wasm | 2021 | N/A | Proprietary | No | Yes | Ephemeral | Browser-limited | Short & Medium-Running |
| [**Replit** ↓](#45-replit-collaborative-browser-based-development) | Containers/VMs | 2016 | N/A | Proprietary | No | Yes | Persistent | Full | Short & Long-Running |
| [**Cloudflare Workers** ↓](#46-cloudflare-workers-edge-computing-with-v8-isolates) | V8 Isolates | 2017 | N/A | Proprietary | No | Yes | Ephemeral | Edge-limited | Short-Running |
| [**Fly.io** ↓](#47-flyio-modern-application-hosting-with-microvms) | MicroVMs (Firecracker) | 2017 | N/A | Proprietary | No | Yes | Persistent | Full | Short & Long-Running |
| [**Kata Containers** ↓](#48-kata-containers-secure-container-runtime) | MicroVM Containers | 2017 | 5.2k+ | Apache-2.0 | Yes | No | Persistent | Full | Long-Running & Stateful |
| [**CodeSandbox** ↓](#49-other-notable-platforms--cloud-development-environments-cdes) | MicroVM & Browser | 2017 | 13.4k+ | Proprietary / OSS Parts | No | Yes | Persistent | Full | Short & Long-Running |
| [**Gitpod** ↓](#49-other-notable-platforms--cloud-development-environments-cdes) | Containers | 2020 | 12.9k+ | AGPL-3.0 | Yes | Yes | Persistent | Full | Long-Running & Stateful |
| [**Coder** ↓](#49-other-notable-platforms--cloud-development-environments-cdes) | Containers / VMs | 2019 | 8.1k+ | AGPL-3.0 | Yes | Yes | Persistent | Full | Long-Running & Stateful |
| [**llm-sandbox** ↓](#410-llm-sandbox-a-self-hosted-library-for-llm-generated-code) | Containers (Docker/Podman/K8s) | Jul 2024 | 1.1k+ | MIT | Yes (Primary) | No | Persistent & Ephemeral | Configurable | Short & Long-Running |
| [**Docker Sandboxes** ↓](#411-docker-sandboxes-disposable-microvms-for-ai-coding-agents) | MicroVMs (custom VMM) | 2026 | N/A | Proprietary | Yes (Only) | No | Ephemeral (configurable mounts) | Configurable + secret proxy | Short-Running (agent sessions) |
| [**Vercel Sandbox** ↓](#412-vercel-sandbox-firecracker-microvms-for-agent-workloads) | Firecracker (MicroVM) | Jan 2026 (GA) | 174 (SDK) | Apache-2.0 (SDK) / Proprietary (platform) | No | Yes | Ephemeral | Full | Short-Running |
| [**AWS Bedrock AgentCore** ↓](#413-aws-bedrock-agentcore-managed-agent-runtime--code-interpreter) | Firecracker (MicroVM) | Oct 2025 (GA) | N/A | Proprietary | No | Yes | Ephemeral (session-scoped) | Configurable (VPC/PrivateLink) | Short & Interactive Sessions |
| [**Apple Containerization** ↓](#414-apple-containerization-vm-backed-containers-for-macos) | MicroVMs (Virtualization.framework) | Jun 2025 | 48.9k+ | Apache-2.0 | Yes (Only) | No | Persistent | Full | Development, CI/CD |
| [**NVIDIA OpenShell** ↓](#415-nvidia-openshell-policy-governed-sandboxing-for-autonomous-agents) | Docker/Podman/MicroVM/K8s (policy layer) | Feb 2026 | 8.1k+ | Apache-2.0 | Yes (Only) | No | Policy-Restricted | Policy-Restricted (egress allow/deny) | Long-Running Agent Fleets |
| [**Koyeb Sandboxes** ↓](#416-koyeb-sandboxes-bare-metal-microvms-for-ai-agents) | MicroVMs (bare metal) | Nov 2025 (preview) | 2 (SDK) | Apache-2.0 (SDK) / Proprietary (platform) | No | Yes | Persistent (session-scoped) | Configurable | Short & Long-Running |
| [**Amazing Sandbox** ↓](#417-amazing-sandbox-a-local-cli-wrapper-over-os-native-sandboxing) | Docker / Seatbelt / Bubblewrap | Dec 2025 | 165 | MIT | Yes (Only) | No | Configurable | Configurable | Short-Running (CLI tool invocations) |
| [**nono** ↓](#418-nono-per-tool-call-brokered-sandboxing-for-agents) | OS-native (no container/VM) | 2026 | 3.6k+ | Apache-2.0 | Yes (Only) | No | Scoped per profile | Proxy-brokered, per-endpoint | Short-Running (per tool call) |
| [**Deno Sandbox** ↓](#419-deno-sandbox-firecracker-microvms-on-deno-deploy) | Firecracker (MicroVM) | Feb 2026 (beta) | 6 (SDK) | MIT (SDK) / Proprietary (platform) | No | Yes | Ephemeral | Configurable (allowlist + proxy) | Short-Running |
| [**SandBase Harness** ↓](#420-sandbase-harness-multi-backend-runtime-for-ai-agents) | Local process / Docker / Kubernetes / worker | Jul 2026 | 620+ | Apache-2.0 | Yes (Only) | No | Session-scoped | Configurable by backend | Short & Long-Running Agent Sessions |

## **4\. In-Depth Platform Profiles**

This section provides a detailed, structured analysis of each major platform, expanding on the data presented in the feature matrix.

### **4.1. e2b: The AI Agent Sandbox Runtime**

* **Overview:** e2b is an open-source, secure cloud runtime explicitly engineered for the needs of AI applications and autonomous agents. It provides developers with sandboxed cloud environments powered by Firecracker microVMs, enabling the safe execution of AI-generated code. The platform is heavily focused on providing a seamless developer experience through its SDKs and is designed to be the backend infrastructure for agentic workflows.  
* **GitHub:** [e2b-dev/E2B](https://github.com/e2b-dev/E2B)
* **Website:** [e2b.dev](https://e2b.dev)  
* **Launch Date:** The "Custom Sandboxes" feature, a core part of its offering, was launched on November 7, 2023. The project has been active for longer, with notable partnerships like the one with Groq announced in April 2025, indicating its adoption in production systems.  
* **GitHub Stars:** The project has garnered significant community interest, with approximately 8,900 stars.  
* **License:** e2b is licensed under the permissive **Apache-2.0 License**, which allows for broad use and modification without the copyleft restrictions of licenses like AGPL.  
* **Hosting:**  
  * **SaaS:** Yes. The primary offering is a fully managed cloud service with a tiered pricing model (Hobby and Pro). Pricing is based on a monthly subscription plus usage-based costs for CPU and memory, metered per second of runtime.  
  * **Self-Hosted:** Yes. e2b provides a self-hosting guide and Terraform scripts for deploying the infrastructure on your own cloud provider account. It currently supports GCP, with support for AWS in progress.  
* **Capabilities:**  
  * **Filesystem Access:** The sandboxes provide full filesystem I/O capabilities. The SDKs for Python and JavaScript allow for programmatic uploading and downloading of files, making it easy to provide context to or retrieve artifacts from an agent. The environments are persistent, meaning filesystem changes and installed packages can be maintained across multiple execution calls within a single session.  
  * **Network Access:** Sandboxes have full, unrestricted internet access by default. Furthermore, any service running inside the sandbox (e.g., a web server) can be exposed to the public internet via a unique, secure URL provided by the e2b platform, facilitating use cases like hosting generated web apps or providing APIs from within the sandbox.  
  * **Workload Suitability:** e2b is highly versatile and well-suited for both **short-lived** and **long-running** workloads. The fast startup time (\~150-200ms) is ideal for ephemeral tasks like running a single code snippet for data analysis. The Pro plan supports sessions up to 24 hours long, making it robust enough for complex, stateful agentic tasks, development environments, or demanding reinforcement learning training loops that require persistent state.

### **4.2. Daytona: Secure & Elastic Infrastructure for AI Code**

* **Overview:** Daytona positions itself as a comprehensive platform for both secure AI code execution and enterprise development environment management. It emphasizes lightning-fast sandbox startup times (under 200ms), stateful persistence, and a robust SDK for programmatic control. It aims to provide a secure, elastic runtime for AI agents while also serving as a full-featured Cloud Development Environment (CDE). **As of June 2026, Daytona is no longer self-hostable:** the company moved its production codebase to a closed-source repository, citing the risk of AI-assisted attackers using a public isolation/kernel-boundary implementation as a blueprint for exploits. See their own announcement, [Daytona is going closed source. Here's why.](https://www.daytona.io/dotfiles/updates/daytona-is-going-closed-source), for the full rationale. The public `daytonaio/daytona` repository still exists and remains AGPL-3.0-licensed, but it is frozen at the last open release (v0.190.0) and receives no further updates, fixes, or security patches. For a detailed comparison with other AI sandboxing solutions, see this [analysis of Daytona vs microsandbox](https://pixeljets.com/blog/ai-sandboxes-daytona-vs-microsandbox/).  
* **GitHub:** [daytonaio/daytona](https://github.com/daytonaio/daytona) (archived at v0.190.0; no longer maintained)
* **Website:** [daytona.io](https://daytona.io)  
* **Launch Date:** The company was founded in 2023, and its open-source version gained significant community traction and funding in mid-2024. It went closed-source in June 2026.  
* **GitHub Stars:** The frozen open-source repository sits at over 21,000 stars, but the actively developed product is no longer open source.  
* **License:** **Proprietary.** Daytona's core product moved to a closed-source codebase in June 2026. The legacy public repository is still under **AGPL-3.0**, but it only reflects the product as it was before the switch, and isn't getting further development.  
* **Hosting:**  
  * **SaaS:** Yes. Daytona offers a managed cloud platform with a usage-based, pay-as-you-go pricing model for compute, memory, and storage resources.  
  * **Self-Hosted:** No, not anymore. Self-hosting used to be a core part of Daytona's offering, but since the move to closed source, there's no way to deploy a current Daytona instance on your own infrastructure. Existing self-hosted deployments are stuck on the last public release and won't get security patches.  
* **Capabilities:**  
  * **Filesystem Access:** Daytona provides a full, persistent filesystem within its sandboxes. A key feature is the ability to archive inactive sandboxes, which moves the entire filesystem state to cost-effective object storage for long-term preservation and later retrieval. The SDK includes a filesystem API for programmatic file operations.  
  * **Network Access:** Sandboxes are equipped with network access. They can expose services running on specific ports via a public preview link. For private sandboxes, this link is secured with an access token to ensure controlled access.  
  * **Workload Suitability:** Daytona is explicitly designed for **long-running, stateful** workloads. Features like automatically stopping inactive sandboxes and archiving them after a configurable period are tailored to optimize costs for persistent environments that are not always in active use. This makes it ideal for full-fledged development workspaces and complex, multi-step agentic tasks that require state to be preserved indefinitely.  
* **Underlying Technology:** Daytona uses **container-based isolation** with OCI/Docker compatibility. The platform supports any OCI/Docker image for sandbox creation and achieves sub-90ms startup times through optimized container orchestration rather than microVM technology.

Daytona's core product was already AGPL-3.0, a copyleft license that requires modifications made accessible over a network to also be released under the same terms. That already pushed some large enterprises toward a commercial license instead. In June 2026, Daytona went further and closed the source entirely, arguing that a public sandbox implementation just hands attackers a blueprint for the kernel boundary they're trying to break. e2b and microsandbox, by contrast, are still Apache-2.0 and openly self-hostable. If you're picking infrastructure to self-host long-term, it's worth checking a project's funding model and incentives, not just its license file today.

### **4.3. microsandbox: Self-Hosted MicroVMs for Untrusted Code**

* **Overview:** microsandbox is a self-hosted platform singularly focused on providing maximum security for untrusted code execution. Its core value proposition is combining the hardware-level isolation of microVMs (powered by libkrun) with the sub-200ms startup speed of containers and the complete control afforded by a self-hosted model. It is designed to solve the security-speed-control trade-off without compromise.  
* **GitHub:** [microsandbox/microsandbox](https://github.com/microsandbox/microsandbox)
* **Website:** [docs.microsandbox.dev](https://docs.microsandbox.dev)  
* **Launch Date:** The initial public release (v0.1.0) was on May 20, 2025.  
* **GitHub Stars:** The project has accumulated approximately 3,300 stars since its launch.  
* **License:** microsandbox uses the permissive **Apache-2.0 License**, making it straightforward for companies to adopt and integrate into commercial products.  
* **Hosting:**  
  * **SaaS:** No. The platform is explicitly and exclusively a **self-hosted** solution. This is a core part of its identity, emphasizing "Your Infrastructure" and giving users full control and data privacy.  
  * **Self-Hosted:** Yes, this is the only deployment model. Users install and run the msb server component on their own hardware or cloud instances.  
* **Capabilities:**  
  * **Filesystem Access:** The platform supports both persistent and ephemeral filesystems. When using the project-based workflow (msr), file changes and installations within a sandbox are automatically persisted to a local ./menv directory on the host. This allows a developer to stop and restart a sandbox without losing their work. For one-off tasks, it also supports fully temporary sandboxes (msx) that leave no trace after execution.  
  * **Network Access:** The microsandbox core server is responsible for managing networking for the sandboxes. While detailed network configuration guides are not present in the primary documentation snippets, use cases such as "Web Browsing Agent" and "Instant App Hosting" strongly imply that sandboxes can be configured with controlled network access to fulfill these roles.  
  * **Workload Suitability:** microsandbox is highly flexible, catering to both **short-lived, stateless** tasks and **long-running, stateful** workloads. The msx command is designed for quick, ephemeral executions, while the project-based msr command with its persistent state is ideal for ongoing development work or complex, multi-step processes where context must be maintained.

### **4.4. WebContainers: Browser-Native Development Runtime**

* **Overview:** [WebContainers](https://webcontainers.io) represents a fundamentally different approach to code sandboxing by bringing server-side development entirely into the browser. Developed by StackBlitz, this technology creates a browser-based Node.js runtime using WebAssembly that can run package managers, development servers, and full-stack frameworks without any remote infrastructure.
* **GitHub:** N/A (proprietary)
* **Website:** [webcontainers.io](https://webcontainers.io), [stackblitz.com](https://stackblitz.com)  
* **Technology:** The platform leverages **Browser-based Node.js/WebAssembly** to create isolated execution environments that run entirely within the browser's security sandbox. This approach eliminates the need for remote servers while providing genuine Node.js compatibility.  
* **Launch Date:** WebContainers technology was first introduced in **2021** as part of StackBlitz's browser-based IDE platform.  
* **License:** **Proprietary** technology with enterprise licensing options available for embedding in other platforms.  
* **Hosting:**  
  * **SaaS:** Yes. The primary offering is integrated into StackBlitz's cloud IDE platform, with enterprise licensing available for custom integrations.  
  * **Self-Hosted:** No. The technology runs in browsers but requires StackBlitz's proprietary runtime.  
* **Capabilities:**  
  * **Filesystem Access:** **Ephemeral** - provides a virtual filesystem that exists only during the browser session. Files can be persisted through browser storage mechanisms or exported.  
  * **Network Access:** **Browser-limited** - can make HTTP requests subject to browser CORS policies and security restrictions. Cannot run traditional server applications accessible from external networks.  
  * **Workload Suitability:** Ideal for **short to medium-running** development tasks, prototyping, tutorials, and educational environments. Performance claims of up to 10x faster package installation than local development make it suitable for rapid iteration workflows.  
* **Unique Value Proposition:** WebContainers eliminates server infrastructure costs entirely while providing instant, disposable development environments. This makes it particularly valuable for interactive tutorials, low-code platforms, and AI development environments where traditional server-based sandboxes would be cost-prohibitive at scale.

### **4.5. Replit: Collaborative Browser-Based Development**

* **Overview:** [Replit](https://replit.com) is a browser-based development platform that emphasizes collaboration, education, and rapid prototyping. Founded in 2016, it has become one of the most popular platforms for learning to code and building quick prototypes. The platform provides instant development environments without any local setup, supporting dozens of programming languages and frameworks.
* **GitHub:** [replit](https://github.com/replit)
* **Website:** [replit.com](https://replit.com)  
* **Technology:** Replit uses **container and VM-based isolation** to provide secure, isolated development environments. Each "Repl" (development environment) runs in its own sandboxed container with full filesystem access and network capabilities.  
* **Launch Date:** **2016** - Originally founded as a simple online code editor, it has evolved into a comprehensive development platform.  
* **License:** **Proprietary** platform with both free and paid tiers.  
* **Hosting:**  
  * **SaaS:** Yes. Replit is primarily a cloud-based SaaS platform with tiered pricing based on usage and features.  
  * **Self-Hosted:** No. The platform is exclusively cloud-based.  
* **Capabilities:**  
  * **Filesystem Access:** **Persistent** - Each Repl maintains its filesystem state between sessions, allowing for ongoing project development.  
  * **Network Access:** **Full** - Repls can make external network requests and can expose web services via public URLs.  
  * **Workload Suitability:** Ideal for **short to long-running** development tasks, particularly educational projects, collaborative coding, and rapid prototyping. The platform excels at quick iterations and sharing code with others.  
* **Unique Features:** Real-time collaboration, integrated AI coding assistance, one-click deployment, and strong community features make it particularly popular for education and team development.

### **4.6. Cloudflare Workers: Edge Computing with V8 Isolates**

* **Overview:** [Cloudflare Workers](https://workers.cloudflare.com) represents the edge computing paradigm, running code across Cloudflare's global network of 275+ data centers. Launched in 2017, it uses V8 Isolates to provide extremely fast cold starts and global distribution, making it ideal for serverless functions that need to run close to users worldwide.
* **GitHub:** N/A (proprietary)
* **Website:** [workers.cloudflare.com](https://workers.cloudflare.com), [developers.cloudflare.com/workers](https://developers.cloudflare.com/workers)  
* **Technology:** **V8 Isolates** - Uses the same isolation technology as web browsers to provide lightweight, secure execution environments with near-zero cold start times.  
* **Launch Date:** **2017** - Introduced as part of Cloudflare's expansion into edge computing services.  
* **License:** **Proprietary** platform with usage-based pricing.  
* **Hosting:**  
  * **SaaS:** Yes. Exclusively a managed service running on Cloudflare's global edge network.  
  * **Self-Hosted:** No. The value proposition is entirely based on Cloudflare's global infrastructure.  
* **Capabilities:**  
  * **Filesystem Access:** **Ephemeral** - No persistent filesystem. State can be maintained using Workers KV (key-value storage) or Durable Objects.  
  * **Network Access:** **Edge-limited** - Can make HTTP requests but operates within browser-like security constraints. Optimized for HTTP request/response patterns.  
  * **Workload Suitability:** Exclusively for **short-running** tasks (10ms-30s execution time). Perfect for API endpoints, edge logic, and request transformation.  
* **Performance:** Exceptional performance with 0ms cold starts and sub-100ms global latency. Claims to be up to 10x less expensive than traditional serverless platforms.

### **4.7. Fly.io: Modern Application Hosting with MicroVMs**

* **Overview:** [Fly.io](https://fly.io) is a developer-focused cloud platform that runs applications using hardware-virtualized containers (microVMs) across 35 global regions. Founded in 2017, it bridges the gap between traditional VPS hosting and modern serverless platforms, offering the flexibility of VMs with the convenience of containerized deployment.
* **GitHub:** N/A (proprietary)
* **Website:** [fly.io](https://fly.io), [fly.io/docs](https://fly.io/docs)  
* **Technology:** **MicroVMs (Firecracker)** - Uses Firecracker microVMs to provide hardware-level isolation while maintaining fast boot times (250ms or less).  
* **Launch Date:** **2017** - Initially focused on CDN services, evolved into a full application hosting platform.  
* **License:** **Proprietary** platform with transparent, usage-based pricing.  
* **Hosting:**  
  * **SaaS:** Yes. Fully managed platform running on Fly.io's own hardware infrastructure.  
  * **Self-Hosted:** No. The platform is built around Fly.io's specific hardware and network infrastructure.  
* **Capabilities:**  
  * **Filesystem Access:** **Persistent** - Applications can use persistent volumes and maintain state between deployments.  
  * **Network Access:** **Full** - Complete network access with automatic global load balancing and zero-config private networking between instances.  
  * **Workload Suitability:** Supports both **short and long-running** workloads. Can scale from single request handling to always-on applications with thousands of instances.  
* **Unique Value Proposition:** Combines the isolation benefits of microVMs with a developer-friendly deployment experience. Particularly strong for applications that need global distribution but require more than what edge computing platforms can provide.

### **4.8. Kata Containers: Secure Container Runtime**

* **Overview:** [Kata Containers](https://katacontainers.io) is an open-source container runtime that delivers the speed of containers with the security of virtual machines. Launched in 2017 by the Open Infrastructure Foundation, it represents a unique approach to container security by running each container in its own lightweight virtual machine. This hybrid approach addresses the fundamental security concerns of traditional container runtimes while maintaining container ecosystem compatibility.
* **GitHub:** [kata-containers/kata-containers](https://github.com/kata-containers/kata-containers)
* **Website:** [katacontainers.io](https://katacontainers.io)  
* **Technology:** **MicroVM Containers** - Kata Containers creates lightweight virtual machines for each container, providing hardware-enforced isolation while maintaining compatibility with the container ecosystem. It supports multiple hypervisors including QEMU, Cloud-Hypervisor, and Firecracker.  
* **Launch Date:** **December 2017** - Emerged from the merger of Intel Clear Containers and Hyper runV projects.  
* **License:** **Apache-2.0** - Fully open-source with permissive licensing.  
* **Hosting:**  
  * **SaaS:** No. Kata Containers is infrastructure software that cloud providers and enterprises deploy in their own environments.  
  * **Self-Hosted:** Yes. This is the primary deployment model, integrated into container orchestration platforms like Kubernetes.  
* **Capabilities:**  
  * **Filesystem Access:** **Persistent** - Full filesystem capabilities within the container/VM hybrid, with standard container volume mounting and storage options.  
  * **Network Access:** **Full** - Complete networking capabilities with container-standard networking models and Kubernetes networking integration.  
  * **Workload Suitability:** Ideal for **long-running, stateful** workloads that require strong security isolation. Perfect for multi-tenant environments, untrusted code execution, and compliance-heavy workloads where container escape vulnerabilities are unacceptable.  
* **Unique Value Proposition:** Kata Containers solves the "container vs. VM" dilemma by providing both. Organizations get the operational benefits of containers (fast startup, density, orchestration) with the security guarantees of VMs (hardware isolation, dedicated kernel). This makes it particularly valuable for production environments running untrusted workloads or requiring regulatory compliance.

### **4.9. Other Notable Platforms & Cloud Development Environments (CDEs)**

While the platforms above are specialized sandboxing runtimes, the broader category of Cloud Development Environments (CDEs) also relies heavily on sandboxing technology to function. They provide a useful point of comparison.

* **[CodeSandbox](https://codesandbox.io):** A pioneer in the online IDE space, CodeSandbox provides a complete development experience in the browser. It cleverly uses two different sandboxing technologies. For lightweight frontend prototyping, it uses  
  **Browser Sandboxes**, where code is executed directly in the browser. For more complex, full-stack development, it uses powerful **VM Sandboxes** built on microVMs, offering a backend environment with full terminal access. While it has [open-source components](https://github.com/codesandbox), its core platform is a SaaS offering.  
* **[Gitpod](https://gitpod.io) & [Coder](https://coder.com):** These are leading open-source CDEs that focus on creating ephemeral, reproducible development environments directly from a Git repository context. Their primary goal is to solve the "works on my machine" problem by standardizing environments using configuration files like  
  .gitpod.yml or devcontainer.json.  
  * **Sandboxing Approach:** Their isolation is typically based on **containers** (e.g., Docker). However, their architectures are sophisticated. [Gitpod employs a zero-trust model](https://www.gitpod.io/docs/flex/introduction/zero-trust) with a central management plane and runners that deploy environments within a customer's own cloud infrastructure, ensuring source code never leaves the network perimeter. [Coder uses Terraform as its provisioning engine](https://coder.com/docs/admin/infrastructure/architecture), which provides immense flexibility. A Coder template can define a workspace as a Docker container, a Kubernetes pod, or even a full VM on a cloud provider, allowing administrators to choose the appropriate level of isolation for their needs. Both projects use the AGPL-3.0 license for their open-source editions and offer enterprise versions with commercial licenses.


### **4.10. llm-sandbox: A Self-Hosted Library for LLM-Generated Code**

* **Overview:** llm-sandbox differs from most entries here in that it is a *library* rather than a platform. There is no control plane and no daemon of its own: it is a `pip install` that turns container runtimes you already operate into a sandbox for LLM-generated code. A single context-managed session takes a code string and returns stdout, stderr, exit code and any generated artifacts. The same API runs over Docker, Podman or Kubernetes, so a workflow prototyped on a laptop runs unchanged on a cluster.
* **GitHub:** [vndee/llm-sandbox](https://github.com/vndee/llm-sandbox)
* **Website:** [vndee.github.io/llm-sandbox](https://vndee.github.io/llm-sandbox/)
* **Launch Date:** First public release (0.1.0) in July 2024.
* **GitHub Stars:** Approximately 1,100.
* **License:** **MIT License**, permissive and straightforward for commercial adoption.
* **Hosting:**
  * **SaaS:** No. There is no hosted offering.
  * **Self-Hosted:** Yes, exclusively. Execution happens on whatever Docker, Podman or Kubernetes environment the user already runs.
* **Capabilities:**
  * **Filesystem Access:** Both ephemeral and persistent. Sessions are ephemeral by default; an optional pool recycles warm containers across runs. Artifacts written inside the container, including matplotlib and ggplot2 plots, are extracted and returned as structured objects rather than lost on teardown.
  * **Network Access:** Configurable per session, from full access to fully isolated (`network_mode="none"`), alongside read-only root filesystems, capability dropping and CPU/memory limits.
  * **Workload Suitability:** Suits both short-lived stateless execution and longer stateful work. An interactive session type keeps a persistent IPython kernel for notebook-style sequences, and container pooling targets loops that execute code many times per task.
  * **Languages:** Python, JavaScript, Java, C++, Go, R and Ruby, with automatic dependency installation per language.
  * **Security Model:** A configurable policy can screen code against patterns before execution, though it is advisory — the caller checks it and decides. The enforced layer is the container runtime. Note that this is **container isolation, not VM isolation**; it inherits the threat model of the chosen backend and makes no kernel-level guarantees.

### **4.11. Docker Sandboxes: Disposable MicroVMs for AI Coding Agents**

* **Overview:** Docker Sandboxes (CLI: `sbx`) runs AI coding agents, such as Claude Code, Gemini CLI, GitHub Copilot CLI, Codex, OpenCode, and Kiro, inside disposable microVMs on the developer's own machine, so an agent can install packages, edit configs, and spin up its own nested Docker containers without ever touching the host.
* **Website:** [docker.com/products/docker-sandboxes](https://www.docker.com/products/docker-sandboxes/)
* **GitHub:** [docker/sbx-releases](https://github.com/docker/sbx-releases) (binary releases only; the CLI itself is closed-source)
* **Launch Date:** In development through 2025 with regular updates; reached general availability in early 2026.
* **GitHub Stars:** N/A (proprietary; no public source repository).
* **License:** **Proprietary**, free for individual use. **Docker AI Governance**, a paid add-on, adds org-wide policy enforcement (network rules, filesystem restrictions, MCP governance) across a team's sandboxes.
* **Hosting:**
  * **SaaS:** No. Docker Sandboxes is a local tool that runs on the developer's own machine, not a hosted service.
  * **Self-Hosted:** Yes, exclusively, via `sbx` (installed with `brew`, `winget`, or `apt`). Notably, it requires signing in to a Docker account even to run a fully local, offline-capable sandbox, which was the single most-criticized aspect of the launch.
* **Underlying Technology:** Rather than reuse Firecracker (Linux/KVM-only) or shim it onto other platforms, Docker built its own VMM that talks directly to each OS's native hypervisor, Hypervisor.framework on macOS, Windows Hypervisor Platform on Windows, and KVM on Linux, so the same sandbox model works cross-platform without a Linux-first tool bolted onto it. Each agent session boots into its own microVM with a dedicated kernel.
* **Capabilities:**
  * **Filesystem Access:** Only the project workspace is mounted by default; additional mounts and setup steps are defined via "Kits" (YAML configs), which some early users found limiting compared to arbitrary volume mounts.
  * **Network Access:** Configurable per sandbox. Secrets are stored in the OS keychain and injected as auth headers by a network proxy only when the destination hostname matches, so the agent process itself never holds a usable credential. An MCP Gateway exposes a single endpoint to the sandbox, keeping OAuth tokens on the host.
  * **Workload Suitability:** Built for **short-lived, disposable** agent sessions rather than long-running persistent environments, one sandbox per coding task, torn down when it's done.
* **Reception:** Commenters praised the cross-platform native-hypervisor architecture but were sharply critical of the mandatory account login for a purely local tool, and several pointed to open-source alternatives without that requirement, including [**NVIDIA OpenShell** ↓](#415-nvidia-openshell-policy-governed-sandboxing-for-autonomous-agents) and [**Incus** ↓](#incus-system-containers) paired with agent CLIs.

### **4.12. Vercel Sandbox: Firecracker MicroVMs for Agent Workloads**

* **Overview:** Vercel Sandbox is a managed, ephemeral compute primitive for running untrusted or AI-generated code, built on "Hive," Vercel's internal platform for orchestrating Firecracker microVM clusters across regions. It's pitched as the execution layer for agents that clone repos, install dependencies, and run tests, mainly for teams already building on Vercel.
* **GitHub:** [vercel/sandbox](https://github.com/vercel/sandbox) (open-source CLI/SDK; the hosted execution platform itself is proprietary)
* **Website:** [vercel.com/sandbox](https://vercel.com/sandbox)
* **Launch Date:** Announced mid-2025; reached general availability on **January 30, 2026**.
* **GitHub Stars:** The SDK/CLI repository has approximately 174 stars; as with other major-cloud sandbox offerings, the star count reflects the client library rather than the managed platform itself.
* **License:** The SDK is **Apache-2.0**. The hosted Sandbox platform is a proprietary, usage-billed Vercel service.
* **Hosting:**
  * **SaaS:** Yes. This is the only way to run it; Sandbox executes exclusively on Vercel's infrastructure.
  * **Self-Hosted:** No.
* **Capabilities:**
  * **Filesystem Access:** Ephemeral. Each sandbox is a full Linux microVM with its own filesystem, process space, sudo access, and package managers, torn down at the end of the task.
  * **Network Access:** Full outbound access per sandbox.
  * **Workload Suitability:** **Short-running** by design; billing is active-CPU-only, so cost accrues only while code is actively executing, not while the sandbox sits idle. This makes it better suited to one-off agent tasks than to long-lived, stateful development sessions.

### **4.13. AWS Bedrock AgentCore: Managed Agent Runtime & Code Interpreter**

* **Overview:** Amazon Bedrock AgentCore is AWS's managed platform for building, deploying, and operating AI agents "at scale," largely framework- and model-agnostic. Two of its built-in tools are directly relevant here: **Code Interpreter**, which gives an agent a secure sandbox to execute code across multiple languages, and **Runtime**, which now supports interactive shell sessions with state (environment variables, working directory, running processes) that persists for the life of the session.
* **Website:** [aws.amazon.com/bedrock/agentcore](https://aws.amazon.com/bedrock/agentcore/)
* **GitHub:** [aws/bedrock-agentcore-sdk-python](https://github.com/aws/bedrock-agentcore-sdk-python) (open-source SDK; the managed service itself is proprietary)
* **Launch Date:** Preview on **July 16, 2025**; general availability on **October 13, 2025**.
* **GitHub Stars:** The Python SDK has approximately 750 stars; the managed service has no public star count.
* **License:** SDKs are **Apache-2.0**. The AgentCore service is a proprietary, usage-billed AWS offering.
* **Hosting:**
  * **SaaS:** Yes, exclusively. AgentCore is a fully managed AWS service; September 2025 updates added VPC connectivity and AWS PrivateLink for private network access to sandboxes.
  * **Self-Hosted:** No.
* **Underlying Technology:** Each Code Interpreter or Browser session is a strict one-session-one-microVM isolation model, provisioning a dedicated **Firecracker** microVM per session (the same technology underlying AWS Lambda), with no execution state, filesystem artifacts, or memory contents persisting between sessions.
* **Capabilities:**
  * **Filesystem Access:** Ephemeral and session-scoped by default; nothing survives past the session's teardown.
  * **Network Access:** Configurable, including private connectivity via VPC/PrivateLink for enterprise network requirements.
  * **Workload Suitability:** Suited to both quick, one-shot code execution (Code Interpreter) and longer **interactive** agent sessions (Runtime's persistent shell), though state does not survive past a given session's lifetime.

### **4.14. Apple Containerization: VM-Backed Containers for macOS**

* **Overview:** Unveiled at WWDC 2025, Apple's Containerization framework (and its `container` CLI) runs OCI-compliant Linux container images natively on Apple Silicon Macs, but instead of one shared Linux VM hosting all containers (the model most Docker Desktop-on-Mac setups use), each container gets its **own** lightweight, purpose-built Linux VM via macOS's Virtualization framework. It's a general-purpose containerization tool rather than an AI-agent-specific product, but it's increasingly used as the isolation substrate for running coding agents on the Mac (community projects like `sandboxy` build directly on it).
* **GitHub:** [apple/container](https://github.com/apple/container) (the user-facing CLI); [apple/containerization](https://github.com/apple/containerization) (the underlying Swift framework)
* **Website:** [developer.apple.com](https://developer.apple.com/) (framework documentation; part of macOS 26 "Tahoe")
* **Launch Date:** Announced **June 2025** at WWDC; open-sourced the same year.
* **GitHub Stars:** `apple/container` has approximately 48,900 stars; `apple/containerization` has approximately 8,900 stars.
* **License:** **Apache-2.0**, Apple's standard open-source license.
* **Hosting:**
  * **SaaS:** No. This is local developer tooling for macOS, not a hosted service.
  * **Self-Hosted:** Yes, exclusively, and only on Apple Silicon Macs.
* **Capabilities:**
  * **Filesystem Access:** Persistent, standard OCI volume semantics per container.
  * **Network Access:** Full, with per-container network namespaces.
  * **Workload Suitability:** General-purpose container workloads on macOS, development environments, and CI/CD, anywhere a per-container dedicated kernel is preferable to a single shared Linux VM.

### **4.15. NVIDIA OpenShell: Policy-Governed Sandboxing for Autonomous Agents**

* **Overview:** OpenShell is NVIDIA's open-source runtime for sandboxing autonomous AI agents, enforcing declarative YAML policies across four layers: filesystem (restrict reads/writes to allowed paths), network (block unauthorized outbound connections), process (block privilege escalation and dangerous syscalls), and inference (a policy layer over the agent's own model calls). It's backend-agnostic, running on Docker, Podman, microVMs, or Kubernetes, and on Kubernetes it defines a `Sandbox` custom resource that a controller reconciles into scheduled pods, built on the Kubernetes SIG Agent Sandbox project.
* **GitHub:** [NVIDIA/OpenShell](https://github.com/NVIDIA/OpenShell)
* **Website:** [docs.nvidia.com/openshell](https://docs.nvidia.com/openshell/)
* **Launch Date:** First released **February 2026**.
* **GitHub Stars:** Approximately 8,100, just months after its first release, helped along by NVIDIA's name and reach.
* **License:** **Apache-2.0**, fully open-source.
* **Hosting:**
  * **SaaS:** No. OpenShell is infrastructure software you deploy yourself.
  * **Self-Hosted:** Yes, exclusively, on whatever Docker, Podman, microVM, or Kubernetes infrastructure you already run. The Kubernetes Helm chart is explicitly marked experimental and not for production use yet.
* **Capabilities:**
  * **Filesystem Access:** Policy-restricted, allow/deny lists per sandbox rather than an all-or-nothing mount.
  * **Network Access:** Policy-restricted egress; unauthorized outbound connections are blocked by default rather than left to the caller to configure.
  * **Workload Suitability:** Designed for **long-running fleets** of agents on Kubernetes as much as for a single local sandbox, with the same policy model applying at either scale.

### **4.16. Koyeb Sandboxes: Bare-Metal MicroVMs for AI Agents**

* **Overview:** Koyeb Sandboxes are Koyeb's managed, fully isolated environments for running AI-agent code, code generation, and other dynamic workloads. Koyeb is a Paris-founded serverless and GPU cloud provider (about $8.6M raised as of its last public round); in 2026 it agreed to be folded into Mistral AI's compute platform, so its infrastructure now sits behind one of the larger foundation-model labs rather than running as a standalone startup.
* **GitHub:** [koyeb/koyeb-sandbox-sdk-js](https://github.com/koyeb/koyeb-sandbox-sdk-js) (open-source SDK; the platform itself is proprietary)
* **Website:** [koyeb.com](https://www.koyeb.com/)
* **Launch Date:** Public preview on **November 19, 2025**. The underlying sandbox primitives had already been used by Koyeb customers since early 2025.
* **GitHub Stars:** The SDK repo has 2 stars; as with other managed cloud sandbox platforms, the star count reflects the client library, not the service itself.
* **License:** The SDK is **Apache-2.0**. The Sandboxes platform is a proprietary, usage-billed Koyeb service.
* **Hosting:**
  * **SaaS:** Yes, exclusively. Sandboxes run on Koyeb's own infrastructure.
  * **Self-Hosted:** No.
* **Underlying Technology:** MicroVMs run directly on bare-metal servers rather than on top of another provider's VMs, with cold starts as low as 250ms and automatic scale-to-zero when a sandbox is idle.
* **Capabilities:**
  * **Filesystem Access:** Persistent for the life of the session; files can be read, written, and managed through the Python or JavaScript SDK.
  * **Network Access:** Configurable; ports can be exposed for both short-lived and long-running workloads.
  * **Workload Suitability:** Covers both quick one-off code execution and longer background processes. It's in the same category as Vercel Sandbox and AWS Bedrock AgentCore, but running on Koyeb's own bare-metal fleet instead of a hyperscaler's.

### **4.17. Amazing Sandbox: A Local CLI Wrapper Over OS-Native Sandboxing**

* **Overview:** Amazing Sandbox (`asb`) is a small CLI that runs a given command, package manager, or coding agent inside whatever sandboxing primitive is native to your OS: Docker by default, Seatbelt on macOS, or Bubblewrap on Linux. The goal is narrow and practical: stop a compromised npm package or an overzealous AI agent from reading your whole disk or wiping files it shouldn't touch, without asking you to learn a new platform.
* **GitHub:** [ashishb/amazing-sandbox](https://github.com/ashishb/amazing-sandbox)
* **Website:** [ashishb.net/programming/amazing-sandbox](https://ashishb.net/programming/amazing-sandbox/)
* **Launch Date:** First commit in December 2025.
* **GitHub Stars:** 165.
* **License:** **MIT**.
* **Hosting:**
  * **SaaS:** No.
  * **Self-Hosted:** Yes, exclusively. It runs as a single Go binary (`go install` or a downloaded release) on the developer's own machine.
* **Capabilities:**
  * **Filesystem Access:** Read-write to the current directory and any explicitly referenced files by default; `-x` drops read access to the current directory, `-r` makes referenced directories read-only.
  * **Network Access:** Full by default; `-n` disables it for an air-gapped run, useful for linters and other tools that have no legitimate reason to reach the internet.
  * **Workload Suitability:** Built for **short-lived, one-off invocations**, wrapping a single command, not a persistent environment. It caches config directories for Claude Code, OpenAI Codex, and Gemini CLI so those agents don't need to re-authenticate on every run, and supports Python, JS/TS, Go, Rust, Ruby, Haskell, and Zig toolchains out of the box.

### **4.18. nono: Per-Tool-Call Brokered Sandboxing for Agents**

* **Overview:** nono is a local, OS-native sandbox for AI coding agents built by the team behind Sigstore. It runs directly on the developer's machine with no daemon, no container, and no VM: it wraps a process in the host kernel's own isolation primitives instead of virtualizing anything. Its defining feature is that it doesn't stop at sandboxing the agent process once. When an agent delegates to a real tool, `git`, `gh`, `curl`, `kubectl`, an MCP server, nono's broker re-sandboxes that specific call under its own policy, filesystem grants, network rules, and credentials, so a compromised `git` invocation can't inherit the agent's broader permissions.
* **GitHub:** [nolabs-ai/nono](https://github.com/nolabs-ai/nono) (project moved to this org from `always-further`; older references may still point there)
* **Website:** [nono.sh](https://nono.sh)
* **Launch Date:** GitHub history under the current org starts January 2026; the project existed earlier under the `always-further` namespace before the move.
* **GitHub Stars:** Approximately 3,630.
* **License:** **Apache-2.0**.
* **Hosting:**
  * **SaaS:** No.
  * **Self-Hosted:** Yes, exclusively. Installed via curl, Homebrew, or native OS packages (Debian, Fedora, Arch, RHEL, openSUSE, Nix), with support for macOS, Linux, and Windows via WSL2.
* **Capabilities:**
  * **Filesystem Access:** Scoped per profile; an agent gets read/write access to whatever paths its policy grants and nothing else. SSH keys, cloud credentials, and the rest of the disk are invisible by default.
  * **Network Access:** Brokered through a proxy with L7 (method + path) filtering per credential. Secrets are never handed to the sandboxed process directly; the broker injects them only when a request matches an allowed endpoint, the same credential-injection-proxy pattern covered in [Section 5](#5-credential-injection-proxies-keeping-secrets-out-of-sandboxed-code), built directly into nono rather than run as a separate tool.
  * **Workload Suitability:** **Short-lived, per-invocation** micro sandboxes rather than a persistent environment: a fresh sandbox spawns for each tool call and is destroyed afterward, with a sealed, Merkle-hashed audit trail left behind.
* **Unique Value Proposition:** Most sandboxes draw one boundary around the whole agent session. nono draws a separate boundary around each tool the agent calls, so a single overly broad grant to the agent doesn't automatically extend to everything it invokes. It also ships a registry of pre-signed profiles (Sigstore-attested) for popular agents like Claude Code, Codex, and OpenCode.

### **4.19. Deno Sandbox: Firecracker MicroVMs on Deno Deploy**

* **Overview:** Deno Sandbox is Deno's managed service for running untrusted or AI-generated code in the cloud, announced in beta on February 3, 2026. Each sandbox is a dedicated Firecracker microVM running in Deno Deploy's own infrastructure. It's in the same category as Vercel Sandbox, Koyeb Sandboxes, and AWS Bedrock AgentCore: a hosted alternative to running your own sandbox infrastructure, backed by a large platform rather than a smaller independent one.
* **GitHub:** [denoland/sandbox-py](https://github.com/denoland/sandbox-py) (Python SDK; a JavaScript/TypeScript SDK is also offered, and the platform itself is proprietary)
* **Website:** [deno.com/deploy/sandbox](https://deno.com/deploy/sandbox)
* **Launch Date:** Announced **February 3, 2026**, currently in beta.
* **GitHub Stars:** The Python SDK has 6 stars; as with other managed cloud sandbox platforms, this reflects the client library rather than the service.
* **License:** The SDK is **MIT**. The Sandbox platform itself is a proprietary, usage-billed Deno Deploy service.
* **Hosting:**
  * **SaaS:** Yes, exclusively, running in US and Europe regions today, with Asia planned.
  * **Self-Hosted:** No.
* **Underlying Technology:** Each sandbox is its own **Firecracker microVM**, the same technology behind AWS Lambda, with defaults of 2 vCPUs, 1.2 GiB memory, and 10GB disk (adjustable at creation). Boot times are under a second, with examples as fast as 93ms.
* **Capabilities:**
  * **Filesystem Access:** Ephemeral, scoped to the sandbox's own disk allocation.
  * **Network Access:** Configurable via an `allowNet` allowlist; all outbound traffic routes through a proxy that enforces the policy, and secrets are injected at the network layer only for approved destinations rather than being exposed to the running code, again the same credential-injection-proxy pattern covered in [Section 5](#5-credential-injection-proxies-keeping-secrets-out-of-sandboxed-code), here built into the platform itself.
  * **Workload Suitability:** **Short-running**, billed per CPU-hour and memory-hour ($0.05/CPU-hour, $0.016/GiB-hour), which fits one-off agent tasks better than a persistent, always-on development environment.

### **4.20. SandBase Harness: Multi-Backend Runtime for AI Agents**

* **Overview:** SandBase Harness is an open-source, local-first runtime that gives AI agents a common session and tool-execution layer across local processes, per-session Docker containers, Kubernetes workloads, and self-hosted workers. It is broader than a sandbox library: the same runtime also manages model routing, memory, permissions, credentials, approvals, and MCP tools. Sandboxing is therefore a selectable execution backend rather than the entire product.
* **GitHub:** [sandbaseai/sandbase-harness](https://github.com/sandbaseai/sandbase-harness)
* **MCP Registry:** [io.github.sandbaseai/sandbase-harness](https://registry.modelcontextprotocol.io/v0.1/servers?search=io.github.sandbaseai%2Fsandbase-harness)
* **Launch Date:** First public release in July 2026.
* **GitHub Stars:** More than 620.
* **License:** **Apache-2.0**.
* **Hosting:**
  * **SaaS:** No. The project does not provide a hosted sandbox service.
  * **Self-Hosted:** Yes, exclusively. The API and workers run on infrastructure controlled by the user.
* **Capabilities:**
  * **Filesystem Access:** Session-scoped. Docker sessions use a dedicated container; Kubernetes and worker sessions execute in their configured remote environments. The local backend runs directly on the host and should not be treated as a security boundary.
  * **Network Access:** Inherits the selected backend's network configuration. Network isolation is not automatically equivalent across local, Docker, Kubernetes, and worker modes.
  * **Workload Suitability:** Supports one-shot tool execution as well as persistent, multi-turn agent sessions with streamed events, memory, and resumable runtime state.
  * **Agent Integration:** Exposes sessions, prompts, tools, events, and snapshots through an official MCP server, allowing MCP-compatible clients to use the same managed execution layer.
  * **Security Model:** Docker mode provides container isolation and Kubernetes mode relies on the cluster's pod and policy boundaries. As with other container-backed systems, neither is inherently a microVM boundary; deployment hardening and backend policy determine the effective isolation strength.

## **5\. Credential Injection Proxies: Keeping Secrets Out of Sandboxed Code**

A sandbox stops an agent from touching your filesystem or process tree, but it doesn't answer a separate question: how does that agent call an authenticated API without ever holding the actual API key? Several platforms above, [**Docker Sandboxes** ↓](#411-docker-sandboxes-disposable-microvms-for-ai-coding-agents), [**Vercel Sandbox** ↓](#412-vercel-sandbox-firecracker-microvms-for-agent-workloads), [**AWS Bedrock AgentCore** ↓](#413-aws-bedrock-agentcore-managed-agent-runtime--code-interpreter), [**nono** ↓](#418-nono-per-tool-call-brokered-sandboxing-for-agents), and [**Deno Sandbox** ↓](#419-deno-sandbox-firecracker-microvms-on-deno-deploy), all answer it the same way: route outbound traffic through a proxy, give the sandboxed code a placeholder token instead of the real credential, and have the proxy swap in the real value only when a request matches an allowed destination. If the sandbox is compromised, whatever it exfiltrates is a token that's worthless outside the proxy.

The two projects below implement that pattern as standalone, general-purpose tools rather than as a feature bundled into one specific sandbox platform, so they can sit in front of any sandbox, container, or CI job you already run.

### **5.1. iron-proxy: An Egress Firewall for Untrusted Workloads**

* **Overview:** iron-proxy is a MITM egress proxy with a built-in DNS server that sits between an untrusted workload (a CI job, an AI coding agent, a sandboxed container) and the internet. It enforces default-deny at the network boundary: every outbound request is blocked unless the destination matches an explicit allowlist. Workloads use proxy tokens instead of real secrets, and iron-proxy swaps in the actual credential at egress, so a compromised workload can only exfiltrate a token that does nothing outside the proxy.
* **GitHub:** [paradigmxyz/iron-proxy](https://github.com/paradigmxyz/iron-proxy) (branded `iron-proxy` / iron.sh; built by Paradigm)
* **Website:** [docs.iron.sh](https://docs.iron.sh)
* **Launch Date:** First released April 2026.
* **GitHub Stars:** Approximately 600, with commits as recently as this month.
* **License:** **Apache-2.0**.
* **Hosting:**
  * **SaaS:** No.
  * **Self-Hosted:** Yes, exclusively. Ships as a single binary with a single YAML config, plus a Docker image.
* **Capabilities:**
  * **Filesystem Access:** Not applicable; iron-proxy only mediates network traffic, it doesn't sandbox a filesystem itself.
  * **Network Access:** Default-deny egress by domain/CIDR allowlist, with an upstream IP deny list that blocks allowlisted hostnames from resolving to cloud metadata endpoints or loopback (closing an SSRF/DNS-rebinding gap). Also proxies WebSocket and Server-Sent Events connections, and includes an optional PostgreSQL MITM listener that enforces per-tenant `SET ROLE` isolation over a shared service-account connection.
  * **Workload Suitability:** Designed to run alongside CI pipelines, GitHub Actions, and AI coding agents (Claude Code, Cursor, Codex), logging a structured, per-request audit trail of what was allowed, blocked, and swapped.

### **5.2. Infisical Agent Vault: A Local TLS-Terminating Credential Proxy**

* **Overview:** Agent Vault is a local forward proxy for AI agents from Infisical, the team behind a widely used open-source secrets manager. Point an agent's `HTTPS_PROXY` at it, and instead of adding another agent-specific abstraction, it works one layer down at the HTTPS level: it terminates TLS with a locally trusted certificate authority, presents itself as the upstream service, strips whatever credential the agent attached to the plaintext request, injects the correct one, and only then opens the real connection upstream.
* **GitHub:** [Infisical/agent-vault](https://github.com/Infisical/agent-vault)
* **Website:** [infisical.com](https://infisical.com)
* **Launch Date:** Launched in research preview; repository history starts **March 2026**.
* **GitHub Stars:** Approximately 2,060, with commits as recently as this month.
* **License:** Source-available: MIT for the core, with an enterprise-licensed `ee/` subset, the same split Infisical uses on its main platform.
* **Hosting:**
  * **SaaS:** No.
  * **Self-Hosted:** Yes, exclusively, running as a local sidecar next to the agent it's protecting.
* **Capabilities:**
  * **Filesystem Access:** Not applicable; it's a network proxy, not a filesystem sandbox.
  * **Network Access:** All HTTPS traffic from the agent passes through the proxy by design (that's the entire mechanism); credential swapping happens per matched request rather than per allowlisted domain.
  * **Workload Suitability:** Built for agent harnesses specifically, with support called out for Claude Code, OpenClaw, Hermes, and custom agents, rather than being a general CI/workload egress tool the way iron-proxy is.

## **6\. Docker vs MicroVM for Sandboxing**

When evaluating sandboxing solutions, one of the most fundamental architectural decisions is choosing between container-based isolation (Docker/OCI) and microVM-based isolation. This choice significantly impacts performance, security, and operational complexity.

### **Performance Characteristics**

**Containers (Docker) Performance Advantages:**
- **Startup Speed**: OCI runtimes can spawn containers orders of magnitude faster than Firecracker microVMs
- **Resource Overhead**: Minimal overhead when sharing kernel and base container layers
- **Memory Efficiency**: Shared kernel and base layers reduce overall memory footprint
- **Mature Ecosystem**: Extensive tooling and optimization for long-running deployments

**MicroVMs Performance Considerations:**
- **Cold Start Latency**: Higher initial startup cost compared to containers
- **Resource Isolation**: Each VM requires its own kernel and memory space
- **Hardware Requirements**: Depends on hardware virtualization support

### **Security Trade-offs**

**Containers Security Model:**
- Containers are "basically fancy processes" with namespace-based isolation
- Shared kernel creates potential attack surface
- Suitable for trusted, homogeneous workloads
- Enhanced security available through technologies like gVisor (application kernels)

**Real-World Container Escape Statistics:**
- **94% of organizations** reported serious container security incidents in the last 12 months
- **69% of incidents** involved misconfigurations rather than vulnerabilities
- **87% of container images** in production contain critical or high-severity vulnerabilities
- **60% of organizations** were vulnerable to the major 2024 "Leaky Vessels" container escape vulnerabilities
- Most container escapes occur through **misconfigurations** (privileged containers, sensitive mounts) rather than novel CVE exploits

**MicroVMs Security Advantages:**
- **Hardware-Level Isolation**: Dedicated guest kernel per sandbox
- **Stronger Boundaries**: Hardware-enforced isolation between workloads
- **Reduced Attack Surface**: Host kernel not directly accessible to guest code
- **Multi-Tenancy**: Superior isolation for untrusted code from different sources
- **Escape Mitigation**: Hardware virtualization makes container escape techniques ineffective

### **Use Case Recommendations**

**Choose Containers When:**
- Running internal, trusted applications
- Deploying homogeneous workloads that benefit from shared base layers
- Startup performance is critical
- Operating within a mature container ecosystem
- Managing long-running, stateful services

**Choose MicroVMs When:**
- Executing untrusted code from external sources
- Providing multi-tenant sandboxing services
- Security isolation is paramount
- Running diverse OS/kernel requirements
- Building hosting platforms or serverless infrastructure

### **Hybrid Approaches**

Some platforms combine both approaches:
- **Kata Containers**: Provides container-compatible API with VM-level isolation
- **gVisor**: Application kernel approach offering intermediate security
- **Platform-Specific**: Many cloud providers use MicroVMs for hosting but containers for application deployment

### **Practical Considerations**

Most organizations currently prefer containers because:
- Established tooling and operational expertise
- Simpler deployment and management workflows
- Better resource utilization for trusted workloads
- More mature monitoring and debugging capabilities

However, the emergence of fast-booting microVM technologies like Firecracker has made the performance gap less significant, making security considerations more important in the decision matrix.

**Container Escape Risk Assessment:**
- **Low Risk**: Internal development environments, trusted CI/CD pipelines, homogeneous application stacks
- **Medium Risk**: Multi-tenant SaaS platforms with authenticated users, plugin systems with code review
- **High Risk**: Public code execution services, AI agent sandboxes, untrusted user-generated code

**Mitigation Strategies:**
- **For Containers**: Use security-focused configurations, avoid privileged containers, implement least-privilege access, consider gVisor for additional isolation
- **For MicroVMs**: Accept higher resource overhead for stronger isolation guarantees, ideal for zero-trust environments

**Key Takeaway**: Use containers for internal, trusted applications where performance and ecosystem maturity matter most. Use MicroVMs for untrusted code execution, multi-tenant platforms, or when security isolation is non-negotiable. Given that 69% of container incidents involve misconfigurations rather than vulnerabilities, proper operational security practices are crucial regardless of the technology choice.

## **7\. Choosing Your Sandbox: A Decision Framework**

Choosing the right sandboxing solution depends on your specific requirements. Consider your project's security needs, workload types, operational preferences, and primary use case. This framework provides decision criteria to guide your selection.

### **Axis 1: Security vs. Performance vs. Compatibility**

This is the most important trade-off. Your choice depends on your threat model.

* **For Maximum Security:** If your application runs highly untrusted or potentially malicious code from the public internet, and you need the strongest possible isolation, choose a **microVM-based solution**. The hardware-enforced boundary from a dedicated guest kernel provides the best defense against container escape vulnerabilities.  
  * **Recommended:** **microsandbox**, **e2b**, **Daytona** (SaaS only, see [4.2](#42-daytona-secure--elastic-infrastructure-for-ai-code)), **Vercel Sandbox**, **AWS Bedrock AgentCore**, and **Docker Sandboxes**. All of these are built on per-session microVMs with a dedicated guest kernel.  
* **For Balanced Security and Compatibility:** If you need stronger isolation than standard containers but can't use hardware virtualization, an application kernel is a good choice. It reduces the attack surface without requiring hardware virtualization.  
  * **Recommended:** **gVisor**.  
* **For Maximum Performance and Speed:** If your workload is well-defined, you have some trust in the code, and startup time and resource overhead are most critical (e.g., high-volume, short-lived edge functions), a language-runtime-based sandbox is most efficient.  
  * **Recommended:** A platform built on **WebAssembly (WASM)** or **V8 Isolates**.

### **Axis 2: Stateless Functions vs. Stateful Workloads**

The nature of your workload - whether it's a one-off task or a long-running process - is important.

* **For Stateless/Ephemeral Tasks:** If you need to run quick, isolated tasks that don't require preserved state (e.g., grading code submissions, data transformations), most solutions work. However, those optimized for fast, ephemeral execution are better.  
  * **Recommended:** **microsandbox** in its temporary mode (msx) is explicitly designed for this.  
    **e2b**'s fast startup also makes it a strong contender.  
    **Vercel Sandbox** and **AWS Bedrock AgentCore**'s Code Interpreter are both purpose-built for one-shot, torn-down-after-use execution, and **Docker Sandboxes** targets one disposable sandbox per agent task rather than a persistent environment.  
* **For Stateful/Long-Running Processes:** If your use case requires a persistent environment where the filesystem can be modified, dependencies can be installed, and state is preserved across multiple interactions (e.g., an interactive AI coding assistant, a full development workspace, a multi-step agent), you need a platform with robust persistence features.  
  * **Recommended:** **Daytona**, **e2b** (Pro plan), and **microsandbox** (project mode) are all explicitly designed to support long-running, stateful workloads. CDEs like  
    **Gitpod** and **Coder** also excel at this.

### **Axis 3: SaaS Convenience vs. Self-Hosted Control**

Your organization's operational model and compliance requirements will determine your hosting strategy.

* **For a Managed Service (SaaS):** If you want to accelerate development and offload the operational burden of managing sandboxing infrastructure, a SaaS platform is the best choice. These platforms offer usage-based pricing and handle all the scaling, maintenance, and security of the underlying infrastructure.  
  * **Recommended:** **e2b** and **Daytona** provide mature, feature-rich SaaS offerings (Daytona is SaaS-only as of June 2026, see its [4.2 profile](#42-daytona-secure--elastic-infrastructure-for-ai-code), so it no longer fits the self-hosted list below). **Vercel Sandbox**, **AWS Bedrock AgentCore**, **Koyeb Sandboxes**, and **Deno Sandbox** are also SaaS-only, and are the natural choice if you're already building on Vercel, AWS, Koyeb, or Deno Deploy respectively.  
* **For Full Control (Self-Hosted):** If you have strict data sovereignty, regulatory compliance (e.g., GDPR), or security policies that mandate running all infrastructure within your own network perimeter, a self-hosted solution is necessary.  
  * **Recommended:** **microsandbox** is self-hosted by design and is the most straightforward choice for this model.  
    **e2b**, **Gitpod**, and **Coder** also offer robust self-hosting options, typically as part of their enterprise offerings (**Daytona** offered this previously but no longer does; see [4.2](#42-daytona-secure--elastic-infrastructure-for-ai-code)). **Docker Sandboxes** and **NVIDIA OpenShell** are self-hosted by design for local agent sandboxing, **Apple Containerization** fills the same role specifically on Apple Silicon Macs, and **Amazing Sandbox** and **nono** are both lightweight options for wrapping individual commands or tool calls rather than running a full persistent agent environment, with **nono** additionally sandboxing each tool the agent delegates to, not just the agent process itself.

### **Axis 4: AI/Agent-Specific vs. General-Purpose**

Finally, consider whether you need a tool tailored for a specific domain or a more general-purpose platform.

* **For AI-Centric Workflows:** If you are building AI agents, code interpreters, or other LLM-powered applications, choosing a platform that is explicitly optimized for this domain can provide significant advantages. Their SDKs and features are often designed to solve common problems in agentic development.  
  * **Recommended:** **e2b** and **Daytona** are heavily focused on the AI and agent use case. Among the newer entrants, **Docker Sandboxes** and **Vercel Sandbox** are both purpose-built for coding-agent workloads, **AWS Bedrock AgentCore** targets agents within the broader AWS agent-platform stack, and **NVIDIA OpenShell** adds policy-governed sandboxing specifically for autonomous agent fleets.  
* **For General-Purpose Execution/Development:** If your needs are broader, such as providing general-purpose development environments or a secure runtime for a variety of applications, a more general platform may be a better fit.  
  * **Recommended:** **microsandbox** is a powerful, general-purpose secure execution engine.  
    **Coder** and **Gitpod** are leading general-purpose Cloud Development Environments. **Apple Containerization** is the general-purpose choice for VM-backed containers on macOS specifically.

## **8\. Contributing**

This is a living document. The field of code sandboxing is evolving rapidly. If you see inaccuracies, have experience with these or other platforms, or want to suggest additions, please open an issue or submit a pull request. Your contributions are welcome!
