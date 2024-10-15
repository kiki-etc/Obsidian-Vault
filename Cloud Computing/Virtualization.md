The cloud relies on **virtualization** to allow multiple virtual machines (VMs) to run on a single **physical machine (PM)**. Virtualization creates the illusion for the user that their VM is running on dedicated hardware, but in reality, multiple VMs share the same underlying physical resources.
### What is Virtualization?
**Virtualization** refers to the process of abstracting physical computing resources like servers, storage, and networks, allowing multipleaz independent environments to operate on the same hardware. A **hypervisor** manages resource allocation to these environments, ensuring efficient use of hardware and isolation between them.<br>
Virtualization allows multiple operating systems to run simultaneously on a single machine. Each virtual machine operates as if it were running on its own physical hardware.<br>
A **Virtual Machine** gives the user the illusion of running on dedicated hardware. It acts as a complete system with its own OS, but it shares the physical resources of the underlying hardware.
**Containers** are a lightweight alternative to VMs. They virtualize the OS rather than the entire system, making them quicker to start and consume fewer resources.
**Virtualization in the Cloud** forms the backbone of cloud computing, enabling **multi-tenancy**, where multiple clients share the same cloud infrastructure.
### Advantages and Disadvantages of Cloud Computing
**Advantages**:
 - **Multiplexing**: Multiple VMs share system resources, increasing efficiency.
 - **Lower Maintenance Overhead**: Cloud providers like AWS, Microsoft Azure, and Google Cloud handle hardware and software maintenance.
 - **Flexibility**: VMs can be moved between machines for maintenance or fault tolerance.
 - **Cost Efficiency**: The "pay-as-you-go" model eliminates the need to invest in underutilized servers.

**Disadvantages**:
 - **Performance Overhead**: Accessing servers via the internet introduces latency.
 - **Higher Costs**: Running applications in the cloud can become expensive with heavy usage.
### Virtualization Terminology
   - **Guest OS**: The operating system running inside a VM.
   - **Host OS**: The operating system running on the physical machine (PM).
   - **Hypervisor (VMM)**: A piece of software that allows multiple VMs to run on a single PM by controlling the distribution of hardware resources.
     - **Type 1 Hypervisor**: Runs directly on the hardware without a host OS. E.g., VMware ESXi.
     - **Type 2 Hypervisor**: Runs as an application on top of a host OS. E.g., VirtualBox, VMware Workstation.
### Challenges to Virtualization
**Hardware Control**: The guest OS expects full control of the hardware, but the **virtual machine monitor (VMM)** must manage and distribute access to multiple guests.<br>
**Design Approaches for VMM**:
     - **Hardware-assisted virtualization**: Modern CPUs (like Intel VT or AMD-V) have built-in support for virtualization.
     - **Full virtualization**: The guest OS runs unmodified and assumes full hardware control.
     - **Paravirtualization**: The guest OS is modified to be aware that it is running in a virtualized environment.
### Techniques for Virtualization
   - **CPU Virtualization**: Ensuring that each VM gets fair access to the CPU.
   - **Memory Virtualization**: VMs can use a virtual memory address space distinct from the physical memory of the PM.
   - **I/O Virtualization**: Enables virtual machines to perform I/O operations on devices like network adapters and storage as if they had direct access.
### VM Live Migration
   - **Live Migration**: VMs can be transferred between physical machines without disrupting the services they are running.
   - **Use Cases**: Maintenance, fault tolerance, and load balancing.
   - **Checkpointing**: Related to VM migration, it saves the state of a VM to enable recovery or rollback.
### Containers
**Containers vs. VMs**: 
- Containers virtualize the OS and are lighter than VMs, making them faster to start and more efficient in resource usage.
- Technologies like **Docker** and **Kubernetes** are key frameworks for container management.

**Linux Concepts**:
- **Namespaces**: Isolate system resources for containers.
- **Cgroups**: Control resource allocation (CPU, memory, I/O) for containers.
### Cloud Storage
   - **Traditional Databases vs. Cloud Storage**:
     - Older systems used **relational databases**, but modern cloud storage solutions are often simpler and more scalable, like **key-value stores** (e.g., Amazon Dynamo, Google Bigtable).
   - **Optimizations**:
     - **Caching** is used to speed up data access.
     - Cloud storage systems are designed for distributed environments with fault tolerance and high availability in mind.
   - **Application-specific storage**: For example, Facebook’s **Haystack** is optimized for storing and retrieving photos.
### **Additional Notes**:
**Public Cloud vs. Private Cloud**:
 - **Public cloud** is accessible to anyone willing to pay for services (e.g., AWS, Azure).
 - **Private cloud** is used internally within an organization.

**Fault Tolerance in Cloud Computing**: 
 - Cloud systems are designed to handle failures gracefully, often through techniques like data replication, VM migration, and failover mechanisms.
