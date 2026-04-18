# Resources

- https://eunomia.dev/tutorials/
- complete source code of the tutorial:
https://github.com/eunomia-bpf/bpf-developer-tutorial

# Introduction 

This is a development tutorial for eBPF based on CO-RE (Compile Once, Run Everywhere). 

It provides practical eBPF implementation examples from beginner to advanced, and real-world applications.  

Instead of BCC, we'll use frameworks like libbpf, Cilium, libbpf-rs, and eunomia-bpf for development, with examples in languages such as C, Go, and Rust.  

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

# Why does it matter for DevOps?

eBPF is increasingly important for DevOps (and SRE/platform‑engineering) roles, especially in cloud‑native and Kubernetes environments.  

eBPF powers many modern tools you’ll touch daily: **Cilium** (Kubernetes networking/Service Mesh), **Falco** (runtime security), and various observability agents that collect metrics without changing application code.  

It gives you deep, low‑overhead visibility into networking, syscalls, and system‑level performance, which is crucial for debugging latency, packet drops, or I/O bottlenecks without having to implement instrumentation inside the app.  

As a DevOps engineer, you don’t necessarily need to write raw eBPF C programs, but understanding how eBPF‑powered tools work (Cilium, Falco, etc.) is becoming a real differentiator in production‑grade Kubernetes and Linux‑centric environments.

# Use Cases

## Use Case #1

eBPF can be used to prevent circular dependency issues during application deployment:
- https://github.blog/engineering/infrastructure/how-github-uses-ebpf-to-improve-deployment-safety/

