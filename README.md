# my-k8s-cheatsheet
 This Cheatsheet servers the purpose to support Arthur Gassmann by the most important cmds and tricks for K8s tasks 

It is structured by the Certifications

# CKA Kubectl cmds

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
- Encode Secrets in Linux with base64 ```echo -n 'value' | base64```
- Decode Secrets ```echo -n 'value' | base64 --decode```
- Get a Shell to the container (in this example two containers) ```kubectl exec -it two-containers -c nginx-container -- /bin/bash```
- Force recreation when editing fails ```kubectl replace --force -f /tmp/kubectl-edit-1234.yaml```
- Set horizontal pod autoscaler with imperative command ```kubectl autoscale deployment <name> --cpu-percent=50 --min=1 --max=10```
- Drain all the pods running on a node and ignore daemonset ```kubectl drain node-01 --ignore-daemonsets```
- Make a node unschedulable but do not drain existing running pods ```kubectl cordon node01```
- Make a node schedulable again (usually after an upgrade) ```kubectl uncordon node01```
- Check the release of the OS ```cat /etc/*release* ```
- Before Cluster Update for Kubeadm, check the Package Distribution List and adapt the number ```vim /etc/apt/sources.list.d/kubernetes.list```
- Backup all K8s Objects in a declarative yaml ```kubectl get all --all-namespaces -o yaml > all.yaml```
- Inspect Service Logs ```journalctl -u etcd.service -l```
- If pods are down - go to docker and use ```docker ps aux --> docker logs <containerid>```

# CKA Important Kubernetes Documentation links
- Kubernetes API & Architecture
-- https://kubernetes.io/docs/concepts/overview/kubernetes-api/
-- https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api-conventions.md
-- https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api_changes.md
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
- Configmaps and how to get values from it: https://kubernetes.io/docs/concepts/configuration/configma:
- Secrets: https://kubernetes.io/docs/concepts/configuration/secret/ 
- Important: you need to define the secret type: https://kubernetes.io/docs/reference/kubectl/generated/kubectl_create/kubectl_create_secret_generic/ 
- Encrypt data at Rest for Secrets: https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/ 
- Secret Store CSI Driver: https://www.youtube.com/watch?v=MTnQW9MxnRI 
- Distribute Secrets securely: https://kubernetes.io/docs/tasks/inject-data-application/distribute-credentials-secure/#define-container-environment-variables-using-secret-data
- Multicontainer pod: https://kubernetes.io/docs/concepts/workloads/pods/#how-pods-manage-multiple-containers
- Editing a pod to add sidecar container: https://kubernetes.io/docs/tasks/access-application-cluster/communicate-containers-same-pod-shared-volume/
- Sidecar Containers (part of CKAD): https://kubernetes.io/docs/concepts/workloads/pods/sidecar-containers/
- Init Containers: https://kubernetes.io/docs/concepts/workloads/pods/init-containers/
- Autoscaling: https://kubernetes.io/docs/concepts/workloads/autoscaling/ 
- Using Kubeadm for upgrade cluster: https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/
- Changing the Package Distribution Version has to be done BEFORE: https://v1-31.docs.kubernetes.io/docs/tasks/administer-cluster/kubeadm/change-package-repository/ 
- Backup ETCD https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/#backing-up-an-etcd-cluster
- ETCD Github Recovery Section: https://github.com/etcd-io/website/blob/main/content/en/docs/v3.5/op-guide/recovery.md
- Disaster Recovery for Kubernetes: https://www.youtube.com/watch?v=qRPNuT080Hk
- Kubernetes Certificate Health Check Spreadsheet: https://github.com/mmumshad/kubernetes-the-hard-way/tree/master/tools
- Managing TLS in Kubernetes: https://kubernetes.io/docs/tasks/tls/managing-tls-in-a-cluster/ 
- Certificate Signing Request k8s object Creation: https://kubernetes.io/docs/tasks/tls/managing-tls-in-a-cluster/#create-a-certificatesigningrequest-object-to-send-to-the-kubernetes-api
- Theory about Certificate Signing Request: https://kubernetes.io/docs/reference/access-authn-authz/certificate-signing-requests/

# Security specific Notes
- SSL CA Certification Generation
-- Key Generation for CA: ```openssl genrsa -out ca.key 2048```
-- CA Signing Request ```openssl req -new -key ca.key -subj "/CN=KUBERNETES-CA" -out ca.csr```
-- Signing CA ```openssl x509 -req -in ca.csr -signkey ca.key -out ca.crt```
SSL Admin Certiciation Generation
-- Key generation for admin ```openssl genrsa -out admin.key 2048```
-- Signing Request (don't forget to add the group) ```openssl req -new -key admin.key -subj "/CN=kube-admin/O=system:masters" -out admin.csr```
-- Signing Admin Certificate with CA ```openssl x509 -req -in admin.csr -CA ca.csr -CAKey ca.key -out admin.crt```
- SSL Server Certificates for ETCD will be created the same way but need to be added to the etcd config. If there are peer clients, this also has to be mentioned
- Kubeapi server needs to have ALL DNS in the certificate incl. the IP Addresses in an openssl.cnf file where the DNS are listed
- Kubelet Server Certification has to be created for each node and with its node name. It has to be added to the kubelet.config then
- Decode SSL Certificates for Verification: ```openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout```