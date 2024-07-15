# OCP Assisted Install Lab

## Provision
To provision the lab you must have an AWS account with an access key and secret key

1. Create the VMs first by executing `ansible-playbook -i ./inventory create_vms.yaml`
1. Configure the VMs with all the necessary tooling and files with `ansible-playbook -i ./inventory -i ./inventory_aws_ec2.yml configure_lab.yaml`