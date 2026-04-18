# Resources

- https://eunomia.dev/tutorials/

# Introduction 

This is a development tutorial for eBPF based on CO-RE (Compile Once, Run Everywhere). 

It provides practical eBPF implementation examples from beginner to advanced, and real-world applications.  

Instead of BCC, we'll use frameworks like libbpf, Cilium, libbpf-rs, and eunomia-bpf for development, with examples in languages such as C, Go, and Rust.  

>[!note]
>eBPF (extended Berkeley Packet Filter) is a Linux-kernel technology that lets you run small sandboxed programs 

# Use Cases

## Use Case #1

eBPF can be used to prevent circular dependency issues during application deployment:
- https://github.blog/engineering/infrastructure/how-github-uses-ebpf-to-improve-deployment-safety/


