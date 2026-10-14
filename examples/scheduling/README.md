# Kubernetes Scheduling Examples

These manifests were created and tested as part of the **Games Legend Kubernetes project** on a local Docker Desktop Kubernetes cluster.

## Topics Covered

### 1. Taints & Tolerations

Controls which Pods are allowed to run on specific Nodes.

* `scheduling-test.yaml`

  * Demonstrates how a Toleration allows a Pod to run on a tainted Node.

### 2. nodeSelector

Schedules a Pod onto a Node with a specific label.

* `scheduling-selector.yaml`

### 3. Required Node Affinity

Requires the Pod to be scheduled on a Node matching specific conditions.

* `scheduling-affinity.yaml`

### 4. Preferred Node Affinity

Expresses a scheduling preference without making it mandatory.

* `scheduling-preferred.yaml`

## Main Concepts

```text
Taint        → Restricts which Pods can run on a Node

Toleration   → Allows a Pod to tolerate a Node's Taint

nodeSelector → Selects Nodes using labels

nodeAffinity → Provides more flexible Node selection rules
```

## Notes

These examples are for learning and demonstration purposes.

They are not required for the main Games Legend application deployment.

