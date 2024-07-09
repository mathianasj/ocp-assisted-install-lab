# Install OCP with Assisted Installer

This lab will walk through the steps to install a cluster using the assisted installer agent entirely on prem.

## Steps

### Create the agent boot iso
1. Open your web browser and navigate to http://{ip from instructor}:6080
1. Click on the "Connect" button underneath the noVNC logo
1. Enter the password "supersecret"
1. You will be presented with a RHEL 9 desktop that has firefox, and a termial that you can interact with your lab environment.
1. Click through any new windows that appear
1. Click the upper left of the screen button "Activities"
1. Click on the terminal icon in the bottom center of the screen.
1. Create the ignition iso that the "baremetal" vms will boot to install a cluster
1. Execute `openshift-install agent create image --dir=./my-cluster`
1. Copy the iso to the vmhosts directory that the vms are preconfigured to first boot from
1. Execute `cp my-cluster/agent.x86_64.iso /vmhosts/`

### Install the Cluster

1. Click on the upper button "Activities"
1. Click on the Firefox button on the bottom center of the screen
1. Navigate the Firefox browser to http://127.0.0.1:9090
1. Login to the console with username ec2-user and password supersecret
1. Navigate on the left of the screen to the "virtual machines" tab
1. Start all of the vms provided

### Track the Install

#### UI
1. You will be able to monitor the master0 console by clicking on it from the virtual machines tab

#### Console
1. Switch back to the terminal
