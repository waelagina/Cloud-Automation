# **Ansible Playbooks for Automating Workload Migration to On-Premises OpenShift Clusters**

This repository contains a collection of **Ansible playbooks** designed to automate the migration of workloads from public clouds (AWS, Azure, GCP) to on-premises **OpenShift clusters**. The solution leverages **KVM virtualization** and integrates with **Tekton CI/CD pipelines** to monitor resource utilization, provision worker nodes, and migrate workloads seamlessly.

---

## **Overview**

The primary goal of this project is to help organizations optimize their cloud spending by migrating non-critical, long-running workloads from public clouds to on-premises infrastructure. By automating the provisioning of KVM-based worker nodes and integrating them into OpenShift clusters, this solution maximizes the utilization of underutilized physical servers or VMware-hosted VMs.

Key features:
- **Automated KVM Setup**: Provisions KVM virtualization on physical servers or VMware-hosted VMs with nested virtualization enabled.
- **OpenShift Worker Node Provisioning**: Dynamically creates and configures worker nodes for OpenShift clusters.
- **Workload Migration**: Uses tools like **Velero** to back up and restore workloads from public clouds to on-premises OpenShift clusters.
- **Resource Monitoring**: Integrates with Tekton CI/CD pipelines to monitor on-premises resource utilization and identify underutilized servers.
- **End-to-End Automation**: Fully automates the process from identifying resources to provisioning worker nodes and migrating workloads.

---

## **Repository Structure**

The repository is organized as follows:

```
├── ansible/
│   ├── kvm_setup.yml                # Playbook to set up KVM on physical servers or VMware-hosted VMs
│   ├── create_worker_node.yml       # Playbook to create OpenShift worker nodes using KVM
│   ├── add_worker_to_openshift.yml  # Playbook to add new worker nodes to the OpenShift cluster
│   ├── migrate_workloads.yml        # Playbook to migrate workloads using Velero
│   └── vars/
│       ├── main.yml                 # Variables for KVM and OpenShift configurations
│       └── credentials.yml          # Secure credentials (add to .gitignore before use)
├── tekton/
│   ├── pipeline.yaml                # Tekton pipeline definition for resource monitoring and automation
│   └── tasks/
│       ├── monitor_resources.yaml   # Task to monitor on-premises resource utilization
│       └── trigger_ansible.yaml     # Task to trigger Ansible playbooks
├── scripts/
│   ├── approve_csrs.sh              # Script to auto-approve Certificate Signing Requests (CSRs) for OpenShift worker nodes
│   └── cleanup.sh                   # Script to clean up unused resources after migration
└── README.md                        # This file
```

---

## **Prerequisites**

Before using these playbooks, ensure the following prerequisites are met:

### **1. Ansible**
- Install Ansible on your control machine:
  ```bash
  sudo yum install ansible -y
  ```
- Ensure the Ansible user has root privileges and passwordless sudo access on target hosts.

### **2. KVM and Libvirt**
- Physical servers or VMware-hosted VMs must have KVM and `libvirt` installed.
- Enable nested virtualization if using VMware.

### **3. OpenShift Cluster**
- An existing OpenShift cluster is required.
- Generate an Ignition configuration file for new worker nodes.

### **4. Tekton Pipelines**
- Install Tekton on your CI/CD environment to enable automated workflows.

### **5. Velero**
- Install Velero to back up and restore Kubernetes resources.

---

## **Usage**

### **Step 1: Clone the Repository**
```bash
git clone https://github.com/yourusername/ansible-kvm-openshift.git
cd ansible-kvm-openshift
```

### **Step 2: Configure Variables**
Update the variables in `ansible/vars/main.yml` to match your environment:
```yaml
kvm_host: "192.168.1.10"
openshift_cluster_api: "https://api.openshift-cluster.example.com:6443"
worker_node_image: "http://local-server/rhcos-image.qcow2"
ignition_file_url: "http://local-server/ignition.config"
```

### **Step 3: Run the Playbooks**
#### **Set Up KVM**
```bash
ansible-playbook ansible/kvm_setup.yml
```

#### **Create Worker Nodes**
```bash
ansible-playbook ansible/create_worker_node.yml
```

#### **Add Worker Nodes to OpenShift**
```bash
ansible-playbook ansible/add_worker_to_openshift.yml
```

#### **Migrate Workloads**
```bash
ansible-playbook ansible/migrate_workloads.yml
```

### **Step 4: Monitor and Automate**
Deploy the Tekton pipeline to monitor resource utilization and trigger Ansible playbooks automatically:
```bash
kubectl apply -f tekton/pipeline.yaml
```

---

## **Scripts**

### **Auto-Approve CSRs**
Run the following script to approve Certificate Signing Requests (CSRs) for new worker nodes:
```bash
./scripts/approve_csrs.sh
```

### **Clean Up Resources**
Run the cleanup script to remove unused resources:
```bash
./scripts/cleanup.sh
```

---

## **Contributing**

We welcome contributions to improve this repository! If you’d like to contribute:
1. Fork the repository.
2. Create a new branch (`git checkout -b feature/your-feature`).
3. Commit your changes (`git commit -m "Add feature"`).
4. Push to the branch (`git push origin feature/your-feature`).
5. Open a pull request.



---

## **Contact**

For questions or feedback, feel free to reach out:
- Email: waelagina@gmail.com
- LinkedIn: www.linkedin.com/in/waelagina
- GitHub: https://github.com/waelagina

