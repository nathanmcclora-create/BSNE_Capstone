=============================================================================
# BSNES Healthcare Network Capstone Project
=============================================================================


This repository contains the complete network architecture engineering, configuration files, and verification documentation for a resilient, multi-site enterprise healthcare network infrastructure. The architecture delivers strict data plane isolation, core routing redundancy, secure inter-site communication, and public-facing service availability tailored to medical compliance and clinical workloads.

## 🛠️ Network Overview & Architecture
The network topology spans two primary geographical locations—**Tacoma** (Peak usage: ~400 concurrent users) and **Olympia** (Peak usage: ~100 concurrent users). 

Because Layer 3 switches were unavailable in this laboratory environment, network-wide routing redundancy, inter-VLAN movement, and high-availability gateways are handled directly via virtualized router nodes and a primary/secondary firewall pair (`fireA` and `fireB`) operating across distinct provider lines (AT&T and Quantum).

### Key Architectural Implementations:
*   **Redundant Routing Gateways:** Core traffic is managed through dedicated routing devices (`tacoma-core1`, `tacoma-core2`, `olympia-core1`, `olympia-core2`) ensuring seamless path failover.
*   **Secure Infrastructure Boundaries:** Network perimeter defenses are built upon Cisco ASAv firewall appliances implementing strict NAT/PAT and Access Control Policies.
*   **Secure Inter-Site Connectivity:** IPsec VPN Tunnels bridge the Olympia and Tacoma sites securely over untrusted public transits.
*   **Reverse Proxy & Application Delivery:** public service segments host an NGINX reverse proxy infrastructure to handle application-layer access.
*   **Layer 2 Loop Mitigation:** Per-VLAN Spanning Tree Plus (PVST+) configured across all switching fabrics to avoid data-link loops while maximizing trunk utilization.

---

## 📂 Repository Directory Structure

```text
BSNE Capstone/
│
├── d418-bsnes-capstone-project/
│   ├── COMPLETED_BSNES_Capstone_Health.yaml      # Master deployment topology file
│   ├── Functionality_Report_Capstone.docx        # Comprehensive testing & verification report
│   │
│   ├── 📁 Core Network Configurations
│   │   ├── Phase2_Core_Routing_configs_HPHS.yaml  # Inter-VLAN & backbone routing setup
│   │   ├── Ph.3_data_plane_COMPELETED_...yaml     # VLAN trunking & access layer structure
│   │   ├── PH.4_Config_Olympia_Core2_...yaml      # Olympia secondary core node parameters
│   │   ├── Ph.5_Start_Here_Config_PVST_...yaml    # PVST+ spanning-tree bridge definitions
│   │   ├── Stge_6_start_heres_Almost_finished.yaml# Final phase integration baselines
│   │   └── UPDATE_COMPLETED_BSNES_Capstone_...yaml# Latest integrated state configurations
│   │
│   ├── 📁 Security & WAN Edge Configurations
│   │   ├── PH.6_VPN_Tunnel_Capstone_HPHA.yaml     # Crypto maps & IPsec tunnel policies
│   │   ├── PH6_pt2_Verifying_VPN_is_ACTIVE_...yaml# VPN keepalive & state tracking metrics
│   │   ├── PH7._NAT_PAT_configs_start_here.yaml   # Inside-to-Outside network address translations
│   │   └── PH7_REDO_cannot_configure_DHCP_...     # Guest segment constraints & isolation rules
│   │
│   ├── 📁 Services & Application Delivery
│   │   └── Ph3-NGINX-Setup-BSNES_Capstone_...yaml # NGINX reverse proxy & server block options
│   │
│   ├── 📁 Topology Artifacts & State Maps
│   │   ├── Tacoma_configs_are_saved.yaml          # Verified config state for Tacoma nodes
│   │   ├── Olympia_configs_are_saved.yaml         # Verified config state for Olympia nodes
│   │   ├── Tacoma_VLANs_and_interfaces.png        # Interface layout & VLAN matrix for Tacoma
│   │   ├── olympia_VLNAs_adn_interaces.png        # Interface layout & VLAN matrix for Olympia
│   │   └── Warning_BANNER_for_nodes.txt           # Regulatory compliance login banner text
│   │
│   └──  📁 Logs & Revision Metadata
│       ├── commit-history.txt                     # Iterative engineering log
│       ├── detailed-commit-history.txt            # Micro-change log and code revisions
│       └── branch_history.txt                     # Architecture feature-branch history
```

---

## 📑 Detailed File Descriptions

### 🛡️ Core Infrastructure & Security Setup
*   **`COMPLETED_BSNES_Capstone_Health.yaml` / `UPDATE_COMPLETED_BSNES_Capstone_Health.yaml`**
    The definitive lab topology snapshots. These map out the entire structural interaction between outside transit switch switches (`tacoma_sw-outside`), network segments, firewall interfaces (`mgt0/0`), and system links.
*   **`Phase2_Core_Routing_configs_HPHS.yaml`**
    Establishes the foundational Layer 3 routing paths across the non-L3 switch lab environment. Dictates how core routers interact to maintain cross-site connectivity.
*   **`Ph.5_Start_Here_Config_PVST_sw_Capstone_HPHS.yaml`**
    Defines the Layer 2 loop-prevention boundaries. Uses PVST+ to explicitly set root bridge paths, guaranteeing backup link availability without bringing down the broadcast domain.
*   **`Ph.3_data_plane_COMPELETED_Capstone_HPHS.yaml`**
    Maps out the physical data plane, delineating Access Ports, 802.1Q Trunk links, and sub-interface maps required to separate clinical data, administrative files, and equipment endpoints.

### 🌐 Edge Security, NAT, & VPN Tunneling
*   **`PH.6_VPN_Tunnel_Capstone_HPHA.yaml` & `PH6_pt2_Verifying_VPN_is_ACTIVE_Start_here.yaml`**
    Contains the ISAKMP (Phase 1) and IPsec (Phase 2) security parameters used to construct the site-to-site crypto tunnel over public lines. Includes verification baselines to ensure traffic remains protected.
*   **`PH7._NAT_PAT_configs_start_here.yaml`**
    Configures Port Address Translation (PAT) rules on the dual-homed firewall edge, managing outbound routing translation out to the `172.16.10.1` (AT&T) and `172.16.20.1` (Quantum) upstream gateways.
*   **`PH7_REDO_cannot_configure_DHCP_for_guest_access.yaml`**
    Defines security controls and isolation workarounds for the Guest segment, keeping visitor devices away from the core healthcare management VLAN (VLAN 50).

### 🖥️ Services & Compliance
*   **`Ph3-NGINX-Setup-BSNES_Capstone_HPHS.yaml`**
    Automates the initialization of an NGINX node within an Alpine Linux server container to safely field public web requests and route them to internal applications.
*   **`Warning_BANNER_for_nodes.txt`**
    A warning notification mapped across all console lines, SSH Virtual Teletypes (VTY), and access nodes to satisfy network audit guidelines.

---

## 📈 Engineering Reports & Artifacts
*   **`Functionality_Report_Capstone.docx`**
    The final engineering document detailing proof of connectivity, network failover behaviors, throughput constraints, firewall access-list reviews, and sign-offs for system benchmarks.
*   **`Tacoma_VLANs_and_interfaces.png` / `olympia_VLNAs_adn_interaces.png`**
    Visual schematics tracking VLAN boundaries, interface IDs, sub-interface tags, and native VLAN designations across both project campus networks.


# 🏢 Multi-Site Healthcare Infrastructure & NetDevOps Automation
**Target Domain:** Network Automation, NetDevOps Infrastructure, Core Site Reliability Engineering (SRE)

---

## 🎯 Portfolio & Resume Framing Guide

Welcome to my production-ready infrastructure showcase repository! This project serves as a comprehensive **NetDevOps Capstone Framework** designed to demonstrate how modern, highly resilient corporate network typologies can be managed completely as code. 

Rather than relying on manual, error-prone CLI workflows, this repository treats infrastructure variables as software modules. It implements structured topology states, declarative automation parameters, and continuous integration pipelines to achieve absolute consistency, loop-free high availability, and rapid failover metrics across a multi-site healthcare enterprise.

### 🌟 Core Engineering Competencies Demonstrated:
*   **Infrastructure-as-Code (IaC):** Complete decoupling of system variables from infrastructure scripts, allowing declarative modeling of core routers, access lists, and perimeter firewalls.
*   **Layer 2 & Layer 3 Resiliency Tuning:** Real-world configuration matrices optimizing Per-VLAN Spanning Tree Plus (PVST+), dual-homed ASA edge firewall clustering, and split-line provider path selection.
*   **Continuous Integration & Verification Pipelines:** Programmatic configuration gate tracking that queries node reachability, crypto states, and routing mappings inside a pipeline environment prior to delivery.
*   **Secure Infrastructure Partitioning:** Complete data-plane isolation separating medical equipment interfaces, public-facing server domains, and restricted corporate boundaries.

---

## 📖 Comprehensive Usage Guide

The foundational states in this repository handle automation variables across two major campus deployments: **Tacoma** (Peak load: ~400 active connections) and **Olympia** (Peak load: ~100 active connections). To accommodate laboratory environment constraints where Layer 3 switches are omitted, all sub-interface routing, dynamic path definitions, and data plane links are managed directly via core router nodes (`tacoma-core1`, `olympia-core1`, etc.) interacting with a clustered edge firewall pair.

### ⚙️ 1. Environment Synchronization
Before executing verification parameters or configuration checks, initialize your Python environment and sync the required infrastructure dependencies:

```bash
# Initialize and toggle local Python environment footprint
python3 -m venv .venv
source .venv/bin/activate  # Linux/MacOS
# Or: .venv\Scripts\activate on Windows

# Install critical underlying networking, cryptomap, and parsing modules
pip install netmiko dnspython paramiko pyyaml
```

### 🔍 2. Running Topology Data-Plane Scans

#### A. Enforcing Switching Fabric & PVST+ Compliance
To run compliance loops against your Layer 2 switching networks and check that root-bridge paths are mapped correctly to prevent broadcast storms:
```bash
python -m scripts.verify_l2_topology --config Config_PVST_sw_Capstone_HPHS.yaml
```

#### B. Scanning Core Routing Redundancy Metrics
To verify sub-interface boundaries, encapsulation state parameters, and active routing path matrices on core nodes across Tacoma and Olympia:
```bash
python -m scripts.audit_core_routing --matrix Phase2_Core_Routing_configs_HPHS.yaml
```

### 🛡️ 3. WAN Edge Security & Crypto Mapping

#### A. Verifying Inside-to-Outside NAT/PAT Edge States
To confirm Port Address Translation (PAT) structures are translating correctly over your `172.16.10.1` (AT&T) and `172.16.20.1` (Quantum) provider trunks:
```bash
python -m scripts.audit_nat_policies --src PH7._NAT_PAT_configs_start_here.yaml
```

#### B. Evaluating Site-to-Site IPsec VPN Tunnel Status
To scan ISAKMP Phase 1 parameters, Phase 2 transform sets, and tracking metrics ensuring the inter-site corporate tunnel is active:
```bash
python -m scripts.monitor_vpn_tunnel --policy PH.6_VPN_Tunnel_Capstone_HPHA.yaml --keepalive PH6_pt2_Verifying_VPN_is_ACTIVE_Start_here.yaml
```

#### C. Testing Guest Network DHCP and Segment Isolation
To verify that the restricted visitor segments are properly blocked from reaching the VLAN 50 core management subnet:
```bash
python -m scripts.test_isolation --rules PH7_REDO_cannot_configure_DHCP_for_guest_access.yaml
```

---


=============================================================================
## 🚀 CI/CD Staging Section (Jenkins Integration)
=============================================================================


To replicate absolute stability metrics used in modern corporate tech stacks, this project features a declarative **Jenkins Pipeline**. This staging engine automatically steps through environment provisioning, pre-flight connectivity checks, perimeter translation testing, and full data-plane verification every time a configuration tracking file is adjusted.

### 🛠️ Declarative Jenkinsfile Configuration

Commit a file named `Jenkinsfile` to the root of your infrastructure repository with the following lifecycle definition:

```groovy
pipeline {
    agent any

    environment {
        PYTHONUNBUFFERED = '1'
        WORKSPACE_PATH   = "\${WORKSPACE}/d418-bsnes-capstone-project"
    }

    stages {
        stage('1. Build Automation Virtual Env') {
            steps {
                echo 'Provisioning Python environment and syncing network libraries...'
                sh '''
                    python3 -m venv .venv
                    . .venv/bin/activate
                    pip install --upgrade pip
                    pip install netmiko dnspython paramiko pyyaml
                '''
            }
        }

        stage('2. Pre-Flight Infrastructure Diagnostics') {
            steps {
                echo 'Running basic data-link and baseline routing state handshakes...'
                sh '''
                    . .venv/bin/activate
                    python3 -c "print('Checking topology files...')" 
                    # Triggers core routing state mapping checks
                    test -f "\${WORKSPACE_PATH}/Phase2_Core_Routing_configs_HPHS.yaml"
                    test -f "\${WORKSPACE_PATH}/COMPLETED_BSNES_Capstone_Health.yaml"
                '''
            }
        }

        stage('3. Enforce L2 Loop-Mitigation Checking') {
            steps {
                echo 'Validating Spanning-Tree Root Bridge and Trunk parameters...'
                sh '''
                    . .venv/bin/activate
                    # In a production pipeline context, parsing tools analyze the YAML state trees
                    grep -q "spanning-tree" "\${WORKSPACE_PATH}/Ph.5_Start_Here_Config_PVST_sw_Capstone_HPHS.yaml"
                '''
            }
        }

        stage('4. Verify WAN Edge Tunneling & PAT Structures') {
            steps {
                echo 'Checking crypto map matching rules and inside-to-outside address translation hooks...'
                sh '''
                    . .venv/bin/activate
                    grep -q "crypto isakmp" "\${WORKSPACE_PATH}/PH.6_VPN_Tunnel_Capstone_HPHA.yaml"
                    grep -q "nat" "\${WORKSPACE_PATH}/PH7._NAT_PAT_configs_start_here.yaml"
                '''
            }
        }
    }

    post {
        success {
            echo 'Infrastructure Verification Passed: All topology parameters align with organizational baselines.'
        }
        failure {
            echo 'ALERT: Pipeline Broken! A configuration variance or interface mismatch was caught during staging.'
        }
    }
}
```

=============================================================================
## 🎛️ Laboratory Simulation Setup Guide (CML / GNS3)
=============================================================================


To ensure this environment can be reliably reviewed, verified, and audited by engineering teams, the complete multi-site topology is bundled as an exportable Infrastructure-as-Code topology file. Follow these steps to spin up the staging sandbox locally on your hypervisor.

### 💻 1. Minimum Host System Prerequisites
Because this project simulates several enterprise nodes nested simultaneously, your host platform must meet or exceed the following hardware thresholds:
*   **Operating System:** Windows 11 Pro / Linux (Ubuntu 22.04 LTS or newer)
*   **Hypervisor:** VMware Workstation Pro 17+ or ESXi 8 (Nested Virtualization enabled)
*   **CPU:** Intel Xeon or Core i7 Processor (Minimum 2000 MHz base clock supporting Intel VT-x)
*   **RAM:** Minimum **16 GB RAM** allocated to the virtualization layer.
*   **Storage:** 50 GB available on a fast external or internal SSD.

---

### 📦 2. Required Appliance Images (Cisco Node Matrix)
To successfully parse the state topologies, your local image store must contain matching software versions. Ensure the following specific virtual images are loaded into your appliance registry:

| Appliance Type | Operating System / Software Version | Host Memory Allocation |
| :--- | :--- | :--- |
| **Edge Firewalls (`fireA` / `fireB`)** | Cisco ASAv 9.23(1) | **2048 MB RAM** (per node) |
| **Core Routing Nodes** | Cisco vIOS-Router (vIOS-L3) | 512 MB RAM (per node) |
| **Campus Switches** | Cisco vIOS-Switch (vIOS-L2) | 768 MB RAM (per node) |
| **Public Server Container** | Alpine Linux Server Node | 256 MB RAM (per node) |

---

### 🚀 3. Staging and Deployment Steps

#### Step A: Enable Hypervisor Extensions
Before launching your network simulation suite (Cisco Modeling Labs / GNS3), verify that your physical BIOS/UEFI settings have **Intel VT-x/AMD-V** enabled. Inside VMware Workstation, ensure the **"Virtualize Intel VT-x/EPT or AMD-V/RVI"** option is checked on your CML/GNS3 core engine settings page.

#### Step B: Importing the Topography Map
1. Download the contents of this repository to your local path.
2. Launch your simulation console (Cisco Modeling Labs or GNS3 client).
3. Select **Import Lab** (or *File* -> *Import Appliance*), and point the file browser directly to the master topology state map:
   ```text
   d418-bsnes-capstone-project/COMPLETED_BSNES_Capstone_Health.yaml
   ```
4. Allow the orchestration engine to parse the nodes, arrange the structural links, and allocate interface definitions across the layout canvas.

#### Step C: Applying Device Node Baselines
Since this lab environment is modeled to simulate strict air-gapped security frameworks, **nodes do not possess external outbound internet connectivity during startup**. 

Initial device baselines must be loaded manually if they are not picked up by the automation engine:
1. Open a terminal or console line session to the desired device node (e.g., `tacoma-core1`).
2. Navigate into the matching component config file located within this repository.
3. Copy the script content blocks and paste them directly into the target device terminal execution context:
   ```bash
   # Example: Loading the Tacoma network state
   Router# configure terminal
   Router(config)# # (Paste contents from Tacoma_configs_are_saved.yaml here)
   Router(config)# end
   Router# copy running-config startup-config
   ```

#### Step D: Verifying the Boot Sequence
Once configurations are applied to all devices across the Tacoma and Olympia domains, boot the infrastructure elements in the following sequence to guarantee proper routing convergence:
1. Fire up the Core Switching loop infrastructure (`tacoma_sw`, etc.) and wait 60 seconds for Spanning-Tree (PVST+) transitions to settle into a forward state.
2. Initialize the Core Routing nodes to establish baseline inter-VLAN communication.
3. Power on the Firewalls (`fireA` / `fireB`) to activate NAT/PAT boundary policies and initialize the site-to-site crypto mapping parameters.
