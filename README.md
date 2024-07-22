# P2P

## Requirements
- Ansible: =>2.17.1
- Ubuntu >=22.04 (as a target machine)

The workload has been tested running from a Mac machine running against a remote Ubuntu server,
acting as a "local" machine, due to resource constraints and Gaia node requirements."

## Usage
1. Enter the `ansible` directory:
```cd ansible```
2. Install the required Ansible Galaxy roles:
```ansible-galaxy install -r requirements.yml```  
3. Run the following to install the pre-requisites
```ansible-playbook -i $SERVER_IP -e $PATH_TO_THE_PRIV_KEY playbooks/minikube.yml```  
4. Run the following to install the ArgoCD/Kubernetes components 
```ansible-playbook -i $SERVER_IP -e $PATH_TO_THE_PRIV_KEY playbooks/kubernetes.yml```  
  
After deploying the resources above, there should be a Minikube cluster present on a machine,
with the following components running as Kubernetes resources:
- ArgoCD
- Prometheus
- Grafana
- Gaia testnet node

## Known Bugs and Issues
### Ansible
1. Some package versions installed on the server are pinned to the latest version. This is NOT considered safe.
2. Python installs pip packages insecurely. This can't be avoided (without a venv) in new Python versions.
3. Hosts file should be populated with addresses directly, rather than passing values from the command line.
4. The `minikube start --driver=docker` task is known to cause issues on occasion.`

### ArgoCD
1. ArgoCD uses Applications. These should be generated automatically from a common ApplicationSet object, or using some kind of methodology,
like application-of-applications.
2. Not a HA installation. In a production-ready system should be installed in the HA manner, possibly in a separate cluster with other tooling
services.

### Node App
1. Highly unoptimized. Not clear resource utilization set. 
2. Slow execution of the initContainer. Genesis file could be prebaked in the image itself to save on the startup time.
3. Additional app parameters should be optimized for optimal performance.

### Miscellaneous
1. For obvious reasons, minikube should never be used in production. The setup uses that underlying software for test reasons.
2. Underlying infra (excluding the requirement that the workload should be ran on the local machine), should be provisioned via IaC (such as Terraform).
3. Linting should be performed against all files. 
4. There are a lot of inconsistencies in the form the configuration was written. 
5. And many more.
