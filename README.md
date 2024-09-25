## Resources created 
- vpc with 3 private an dpublic subnets
- 1 nat gateway
- Security groups for load balancer, and k8s nodes
- key pair for the nodes
- network load balancer and target group for the master nodes
- ansible hosts for master and worker nodes

### ssh to private instance through public jumpbox (bastion)
ssh -i private-key.pem -o ProxyCommand="ssh -i private-key.pem -W %h:%p ubuntu@<bastion_ublic_ip>" ubuntu@<private_ip>


### Places to modify for adding master node
1. add new master node ec2 instance resource
2. add the resource in null_resource.update_hosts
3. Add aws_lb_target_group_attachment for the new master
4. Add ansible host for new master in ansible.tf
5. Add output for the private ip of new master

Can run terraform apply which will add the new master to cluster

-------

Before removing a master resource, drain and delete the node with kubectl first
kubectl drain master3 --ignore-daemonsets
kubectl delete node master3
