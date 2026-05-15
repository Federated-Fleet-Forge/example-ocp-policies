# Lab: ZTP source secrets in `policies` (hub)

[`hub-cluster-secrets-policygenerator.yaml`](../hub-cluster-secrets-policygenerator.yaml) replicates secrets using `fromSecret "policies" "<name>" ...`. This lab policy seeds **`policies`** so that replication can resolve.

**Order:** let **`policy-hub-lab-ztp-source-secrets`** reconcile on the hub **before** **`policy-hub-secrets-ztp`** (same hub is fine once `policies` holds the three source secrets).

## Pull secrets (from cluster, not from Git)

[`pull-secret-from-cluster.yaml`](pull-secret-from-cluster.yaml) and [`pull-secret-workload-clusters-from-cluster.yaml`](pull-secret-workload-clusters-from-cluster.yaml) use PolicyGenerator **`fromSecret "openshift-config" "pull-secret" ".dockerconfigjson"`** so the hub’s install pull secret is copied into **`policies`** as **`pull-secret`** and **`pull-secret-workload-clusters`** (same payload; adjust the second manifest if spokes need a different registry config).

Run **PolicyGenerator** (e.g. `policy-generator` / `kustomize` with the plugin) **with a kubeconfig that can read `openshift-config/pull-secret` on the hub**, so `fromSecret` can resolve. Treat generated bundles like secrets if they embed resolved data.

## BMC secret (Sushy / sushy-lab)

[`bmh-secret-sushy-lab.yaml`](bmh-secret-sushy-lab.yaml) enforces **`bmh-secret`** in **`policies`** with empty username/password for typical [sushy-lab](https://github.com/ngner/sushy-lab) (no HTTP Basic Auth). Change if your Redfish endpoint uses auth.

## BareMetalHost site config

BMH `credentialsName: bmh-secret` and `redfish-virtualmedia+http://...` URLs must match your Sushy host and system UUIDs; see **sushy-lab** `vms/README.md` and **example-ocp-ztp** `siteconfigs/networklab/siteconfig.yaml`.
