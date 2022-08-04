ENVIRONMENT SPAWNING
CREATE CLUSTER USING KUBEADM
KUBEADM :
Kubeadm automates the installation and configuration of Kubernetes components such as the API server, Controller Manager, and Kube DNS. It does not, however, create users or handle the installation of operating-system-level dependencies and their configuration.

Prerequisites :
Server Configurations
To install Kubernetes, we use three servers with the following system specifications.

Master server - 4 CPU and 4096 RAM Worker1 server - 2 CPU and 4096 RAM Worker2 server - 2 CPU and 4096 RAM

All servers use Ubuntu 18.04 and each uses the following IP and DNS.

Master server - IP-Address Worker1 server - IP-Address Worker2 server - IP-Address

You have installed docker on your machine.

Installation :
Click Here to Install

Now let's how we create a cluster using kubeadm :
Prerequisites for creating cluster :
An SSH key pair on your local Linux/macOS/BSD machine. If you haven’t used SSH keys before, you can learn how to set them up by following this explanation of how to set up SSH keys on your local machine.

https://www.digitalocean.com/community/tutorials/ssh-essentials-working-with-ssh-servers-clients-and-keys#generating-and-working-with-ssh-keys

Three servers running Ubuntu 20.04 with at least 2GB RAM and 2 vCPUs each. You should be able to SSH into each server as the root user with your SSH key pair.

Make sure ansible is installed on your system. You can refer below link to install ansible.

Click Here: https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-the-control-machine

Steps for creating cluster :
Firstly you setup three instance on your aws cloud name as :

-worker-1 -worker-2 -control-plane

Step 1 — Setting Up the Workspace Directory and Ansible Inventory File

mkdir ~/kube-cluster cd ~/kube-cluster

This directory will be your workspace for the rest of the tutorial and will contain all of your Ansible playbooks. It will also be the directory inside which you will run all local commands.

Create a file named ~/kube-cluster/hosts using nano or your favorite text editor:

nano ~/kube-cluster/hosts

Add the following text to the file, which will specify information about the logical structure of your cluster:

Click Here to hosts

Step 2 — Creating a Non-Root User on All Remote Servers

Create a file named ~/kube-cluster/playbook.yml in the workspace :

nano ~/kube-cluster/playbook.yml

Click Here to Playbook

Next, run the playbook locally:

ansible-playbook -i hosts ~/kube-cluster/initial.yml

Step 3 — Installing Kubernetetes’ Dependencies

Create a file named ~/kube-cluster/kube-dependencies.yml in the workspace:

nano ~/kube-cluster/kube-dependencies.yaml

Click Here to Playbook

Next, run the playbook locally with the following command:

ansible-playbook -i hosts ~/kube-cluster/kube-dependencies.yaml

Step 4 — Setting Up the Control Plane Node

Create an Ansible playbook named control-plane.yml on your local machine:

nano ~/kube-cluster/control-plane.yaml

Click Here to Playbook

Run the playbook locally with the following command:

ansible-playbook -i hosts ~/kube-cluster/control-plane.yaml

To check the status of the control plane node, SSH into it with the following command :

ssh ubuntu@control_plane_ip

Once inside the control plane node, execute:

kubectl get nodes

You will now see the following output:

OUTPUT

Step 5 — Setting Up the Worker Nodes

Navigate back to your workspace and create a playbook named workers.yaml:

nano ~/kube-cluster/workers.yaml

Click Here to Playbook

Run the playbook by locally with the following command:

ansible-playbook -i hosts ~/kube-cluster/workers.yaml

Step 6 — Verifying the Cluster

ssh ubuntu@control_plane_ip

Then execute the following command to get the status of the cluster:

kubectl get nodes

You will see output similar to the following:

OUTPUT

Now we see the cluster info :
run following command :

kubectl cluster-info

OUTPUT

Footer
