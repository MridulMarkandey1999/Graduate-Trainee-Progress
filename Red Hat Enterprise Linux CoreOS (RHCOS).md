## About RHCOS

- Red Hat Enterprise Linux CoreOS (RHCOS) represents single-purpose container operating system technology.RHCOS combines the quality standards of Red Hat Enterprise Linux (RHEL) with the automated, remote upgrade features from Container Linux.

- RHCOS is supported only as a component of OpenShift Container Platform 4.7 for all OpenShift Container Platform machines. RHCOS is the only supported operating system for OpenShift Container Platform **control plane, or master, machines**. 

- While RHCOS is the default operating system for all cluster machines, you can create **compute machines, which are also known as worker machines,** that use RHEL as their operating system.

## Key RHCOS features

- **Based on RHEL:** The underlying operating system consists primarily of RHEL components. The same quality, security, and control measures that support RHEL also support RHCOS. For example, RHCOS software is in RPM packages, and each RHCOS system starts up with a RHEL kernel and a set of services that are managed by the systemd init system.

- **Controlled immutability:** Although it contains RHEL components, RHCOS is designed to be managed more tightly than a default RHEL installation. Management is performed remotely from the OpenShift Container Platform cluster. When you set up your RHCOS machines, you can modify only a few system settings. This controlled immutability allows OpenShift Container Platform to store the latest state of RHCOS systems in the cluster so it is always able to create additional machines and perform updates based on the latest RHCOS configurations.

- **CRI-O container runtime:** Although RHCOS contains features for running the OCI- and libcontainer-formatted containers that Docker requires, it incorporates the CRI-O container engine instead of the Docker container engine. By focusing on features needed by Kubernetes platforms, such as OpenShift Container Platform, CRI-O can offer specific compatibility with different Kubernetes versions. CRI-O also offers a smaller footprint and reduced attack surface than is possible with container engines that offer a larger feature set. At the moment, CRI-O is only available as a container engine within OpenShift Container Platform clusters.

- **Set of container tools:** For tasks such as building, copying, and otherwise managing containers, RHCOS replaces the Docker CLI tool with a compatible set of container tools. The podman CLI tool supports many container runtime features, such as running, starting, stopping, listing, and removing containers and container images. The skopeo CLI tool can copy, authenticate, and sign images. You can use the crictl CLI tool to work with containers and pods from the CRI-O container engine. While direct use of these tools in RHCOS is discouraged, you can use them for debugging purposes.

- **rpm-ostree upgrades:** RHCOS features transactional upgrades using the rpm-ostree system. Updates are delivered by means of container images and are part of the OpenShift Container Platform update process. When deployed, the container image is pulled, extracted, and written to disk, then the bootloader is modified to boot into the new version. The machine will reboot into the update in a rolling manner to ensure cluster capacity is minimally impacted.

- **bootupd firmware and bootloader updater:** Package managers and hybrid systems such as rpm-ostree do not update the firmware or the bootloader. With bootupd, RHCOS users have access to a cross-distribution, system-agnostic update tool that manages firmware and boot updates in UEFI and legacy BIOS boot modes that run on modern architectures, such as x86_64, ppc64le, and aarch64.

For information about how to install bootupd, see the documentation for Updating the bootloader using bootupd for more information.

- **Updated through the Machine Config Operator:** In OpenShift Container Platform, the Machine Config Operator handles operating system upgrades. Instead of upgrading individual packages, as is done with yum upgrades, rpm-ostree delivers upgrades of the OS as an atomic unit. The new OS deployment is staged during upgrades and goes into effect on the next reboot. If something goes wrong with the upgrade, a single rollback and reboot returns the system to the previous state. RHCOS upgrades in OpenShift Container Platform are performed during cluster updates.

For RHCOS systems, the layout of the rpm-ostree file system has the following characteristics:

1. /usr is where the operating system binaries and libraries are stored and is read-only. We do not support altering this.

2. /etc, /boot, /var are writable on the system but only intended to be altered by the Machine Config Operator.

3. /var/lib/containers is the graph storage location for storing container images.

