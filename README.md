# microshift-kubevirt

From ACM or

```bash
oc apply -k overlays/fedora-cloud
```

## manually

```bash
LATEST=$(curl -L https://storage.googleapis.com/kubevirt-prow/devel/nightly/release/kubevirt/kubevirt/latest-arm64)
echo $LATEST
LATEST=20220331 # If the latest version does not work

oc apply -f https://storage.googleapis.com/kubevirt-prow/devel/nightly/release/kubevirt/kubevirt/${LATEST}/kubevirt-operator.yaml
oc apply -f https://storage.googleapis.com/kubevirt-prow/devel/nightly/release/kubevirt/kubevirt/${LATEST}/kubevirt-cr-arm64.yaml

oc adm policy add-scc-to-user privileged -n kubevirt -z kubevirt-operator

# The .status.phase will show Deploying multiple times and finally Deployed
oc get kubevirt.kubevirt.io/kubevirt -n kubevirt -o=jsonpath="{.status.phase}" -w
oc -n kubevirt wait kv kubevirt --for condition=Available --timeout=300s
oc get pods -n kubevirt
```
