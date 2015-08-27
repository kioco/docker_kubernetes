###This automation deployment has written following architecture in the image diagram (please inorge IP address in image diagram)
###This is apply successfull for CentOS 7
###Please open setup.yml and flannel.yml to review host master and nodes. Or you can edit on this.


Step 1: Make sure that from Ansible deployment server can access to the servers adding in inventory file by ssh key without password.

Step 2: Set up the actual kubernetes cluster and docker

    $ sudo ansible-playbook -i inventory setup.yml

Step 3: Set up flannel, a network overlay (optional)

    $ sudo ansible-playbook -i inventory flannel.yml

Again, you need to edit `group_vars/all.yml`.

Assign the entire address space which flannel should use for an overlay
network. Again, your network infrastructure does not need to know about and
should not use any addresses in this range. Pods will be assigned addresses
in this range, but the routing between pods will be over the flannel controlled
overlay network, NOT your network infrastructure.

Step 4: Verify Kubernetes cluster and flannel

On master:
    $sudo kubectl get nodes

Result command:
    10.76.0.224   kubernetes.io/hostname=10.76.0.224   Ready
    10.76.0.225   kubernetes.io/hostname=10.76.0.225   Ready


############
Deploy with different module, hosts, groups:
EX: $sudo ansible-playbook -i inventory -l minions --tags "minionshaproxy,minionskeepalived" setup.yml
