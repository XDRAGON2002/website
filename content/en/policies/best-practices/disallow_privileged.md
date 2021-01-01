---
type: "docs"
title: Disallow Privileged
linkTitle: Disallow Privileged
weight: 5
description: >
    Privileged containers are defined as any container where the container uid 0 is mapped to the host’s uid 0. A process within a privileged container can get unrestricted host access. With `securityContext.allowPrivilegeEscalation` enabled, a process can gain privileges from its parent.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//best-practices/disallow_privileged.yaml" target="-blank">/best-practices/disallow_privileged.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: disallow-privileged
  annotations:
    policies.kyverno.io/category: Security
    policies.kyverno.io/description: Privileged containers are defined as any 
      container where the container uid 0 is mapped to the host’s uid 0. 
      A process within a privileged container can get unrestricted host access. 
      With `securityContext.allowPrivilegeEscalation` enabled, a process can 
      gain privileges from its parent. 
spec:
  validationFailureAction: audit
  rules:
  - name: validate-privileged
    match:
      resources:
        kinds:
        - Pod
    validate:
      message: "Privileged mode is not allowed. Set privileged to false"
      pattern:
        spec:
          containers:
          - =(securityContext):
              =(privileged): false
  - name: validate-allowPrivilegeEscalation
    match:
      resources:
        kinds:
        - Pod
    validate:
      message: "Privileged mode is not allowed. Set allowPrivilegeEscalation to false"
      pattern:
        spec:
          containers:
          - securityContext:
              allowPrivilegeEscalation: false


```