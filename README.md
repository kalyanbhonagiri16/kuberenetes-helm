# kuberenetes-helm
This repository lets you install kubernetes on linux/mac and write helm charts

Kuberenetes installation on Ubuntu kubeadm:


Master and Slave:

	$ sudo apt-get update && sudo apt-get install -y apt-transport-https
	$ sudo apt-get -y install docker.io
	$ sudo systemctl start docker
	$ sudo systemctl enable docker

	# To make sure docker is installed run this command

	$ sudo docker ps

	# Installing Kubernetes

	$ sudo curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add
	$ vi /etc/apt/sources.list.d/kubernetes.list
 		and add this line 
		"deb http://apt.kubernetes.io/ kubernetes-xenial main"
	$ sudo apt-get update 

	Master:

		$ sudo apt-get install -y kubelet kubeadm kubectl kubernetes-cni
		$ sudo kubeadm init --pod-network-cidr=192.168.0.0/16
		$ sudo mkdir -p $HOME/.kube
		$ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
		$ sudo chown $(id -u):$(id -g) $HOME/.kube/config
		
		# you can use either flannel/calico. I'm using flannel here

		$ sudo kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
    $ sudo kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/k8s-manifests/kube-flannel-rbac.yml

		# Checking if installation is successful
		$ sudo kubectl get pods --all-namespaces # this should show all pods as running
		$ sudo kubectl get nodes # this will show your host as master

		# to reset and try again
		$ sudo kubeadm reset # and repeat the commands for master

	Slave:
		$ sudo apt-get install -y kubelet kubectl kubernetes-cni
		$ sudo kubeadm join --token TOKEN MASTER_IP:6443 

		# you can get the token from master by this command
		$ sudo kubeadm token create --print-join-command  



	$ sudo 	kubectl get nodes # this command on master should show that the slave is added

	# by default master nodes is tainted, if you want to let pods get deployed on master
	$ kubectl taint nodes --all node-role.kubernetes.io/master-	
