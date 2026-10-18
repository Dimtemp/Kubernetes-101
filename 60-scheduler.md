# Scheduler

## Labels
- key-value pairs
- assigned to Kubernetes resources to categorize and organize them
- selection criteria for objects
- crucial for the functionality of Services, Deployments, and other Kubernetes resources

## Annotations
- key-value pairs
- store non-identifying metadata
- not used by Kubernetes itself for resource identification


## NodeSelector
- uses a label selector to select those nodes that carry a certain label
- Example: Only nodes with disktype=ssd should be considered for the Pod

61-nodeselector.yaml

The scheduler exclusively searches for nodes that have disktype=ssd as a label. If it doesn't find a suitable node, the Pod remains in Pending until a node becomes available.

## NodeAffinity
is an advanced, more flexible variant of the NodeSelector concept. It allows you to create expressions that encompass more than a simple key-value comparison. You can also set preferences for how urgently a rule must be followed (required vs. preferred).

62-nodeaffinity.yaml

Only nodes with highmem=true are permitted. If no such node can be found, the Pod remains forever in Pending status.

63-nodeaffinity2.yaml

The scheduler prefers nodes with highmem=true but is not obligated to strictly follow this rule. If no node has the label, the Pod will still be started on any node, provided other prerequisites are met.

## PodAffinity
Pod A should preferably run on the same node as Pod B (e.g., because they communicate heavily with each other). 

64-podaffinity.yaml

This deployment template requires that each Pod (with app=web) preferably runs on a node where at least one Pod with app=cache is already active. topologyKey = kubernetes.io/hostname means: On this node, a matching cache Pod is already running.

## PodAntiAffinity
Pods should not be placed on the same node (e.g., to distribute multiple instances of an application and increase failure safety).

65-podantiaffinity.yaml

This deployment ensures that no two Pods with app=cache end up on the same node. This increases failure safety and automatically distributes Pods across different nodes.

## best practices
- label strategy forms the foundation for a smooth scheduling process in Kubernetes. Define fixed keys like app, env, or tier 
- recommended to use PreferredDuringSchedulingIgnoredDuringExecution instead of the hard requiredDuringSchedulingIgnoredDuringExecution

## example

set a label for a node:

```bash
kubectl label node my-node disktype=ssd
```

66-ssd.yaml
This ensures ssd-pod is always placed on my-node, provided no other node also carries this label.

PodAntiAffinity with Deployment

Assuming you want three instances of your application named redis that should never share a node:

## Trouble

```bash
kubectl apply --dry-run=client
```

test a yaml file

```bash
kubectl describe pod <pod-name>
```

Possible outcome:
- no nodes available with label X

```bash
kubectl get nodes --show-labels
```

Resource capacities might be exhausted.
