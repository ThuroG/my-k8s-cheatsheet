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
- Context allows to switch easily between clusters and with the correct user ```kubectl use-context <context-name>```
- Use Context from another Konfigfile ```kubectl config use-context research --kubeconfig /path/to/ext-kube-config-file```
- Add Kubeconfig to ./bashrc file as env variable = ```vi ~/.bashrc --> export KUBECONFIG=/root/my-kube-config --> source ~/.bashrc```
- Show local API Groups ```curl http://localhost:6443 -k```
- Show the resource in API Groups ``` curl http://localhost:6443/apis -k | grep "name"```
- Check namespaced API Groups ```kubectl api-resources --namespaced=true```
- Check clusterscoped API Groups ```kubectl api-resources --namespaced=false```
- Example of line counts for kubectl output (-1 since header will also be count) ```k get clusterroles | wc -l```
- Check Kubernetes secret (token) in a pod since it is mounted: ```kubectl exec -it <podname> -- cat /var/run/secrets/kubernetes.io/serviceaccount/token```
- Decode Token with jq ```jq -R 'split(".") | select(length > 0) | .[0],.[1] | @base64d | fromjson' <<< <token>```
- Kubectl get pods in yaml output ```kubectl get po -o yaml```
- Set Service account for a deployment ```kubectl set serviceaccount deployment nginx-deployment serviceaccount1```
- Checkout Linux Capabilities on the host (or in the docker container) ```cat /usr/include/linux/capability.h```
- Add capabilities for docker run if not given ```docker run --cap-add MAC_ADMIN ubuntu```
- Drop capabilities ```docker run --cap-drop KILL ubuntu```
- Or use privileged flag to use all capabilities ```docker run --privileged ubuntu```
- Cat log from a running pod ```kubectl exec webapp -- cat /log/app.log```


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
- Set KubeConfigs correctly: https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/
- Merging KubeConfigs: https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/
- API Groups: https://kubernetes.io/docs/reference/using-api/ 
- Set Kubectl Proxy in order to communicate with the API Groups: https://kubernetes.io/docs/reference/kubectl/generated/kubectl_proxy/
- Use Rolebased Access Control: https://kubernetes.io/docs/reference/access-authn-authz/rbac/
- Check if user can perform certain actions (Can-I): https://kubernetes.io/docs/reference/kubectl/generated/kubectl_auth/kubectl_auth_can-i/ 
- Serviceaccounts in K8s: https://kubernetes.io/docs/concepts/security/service-accounts/ 
- Configure Pod to use ServiceAccount: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/
- Set Serviceaccount for a running k8s deployment object: https://kubernetes.io/docs/reference/kubectl/generated/kubectl_set/kubectl_set_serviceaccount/
- Use a private registry for kubernetes images: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/
- Securing Pods with SecurityContext: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/
- Pod Security Standards: https://kubernetes.io/docs/concepts/security/pod-security-standards/
- Use Network Policies: https://kubernetes.io/docs/concepts/services-networking/network-policies/
- Kubectx Tool to switch easily between contexts (kubectx cmd) and namespaces (kubens cmd): https://github.com/ahmetb/kubectx
- Volumes: https://kubernetes.io/docs/concepts/storage/volumes/
- Configure a pod  to use a volume as storage: https://kubernetes.io/docs/tasks/configure-pod-container/configure-volume-storage/
- Use PersistentVolumes as Storage: https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/
- PersistentVolumes: https://kubernetes.io/docs/concepts/storage/persistent-volumes/
- Difference between Volumes and PersistentVolumes (Volume is storage within the Pod while PV is outside and needs to be claimed): https://stackoverflow.com/questions/51420621/what-is-the-difference-between-a-volume-and-persistent-volume
- Persistent Volumes - Claim as volumes: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#claims-as-volumes%5C
- Storage Class: https://kubernetes.io/docs/concepts/storage/storage-classes/
- Run a shell in a container: https://kubernetes.io/docs/tasks/debug/debug-application/get-shell-running-container/ 

# CustomResourceDefinition
- Create CustomResourceDefinition: https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definitions/
- More info about CRD: https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/
- Sample Controller based on GoLang: https://github.com/kubernetes/sample-controller
- Use a K8s Operator to combine CRD and Controller: https://kubernetes.io/docs/concepts/extend-kubernetes/operator/
- Awesome Operators: https://github.com/operator-framework/awesome-operators 
- Check Operator Hub: https://operatorhub.io/ 

# SSL Certificate specific Notes
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

# Example of claiming Pod to PVC
```
apiVersion: v1
kind: Pod
metadata:
 name: mypod
spec:
 containers:
  - name: myfrontend
   image: nginx
   volumeMounts:
   - mountPath: "/var/www/html"
    name: mypd
 volumes:
  - name: mypd
   persistentVolumeClaim:
    claimName: myclaim
```
# Ingress Controller
- Use to create paths and redirection for the DNS Host
- Use one of the following tools for Ingress Controller as K8s does not have an in house solution: Nginx, HAProxy, Istio or Traefik
- Based on the chosen technology, create Ingress Resources (k8s objects)
- Check documentation of k8s: https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/
- Nginx Ingress Controller documentation: https://docs.nginx.com/nginx-ingress-controller/ 
- Imperative way to create ingress resource ```kubectl create ingress ingress-test --rule="wear.my-online-store.com/wear*=wear-service:80"**```
- Kubectl create ingress documentation: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-ingress-em-  
- Ingress documentation: https://kubernetes.io/docs/concepts/services-networking/ingress
- Path types documentation: https://kubernetes.io/docs/concepts/services-networking/ingress/#path-types
- Example of reWriting the URL Path: ```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: test-ingress
  namespace: critical-space
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - http:
      paths:
      - path: /pay
        backend:
          serviceName: pay-service
          servicePort: 8282
 ```
 - Another example of rewriting the target:```
 apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /$2
  name: rewrite
  namespace: default
spec:
  rules:
  - host: rewrite.bar.com
    http:
      paths:
      - backend:
          serviceName: http-svc
          servicePort: 80
        path: /something(/|$)(.*)
        ```
- Rewrite Target Nginx documentation: https://kubernetes.github.io/ingress-nginx/examples/rewrite/
- It is better to use Gateway as it is a kubernetes native object and will solve Ingress in future (also for UDP, GRPC etc.): https://kubernetes.io/docs/concepts/services-networking/gateway/


# Networking Basics
- Display link layer or activate a network interface ```ip link show```
- Assign Ip address to the client: ```ip addr add 192.168.1.10/24 dev eth0```
- Display routing table and gateways ```route```
- Add gateway to client (has to be done on every machines who wants to communicate) ```ip route add 192.168.2.0/24 via 192.168.1.1```
- Connect to internet
-- Add gateway to internet ```ip route add 172.217.194.0/24 via 192.168.1.1```
-- Make it the default route gateway ```ip route add default via 192.168.2.1```
- Linux does not forward packages by default. It is set to 0 in. If set to 1 it will be forwarded ```cat /proc/sys/net/ipv4/ip_forward```& ```/etc/sysctl.conf --> net.ipv4.ip_forward```
- set DNS for an IP address in etc/hosts (any name can be there and it is highest priority)```cat >> /etc/hosts```
- Use DNS Server to look it up in the resolve.conf ```/etc/resolve.conf nameserver <ipaddress>```
- If there are two same entries, the etc/host will be prioritized. It can be changed in ```cat /etc/nsswitch.conf hosts: files dns```
- Forward All to 8.8.8.8 for unknown
- Make an entry for search in /etc/resolv.conf ```search mycompany.com prod.mycompany.com```
- nslookup to check cname (it is not looking the local /etc/hosts) ```nslookup <hostname or ipaddress>```
- dig - similar but gives back more information
- K8s Documentation how networking works there: https://github.com/kubernetes/dns/blob/master/docs/specification.md
- Setting up Coredns as DNS Server: https://coredns.io/plugins/kubernetes/
- Create a network namespace (in this example red) on linux with ```ip netns add red```
- List network ns ```ip netns```
- Run command in ns with ```ip netns exec red  ip link``` is similar to ```ip -n red link```
- Link two ns is similar as link two pcs but it needs ```veth-<ns>``` as argument.
-- First you need create a link in the namespace(it represents the cable in the real world and give it the name where it shoulds connect: here it is veth-blue) ```ip link add veth-red type veth peer name veth-blue```
-- Second you need to attach it and bind it```ip link set veth-red netns red``` && ```ip link set veth-red netns blue```
- Create a switch so that you have a virtual private network with Linux Bridge (here it is called v-net-0) ```ip link add v-net-0 type bridge```
- Delete a connection with ```ip -n red link del veth-red```
- Make sure that the bridge also gets a ip address ```ip addr add 192.168.15.5/24 dev v-net-0```
Route external traffic out of the VPN with ```ip netns exec blue ip route add 192.168.1.0/24 via 192.168.15.5```
- You also need to enable NAT so that the outwork network can communicate with the VPN: ```iptables -t nat -A POSTROUTING -s 192.168.15.0/24 -j MASQUERADE``` --> it makes sure that traffic is seen as from the local host and not something unknown
- Show the network routes with ```ip netns exec blue route```
- add a Default gateway to forward traffic ```ip netns exec blue ip route add default via 192.168.15.5```here it has to be set in every ns so that it forwards the traffic to the default gateway (which is 192.168.15.5)
- add a Port forwarding role ```iptables -t nat -A PREROUTING --dport 80 --to-destination 192.168.15.2:80 -j DNAT```
- If you cannot ping from one ns to the other, make sure that NETMASK is set for the IPAddress.
- Alternatively, you could check the FirewallID/IP Table rules. Add rules to IP Tables to allow traffict from one ns to another.
- Show only the connection bridge: ```ip link show eth0```
- Show the default gateway route ```ip route show default```
- Use netstat to list all used ports by (k8s) services ```netstat -nplt```
- Check which port has how many client connections open: ```netstat -anp | grep etcd```
- run docker container with a NONE network ```docker run --network NONE nginx```
- run docker container with a port like the host ```docker run --network nging```
- use a bridge (preferred) - which is created by default (check ```docker network ls```)
- Whenever we create a container, a ns is created (check ```ip netns```)
- It automatically creates a bridge cable (check with ```ip -n <ns of docker> addr```)
- Show all IP addresses configured ```ip a```or ```ip link```
- You need port mapping so that you can bind inner port to an outer port with ```docker run -p 8080:80 nginx```
- CNI = Container network interface which is the standardization for bridge network. (bridge program)
- CNI defines how the plugin should be developed for example
-- Container Runtime must create network namespace
-- Identify network the container must attach to
-- Must support command line arguments ADD/DEL/CHECK
- Bridge comes in with already available plugins such as VLAN, IPVLAN
- Docker does not follow that standard but you could link it to a CNI bridge with crating first the docker with a NONE nework and then run the ```bridge add <container ns id> /var/run/nets/<container ns id>```
-- This is also how K8s does it (that's why they say they did not "support Docker" anymore)
- K8s has also its rules and uses bridges and predefined ports which are used by kubernetes object such as:
-- kube-api = 6443
-- kubelet = 10250
-- kube-scheduler = 10259
-- kube-controller-manager = 10257
-- etcd = 2379
-- Services port range on worker nodes (30000 - 32767)
- Check required ports documentation for troubleshooting: https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#check-required-ports
- Kubernetes Network plugins: https://kubernetes.io/docs/concepts/cluster-administration/addons/
- How to implement k8s networking model: https://kubernetes.io/docs/concepts/cluster-administration/networking/#how-to-implement-the-kubernetes-networking-model
- Every POD should have an IP Address
- Evey POD should be able to communicate with every other POD in the same node and on other nodes without NAT
- Container Runtime looks over /etc/cni/net.d/net-script.conflist and checks which comd to run from /opt/cni/bin/net-script.sh. For example ```./net-script.sh add <container> <namespace>```
- Container Network installed in ```/opt/cni/bin```
- Container runtime (which is responsible for  running a container) is installed in ```/etc/cni/net.d```
- Weave released a new CNI installation link: https://www.weave.works/blog/weave-cloud-end-of-service
- Weave install Cmnd```kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml```
- Weave Reference links: https://www.weave.works/docs/net/latest/kubernetes/kube-addon/#-installation
- https://github.com/weaveworks/weave/releases
- Weave is installed on each node with a Daemon Set and should be used for Troubleshooting if there are problems with the traffic
- Get the configured container runtime endpoint from KUBELET: ```ps -aux | grep kubelet | grep --color container-runtime-endpoint```
- IP address management (IPAM) has to be configured in ```/etc/cni/net.d/net-script.conf```
- Show default cluster ip range ```kube-api-server --service-cluster-ip-range ipNet ```
- Shwo default cluster ip range ```ps aux | grep kube-api-server```
- Show assigned ip of services in iptables ```iptables -L -t nat | grep db-service```
- Check also log of kube-proxy to see what configuration it has such as proxy type ```kubectl logs -n kube-system pod/kube-proxy-5df7v```
- Inspect the setting on kube-api server by running on command ```cat /etc/kubernetes/manifests/kube-apiserver.yaml   | grep cluster-ip-range```
- In Kubernetes, if you want to access a pod within the same namespace you can just point to the service name ```https://web-service```
- If you need to access a service from a different namespace you can access it via ```https://web-service.namespace.svc.cluster.local```
- Normally, Kubernetes does not create DNS entries for pod but you can explicit create it but it uses tha IP address (it replaces the dots to dashes. Ex: 10-240-12-0)
- Every time a pod or service is created, it saves an entry in CoreDNS. Normally called kube-dns in K8s
- CoreDNS is saved in /etc/coredns/Corefile and is also placed as configmap ```kubectl get configmap -n kube-system```

