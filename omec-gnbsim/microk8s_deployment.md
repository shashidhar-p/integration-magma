# Quick guide to deploy Omec Gnbsim using microk8s

## Prerequisites:
### I. Microk8s:(ref:  https://microk8s.io/docs/getting-started )
1.  Install microk8s:  
    ```bash 
    sudo snap install microk8s --classic --channel=1.26
    ```
2. Join the group
   ```bash
   sudo usermod -a -G microk8s $USER
   ```
   ```bash
   sudo chown -f -R $USER ~/.kube
   ```
   ```bash
   newgrp microk8s
   ```
3. Check the status
   ```bash
   microk8s status --wait-ready
   ```
4. Add Alias 
   ```bash
   sudo vim ~/.bashrc
   ```
   Add this at the end of file ~/.bashrc
   ```bash
   alias kubectl='microk8s kubectl'
   ```
   ```bash
   source ~/.bashrc
   ```
### II. Multus CNI plugin: 
1. Clone the Multus CNI repo:
   ```bash
   git clone https://github.com/k8snetworkplumbingwg/multus-cni.git && cd multus-cni
   ```
2. Apply Multus daemonset
   ```bash
   kubectl apply -f ./deployments/multus-daemonset.yml
   ```
3. Verify that you have Multus pods running:
   ```bash
   kubectl get pods --all-namespaces | grep -i multus
   ```
4. Check that Multus is running:
   ```bash
   kubectl get pods -A | grep multus
   ```


## Deployment:
1. Create Network Attachment Definition
    ```bash
    cat <<EOF | kubectl create -f -
    apiVersion: "k8s.cni.cncf.io/v1"
    kind: NetworkAttachmentDefinition
    metadata:
       name: macvlan-conf
    spec:
       config: '{
          "cniVersion": "0.3.1",
          "type": "macvlan",
          "master": "enp0s8",
          "mode": "bridge",
          "ipam": {
             "type": "host-local",
             "subnet": "192.168.60.0/24",
             "rangeStart": "192.168.60.200",
             "rangeEnd": "192.168.60.216",
             "routes": [
                 { "dst": "0.0.0.0/0" }
             ],
             "gateway": "192.168.60.1"
          }
     }'
    EOF
    ``` 
2. Verify the configuration
   ```bash
   kubectl get network-attachment-definitions
   ```
   ```bash
   kubectl describe network-attachment-definitions macvlan-conf
   ```
3. Deploy sample applications with network attachment 
    ```bash
    cat <<EOF | kubectl apply -f - 
    apiVersion: v1
    kind: Pod
    metadata:
      name: app1
      annotations:
        k8s.v1.cni.cncf.io/networks: macvlan-conf
    spec:
      containers:
      - name: app1
        command: ["/bin/sh", "-c", "trap : TERM INT; sleep infinity & wait"]
        image: alpine
    EOF
    ```
4. Verify that the additional interfaces were created on these application pods using the defined network attachment:
   ```bash
   kubectl exec -it app1 -- ip a
   ```
 
- - -
<br/>


