---
type: "docs"
title: Validate Userid Groupid Fsgroup
linkTitle: Validate Userid Groupid Fsgroup
weight: 25
description: >
    All processes inside the pod can be made to run with specific user and groupID by setting 'runAsUser' and 'runAsGroup' respectively. 'fsGroup' can be specified to make sure any file created in the volume with have the specified groupID. These options can be used to validate the IDs used for user and group.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//other/restrict_usergroup_fsgroup_id.yaml" target="-blank">/other/restrict_usergroup_fsgroup_id.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: validate-userid-groupid-fsgroup
  annotations:
    policies.kyverno.io/category: Security Context
    policies.kyverno.io/description: All processes inside the pod can be made to run with specific user 
      and groupID by setting 'runAsUser' and 'runAsGroup' respectively. 'fsGroup' can be specified 
      to make sure any file created in the volume with have the specified groupID. These options can be 
      used to validate the IDs used for user and group.
spec:
  rules:
  - name: validate-userid
    match:
      resources:
        kinds:
        - Pod
    validate:
      message: "User ID should be 1000"
      pattern:
        spec:
          securityContext:
            runAsUser: '1000'
  - name: validate-groupid
    match:
      resources:
        kinds:
        - Pod
    validate:
      message: "Group ID should be 3000"
      pattern:
        spec:
          securityContext:
            runAsGroup: '3000'
  - name: validate-fsgroup
    match:
      resources:
        kinds:
        - Pod
    validate:
      message: "fsgroup should be 2000"
      pattern:
        spec:
          securityContext:
            fsGroup: '2000'
```