# Resources

- https://eunomia.dev/tutorials/
- complete source code of the tutorial:
https://github.com/eunomia-bpf/bpf-developer-tutorial

# Introduction 

This is a development tutorial for eBPF based on CO-RE (Compile Once, Run Everywhere). 

It provides practical eBPF implementation examples from beginner to advanced, and real-world applications.  

Rather than focusing on traditional tools like BCC, we'll use modern frameworks like libbpf, Cilium, libbpf-rs, and eunomia-bpf for development, with examples in languages such as C, Go, and Rust.  

>[!note]
>BCC (BPF Compiler Collection) is a set of tools leveraging eBPF for kernel tracing.

# What is eBPF?

eBPF (extended Berkeley Packet Filter) is a Linux-kernel technology that lets you run small sandboxed programs directly inside the kernel without modifying the kernel source code or loading kernel modules.

eBPF programs are triggered by events such as system calls, network packets, or tracepoints. 

They can inspect, filter, or even modify kernel behavior (for example, changing how packets are routed, or logging which system calls a process makes).  

In short, eBPF is a way to safely extend the Linux kernel at runtime for networking, observability, and security, mainly in the cloud‑native ecosystem.

## Safety and performance

Before running, each eBPF program is checked by a kernel‑space verifier that ensures it cannot crash, hang, or corrupt the kernel.  

The kernel then typically JIT‑compiles the program to native machine code, giving near‑native performance while still remaining safe.  

>[!note]
>JIT-compiling (Just‑In‑Time compilation) is a technique where code is compiled into machine instructions while the program is running, rather than before it starts.

# Why does eBPF matter for DevOps?

eBPF is increasingly important for DevOps (and SRE/platform‑engineering) roles, especially in cloud‑native and Kubernetes environments.  

eBPF powers many modern tools you’ll touch daily: **Cilium** (Kubernetes networking/Service Mesh), **Falco** (runtime security), and various observability agents that collect metrics without changing application code.  

It gives you deep, low‑overhead visibility into networking, syscalls, and system‑level performance, which is crucial for debugging latency, packet drops, or I/O bottlenecks without having to implement instrumentation inside the app.  

As a DevOps engineer, you don’t necessarily need to write raw eBPF C programs, but understanding how eBPF‑powered tools work (Cilium, Falco, etc.) is becoming a real differentiator in production‑grade Kubernetes and Linux‑centric environments.

# 0. Introduction to Core Concepts and Tools

## What is LLVM?

In the world of eBPF, LLVM (Low-Level Virtual Machine) is essentially the factory where your high-level code is transformed into something the Linux kernel can understand and execute.  

If you're writing eBPF programs, you aren't writing raw bytecode (which looks like hex or assembly); you’re likely writing in C or Rust. LLVM is the toolchain that bridges that gap.  

## Why do we need LLVM?

Before LLVM added support for the eBPF "target" (around 2015), writing eBPF was incredibly difficult.  

Without LLVM, eBPF would likely still be a niche tool used only by kernel networking experts. LLVM made it accessible to general systems engineers and SREs.

Here is why LLVM changed the game:  

### Standard Tooling 

LLVM allowed developers to use familiar languages like C or Rust instead of manual bytecode.

### BTF (BPF Type Format):

LLVM helps generate debug information that allows eBPF programs to be "portable" across different kernel versions (Compile Once – Run Everywhere).  

### Verification Prep

The LLVM compiler is aware of the kernel's strict "Verifier" rules. It tries to generate code that won't be rejected by the kernel for being unsafe (e.g., preventing infinite loops).

## The role of LLVM in the eBPF pipeline 

LLVM isn't just a single compiler; it’s a collection of modular compiler technologies. 
In the eBPF workflow, it performs three critical roles:

### 1. The Front-End (Clang)

You write your eBPF program in a subset of C. Clang (the LLVM front-end) takes that C code and turns it into LLVM Intermediate Representation (IR). This is a platform-independent version of your code.

### 2. The Optimizer

LLVM analyzes the IR to make it as efficient as possible. Since eBPF programs run inside the kernel, they need to be extremely fast and lightweight. LLVM strips out dead code and optimizes logic to ensure the program hits performance benchmarks.

### 3. The Back-End (eBPF Target)

This is the "special sauce." LLVM includes a specific eBPF back-end. It takes the optimized IR and translates it into eBPF Bytecode.

### Using Rust instead of C

Writing eBPF in Rust is becoming increasingly popular because it brings Rust's "if it compiles, it's safe" philosophy to the notoriously unforgiving environment of the Linux kernel.  

When you use Rust, you aren't using clang. Instead, you use the Rust compiler (rustc), which also uses LLVM as its backend.  

If you go the Rust route, you’ll likely use one of these two main ecosystems:  

#### 1. Aya

Aya is a "pure Rust" eBPF library. 
Unlike other tools, it does not depend on libbpf (a C library).

Pros: 
- You only need the Rust toolchain to build.
- You can share code easily between your "user-space" program and your "kernel-space" eBPF program.

#### 2. Libbpf-rs

This is a Rust wrapper around the official libbpf C library.

Pro: It is maintained alongside the Linux kernel, ensuring it always supports the latest kernel features.

Con: Requires a C compiler and libbpf headers on your system to build.  

### The "Verifier" remains the Boss

It is important to remember that Rust does not bypass the Kernel Verifier. 

Even if your Rust code is perfectly valid, the Verifier might still reject it if it thinks the program is too complex, could loop forever, or tries to access out-of-bounds memory. 

However, because Rust is so strict about memory safety, you will generally spend much less time fighting "invalid memory access" errors in the verifier than you would with C.

# How to use eBPF programming?

A typical eBPF program involves 2 parts:
- kernel space code (*_kern.c)
- user space code (*_user.c)

The kernel space code defines the logic, while the user space code manages loading and interacting with the kernel.  

However, tools like `libbpf-bootstrap` and the Go eBPF library help simplify this process, allowing for one-time compilation and easier development.

## Tools for eBPF development 

### BCC

A Python-based toolchain that simplifies writing, compiling, and loading eBPF programs. 

It offers many pre-built tracing tools but has limitations with dependencies and compatibility.  

### 

# Common eBPF Use Cases

## Use Case #1

eBPF can be used to prevent circular dependency issues during application deployment:
- https://github.blog/engineering/infrastructure/how-github-uses-ebpf-to-improve-deployment-safety/

