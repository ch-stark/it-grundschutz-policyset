# Disallow SCC RunAsAny

The policy disallows assigning a value of `RunAsAny` to the `RunAsUser` field in SecurityContextConstraints objects. In order to exclude specific SecurityContextConstraints objects from being monitored by Gatekeeper, make sure to add them in the policy's [constraint.yaml](./constraint.yaml) object.

To add the `bar` SecurityContextConstraint to the list of excluded SecurityContextConstraints objects, modify the [constraint.yaml](./constraint.yaml) file -

```
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sNoPrivScc
metadata:
  name: scc-disallow-runasany
...
  parameters:
    allowedSCCs:
    ...
      - "bar"
    ...
```

After applying the updated `constraint.yaml` object, the `RunAsAny` value could be added to the `RunAsUser` attribute in `bar` SecurityContextConstraint object. Editing the `bar` SecurityContextConstraint -
```
$ oc edit scc bar

...
runAsUser:
  type: RunAsAny
...
```