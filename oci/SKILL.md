---
name: oci
description: Oracle Cloud Infrastructure guidance for designing, operating, and troubleshooting OCI services, starting with OCI Kubernetes Engine (OKE). Use when the user asks about OKE cluster design, Terraform or Resource Manager planning, OKE incident troubleshooting, Generic VNIC Attachment, Multus, pod networking, node pools, add-ons, ingress, load balancers, OCIR image pulls, Workload Identity, or Kubernetes workloads on OCI.
---

# Oracle Cloud Infrastructure Skills

Use this domain for practical Oracle Cloud Infrastructure guidance. The current content focuses on OCI Kubernetes Engine (OKE): cluster design, operational troubleshooting, Generic VNIC Attachment (GVA), and Multus multi-interface pod validation.

## How to Use This Domain

1. Start with the routing table below.
2. Read only the OKE file that matches the user's task.
3. Prefer official Oracle documentation and live read-only discovery commands before making design or remediation recommendations.
4. Ask before running commands that create, update, delete, restart, scale, drain, or otherwise mutate OCI or Kubernetes resources.

## Directory Structure

```text
oci/
|-- SKILL.md
`-- oke/
    |-- cluster-design.md
    |-- troubleshooting.md
    |-- gva-node-pools.md
    |-- multus-multihome.md
    |-- skills/
    |-- scripts/
    |-- agents/
    |-- shared/
    |-- examples/
    `-- tests/
```

## Category Routing

| Topic | File |
|-------|------|
| Design or scaffold an OKE cluster, Terraform stack, or OCI Resource Manager stack | Start with `oci/oke/cluster-design.md`, then load `oci/oke/skills/oke-cluster-generator/SKILL.md` |
| Troubleshoot OKE workloads, pods, services, DNS, add-ons, ingress, load balancers, image pulls, storage, Workload Identity, or cluster access | Start with `oci/oke/troubleshooting.md`, then load `oci/oke/skills/oke-troubleshooter/SKILL.md` |
| Configure OKE managed node pools with Generic VNIC Attachment secondary VNIC profiles and Application Resources | Start with `oci/oke/gva-node-pools.md`, then load `oci/oke/skills/oke-gva-deployer/SKILL.md` |
| Deploy or validate Multus NetworkAttachmentDefinitions and multi-interface pods on OKE | Start with `oci/oke/multus-multihome.md`, then load `oci/oke/skills/oke-multihome-deployer/SKILL.md` |

## Key Starting Points

- `oci/oke/cluster-design.md`
- `oci/oke/troubleshooting.md`
- `oci/oke/gva-node-pools.md`
- `oci/oke/multus-multihome.md`

## Operational Tools

The OKE operational skills include deterministic helper tools under `oci/oke/scripts/` and skill-specific helper scripts under `oci/oke/skills/*/scripts/`.

- Read-only discovery and evidence tools may be used to collect context.
- Generate-only tools may produce manifests, commands, Terraform snippets, or reports.
- Any tool or command that creates, updates, deletes, patches, restarts, scales, drains, debugs, assigns IPs, applies manifests, or otherwise mutates OCI or Kubernetes resources requires explicit user approval first.
- `oci/oke/scripts/gva-menu.sh` is allowed to create an OKE node pool for the GVA workflow only after the user approves execution and completes its final `CREATE` confirmation.
- `oci/oke/scripts/node-doctor-run.sh` requires approval before execution because it creates a temporary debug pod and may delete that pod during cleanup.

## Common Multi-Step Flows

| Task | Recommended Sequence |
|------|----------------------|
| Plan a production OKE cluster | `oke/cluster-design.md` |
| Diagnose an OKE service with no load balancer IP | `oke/troubleshooting.md` |
| Build a node pool with workload-specific secondary VNIC profiles | `oke/gva-node-pools.md` -> `oke/multus-multihome.md` if pods need multiple interfaces |
| Validate Multus pod networking on GVA-enabled nodes | `oke/multus-multihome.md` -> `oke/troubleshooting.md` if symptoms remain |
| Investigate OKE workload access to OCI APIs | `oke/troubleshooting.md` |

## Sources

- https://docs.oracle.com/en-us/iaas/Content/ContEng/home.htm
- https://docs.oracle.com/en-us/iaas/Content/ContEng/Tasks/contengAttaching_Multiple_VNICs.htm
- https://docs.oracle.com/en-us/iaas/Content/ContEng/Tasks/contenggrantingworkloadaccesstoresources.htm
- https://github.com/oracle-terraform-modules/terraform-oci-oke
