# AWS Nitro System Hardware Platform

I was having some trouble understanding the how the parts of the Nitro system function from the whitepaper logical diagram

<image src="images/aws_nitro_enclave-turnkey.png" alt="AWS Nitro Enclave Turnkey" />

Under the hood the "Nitro Controller" is a PCIe device that's a fancy NIC, or a "SmartNIC". SmartNICs started with the need to offload network processing from the main CPU and eventually vendors started adding features like remote baseboard management using custom ASICs.

The Nitro Controller evolved to include hypervisor virtualization, security, and storage offloading.

<image src="images/gemini-nitro-hypervisor.jpg" alt="AWS Nitro Hypervisor" />

In practice the Host VM communicates with the Nitro Controller via a virtual PCI device to provision and manage enclaves.

ENCLAVE RESOURCES ARE PROVISIONED FROM HOST RESOURCES NOT ALLOCATED TO THE HOST VM.

Enclaves are highly restricted VMs that boot from an EIF format, which is a container format that wraps a traditional Dockerfile style application in necessary kernel and init code to run as a VM.

Host VM/Enclave communication occurs over a Linux VSOCK which is mediated by the Nitro Controller through the PCIe bus.

At Turnkey the host communication with the Nitro Controller is managed by the [enclave-controller](https://github.com/tkhq/mono/tree/main/src/go/enclave-controller).
