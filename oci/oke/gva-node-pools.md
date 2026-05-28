# OKE Generic VNIC Attachment Node Pools

## Overview

Use this skill to plan, create, or review OCI Kubernetes Engine (OKE) managed node pools that use Generic VNIC Attachment (GVA) secondary VNIC profiles for pod networking.

GVA lets a node pool expose multiple secondary VNIC profiles, each with its own subnet, NSGs, display name, and pod IP allocation. Optional Application Resources let pods request one specific secondary VNIC profile through Kubernetes extended resources.

## When to Use

Use this skill when the task involves:

- Secondary VNIC profiles on OKE managed node pools.
- Application Resources such as `oke-application-resource.oci.oraclecloud.com/frontend`.
- Workload-specific pod network segmentation.
- Creating or updating node pools with multiple pod networking paths.
- Validating pods pinned to a single secondary VNIC profile.

Use `oci/oke/multus-multihome.md` instead when the pod needs multiple interfaces at the same time.

## Prerequisites

Confirm:

- The cluster uses VCN-native pod networking.
- The target node pool is a managed node pool.
- The selected shape supports the required number of VNICs and pod IP capacity.
- Secondary VNIC subnets and NSGs already exist or are included in the infrastructure plan.
- Subnet CIDR capacity is sufficient for the requested `ipCount`.
- OCI CLI and `kubectl` are available when live discovery or validation is requested.
- IAM permissions allow the operator to manage the node-pool and networking resources.

Multiple secondary VNIC pod networking is not supported with Flannel overlay networking.

## Design Inputs

Collect:

| Input | Notes |
|-------|-------|
| Cluster OCID and region | Use the user-provided cluster; do not assume the current context is correct. |
| Node pool create or update | New node pools are safer when changing network model. |
| Node shape | Validate architecture and VNIC limits. |
| Node count and placement | Include availability domains and worker subnet. |
| Kubernetes version and image | Node image version must match the selected node Kubernetes version. |
| Secondary VNIC profile names | Use stable names that describe the network tier. |
| Secondary subnet OCIDs | Each profile can use a different subnet. |
| NSG OCIDs | Apply least-privilege rules per profile. |
| `ipCount` | Size pod IP allocation with headroom. |
| Application Resource name | Use only when pods must select one specific secondary VNIC profile. |

## Application Resource Model

When a secondary VNIC profile has an Application Resource name, OKE exposes a Kubernetes extended resource on the nodes:

```text
oke-application-resource.oci.oraclecloud.com/<resource-name>
```

Pods that use this model must:

- Request exactly one Application Resource type.
- Request and limit one unit of that resource.
- Include the toleration for the node taint:

```yaml
tolerations:
  - key: oci.oraclecloud.com/application-resource-only
    operator: Exists
    effect: NoSchedule
```

Example container resource request:

```yaml
resources:
  requests:
    oke-application-resource.oci.oraclecloud.com/frontend: "1"
  limits:
    oke-application-resource.oci.oraclecloud.com/frontend: "1"
```

Use Application Resources when a pod should be pinned to one selected secondary VNIC profile. Do not use this pod-level Application Resource model for pods that need multiple interfaces through Multus.

## CLI Planning Pattern

When generating an OCI CLI command, build the secondary VNIC profile list explicitly and show it for review before running anything.

Abbreviated JSON shape:

```json
[
  {
    "displayName": "frontend-vnic-attachment",
    "createVnicDetails": {
      "displayName": "frontend-vnic",
      "subnetId": "ocid1.subnet...",
      "nsgIds": ["ocid1.networksecuritygroup..."],
      "assignPublicIp": false,
      "skipSourceDestCheck": false,
      "ipCount": 32,
      "applicationResources": ["frontend"]
    }
  }
]
```

Before execution, confirm:

- Cluster and region.
- Node pool name.
- Placement subnet.
- Shape, OCPUs, memory, and node count.
- Each secondary subnet and NSG.
- Each Application Resource name.
- `ipCount` per profile.
- Whether this is create or update.

## Validation

After the node pool is active, validate in this order:

1. Confirm nodes joined:

```bash
kubectl get nodes -o wide
```

2. Pick one node from the GVA node pool and inspect capacity:

```bash
kubectl describe node <node-name>
```

Check for:

```text
Capacity:
  oke-application-resource.oci.oraclecloud.com/<resource-name>: <count>
```

3. Confirm the node taint when Application Resources are configured:

```text
oci.oraclecloud.com/application-resource-only:NoSchedule
```

4. Deploy or review a test pod that requests exactly one Application Resource and has the matching toleration.

5. Inspect pod scheduling events and assigned IP:

```bash
kubectl describe pod <pod-name> -n <namespace>
kubectl get pod <pod-name> -n <namespace> -o wide
```

6. If the pod remains Pending, check:
   - Resource name spelling and case.
   - Available extended resource capacity.
   - Required toleration.
   - Node selectors or affinity.
   - Subnet pod IP capacity.
   - Shape VNIC limits.

## Common Mistakes

- Trying to use GVA with Flannel networking.
- Combining pod-level Application Resource requests with Multus annotations in the same pod.
- Requesting more than one Application Resource type in a pod.
- Forgetting the `oci.oraclecloud.com/application-resource-only:NoSchedule` toleration.
- Setting `ipCount` without checking subnet capacity and shape VNIC limits.
- Reusing old node-pool JSON payloads without revalidating cluster, region, subnets, NSGs, image, and shape.
- Assuming Application Resource names are case-insensitive.

## Sources

- https://docs.oracle.com/en-us/iaas/Content/ContEng/Tasks/contengAttaching_Multiple_VNICs.htm
- https://docs.oracle.com/en-us/iaas/Content/ContEng/Tasks/contengcreatingclusterusingoke_topic-Using_the_Console_to_create_a_Custom_Cluster_with_Explicitly_Defined_Settings.htm
- https://docs.oracle.com/en-us/iaas/Content/ContEng/Concepts/contengpodnetworking_topic-OCI_CNI_plugin.htm
- https://docs.oracle.com/en-us/iaas/tools/oci-cli/latest/oci_cli_docs/cmdref/ce/node-pool/create.html
- https://docs.oracle.com/en-us/iaas/tools/oci-cli/latest/oci_cli_docs/cmdref/ce/node-pool/update.html
