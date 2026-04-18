# 🔥 brasa

**A lightweight KVM-based Virtual Machine Monitor written in pure Go.**

brasa (Portuguese for "ember") is a minimal VMM designed for container workloads. It uses Linux KVM directly via ioctls — no CGo, no libvirt, no QEMU — to create lightweight virtual machines with the smallest possible footprint.

The goal is to provide a Go-native VMM that integrates with the container ecosystem (Kata Containers, containerd) while keeping the codebase small, auditable, and hackable.

## Why brasa?

Brasa (pronounced BRAH-zah) is Portuguese for ember — the small, intensely hot core left after the flames die down. It's the most efficient part of the fire: no waste, no spectacle, just concentrated heat doing real work.

That's exactly what a VMM for containers should be. Not a roaring bonfire of features like QEMU's 2 million lines of C. Not a blazing inferno that needs an entire Rust ecosystem to assemble. Just the glowing core: KVM ioctls, virtio devices, and a Linux kernel — nothing more.

The name also nods to the project's Brazilian roots and to the tradition of naming VMMs after fire: AWS has Firecracker, Alibaba has Dragonball (dragon fire). Brasa is what remains after both — quieter, smaller, and still hot enough to get the job done.

## Why Go?

The container ecosystem speaks Go. containerd, Kubernetes, the Kata Containers runtime, runc, cri-o — all Go. Yet every VMM in this ecosystem is written in C or Rust, creating a language boundary right at the most security-critical layer.

brasa asks: what if the VMM spoke the same language as everything around it? What if a Go developer contributing to containerd could also read, debug, and contribute to the hypervisor underneath?

This is not an argument that Go is "better" than Rust for VMM code. The rust-vmm ecosystem is excellent. But there's value in a VMM that the existing Go infrastructure community can understand, audit, and extend — without learning a second language and its toolchain.

## Status

🚧 **Early development** — not ready for production use.

## Design principles

1. **Pure Go** — no CGo, no external C libraries. All KVM interaction via `golang.org/x/sys/unix` ioctls.
2. **Minimal device model** — only virtio paravirtualized devices. No legacy hardware emulation.
3. **Direct kernel boot** — skip BIOS/UEFI entirely. Load bzImage + initrd directly into guest memory.
4. **Container-first** — designed to run container workloads, not general-purpose VMs.
5. **Small and auditable** — target < 10k lines of Go for the core VMM.

## Prior art and references

- [bobuhiro11/gokvm](https://github.com/bobuhiro11/gokvm) — Tiny Go KVM hypervisor (~1.5k lines), boots Linux
- [google/novm](https://github.com/google/novm) — Google's experimental Go VMM for containers (archived)
- [kata-containers/govmm](https://github.com/kata-containers/kata-containers/tree/main/src/runtime/pkg/govmm) — Kata's Go library for QEMU management
- [rust-vmm](https://github.com/rust-vmm) — The Rust VMM crate ecosystem (architectural reference)
- [KVM API documentation](https://docs.kernel.org/virt/kvm/api.html) — Linux kernel KVM interface
- [Using the KVM API (LWN)](https://lwn.net/Articles/658511/) — Excellent tutorial on KVM ioctls

## Requirements

- Linux with KVM support (`/dev/kvm` must exist)
- Go 1.22+
- x86_64 architecture (ARM64 planned for later)

## License

Apache-2.0

## Author

Gui Rezende <guilhermebr@gmail.com> — [github.com/guilhermebr](https://github.com/guilhermebr)
