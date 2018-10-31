[![Build Status](https://travis-ci.org/cloudgear-io/azure-bigcompute.png?branch=master)](https://travis-ci.org/cloudgear-io/azure-bigcompute)

<img src="https://www.microsoft.com/favicon.ico" width="90" height="90" /> <img src="https://yepo.com.au/media/catalog/product/cache/1/thumbnail/128x128/9df78eab33525d08d6e5fb8d27136e95/a/h/ahr0cdovl2ltywdlcy5py2vjyxquyml6l2ltzy9nywxszxj5lzi2mje1njq5xzg2mdguanbn.jpg" width="90" height="90" />  <img src="http://img.informer.com/icons/png/128/3096/3096710.png" width="90" height="90" />   <img src="https://static.start.me/favicons/wczxq9siw9fnsc7hvy1a" width="90" height="90" /> <img src="https://copr.fedorainfracloud.org/static/chroot_logodir/epel.png" width="90" height="90" /> <a href="https://www.docker.com/community-edition"><img src="https://www.docker.com/sites/default/files/catkeyboard%402x-min.png" width="90" height="90" /><a /> <img src="https://pbs.twimg.com/profile_images/566244657/InfiniBandLG_reasonably_small.jpg" width="90" height="90" />



Table of Contents
=================

   * [Azure Big Compute](#azure-big-compute)
      * [License for this repository](#license)
        * [PBS Pro Master 4.0.1 AGPL-3.0 usage section for HPC](#pbs-pro-license)
        * [NVIDIA EULA for compute silent installs via this repo](#license-agreements)
		* [SLURM LICENSE AGREEMENT - GPL v2](#slurm-license-agreement---gpl-v2)
      * [MSFT OSCC](#msft-oscc)v
      * [Credits](#credits)
   * [Deploy from Portal and visualize](#deploy-from-portal-and-visualize)
   * [Single or Cluster Topology Examples with Azure CLI](#single-or-cluster-topology-examples-with-azure-cli)
      * [New Azure CLI](#new-azure-cli)
        * [HPC with RDMA over IB](#hpc-with-rdma-over-ib)
        * [GPU Computes](#gpu-computes)
           * [Ubuntu 16.04-LTS](#ubuntu-1604-lts)
	       * [CentOS 7.3](#centos-73)
	     * [Jumpboxes](#jumpboxes)
   * [GPUs for Compute](#gpus-for-compute)
      * [Try CUDA Samples and GROMACS](#try-cuda-samples-and-gromacs)
      * [Unattended NVIDIA Tesla Driver Silent Install without further reboot during provisioning via this repo](#unattended-nvidia-tesla-driver-silent-install-without-further-reboot-during-provisioning-via-this-repo)
          * [Installation of NVIDIA CUDA Toolkit during provisioning via this repo](#installation-of-nvidia-cuda-toolkit-during-provisioning-via-this-repo)
	     * [Secure installation of CUDNN during provisioning via this repo](#secure-installation-of-cudnn-during-provisioning-via-this-repo)
      * [nvidia-docker usage](#nvidia-docker-usage)
         * [DIGITS with docker runtime nvidia 2 and tensorboard within DIGITS](#digits-with-docker-runtime-nvidia-2-and-tensorboard-within-digits)
      * [License Agreements](#license-agreements)   
   * [H-Series and A9 with schedulers](#h-series-and-a9-with-schedulers)
      * [mpirun](#mpirun)
      * [IB](#ib)
      * [Torque and pbspro for CentOS-HPC Skus](#torque-and-pbspro-for-centos-hpc-skus)
   * [Optional usage with OMS](#optional-usage-with-oms)
   * [Reporting bugs](#reporting-bugs)
   * [Patches and pull requests](#patches-and-pull-requests)
   * [Region availability and Quotas for MS Azure Skus](#region-availability-and-quotas-for-ms-azure-skus)
# Azure Big Compute
**[Azure Big Compute](https://azure.microsoft.com/en-us/solutions/big-compute/)**
## License
  * Please see the [LICENSE file](https://github.com/cloudgear-io/azure-bigcompute/blob/master/LICENSE) for licensing information.
## MSFT OSCC 
  * Thought this is a headless mirror on last commit of unmaintained https://github.com/Azure/azure-bigcompute, keeping Code of Conduct guidelines, the same:
  * This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information
see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional
questions or comments.
## Credits
**This repo is inspired by [Christian Smith](https://github.com/smith1511)'s repo https://github.com/smith1511/hpc**
  * https://github.com/Azure/azure-bigcompute - unmaintained - Maintained limited here on last commit headless mirror @ https://github.com/cloudgear-io/azure-bigcompute

# Deploy from Portal and visualize
<a href="https://preview.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fcloudgear-io%2Fazure-bigcompute%2Fmaster%2Fazuredeploy.json" target="_blank">
   <img alt="Deploy to Azure" src="http://azuredeploy.net/deploybutton.png"/>
</a>

<a href="http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2Fcloudgear-io%2Fazure-bigcompute%2Fmaster%2Fclusterazuredeploy.json" target="_blank">  
<img src="http://armviz.io/visualizebutton.png"/> </a> 

For portal Deployment, the following pic might assist.

![azureportaldeploy](https://raw.githubusercontent.com/cloudgear-io/azure-bigcompute/master/azurebigcomputefinal.png)

This project is hosted at:

  * https://github.com/Azure/azure-bigcompute - unmaintained- maintained limited here initiated on last commit @ https://github.com/cloudgear-io/azure-bigcompute

For the latest version, to contribute, and for more information, please go through [this README.md](https://github.com/cloudgear-io/azure-bigcompute/blob/master/README.md).

To clone the current master (development) branch run:

```
git clone git://github.com/cloudgear-io/azure-bigcompute.git
```



## Single or Cluster Topology Examples with Azure CLI


### New Azure CLI

 <code> docker run -dti --restart=always --name=azure-cli-python azuresdk/azure-cli-python && docker exec -ti azure-cli-python bash -c "az login && bash"</code>
<code>To sign in, use a web browser to open the page https://aka.ms/devicelogin and enter the code XXXXXXXXX to authenticate. </code> 

#### HPC with RDMA over IB
	 
* HPC Cluster (each H16R) with PBSPro and no OMS with head login user "azurehpcuser" and intern user "hpcgpu" - minimum 1 head and minimum 1 worker [provided sshpublickey value is supplied below]:

	 ```sh 
	 bash-4.3# az group create -l southcentralus -n tsthpc && az group deployment create -g tsthpc -n tsthpc --template-uri https://raw.githubusercontent.com/cloudgear-io/azure-bigcompute/master/azuredeploy.json --parameters "{\"singleOrCluster\":{\"value\":\"cluster\"},\"DnsLabelPrefix\":{\"value\":\"tsthpc\"},\"AdminUserName\":{\"value\":\"azurehpcuser\"},\"SshPublicKey\":{\"value\":\"XXXXXX\"},\"ImagePublisher\":{\"value\":\"openlogic\"},\"ImageOffer\":{\"value\":\"CentOS-HPC\"},\"ImageSku\":{\"value\":\"7.4\"},\"schedulerpbsORTorqueORslurm\":{\"value\":\"pbspro\"},\"HeadandWorkerNodeSize\":{\"value\":\"Standard_H16R\"},\"WorkerNodeCount\":{\"value\": 1},\"NumDataDisks\":{\"value\":\"32\"}}" --debug
	 ``` 
* HPC Single H16R with PBSPro and no OMS with login user "azurehpcuser" and intern user "hpcgpu"- [provided sshpublickey value is supplied below]:

	 ```sh 
	 bash-4.3# az group create -l southcentralus -n tsthpc && az group deployment create -g tsthpc -n tsthpc --template-uri https://raw.githubusercontent.com/cloudgear-io/azure-bigcompute/master/azuredeploy.json --parameters "{\"singleOrCluster\":{\"value\":\"single\"},\"DnsLabelPrefix\":{\"value\":\"tsthpc\"},\"AdminUserName\":{\"value\":\"azurehpcuser\"},\"SshPublicKey\":{\"value\":\"XXXXXX\"},\"ImagePublisher\":{\"value\":\"openlogic\"},\"ImageOffer\":{\"value\":\"CentOS-HPC\"},\"ImageSku\":{\"value\":\"7.4\"},\"schedulerpbsORTorqueORslurm\":{\"value\":\"pbspro\"},\"HeadandWorkerNodeSize\":{\"value\":\"Standard_H16R\"},\"WorkerNodeCount\":{\"value\": 0},\"NumDataDisks\":{\"value\":\"32\"}}" --debug
	 ``` 

* HPC Cluster (each H16R) with PBSPro with OMS with head login user "azurehpcuser" and intern user "hpcgpu"- minimum 1 head and minimum 1 worker [provided sshpublickey value is supplied below along with oMSWorkSpaceId and oMSWorkSpaceKey]:

	 ```sh 
	 bash-4.3# az group create -l southcentralus -n tsthpc && az group deployment create -g tsthpc -n tsthpc --template-uri https://raw.githubusercontent.com/cloudgear-io/azure-bigcompute/master/azuredeploy.json --parameters "{\"singleOrCluster\":{\"value\":\"single\"},\"DnsLabelPrefix\":{\"value\":\"tsthpc\"},\"AdminUserName\":{\"value\":\"azurehpcuser\"},\"SshPublicKey\":{\"value\":\"XXXXXX\"},\"ImagePublisher\":{\"value\":\"openlogic\"},\"ImageOffer\":{\"value\":\"CentOS-HPC\"},\"ImageSku\":{\"value\":\"7.4\"},\"schedulerpbsORTorqueORslurm\":{\"value\":\"pbspro\"},\"HeadandWorkerNodeSize\":{\"value\":\"Standard_H16R\"},\"WorkerNodeCount\":{\"value\": 1},\"NumDataDisks\":{\"value\":\"32\"},\"oMSWorkSpaceId\":{\"value\": \"xxxxxxxxxx\"},\"oMSWorkSpaceKey\":{\"value\": \"xxxxxxxxx\"}}" --debug
	 ``` 

* HPC Single H16R with PBSPro with OMS with login user "azurehpcuser" and intern user "hpcgpu"-  [provided sshpublickey value is supplied below along with oMSWorkSpaceId and oMSWorkSpaceKey]:

	 ```sh
	 bash-4.3# az group create -l southcentralus -n tsthpc && az group deployment create -g tsthpc -n tsthpc --template-uri https://raw.githubusercontent.com/cloudgear-io/azure-bigcompute/master/azuredeploy.json --parameters "{\"singleOrCluster\":{\"value\":\"single\"},\"DnsLabelPrefix\":{\"value\":\"tsthpc\"},\"AdminUserName\":{\"value\":\"azurehpcuser\"},\"SshPublicKey\":{\"value\":\"XXXXXX\"},\"ImagePublisher\":{\"value\":\"openlogic\"},\"ImageOffer\":{\"value\":\"CentOS-HPC\"},\"ImageSku\":{\"value\":\"7.4\"},\"schedulerpbsORTorqueORslurm\":{\"value\":\"pbspro\"},\"HeadandWorkerNodeSize\":{\"value\":\"Standard_H16R\"},\"WorkerNodeCount\":{\"value\": 0},\"NumDataDisks\":{\"value\":\"32\"},\"oMSWorkSpaceId\":{\"value\": \"xxxxxxxxxx\"},\"oMSWorkSpaceKey\":{\"value\": \"xxxxxxxxx\"}}" --debug
	 ``` 

* HPC (each H16R) Cluster with Torque and no OMS with head login user "azurehpcuser" and intern user "hpcgpu"- minimum 1 head and minimum 1 worker [provided sshpublickey value is supplied below]:

	 ```sh
	 bash-4.3# az group create -l southcentralus -n tsthpc && az group deployment create -g tsthpc -n tsthpc --template-uri https://raw.githubusercontent.com/cloudgear-io/azure-bigcompute/master/azuredeploy.json --parameters "{\"singleOrCluster\":{\"value\":\"cluster\"},\"DnsLabelPrefix\":{\"value\":\"tsthpc\"},\"AdminUserName\":{\"value\":\"azurehpcuser\"},\"SshPublicKey\":{\"value\":\"XXXXXX\"},\"ImagePublisher\":{\"value\":\"openlogic\"},\"ImageOffer\":{\"value\":\"CentOS-HPC\"},\"ImageSku\":{\"value\":\"7.4\"},\"schedulerpbsORTorqueORslurm\":{\"value\":\"Torque\"},\"HeadandWorkerNodeSize\":{\"value\":\"Standard_H16R\"},\"WorkerNodeCount\":{\"value\": 1},\"NumDataDisks\":{\"value\":\"32\"}}" --debug
	 ``` 
* HPC Single H16R  with Torque and no OMS with login user "azurehpcuser" and intern user "hpcgpu"-  [provided sshpublickey value is supplied below]:

	 ```sh
	 bash-4.3# az group create -l southcentralus -n tsthpc && az group deployment create -g tsthpc -n tsthpc --template-uri https://raw.githubusercontent.com/cloudgear-io/azure-bigcompute/master/azuredeploy.json --parameters "{\"singleOrCluster\":{\"value\":\"single\"},\"DnsLabelPrefix\":{\"value\":\"tsthpc\"},\"AdminUserName\":{\"value\":\"azurehpcuser\"},\"SshPublicKey\":{\"value\":\"XXXXXX\"},\"ImagePublisher\":{\"value\":\"openlogic\"},\"ImageOffer\":{\"value\":\"CentOS-HPC\"},\"ImageSku\":{\"value\":\"7.4\"},\"schedulerpbsORTorqueORslurm\":{\"value\":\"Torque\"},\"HeadandWorkerNodeSize\":{\"value\":\"Standard_H16R\"},\"WorkerNodeCount\":{\"value\": 0},\"NumDataDisks\":{\"value\":\"32\"}}" --debug
	 ``` 
* HPC (each H16R) Cluster with Torque with OMS with head login user "azurehpcuser" and intern user "hpcgpu"- minimum 1 head and minimum 1 worker [provided sshpublickey value is supplied below along with oMSWorkSpaceId and oMSWorkSpaceKey]:

	 ```sh 
	 bash-4.3# az group create -l southcentralus -n tsthpc && az group deployment create -g tsthpc -n tsthpc --template-uri https://raw.githubusercontent.com/cloudgear-io/azure-bigcompute/master/azuredeploy.json --parameters "{\"singleOrCluster\":{\"value\":\"cluster\"},\"DnsLabelPrefix\":{\"value\":\"tsthpc\"},\"AdminUserName\":{\"value\":\"azurehpcuser\"},\"SshPublicKey\":{\"value\":\"XXXXXX\"},\"ImagePublisher\":{\"value\":\"openlogic\"},\"ImageOffer\":{\"value\":\"CentOS-HPC\"},\"ImageSku\":{\"value\":\"7.4\"},\"schedulerpbsORTorqueORslurm":{\"value\":\"Torque\"},\"HeadandWorkerNodeSize\":{\"value\":\"Standard_H16R\"},\"WorkerNodeCount\":{\"value\": 1},\"NumDataDisks\":{\"value\":\"32\"},\"oMSWorkSpaceId\":{\"value\": \"xxxxxxxxxx\"},\"oMSWorkSpaceKey\":{\"value\": \"xxxxxxxxx\"}}" --debug
	 ``` 
* HPC single H16R  with Torque with OMS with login user "azurehpcuser" and intern user "hpcgpu"- [provided sshpublickey value is supplied below along with oMSWorkSpaceId and oMSWorkSpaceKey]:

	 ```sh 
	 bash-4.3# az group create -l southcentralus -n tsthpc && az group deployment create -g tsthpc -n tsthpc --template-uri https://raw.githubusercontent.com/cloudgear-io/azure-bigcompute/master/azuredeploy.json --parameters "{\"singleOrCluster\":{\"value\":\"single\"},\"DnsLabelPrefix\":{\"value\":\"tsthpc\"},\"AdminUserName\":{\"value\":\"azurehpcuser\"},\"SshPublicKey\":{\"value\":\"XXXXXX\"},\"ImagePublisher\":{\"value\":\"openlogic\"},\"ImageOffer\":{\"value\":\"CentOS-HPC\"},\"ImageSku\":{\"value\":\"7.4\"},\"schedulerpbsORTorqueORslurm\":{\"value\":\"Torque\"},\"HeadandWorkerNodeSize\":{\"value\":\"Standard_H16R\"},\"WorkerNodeCount\":{\"value\": 0},\"NumDataDisks\":{\"value\":\"32\"},\"oMSWorkSpaceId\":{\"value\": \"xxxxxxxxxx\"},\"oMSWorkSpaceKey\":{\"value\": \"xxxxxxxxx\"}}" --debug
	 ``` 
* HPC (each H16R) Cluster with Slurm and no OMS with head login user "azurehpcuser" and intern user "hpcgpu"- minimum 1 head and minimum 1 worker [provided sshpublickey value is supplied below]:

	 ```sh
	 bash-4.3# az group create -l southcentralus -n tsthpc && az group deployment create -g tsthpc -n tsthpc --template-uri https://raw.githubusercontent.com/cloudgear-io/azure-bigcompute/master/azuredeploy.json --parameters "{\"singleOrCluster\":{\"value\":\"cluster\"},\"DnsLabelPrefix\":{\"value\":\"tsthpc\"},\"AdminUserName\":{\"value\":\"azurehpcuser\"},\"SshPublicKey\":{\"value\":\"XXXXXX\"},\"ImagePublisher\":{\"value\":\"openlogic\"},\"ImageOffer\":{\"value\":\"CentOS-HPC\"},\"ImageSku\":{\"value\":\"7.4\"},\"schedulerpbsORTorqueORslurm\":{\"value\":\"slurm\"},\"HeadandWorkerNodeSize\":{\"value\":\"Standard_H16R\"},\"WorkerNodeCount\":{\"value\": 1},\"NumDataDisks\":{\"value\":\"32\"}}" --debug
	 ``` 
* HPC Single H16R  with Slurm and no OMS with login user "azurehpcuser" and intern user "hpcgpu"-  [provided sshpublickey value is supplied below]:

	 ```sh
	 bash-4.3# az group create -l southcentralus -n tsthpc && az group deployment create -g tsthpc -n tsthpc --template-uri https://raw.githubusercontent.com/cloudgear-io/azure-bigcompute/master/azuredeploy.json --parameters "{\"singleOrCluster\":{\"value\":\"single\"},\"DnsLabelPrefix\":{\"value\":\"tsthpc\"},\"AdminUserName\":{\"value\":\"azurehpcuser\"},\"SshPublicKey\":{\"value\":\"XXXXXX\"},\"ImagePublisher\":{\"value\":\"openlogic\"},\"ImageOffer\":{\"value\":\"CentOS-HPC\"},\"ImageSku\":{\"value\":\"7.4\"},\"schedulerpbsORTorqueORslurm\":{\"value\":\"slurm\"},\"HeadandWorkerNodeSize\":{\"value\":\"Standard_H16R\"},\"WorkerNodeCount\":{\"value\": 0},\"NumDataDisks\":{\"value\":\"32\"}}" --debug
	 ``` 
* HPC (each H16R) Cluster with Slurm with OMS with head login user "azurehpcuser" and intern user "hpcgpu"- minimum 1 head and minimum 1 worker [provided sshpublickey value is supplied below along with oMSWorkSpaceId and oMSWorkSpaceKey]:

	 ```sh 
	 bash-4.3# az group create -l southcentralus -n tsthpc && az group deployment create -g tsthpc -n tsthpc --template-uri https://raw.githubusercontent.com/cloudgear-io/azure-bigcompute/master/azuredeploy.json --parameters "{\"singleOrCluster\":{\"value\":\"cluster\"},\"DnsLabelPrefix\":{\"value\":\"tsthpc\"},\"AdminUserName\":{\"value\":\"azurehpcuser\"},\"SshPublicKey\":{\"value\":\"XXXXXX\"},\"ImagePublisher\":{\"value\":\"openlogic\"},\"ImageOffer\":{\"value\":\"CentOS-HPC\"},\"ImageSku\":{\"value\":\"7.4\"},\"schedulerpbsORTorqueORslurm":{\"value\":\"slurm\"},\"HeadandWorkerNodeSize\":{\"value\":\"Standard_H16R\"},\"WorkerNodeCount\":{\"value\": 1},\"NumDataDisks\":{\"value\":\"32\"},\"oMSWorkSpaceId\":{\"value\": \"xxxxxxxxxx\"},\"oMSWorkSpaceKey\":{\"value\": \"xxxxxxxxx\"}}" --debug
	 ``` 
* HPC single H16R  with Slurm with OMS with login user "azurehpcuser" and intern user "hpcgpu"- [provided sshpublickey value is supplied below along with oMSWorkSpaceId and oMSWorkSpaceKey]:

	 ```sh 
	 bash-4.3# az group create -l southcentralus -n tsthpc && az group deployment create -g tsthpc -n tsthpc --template-uri https://raw.githubusercontent.com/cloudgear-io/azure-bigcompute/master/azuredeploy.json --parameters "{\"singleOrCluster\":{\"value\":\"single\"},\"DnsLabelPrefix\":{\"value\":\"tsthpc\"},\"AdminUserName\":{\"value\":\"azurehpcuser\"},\"SshPublicKey\":{\"value\":\"XXXXXX\"},\"ImagePublisher\":{\"value\":\"openlogic\"},\"ImageOffer\":{\"value\":\"CentOS-HPC\"},\"ImageSku\":{\"value\":\"7.4\"},\"schedulerpbsORTorqueORslurm\":{\"value\":\"slurm\"},\"HeadandWorkerNodeSize\":{\"value\":\"Standard_H16R\"},\"WorkerNodeCount\":{\"value\": 0},\"NumDataDisks\":{\"value\":\"32\"},\"oMSWorkSpaceId\":{\"value\": \"xxxxxxxxxx\"},\"oMSWorkSpaceKey\":{\"value\": \"xxxxxxxxx\"}}" --debug
	 ``` 

#### GPU Computes


##### Ubuntu 16.04-LTS

* Ubuntu GPU Cluster (each NC24) with no scheduler and no OMS with head login user "azuregpuuser" and intern user "gpuclususer"- minimum 1 head and minimum 1 worker [provided sshpublickey value is supplied below]:

	 ```sh 
	bash-4.3# az group create -l eastus -n tstgpu4computes && az group deployment create -g tstgpu4computes -n tstgpu4computes --template-uri https://raw.githubusercontent.com/cloudgear-io/azure-bigcompute/master/azuredeploy.json --parameters "{\"singleOrCluster\":{\"value\":\"cluster\"},\"DnsLabelPrefix\":{\"value\":\"tstgpu4computes\"},\"AdminUserName\":{\"value\":\"azuregpuuser\"},\"SshPublicKey\":{\"value\":\"XXXXXX\"},\"ImagePublisher\":{\"value\":\"Canonical\"},\"ImageOffer\":{\"value\":\"UbuntuServer\"},\"ImageSku\":{\"value\":\"16.04-LTS\"},\"HeadandWorkerNodeSize\":{\"value\":\"Standard_NC24\"},\"WorkerNodeCount\":{\"value\": 1},\"GpuHpcUserName\":{\"value\":\"gpuclususer\"},\"NumDataDisks\":{\"value\":\"32\"}}" --debug 
	``` 

* Ubuntu Single NC24 with no scheduler and no OMS  with head login user "azuregpuuser" and intern user "gpuuser"- [provided sshpublickey value is supplied below]:

	 ```sh 
	 bash-4.3# az group create -l eastus -n tstgpu4computes && az group deployment create -g tstgpu4computes -n tstgpu4computes --template-uri https://raw.githubusercontent.com/cloudgear-io/azure-bigcompute/master/azuredeploy.json --parameters "{\"singleOrCluster\":{\"value\":\"single\"},\"DnsLabelPrefix\":{\"value\":\"tstgpu4computes\"},\"AdminUserName\":{\"value\":\"azuregpuuser\"},\"SshPublicKey\":{\"value\":\"XXXXXX\"},\"ImagePublisher\":{\"value\":\"Canonical\"},\"ImageOffer\":{\"value\":\"UbuntuServer\"},\"ImageSku\":{\"value\":\"16.04-LTS\"},\"HeadandWorkerNodeSize\":{\"value\":\"Standard_NC24\"},\"WorkerNodeCount\":{\"value\": 0},\"GpuHpcUserName\":{\"value\":\"gpuuser\"},\"NumDataDisks\":{\"value\":\"32\"}}" --debug
	 ``` 

* Ubuntu GPU Cluster (each NC24) with no scheduler with OMS  with head login user "azuregpuuser" and intern user "gpuclususer"- minimum 1 head and minimum 1 worker [provided sshpublickey value is supplied below along with oMSWorkSpaceId and oMSWorkSpaceKey]:

	 ```sh  
	 bash-4.3# az group create -l eastus -n tstgpu4computes && az group deployment create -g tstgpu4computes -n tstgpu4computes --template-uri https://raw.githubusercontent.com/cloudgear-io/azure-bigcompute/master/azuredeploy.json --parameters "{\"singleOrCluster\":{\"value\":\"cluster\"},\"DnsLabelPrefix\":{\"value\":\"tstgpu4computes\"},\"AdminUserName\":{\"value\":\"azuregpuuser\"},\"SshPublicKey\":{\"value\":\"XXXXXX\"},\"ImagePublisher\":{\"value\":\"Canonical\"},\"ImageOffer\":{\"value\":\"UbuntuServer\"},\"ImageSku\":{\"value\":\"16.04-LTS\"},\"HeadandWorkerNodeSize\":{\"value\":\"Standard_NC24\"},\"WorkerNodeCount\":{\"value\": 1},\"GpuHpcUserName\":{\"value\":\"gpuclususer\"},\"NumDataDisks\":{\"value\":\"32\"},\"oMSWorkSpaceId\":{\"value\": \"xxxxxxxxxx\"},\"oMSWorkSpaceKey\":{\"value\": \"xxxxxxxxx\"}}" --debug
	 ``` 
* Ubuntu Single NC24 with no scheduler with OMS with head login user "azuregpuuser" and intern user "gpuuser"- [provided sshpublickey value is supplied below along with oMSWorkSpaceId and oMSWorkSpaceKey]:

	 ```sh  
	 bash-4.3# az group create -l eastus -n tstgpu4computes && az group deployment create -g tstgpu4computes -n tstgpu4computes --template-uri https://raw.githubusercontent.com/cloudgear-io/azure-bigcompute/master/azuredeploy.json --parameters "{\"singleOrCluster\":{\"value\":\"single\"},\"DnsLabelPrefix\":{\"value\":\"tstgpu4computes\"},\"AdminUserName\":{\"value\":\"azuregpuuser\"},\"SshPublicKey\":{\"value\":\"XXXXXX\"},\"ImagePublisher\":{\"value\":\"Canonical\"},\"ImageOffer\":{\"value\":\"UbuntuServer\"},\"ImageSku\":{\"value\":\"16.04-LTS\"},\"HeadandWorkerNodeSize\":{\"value\":\"Standard_NC24\"},\"WorkerNodeCount\":{\"value\": 0},\"GpuHpcUserName\":{\"value\":\"gpuuser\"},\"NumDataDisks\":{\"value\":\"32\"},\"oMSWorkSpaceId\":{\"value\": \"xxxxxxxxxx\"},\"oMSWorkSpaceKey\":{\"value\": \"xxxxxxxxx\"}}" --debug
	 ``` 
##### CentOS 7.3

* CentOS  GPU Cluster (each NC24) with no scheduler and no OMS with head login user "azuregpuuser" and intern user "gpuclususer"- minimum 1 head and minimum 1 worker [provided sshpublickey value is supplied below]:

	 ```sh 
	bash-4.3# az group create -l eastus -n tstgpu4computes && az group deployment create -g tstgpu4computes -n tstgpu4computes --template-uri https://raw.githubusercontent.com/cloudgear-io/azure-bigcompute/master/azuredeploy.json --parameters "{\"singleOrCluster\":{\"value\":\"cluster\"},\"DnsLabelPrefix\":{\"value\":\"tstgpu4computes\"},\"AdminUserName\":{\"value\":\"azuregpuuser\"},\"SshPublicKey\":{\"value\":\"XXXXXX\"},\"ImagePublisher\":{\"value\":\"openlogic\"},\"ImageOffer\":{\"value\":\"CentOS\"},\"ImageSku\":{\"value\":\"7.3\"},\"HeadandWorkerNodeSize\":{\"value\":\"Standard_NC24\"},\"WorkerNodeCount\":{\"value\": 1},\"GpuHpcUserName\":{\"value\":\"gpuclususer\"},\"NumDataDisks\":{\"value\":\"32\"}}" --debug 
	``` 

* CentOS Single NC24 with no scheduler and no OMS with head login user "azuregpuuser" and intern user "gpuuser"- [provided sshpublickey value is supplied below]:

	 ```sh 
	 bash-4.3# az group create -l eastus -n tstgpu4computes && az group deployment create -g tstgpu4computes -n tstgpu4computes --template-uri https://raw.githubusercontent.com/cloudgear-io/azure-bigcompute/master/azuredeploy.json --parameters "{\"singleOrCluster\":{\"value\":\"single\"},\"DnsLabelPrefix\":{\"value\":\"tstgpu4computes\"},\"AdminUserName\":{\"value\":\"azuregpuuser\"},\"SshPublicKey\":{\"value\":\"XXXXXX\"},\"ImagePublisher\":{\"value\":\"openlogic\"},\"ImageOffer\":{\"value\":\"CentOS\"},\"ImageSku\":{\"value\":\"7.3\"},\"HeadandWorkerNodeSize\":{\"value\":\"Standard_NC24\"},\"WorkerNodeCount\":{\"value\": 0},\"GpuHpcUserName\":{\"value\":\"gpuuser\"},\"NumDataDisks\":{\"value\":\"32\"}}" --debug
	 ``` 

* CentOS GPU Cluster (each NC24) with no scheduler with OMS with head login user "azuregpuuser" and intern user "gpuclususer"- minimum 1 head and minimum 1 worker [provided sshpublickey value is supplied below along with oMSWorkSpaceId and oMSWorkSpaceKey]:

	 ```sh  
	 bash-4.3# az group create -l eastus -n tstgpu4computes && az group deployment create -g tstgpu4computes -n tstgpu4computes --template-uri https://raw.githubusercontent.com/cloudgear-io/azure-bigcompute/master/azuredeploy.json --parameters "{\"singleOrCluster\":{\"value\":\"cluster\"},\"DnsLabelPrefix\":{\"value\":\"tstgpu4computes\"},\"AdminUserName\":{\"value\":\"azuregpuuser\"},\"SshPublicKey\":{\"value\":\"XXXXXX\"},\"ImagePublisher\":{\"value\":\"openlogic\"},\"ImageOffer\":{\"value\":\"CentOS\"},\"ImageSku\":{\"value\":\"7.3\"},\"HeadandWorkerNodeSize\":{\"value\":\"Standard_NC24\"},\"WorkerNodeCount\":{\"value\": 1},\"GpuHpcUserName\":{\"value\":\"gpuclususer\"},\"NumDataDisks\":{\"value\":\"32\"},\"oMSWorkSpaceId\":{\"value\": \"xxxxxxxxxx\"},\"oMSWorkSpaceKey\":{\"value\": \"xxxxxxxxx\"}}" --debug
	 ``` 
* CentOS Single NC24 with no scheduler with OMS  with head login user "azuregpuuser" and intern user "gpuuser"- [provided sshpublickey value is supplied below along with oMSWorkSpaceId and oMSWorkSpaceKey]:

	 ```sh  
	 bash-4.3# az group create -l eastus -n tstgpu4computes && az group deployment create -g tstgpu4computes -n tstgpu4computes --template-uri https://raw.githubusercontent.com/cloudgear-io/azure-bigcompute/master/azuredeploy.json --parameters "{\"singleOrCluster\":{\"value\":\"single\"},\"DnsLabelPrefix\":{\"value\":\"tstgpu4computes\"},\"AdminUserName\":{\"value\":\"azuregpuuser\"},\"SshPublicKey\":{\"value\":\"XXXXXX\"},\"ImagePublisher\":{\"value\":\"openlogic\"},\"ImageOffer\":{\"value\":\"CentOS\"},\"ImageSku\":{\"value\":\"7.3\"},\"HeadandWorkerNodeSize\":{\"value\":\"Standard_NC24\"},\"WorkerNodeCount\":{\"value\": 0},\"GpuHpcUserName\":{\"value\":\"gpuuser\"},\"NumDataDisks\":{\"value\":\"32\"},\"oMSWorkSpaceId\":{\"value\": \"xxxxxxxxxx\"},\"oMSWorkSpaceKey\":{\"value\": \"xxxxxxxxx\"}}" --debug
	 ``` 
#### Jumpboxes

It is always great to build a Linux secure shell (SSH) jumpbox.
Having a centralized location which can be used to quickly “jump” to any cluster saves a whole bunch of time. Not only that, it opens opportunities for speeding up repetitive chores during testing, deployment especially in a cloud-only environment.

This repository can be used for creating linux jumpboxes preferably Ubuntu-16.04-LTS or CentOS 7.3 as per the distro of choice.

For Linux, it is always a good idea to visit [Azure virtual machines you can use to run your Linux apps and workloads](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/sizes).

One can also create excellent grade clusters by replacing **single** with **cluster** in the template parameters.

* The following CLI example creates a CentOS 7.3 with dns name centospublic in West Europe Region of size: Standard F2s (2 cores, 4 GB memory) with local SSD and 2TB available on /data. azureuser is the admin login user with sudo privileges. the VM name is "centos73".
"azure" is the internal user again with sudo privileges and access to /data . A SSH Key needs to be provided during deployment for "azureuser". Optionally, a OMS WSID and Key may be provided. This would have non-privileged usage of latest Docker CE for CentOS, latest release of docker-compose and latest release of docker-machine.

```sh
az group create -l westeurope -n centospublicwe && az group deployment create -g centospublicwe -n centospublicwe --template-uri https://raw.githubusercontent.com/cloudgear-io/azure-bigcompute/master/azuredeploy.json --parameters "{\"singleOrCluster\":{\"value\":\"single\"},\"DnsLabelPrefix\":{\"value\":\"centospublic\"},\"AdminUserName\":{\"value\":\"azureuser\"},\"SshPublicKey\":{\"value\":\"XXXXX\"},\"ImagePublisher\":{\"value\":\"openlogic\"},\"ImageOffer\":{\"value\":\"CentOS\"},\"ImageSku\":{\"value\":\"7.3\"},\"HeadandWorkerNodeSize\":{\"value\":\"Standard_F2s\"},\"WorkerNodeCount\":{\"value\": 0},\"GpuHpcUserName\":{\"value\":\"azure\"},\"MasterVMName\":{\"value\":\"centos73\"},\"NumDataDisks\":{\"value\":\"2\"}}" --debug
```

* The following CLI example creates a Ubuntu 16.04-LTS with dns name ubuntupublic in West Europe Region of size: Standard F2s (2 cores, 4 GB memory) with local SSD and 2TB available on /data. azureuser is the admin login user with sudo privileges. the VM name is "ubuntu1604".
"azure" is the internal user again with sudo privileges and access to /data . A SSH Key needs to be provided during deployment for "azureuser". Optionally, a OMS WSID and Key may be provided. This would have non-privileged usage of latest Docker CE for Ubuntu 16.04-LTS, latest release of docker-compose and latest release of docker-machine.

```sh
az group create -l westeurope -n ubuntupublicwe && az group deployment create -g ubuntupublicwe -n ubuntupublicwe --template-uri https://raw.githubusercontent.com/cloudgear-io/azure-bigcompute/master/azuredeploy.json --parameters "{\"singleOrCluster\":{\"value\":\"single\"},\"DnsLabelPrefix\":{\"value\":\"ubuntupublic\"},\"AdminUserName\":{\"value\":\"azureuser\"},\"SshPublicKey\":{\"value\":\"XXXXX\"},\"ImagePublisher\":{\"value\":\"Canonical\"},\"ImageOffer\":{\"value\":\"UbuntuServer\"},\"ImageSku\":{\"value\":\"16.04-LTS\"},\"HeadandWorkerNodeSize\":{\"value\":\"Standard_F2s\"},\"WorkerNodeCount\":{\"value\": 0},\"GpuHpcUserName\":{\"value\":\"azure\"},\"MasterVMName\":{\"value\":\"ubuntu1604\"},\"NumDataDisks\":{\"value\":\"2\"}}" --debug
```

 
## GPUs for Compute

[Azure GPUs](http://gpu.azure.com/)

* Entry point is valid for the stated sku presently only for  specific regions of "East-US" or "Southcentral-US". Sku availability per region is [here](https://azure.microsoft.com/en-us/regions/services/#).
* gpu enablement is possible only on approval of quota for sku usage in the stated subscription. Please see this [link](https://blogs.msdn.microsoft.com/girishp/2015/09/20/increasing-core-quota-limits-in-azure/) for instructions on requesting a core quota increase. 
* NVIDIA drivers are OK for Ubuntu 16.04-LTS as well as for CentOS 7.3, both being unattended cluster as well as single install.
* Latest Secure Install of CUDA available (auto updated to 9.0) and [samples](http://developer.download.nvidia.com/compute/cuda/1.1-Beta/x86_website/samples.html) on RAID0 (/data/data default) @ NVIDIA_CUDA-9.0_Samples for Ubuntu and in /usr/local/cuda-9.0/samples for CentOS 7.3.
* One can run all [CUDA Samples](http://developer.download.nvidia.com/compute/cuda/1.1-Beta/x86_website/samples.html) across the cluster and test with latest CUDA and CUDAnn.
* Latest Docker CE both for Ubuntu and CentOS configurable each Head and all compute Nodes. - default is 17.09.x CE.
* Latest docker-compose configurable each Head and compute Nodes. 
* Latest docker-machine configurable.
* Latest new and old azure cli are in both Head and Compute nodes.
* Disk auto mounting is at /'parameter'/data.
* NFS4 is on.
* Strict ssh public key enabled.
* Nodes that share public RSA key shared can be used as direct jump boxes as <code>azuregpuuser@DNS</code>.
* Head and comp nodes work via <code>sudo su - --gpuclususer-- </code> and then direct ssh.
* Internal firewall is off.
* For M60 usage for visualizations using NVIDIA GRID 4.2 for Windows Server 2016, please visit [aka.ms/accessgpu](https://aka.ms/accessgpu)

###  Try CUDA Samples and GROMACS

* Latest Secure Install of CUDA available (auto updated to 9.0) and [samples](http://developer.download.nvidia.com/compute/cuda/1.1-Beta/x86_website/samples.html) on RAID0 (/data/data default) @ NVIDIA_CUDA-9.0_Samples for Ubuntu and in /usr/local/cuda-9.0/samples for CentOS 7.3. just a make within each would suffice post successful provisioning.
* Securely install [GROMACS](http://www.gromacs.org/About_Gromacs) via the following for GPU Usage. 
*  For **both GPU and MPI Usage** please use the following extra **<code>-DGMX_MPI=on</code>** cmake option

```sh
	yum/apt-get install -y cmake

```
   Then,

```sh
	cd /opt && \
	export GROMACS_DOWNLOAD_SUM=e9e3a41bd123b52fbcc6b32d09f8202b && export GROMACS_PKG_VERSION=2016.3 && curl -o gromacs-$GROMACS_PKG_VERSION.tar.gz -fsSL http://ftp.gromacs.org/pub/gromacs/gromacs-$GROMACS_PKG_VERSION.tar.gz && \
	echo "$GROMACS_DOWNLOAD_SUM  gromacs-$GROMACS_PKG_VERSION.tar.gz" | md5sum -c --strict - && \
	tar xfz gromacs-$GROMACS_PKG_VERSION.tar.gz && \
	cd gromacs-$GROMACS_PKG_VERSION && \
	mkdir build-gromacs && \
	cd build-gromacs && \
	cmake .. -DGMX_BUILD_OWN_FFTW=ON -DGMX_GPU=ON -DCUDA_TOOLKIT_ROOT_DIR=/usr/local/cuda-8.0 && \
	make && \
	make install && \
	export PATH=/usr/local/gromacs/bin:$PATH
```
	
Post the above gmx would be available. For further reference please visit latest [GROMACS manual](http://manual.gromacs.org/documentation/2016.3/)



### Unattended NVIDIA Tesla Driver Silent Install without further reboot during provisioning via this repo

  NVIDIA Tesla Driver Silent Install without further reboot installed via <code>azuredeploy.sh</code> in this repository for cluster or single node as follows:
  
 > :grey_exclamation:
 
 > Currently, this need not be required when using secure cuda-repo-ubuntu1604_8.0.61-1_amd64.deb for Azure NC VMs running Ubuntu Server 16.04 LTS.
 
 > **This is required  for NVIDIA Driver with DKMS (Dynamic Kernel Module Support) for driver load surviving kernel updates.**
 
#### Ubuntu 16.04-LTS

```sh 
	service lightdm stop 
	wget  http://us.download.nvidia.com/XFree86/Linux-x86_64/375.39/NVIDIA-Linux-x86_64-375.39.run&lang=us&type=Tesla
	apt-get install -y linux-image-virtual
	apt-get install -y linux-virtual-lts-xenial
	apt-get install -y linux-tools-virtual-lts-xenial linux-cloud-tools-virtual-lts-xenial
	apt-get install -y linux-tools-virtual linux-cloud-tools-virtual
	DEBIAN_FRONTEND=noninteractive apt-mark hold walinuxagent
	DEBIAN_FRONTEND=noninteractive apt-get update -y
	DEBIAN_FRONTEND=noninteractive apt-get install -y build-essential gcc gcc-multilib dkms g++ make binutils linux-headers-`uname -r` linux-headers-4.4.0-70-generic
	chmod +x NVIDIA-Linux-x86_64-375.39.run
	./NVIDIA-Linux-x86_64-375.39.run  --silent --dkms
	DEBIAN_FRONTEND=noninteractive update-initramfs -u
```

#### CentOS 7.3

```sh 
	wget http://us.download.nvidia.com/XFree86/Linux-x86_64/375.39/NVIDIA-Linux-x86_64-375.39.run&lang=us&type=Tesla
	yum clean all
	yum update -y  dkms
	yum install -y gcc make binutils gcc-c++ kernel-devel kernel-headers --disableexcludes=all
	yum -y upgrade kernel kernel-devel
	chmod +x NVIDIA-Linux-x86_64-375.39.run
	cat >>~/install_nvidiarun.sh <<EOF
	cd /var/lib/waagent/custom-script/download/0 && \
	./NVIDIA-Linux-x86_64-375.39.run --silent --dkms --install-libglvnd && \
	sed -i '$ d' /etc/rc.d/rc.local && \
	chmod -x /etc/rc.d/rc.local
	rm -rf ~/install_nvidiarun.sh
	EOF
	chmod +x install_nvidiarun.sh
	echo -ne "~/install_nvidiarun.sh" >> /etc/rc.d/rc.local
	chmod +x /etc/rc.d/rc.local
```

### Installation of NVIDIA CUDA Toolkit during provisioning via this repo
 
 Silent and Secure installation of NVIDIA CUDA Toolkit via <code>azuredeploy.sh</code> in this repository for cluster or single node.
 
#### Ubuntu 16.04-LTS
 
 ```sh
 CUDA_REPO_PKG=cuda-repo-ubuntu1604_8.0.61-1_amd64.deb
 DEBIAN_FRONTEND=noninteractive apt-mark hold walinuxagent
 export CUDA_DOWNLOAD_SUM=1f4dffe1f79061827c807e0266568731 && export CUDA_PKG_VERSION=8-0 && curl -o cuda-repo.deb -fsSL http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/${CUDA_REPO_PKG} && \
     echo "$CUDA_DOWNLOAD_SUM  cuda-repo.deb" | md5sum -c --strict - && \
     dpkg -i cuda-repo.deb && \
     rm cuda-repo.deb && \
     apt-get update -y && apt-get install -y cuda && \
     apt-get install -y nvidia-cuda-toolkit && \
 export LIBRARY_PATH=/usr/local/cuda-8.0/lib64/:${LIBRARY_PATH}  && export LIBRARY_PATH=/usr/local/cuda-8.0/lib64/stubs:${LIBRARY_PATH} && \
 export PATH=/usr/local/cuda-8.0/bin:${PATH}
 ```
 
#### CentOS 7.3
 
  ```sh
	wget http://developer.download.nvidia.com/compute/cuda/repos/rhel7/x86_64/cuda-repo-rhel7-8.0.61-1.x86_64.rpm
	rpm -i cuda-repo-rhel7-8.0.61-1.x86_64.rpm
	yum clean all
	yum install -y cuda
 ```
 
##### CUDA Samples Install

###### Ubuntu 16.04-LTS
 
 [CUDA Samples](http://docs.nvidia.com/cuda/cuda-samples/#new-features-in-cuda-toolkit-8-0)  installed via <code>azuredeploy.sh</code> in this repository cluster or single node in parameterized RAID0 location as follows for Ubuntu:
 
 ```sh
 export SHARE_DATA="/data/data"
 export SAMPLES_USER="gpuuser"
 su -c "/usr/local/cuda-8.0/bin/./cuda-install-samples-8.0.sh $SHARE_DATA" $SAMPLES_USER

 ```

###### Centos 7.3

In /usr/local/cuda-8.0/samples for CentOS 7.3. 

* Just a make within each would suffice post successful provisioning.

#### Secure installation of CUDNN during provisioning via this repo

##### Both Ubuntu 16.04-LTS and CentOS 7.3
The NVIDIA CUDA® Deep Neural Network library (cuDNN) is a GPU-accelerated library of primitives for deep neural networks. 
cuDNN provides highly tuned implementations for standard routines such as forward and backward convolution, pooling, normalization, and activation layers.
cuDNN is part of the NVIDIA Deep Learning SDK and is installed silently as follows via <code>azuredeploy.sh</code> in this repository cluster or single node.

 ```bash
    export CUDNN_DOWNLOAD_SUM=a87cb2df2e5e7cc0a05e266734e679ee1a2fadad6f06af82a76ed81a23b102c8 && curl -fsSL http://developer.download.nvidia.com/compute/redist/cudnn/v5.1/cudnn-8.0-linux-x64-v5.1.tgz -O && \
    echo "$CUDNN_DOWNLOAD_SUM  cudnn-8.0-linux-x64-v5.1.tgz" | sha256sum -c --strict - && \
    tar -xzf cudnn-8.0-linux-x64-v5.1.tgz -C /usr/local && \
    rm cudnn-8.0-linux-x64-v5.1.tgz && \
    ldconfig
  ```
  
#### nvidia-docker usage

nvidia-docker version parameterized binary installation is automated for both Ubuntu 16.04-LTS and CentOS 7.3

The latest nvidia 2.0 runtime for docker is available and auto running post cluster provisiong
##### DIGITS with docker runtime nvidia 2 and tensorboard within DIGITS
DIGITS Latest is available @ 
```sh
http://<Cluster_Public_IP>:5000
```
and Tensorboard @
```sh
http://<Cluster_Public_IP>:6006
```
##### Notes on nvidia-docker usage
Besides, Latest [Installation of NVIDIA CUDA Toolkit during provisioning via this repo](#installation-of-nvidia-cuda-toolkit-during-provisioning-via-this-repo):

nvidia-docker can be leveraged for usage of dockerized CUDA Toolkit Usage as per the test and picture below. This opens up possibilities of using "py" and "gpu" tagged images of [cntk](https://hub.docker.com/r/microsoft/cntk/tags/), [tensorflow](https://hub.docker.com/r/tensorflow/tensorflow/tags/), [theano](https://github.com/Kaixhin/dockerfiles) and [more](https://developer.nvidia.com/deep-learning-frameworks) available as nightly builds from docker hub with jupyter notebooks. Latest gitlab.com/nvidia cudnn RCs can be used for testing.

<code>sudo systemctl start nvidia-docker</code>

<code>nvidia-docker run --rm nvidia/cuda nvidia-smi</code>

![nvidiadocker](https://cloud.githubusercontent.com/assets/3028125/12213714/5b208976-b632-11e5-8406-38d379ec46aa.png)

**More Information available @ https://github.com/NVIDIA/nvidia-docker/wiki**

#### License Agreements
By provisioning via this repository, you agree to the terms of the license agreements for NVIDIA software installed silently.

##### CUDA Toolkit
To view the license for the CUDA Toolkit , [click here](http://docs.nvidia.com/cuda/eula/index.html)

##### CUDA Deep Neural Network library (cuDNN)
To view the license for cuDNN  [click here](https://developer.nvidia.com/cudnn/license_agreement)
 
## H-Series and A9 with schedulers 
 
* [Simulations with Azure Big Compute](https://simulation.azure.com/)

* H-Series Skus added
 * **[H-Series Blog](https://azure.microsoft.com/en-us/blog/availability-of-h-series-vms-in-microsoft-azure/)**

**Details**
* Entry point is valid for the stated sku presently for specific regions. Sku availability per region is [here](https://azure.microsoft.com/en-us/regions/services/#).
* Default quota is always 8 cores per region and it is possible to request quotas for the stated subscription. Please see this [link](https://blogs.msdn.microsoft.com/girishp/2015/09/20/increasing-core-quota-limits-in-azure/) for instructions on requesting a core quota increase. 
* This creates configurable number of disks with configurable size for centos-hpc A9/H16R/H16MR
Creates a Cluster with configurable number of worker nodes each with prebuilt Intel MPI and Direct RDMA for each Head and corresponding compute Nodes.
   * For CentOS-HPC imageOffer for skuName(s) are 7.1
   * Cluster Scheduler can be Torque or PBSPro.
   * __Only Intel MPI__.
   * Latest Docker CE both for Ubuntu and CentOS configurable each Head and all compute Nodes. - default is 17.03 CE.
   * Latest docker-compose configurable each Head and compute Nodes. 
   * Latest docker-machine configurable.
   * Latest new and old azure cli are in both Head and Compute nodes.
   * Disk auto mounting is at /'parameter'/data.
   * NFS4 is on.
   * Strict ssh public key enabled.
   * Nodes that share public id_rsa.pub key for admin user shared can be used as direct jump boxes as <code>azurehpcuser@DNS</code>.
   * Head and comp nodes work via <code>sudo su - --hpc user--</code> and then direct ssh.
   * msft drivers check via rpm -qa msft* or rpm -qa microsoft*
   * Internal firewalld is off.
   * WALinuxAgent disabling and manual workrounds required **ONLY for NC24R- CentOS 7.3**, presently.
     * https://github.com/LIS/lis-next/

### mpirun

All path are set automatically for key 'default' provided users like azureuser/hpc.
for root specific <code>su - root</code> is required.

<code>source /opt/intel/impi/5.1.3.223/bin64/mpivars.sh</code>

<code>mpirun -ppn 1 -n 2 -hosts headN,compn0 -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 hostname</code>  (Cluster Check)

<code>mpirun -hosts headN,compn0 -ppn --processes per node in number-- -n --number of consequtive processes-- -env I_MPI_FABRICS=dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 IMB-MPI1 pingpong</code>
(Base Pingpong stats)

### IB

<code>ls /sys/class/infiniband</code>

<code>cat /sys/class/infiniband/mlx4_0/ports/1/state</code>

<code>/etc/init.d/opensmd start</code> (if required)

<code>cat /sys/class/infiniband/mlx4_0/ports/1/rate</code>

### Torque and pbspro for CentOS-HPC Skus

**All computes would have automatic pbs_mom and head the pbs_mom and pbs_server for latest Torque or Pbspro from their respective master repos made from source during cluster provision time**. No post installation tasks are required post successful cluster deployment except if np is to be increased from 1.

check for Torque or PBSPro via
<code>pbsnodes -a</code>
* [Torque](http://www.adaptivecomputing.com/support/download-center/torque-download/)
* [PBS Pro Master](https://github.com/PBSPro/pbspro/)

#### PBS Pro License

  * [PBS Pro Open Source License Information](https://github.com/PBSPro/pbspro/blob/6733daa7c24ca65c2975908d930d43b18f21caec/src/cmds/scripts/pbs_server#L8-L35)
  * [AGPL](https://www.gnu.org/licenses/agpl-3.0.en.html)

All path are set automatically for key 'default' users like azureuser/hpc/root
for root specific <code>su - root</code> is required.

#### SLURM LICENSE AGREEMENT - GPL v2

  * [SLURM LICENSE AGREEMENT - GPL v2](https://github.com/SchedMD/slurm/blob/master/COPYING#L23)
  * [GNU General Public License, version 2](https://www.gnu.org/licenses/old-licenses/gpl-2.0.en.html)


## Optional usage with OMS

**OMS Setup is optional and the OMS Workspace Id and OMS Workspace Key can either be kept blank or populated post the steps below.**

[Create a free account for MS Azure Operational Management Suite with workspaceName](https://login.mms.microsoft.com/signin.aspx?signUp=on&ref=ms_mms)
* Provide a Name for the OMS Workspace.
* Link your Subscription to the OMS Portal.
* Depending upon the region, a Resource Group would be created in the Subscription like 'mms-weu' for 'West Europe' and the named OMS Workspace with portal details etc. would be created in the Resource Group.
* Logon to the OMS Workspace and Go to -> Settings -> 'Connected Sources'  -> 'Linux Servers' -> Obtain the Workspace ID like <code>ba1e3f33-648d-40a1-9c70-3d8920834669</code> and the 'Primary and/or Secondary Key' like <code>xkifyDr2s4L964a/Skq58ItA/M1aMnmumxmgdYliYcC2IPHBPphJgmPQrKsukSXGWtbrgkV2j1nHmU0j8I8vVQ==</code>
* Add The solutions 'Agent Health', 'Activity Log Analytics' and 'Container' Solutions from the 'Solutions Gallery' of the OMS Portal of the workspace.
* While Deploying the Template just the WorkspaceID and the Key are to be mentioned and all will be registered including all containers in any nodes of the cluster(s).
* Then one can login to https://OMSWorkspaceName.portal.mms.microsoft.com and check all containers running for single or cluster topologies and use Log Analytics and if Required perform automated backups using the corresponding Solutions for OMS.
* Further Solutions can be added like Backup from OMS Workspace.
* OMS usage is Sku/provider/imageoffer agnostic since Dockerized OMS agent would be present in all on latest tag post deployment via this repository.
 * Or if the OMS Workspace and the Machines are in the same subscription, one can just connect the Linux Node sources manually to the OMS Workspace as Data Sources.
 
## Reporting bugs

Please report bugs  by opening an issue in the [GitHub Issue Tracker](https://github.com/cloudgear-io/azure-bigcompute/issues)

## Patches and pull requests

Patches can be submitted as GitHub pull requests. If using GitHub please make sure your branch applies to the current master as a 'fast forward' merge (i.e. without creating a merge commit). Use the `git rebase` command to update your branch to the current master if necessary.

## Region availability and Quotas for MS Azure Skus

* Sku availability per region is [here](https://azure.microsoft.com/en-us/regions/services/#).
* Please see this [link](https://blogs.msdn.microsoft.com/girishp/2015/09/20/increasing-core-quota-limits-in-azure/) for instructions on requesting a core quota increase.
* For more information on Azure subscription and service limits, quota, and constraints, please see [here](https://azure.microsoft.com/en-us/documentation/articles/azure-subscription-service-limits/).
