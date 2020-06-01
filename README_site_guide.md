
~/.kni/kubeconfighub.json

"kubeconfig file, lo encodeas a base64"


```bash
tree .kni/
.kni/
├── id_rsa
├── id_rsa.pub
├── kubeconfighub.json
└── pull-secret.json
```

# Profile

The user should know which exact version of OCP wnats to deploy, and should edit the `requirements.yaml` file
accordingly to download the righ client tools versions.

Also, the user should know which version of kubernetes is using the exact version of OCP.

For example, for OCP 4.4, k8s 1.17 is used. the user should always check [OCP release notes](https://docs.openshift.com/container-platform/4.4/release_notes/ocp-4-4-release-notes.html#ocp-4-4-about-this-release) before.

```bash
cd blueprint-industrial-edge/profiles/production.aws/
vi requirements.yaml
```




# Site

```bash
cd blueprint-industrial-edge/sites
cp -a staging-edge.devcluster.openshift.com/ test-edge-blueprint.emea-1.rht-labs.com/
```

## 00_install

```bash
cd blueprint-industrial-edge/sites/test-edge-blueprint.emea-1.rht-labs.com/00_install-config
vi kustomization.yaml -> Giturl
vi install-config.patch.yaml -> baseDomain, and alterantively, the aws zone for example
vi install-config.name.patch.yaml -> cluster-name
```

## 02_cluster_addons

```bash
cd blueprint-industrial-edge/sites/test-edge-blueprint.emea-1.rht-labs.com/02_cluster_addons/00_acm_registration
vi acm-name-config.patch.yaml -> ClusterName and ClusterNameNS
```
