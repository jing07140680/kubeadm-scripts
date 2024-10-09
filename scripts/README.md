1. install nvidia driver and toolkit and  NVIDIA Container Toolkit:
https://github.com/jing07140680/wyjML/tree/main/cedt
# Set up the stable repository
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get update
sudo apt-get install -y nvidia-docker2
sudo systemctl restart docker
sudo docker run --gpus all -it nvidia/cuda:11.8.0-cudnn8-devel-ubuntu22.04 nvidia-smi


2. install k8s and crio:
https://github.com/jing07140680/kubeadm-scripts
all nodes: script/common.sh
master: script/common.sh + script/master.sh
3. install k8s-device-plugin
make + run sudo ./nvidia-device-plugin on all the nodes.
4. go to master node and kubectl taint nodes <master-node-name> node-role.kubernetes.io/control-plane:NoSchedule- to allow master node using pod
5. apply mainfests where load the container
sudo kubectl get pods -n kube-system	
sudo kubectl describe pod gpu-test-9vclr -n kube-system 
sudo kubectl delete daemonsets gpu-test -n kube-system