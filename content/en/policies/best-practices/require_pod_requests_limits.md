---
type: "docs"
title: Require Pod Requests Limits
linkTitle: Require Pod Requests Limits
weight: 15
description: >
    As application workloads share cluster resources, it is important to limit resources requested and consumed by each pod. It is recommended to require 'resources.requests' and 'resources.limits.memory' per pod. If a namespace level request or limit is specified, defaults will automatically be applied to each pod based on the 'LimitRange' configuration.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//best-practices/require_pod_requests_limits.yaml" target="-blank">/best-practices/require_pod_requests_limits.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: require-pod-requests-limits
  annotations:
    policies.kyverno.io/category: Workload Management
    policies.kyverno.io/description: As application workloads share cluster resources, it is important 
      to limit resources requested and consumed by each pod. It is recommended to require 
      'resources.requests' and 'resources.limits.memory' per pod. If a namespace level request or limit is 
      specified, defaults will automatically be applied to each pod based on the 'LimitRange' configuration.
spec:
  validationFailureAction: audit
  rules:
  - name: validate-resources
    match:
      resources:
        kinds:
        - Pod
    validate:
      message: "CPU and memory resource requests and limits are required"
      pattern:
        spec:
          containers:
          - resources:
              requests:
                memory: "?*"
                cpu: "?*"
              limits:
                memory: "?*"
```