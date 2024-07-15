# Install OCP with Assisted Installer

This lab will walk through the steps to install a cluster using the assisted installer agent entirely on prem.

## Steps

### Create the agent boot iso
1. Open your web browser and navigate to https://{hostname from instructor}:6080
1. Click on the "Connect" button underneath the noVNC logo
1. Enter the password "supersecret"
1. You will be presented with a RHEL 9 desktop that has firefox, and a termial that you can interact with your lab environment.
1. Click through any new windows that appear
1. Click the upper left of the screen button "Activities"
1. Click on the terminal icon in the bottom center of the screen.
1. Create the ignition iso that the "baremetal" vms will boot to install a cluster
1. Review the install-config.yaml
1. Execute `cat ./my-cluster/install-config.yaml`
1. Review the agent-config.yaml
1. Execute `cat ./my-cluster/agent-config.yaml`
1. Execute `openshift-install agent create image --dir=./my-cluster`
1. Copy the iso to the vmhosts directory that the vms are preconfigured to first boot from
1. Execute `cp my-cluster/agent.x86_64.iso /vmhosts/`

### Install the Cluster

1. Click on the upper button "Activities"
1. Click on the Firefox button on the bottom center of the screen
1. Navigate the Firefox browser to http://127.0.0.1:9090
1. Login to the console with username ec2-user and password supersecret
1. Click on the limited access button to the left of help in the top center of the screen to grant adminstrative access
1. Navigate on the left of the screen to the "virtual machines" tab
1. Start all of the vms for the first cluster by clicking run next to the follow
    1. master0
    1. master1
    1. master2
    1. worker0
    1. worker1

### Track the Install

#### UI
1. You will be able to monitor the master0 console by clicking on it from the virtual machines tab

#### Console
1. Switch back to the terminal
1. We will first see the logs for the agent control service
1. Execute `ssh core@192.168.122.2`
1. Execute `journalctl -f`
1. Execute `exit`
1. Setup the kubeconfig to access the cluster operators status by executing `export KUBECONFIG=/home/ec2-user/my-cluster/auth/kubeconfig`
1. See the status of the clusters `oc get clusteroperators`

# Cluster Access
1. Open the terminal and execute `cat ./my-cluster/auth/kubeadmin-password` this will be the kubeadmin password to access the console
1. Switch back to the firefox window and go to the url "https://console-openshift-console.apps.ocp.ocpbaremetal.com"
1. Enter the kubeadmin username and password from the first step

# Install Local Storage
1. Login to the console
1. Click on oeprators on the left and then operatorhub
1. In the filter by keyword enter lvm storage
1. Click on the "LVM Storage tile"
1. Click on the Install button
1. Scroll to the bottom leaving all the defaults and click on "Install"
1. Click on Installed operators on the left tab
1. Click on LVM Storage
1. Click the "Create LVMCluster" button
1. Click on "Create" button
1. Click on the storage tab on the left
1. Click on StorageClasses
1. Click on lvms-vg1
1. Click on 1 annotation
1. Click on add more
1. For the key enter "storageclass.kubernetes.io/is-default-class" and value "true"

# Install Hosted Control Planes
1. Login to the console
1. Click on oeprators on the left and then operatorhub
1. In the filter by keyword enter "advanced cluster management"
1. Click on the "Advanced Cluster Management for Kuberenetes" tile
1. Click the "Install" button in the top left
1. Scroll down and leave all the defaults
1. Click on the "Install" button
1. Click on the "View installed Operators in Namespace open-cluster-management" link
1. Click on "Advanced Cluster Management for Kubernetes"
1. Click on "Create MultiClusterHub" button
1. Leave the default details and click on the "Create" button
1. Wait some time and you will eventually see a pop up with a link "Refresh web console" click on the link
1. From the drop down at the top left of the screen click on local-cluster and select All clusters
1. You will see a pop up "Red Hat Advanced Cluster Management for Kubernetes is not ready" wait a while and click dismiss

# Configure Host Inventory
1. Click on the "Infrastructure" tab on the left
1. Click on "Host inventory"
1. Click on the "configure host inventory settings" link toward the top right of the screen
1. Keep the defaults of the pop up and click on the configure button
1. You will see a "Configuration might take a few minutes".  Wait until that goes away
1. Eventually the "Create infrastructure environment" button will be available and click on it
1. Enter a name for the infrastructure for the lab use default, and for location use default.  For everything else leave the defaults
1. For the pull secret open your terminal window and execute `cat /home/ec2-user/pullsecret.json` copy the output to the pull secret field in firefox.
1. For the ssh public key open your terminal window and execute `cat /home/ec2-user/.ssh/id_rsa.pub` copy the output to the ssh public key field.
1. Click the "Create" button
1. Click the "Add hosts" button in the upper right of the screen
1. Click the with discovery iso menu
1. Click the copy button to the right of the command to download the iso section
1. Switch to the terminal and execute the following commands
    1. `cd /vmhosts/`
    1. Paste the copied command and add ` --no-check-certificate` and run it

# Create an HPC
1. Switch back to firefox and navigate to "https://localhost:9090"
1. Click on the virtual machines on the left
1. Click on worker-2
1. Scroll down to the disks section and click eject on the cdrom row
1. No click on the "insert" button
1. Enter "/vmhosts/discovery.iso" in the custom path
1. Click on insert scroll to the top and click on "run"
1. Repeat steps 1-7 for worker-3
1. Switch back to the openshift console
1. From the menu in the upper left make sure "All clusters" is selected
1. Expand the "Infrastrucure" band on the left
1. Click on the "Host inventory" and then click on default
1. Click on "Hosts" towards the top center of the screen under default
1. Click on "Approve host" for worker2 and worker3
1. Click on the "Clusters" tab
1. Click on "Create cluster"
1. Click the "Host Inventory" tile
1. Click on the "Hosted" tile
1. For the cluster name provide "test1"
1. For the cluster set select default
1. For the basename enter "ocpbaremetal.com"
1. For the pull secret open your terminal window and execute `cat /home/ec2-user/pullsecret.json` copy the output to the pull secret field in firefox.
1. Click the "Next" button
1. Change the "Controller availability policy" to "Single replica"
1. Change the "Infrastructure availability policy" to "Single replica"
1. Leave everything else as the default on this screen and click "Next"
1. For the host address specify api.test1.ocpbaremetal.com
1. Enter 31876 for the Host port
1. For the ssh public key open your terminal window and execute `cat /home/ec2-user/.ssh/id_rsa.pub` copy the output to the ssh public key field.
1. Click on the "Next" button
1. Click the "Create" button