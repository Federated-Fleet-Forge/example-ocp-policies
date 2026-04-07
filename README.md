# example-ocp-policies

ACM PolicyGenerator fleet policies organized by NIST SP 800-53 control families. This repository holds declarative policy sources and placement rules intended to be consumed from the hub by Argo CD ApplicationSets defined in **example-ocp-gitops-base**.

## Directory structure

- **`policygen/`**  
  PolicyGenerator inputs grouped by control family for traceability and review:
  - **`AC/`** — Access Control
  - **`CM/`** — Configuration Management
  - **`SC/`** — System and Communications Protection  

  Additional families or subdivisions may be added using the same pattern.

- **`placements/`**  
  Placement and binding manifests (or related ACM placement artifacts) that scope which managed clusters receive generated policies.

Paths and file names should remain compatible with the ApplicationSet generators and project layout expected by your hub GitOps configuration.

## How policies are deployed

Argo CD **ApplicationSets** in [example-ocp-gitops-base](https://github.com/dusty-seahorse/example-ocp-gitops-base) watch this repository. When you merge changes here, the hub GitOps instance discovers updates and reconciles generated policy resources onto managed clusters according to placements and managed cluster labels.

No direct cluster-side apply is required for day-to-day policy changes if GitOps drift correction is enabled and RBAC is configured as documented in the bootstrap repo.

## Related repositories

- [example-ocp-gitops-base](https://github.com/dusty-seahorse/example-ocp-gitops-base) — Architecture, hub bootstrap, and ApplicationSet definitions ([docs/architecture.md](https://github.com/dusty-seahorse/example-ocp-gitops-base/blob/main/docs/architecture.md))
- [example-ocp-ztp](https://github.com/dusty-seahorse/example-ocp-ztp) — Cluster-specific ZTP configuration and per-cluster policy inputs that complement fleet-wide baselines
