# My RHOSO CRC GitOps Configuration

This repo contains **only** CRC-specific customizations for deploying RHOSO via GitOps.
All base configurations are referenced from upstream via kustomize.

## Structure

```
.
├── applications/          # ArgoCD Application manifests
│   ├── dependencies.yaml  # → Deploys operators
│   ├── controlplane.yaml  # → Deploys control plane
│   └── dataplane.yaml     # → Deploys data plane
├── dependencies/
│   └── kustomization.yaml # References upstream olm-deps
├── controlplane/
│   ├── kustomization.yaml # References upstream + CRC patches
│   ├── crc-nncp.yaml      # CRC single-node networking
│   ├── crc-netconfig.yaml
│   ├── crc-net-attach-def.yaml
│   ├── crc-metallb.yaml
│   └── patch-controlplane.yaml  # Reduces replicas for CRC
└── dataplane/
    ├── kustomization.yaml # References upstream + CRC patches
    ├── patch-nodeset.yaml # CRC EDPM node definitions
    └── patch-deployment.yaml
```

## Upstream References

| Component | Upstream URL |
|-----------|--------------|
| OLM Dependencies | `github.com/openstack-k8s-operators/architecture/lib/olm-deps` |
| ArgoCD Annotations | `github.com/openstack-k8s-operators/gitops/components/argocd/annotations` |
| ControlPlane | `github.com/openstack-k8s-operators/gitops/components/rhoso/controlplane/controlplane` |
| DataPlane | `github.com/openstack-k8s-operators/gitops/components/rhoso/dataplane` |

## Usage

1. **Create secrets** (see main guide - never commit to git)
2. **Push this repo to GitHub**
3. **Apply ArgoCD Applications:**
   ```bash
   oc apply -k applications/
   ```
