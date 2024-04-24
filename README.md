# rhdh-with-sso

Configures Red Hat Developer Hub to integrate with OpenShift authentication.

This is the work of [@sabre1041](https://github.com/sabre1041) turned into kustomize.

## Caviates

1. this is demo-ware only
2. it does not properly handle secret information
3. it doesn't handle the url for oauth in a convienet way so its not very re-useable
4. it only works with the yet to be released RHDH operator v1.1.2 or using the latest midstream janus

## Patching RHDH v1.1.1 to use Janus
While we wait for RHDH operator v1.1.2 to come out that has fixes needed for all this to work, you can patch RHDH operator v1.1.1 to use the janus image:

```sh
oc get ClusterServiceVersion rhdh-operator.v1.1.1 -o json | jq '(.spec.install.spec.deployments[] | select(.name == "rhdh-operator").spec.template.spec.containers[] | select(.name == "manager")).image = "quay.io/janus-idp/operator:latest"' | oc apply -f -
```
