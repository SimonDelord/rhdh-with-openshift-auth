# rhdh-with-sso

Configures Red Hat Developer Hub to integrate with OpenShift authentication.

This is the work of [@sabre1041](https://github.com/sabre1041) turned into kustomize.

## Caviates

1. this is demo-ware only
2. it does not properly handle secret information
3. it doesn't handle the url for oauth in a convienet way so its not very re-useable
4. it only works with the yet to be released RHDH operator v1.1.2 or using the latest midstream janus
5. it dangerously turns on `dangerouslyDisableDefaultAuthPolicy: true` due to
   * https://github.com/backstage/backstage/issues/23760
   * https://issues.redhat.com/browse/RHIDP-2044

## Instructions

1. install RHDH operator
2. if v1.1.2 or later is not available yet, patch to use Janus
   ```sh
   oc get ClusterServiceVersion rhdh-operator.v1.1.1 -o json --namespace rhdh-operator | jq '(.spec.install.spec.deployments[] | select(.name == "rhdh-operator").spec.template.spec.containers[] | select(.name == "manager")).image = "quay.io/janus-idp/operator:latest"' | oc apply -f - --namespace rhdh-operator
   ```
3. update kustomize/rhdh-with-openshift-auth/kustomization.yaml patches with the correct url for your cluster
4. ...
5. deploy RHDH with OpenShift Auth
   ```sh
   oc apply -k kustomization/rhdh-with-openshift-auth`
   ```
6. ...
