The system architecture shown in the diagram is organized into three primary operational domains: the Orchestration and Automation Layer, the Virtualization Infrastructure, and the Integrated Monitoring System.

1. Component Subsystems
Orchestration & Automation Layer:

OpenTofu (Infrastructure as Code): Handles declarative lifecycle management of compute resources by interfacing directly with the hypervisor API.

Ansible (Configuration Management): Manages post-boot operating system configuration, package installations, and system hardening without manual administrative intervention.

Virtualization Infrastructure (Proxmox VE / KVM Cluster):

Serves as the open-source virtualization engine replacing proprietary hypervisors (e.g., VMware ESXi).

Hosts the target Deployed Virtual Machines (VM1, VM2, etc.).

Executes Cloud-Init bootstrap scripts upon first boot to set basic parameters and launch initial configuration plays.

Integrated Monitoring System (Zabbix Stack):

Zabbix Server & Database: The central processing engine and storage backend for system telemetry and metrics, replacing commercial monitoring tools (e.g., PRTG).

Zabbix Web UI: The central administrative interface for system visibility and management.

Zabbix Proxy: Collects metric data and telemetry from deployed virtual machines to reduce direct load on the central server.

Notifications & Reports Engine: Dispatches alerts and system status reports via Email, Slack, or Webhooks.

2. End-to-End Execution Workflow
VM Provisioning Request: The Administrator initiates a infrastructure request via API, CLI, or web interface, triggering the Infrastructure-as-Code engine (Terraform/OpenTofu).

Hypervisor Provisioning: Terraform issues API calls to the Proxmox VE / KVM cluster to instantiate a new Virtual Machine from a pre-configured base template.

Post-Deployment Configuration: Once the VM boots, Cloud-Init bootstraps the initial system settings, and Ansible applies post-deployment configurations, including installing and configuring the Zabbix Agent.

Automated Telemetry Onboarding: Upon initial startup, an auto-registration workflow calls the Zabbix API, registering the host and automatically attaching appropriate monitoring templates.

Continuous Telemetry & Reporting: The guest VM streams historical performance data, system metrics, and alerts to the Zabbix Proxy and Server, providing instant visibility across the infrastructure lifecycle without manual intervention.