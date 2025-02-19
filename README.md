# my-k8s-cheatsheet
 This Cheatsheet servers the purpose to support Arthur Gassmann by the most important cmds and tricks for K8s tasks 

# Kubectl cmds

- Set alias for kubectl 
```alias k=kubectl```
- Get specific information from kubectl describe with grep ```k describe nodes node01 | grep Taints```
-  Save existing Pod as YAML ```kubectl get pod webapp -o yaml >&nbsp;my-new-pod.yaml```
- Run Pod with certain image and a command and save it as static pod ```kubectl run --restart=Never --image=busybox static-busybox --dry-run=client -o yaml --command -- sleep 1000 > /etc/kubernetes/manifests/static-busybox.yaml```
- Show the latest events which happened on K8s ```kubectl get events -o wide```
- Show enabled admission plugins ```kube-apiserver -h | grep enable-admission-plugins```
- Usage of kubectl exec in kubeadm for admission plugins ```kubectl exec kube-apiserver-controlplane -n kube-system -- kube-apiserver -h | grep enable-admission-plugins```
- Show the process of admission-plugins ```ps -ef | grep kube-apiserver | grep admission-plugins```
- Show most important metrics on node ```kubectl top node```
- Show application logs from pod ```kubectl logs -f podname [containerName]```

# Important Kubernetes Documentation links
- https://kubernetes.io/docs/tasks/configure-pod-container/assign-pods-nodes-using-node-affinity/
- https://kubernetes.io/docs/reference/kubectl/generated/kubectl_label/
- https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/
- https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/
- https://github.com/kodekloudhub/certified-kubernetes-administrator-course
- https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands
- https://kubernetes.io/docs/reference/kubectl/generated/kubectl_api-resources/
- https://kubernetes.io/docs/tasks/manage-kubernetes-objects/imperative-command/
- https://kubernetes.io/docs/reference/kubectl/conventions/
- https://kubernetes.io/docs/reference/kubectl/quick-reference/ 
- Limit Resources
-- https://kubernetes.io/docs/concepts/policy/limit-range/
-- https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/
-- https://kubernetes.io/docs/concepts/policy/resource-quotas/ 
- https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/ 
- (First check Kubelet.service if path is there pod-manifest, if not check the kubeconfig.yaml usually here: /var/lib/kubelet/config.yaml) https://kubernetes.io/docs/tasks/configure-pod-container/static-pod/
- Scheduler:
-- https://kubernetes.io/docs/tasks/extend-kubernetes/configure-multiple-schedulers/
-- https://github.com/kubernetes/community/blob/master/contributors/devel/sig-scheduling/scheduling_code_hierarchy_overview.md
-- https://kubernetes.io/blog/2017/03/advanced-scheduling-in-kubernetes/
-- https://jvns.ca/blog/2017/07/27/how-does-the-kubernetes-scheduler-work/
-- https://stackoverflow.com/questions/28857993/how-does-kubernetes-scheduler-work
- Create an own admission controller webhook: https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/
- Deployment, Rollout and Undo https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
- Run Args and Commands in a Pod: https://kubernetes.io/docs/tasks/inject-data-application/define-command-argument-container/
- Configmaps and how to get values from it: https://kubernetes.io/docs/concepts/configuration/configmap/