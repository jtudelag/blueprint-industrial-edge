## How to deploy staging-edge cluster on AWS

First of all you need to have your Amazon Web Service credentials file located in the following path:

`$HOME/.aws/credentials`

This file looks like this:

```
[default]
aws_access_key_id = xxxx
aws_secret_access_key = xxxx
```

The OpenShift installer binary will read that file if aws is set as a platform. From the path where the `knictl` binary is located, and in order to pull our staging-edge site and its requirements, please execute:

```bash
export SITE_NAME='test-edge-blueprint.emea-1.rht-labs.com'
export GIT_REPO='github.com/jtudelag/blueprint-industrial-edge'
alias openshift-install="$HOME/.kni/$SITE_NAME/requirements/openshift-install"
```

```bash
knictl fetch_requirements "$GIT_REPO/sites/$SITE_NAME/"
```

This command will download the site blueprint definition, and all its requirements (oc, openshift-install, kustomize, etc) to the `$HOME/.kni/`. Every site will have a separate directory within that location. The next step involves the actual rendering of the manifests (site + profile + base) into one set of manifests via kustomize that we can pass to the openshift-install binary.

```bash
knictl prepare_manifests "$SITE_NAME"
```

If everything goes well, the command will get out some instructions to deploy the cluster. It's basically asking you to run `openshift-install` binary pointing to where the final manifests created by `knictl` are:

```bash
openshift-install create cluster --dir="$HOME/.kni/$SITE_NAME/final_manifests" --log-level debug
```

Wait until the deployment is completed, and you will information about console endpoint, kubeadmin password and kubeconfig path.

If you have manifests that you want to deploy as Day 2 operations located in any of the 02_cluster-addons or 03_services directories, you can deploy them running the following command:

```bash
knictl apply_workloads "$SITE_NAME"
```

This is basically running kustomize to build and render all the manifests enabling alpha plugins, and apply them via oc/kubectl.
