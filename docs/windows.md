<!-- markdownlint-disable -->
## Windows Managed Node groups
  Windows managed node-groups have a few pre-requisites.

  * Your cluster must contain at least one linux based worker node
  * Your EKS Cluster must have the `AmazonEKSVPCResourceController` and `AmazonEKSClusterPolicy` policies attached
  * Your cluster must have a config-map called amazon-vpc-cni with the following content
  ```yaml
  apiVersion: v1
  kind: ConfigMap
  metadata:
  name: amazon-vpc-cni
  namespace: kube-system
  data:
  enable-windows-ipam: "true"
  ```
  * It's advisable to taint your Windows nodes
  ```yaml
  kubernetes_taints = [{
    key    = "WINDOWS"
    value  = "true"
    effect = "NO_SCHEDULE"
  }]
  ```
  * Any pods that target windows will need to have the have the following attributes set in their manifest
  ```yaml
    nodeSelector:
      kubernetes.io/os: windows
      kubernetes.io/arch: amd64
  ```

https://docs.aws.amazon.com/eks/latest/userguide/windows-support.html
