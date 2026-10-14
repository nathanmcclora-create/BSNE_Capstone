# BSNES Healthcare Network Capstone Project

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
