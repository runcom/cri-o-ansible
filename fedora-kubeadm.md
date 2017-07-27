# Installing on Fedora 26

After running the playbook, follow these commands to boostrap kubernetes
via kubeadm.

1. Install kubernetes-kubeadm

        sudo dnf install kubernetes-kubeadm
1. Attempt initialization

        sudo kubeadm init
1. Start kubelet

        sudo systemctl enable kubelet.service
1. Init will complain that docker.service isn't running so init again with pre-flight checks disabled

        sudo kubeadm init --skip-preflight-checks
1. Follow setup instructions. For example:

        sudo cp /etc/kubernetes/admin.conf $HOME/
        sudo chown $(id -u):$(id -g) $HOME/admin.conf
        export KUBECONFIG=$HOME/admin.conf
1. Check nodes

        $ kubectl get nodes
        NAME                    STATUS    AGE       VERSION
        localhost.localdomain   Ready     55m       v1.6.7
1. If running a single host, make the master node schedulable

        $ kubectl taint nodes --all node-role.kubernetes.io/master-
        node "localhost.localdomain" tainted
1. Run a pod

        $ kubectl run hello-openshift --image=docker.io/openshift/hello-openshift --port=8080
        deployment "hello-openshift" created
1. Check pod

        $ kubectl get pods
        NAME                              READY     STATUS    RESTARTS   AGE
        hello-openshift-694099042-bth0z   1/1       Running   0          3m
