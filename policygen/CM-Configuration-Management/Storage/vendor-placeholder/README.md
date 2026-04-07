# Vendor CSI Placeholder

This directory provides a skeleton for adding a third-party storage CSI driver to the
PolicyGenerator framework. Replace the placeholder values with your vendor's operator
details.

## How to implement

1. Copy this directory and rename it (e.g. `hpe-csi/`, `dell-csi/`, `netapp-trident/`)
2. Update `base/vendor-csi-subscription.yaml`:
   - Set `spec.name` to the operator package name from OperatorHub
   - Set `spec.source` to the correct catalog (e.g. `certified-operators`)
   - Set `spec.channel` to the desired update channel
3. Update `config/vendor-storageclass.yaml`:
   - Set `provisioner` to the CSI driver name
   - Add vendor-specific parameters
4. Add the new paths to a PolicyGenerator. For site-level selection, add to
   `policyGenerator-site-example.yaml` with a placement that matches
   `storageBrand: <your-vendor>`.

## Selection via labels

Clusters are assigned a storage backend using the `storageBrand` label in their
SiteConfig. PolicyGenerator placements select clusters by this label:

| Label Value | Storage Backend | PolicyGenerator |
|---|---|---|
| `odf` | OpenShift Data Foundation | `policyGenerator-group-virt-base.yaml` |
| `lvms` | Logical Volume Manager Storage | Group SNO policies |
| `vendor-csi` | Your vendor CSI | `policyGenerator-site-example.yaml` |

See `placements/` in the repo root for the placement manifests that implement
this selection.
