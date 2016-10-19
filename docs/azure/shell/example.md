### Example

The following example creates a hadoop cluster with `hdp-small-default` blueprint on Standard_D3 instances with 
2X100G attached disks on `default-azure-network` network using `all-services-port` security group. You should copy 
your ssh public key file into your `cbd` working directory with name `id_rsa.pub` and paste your Azure credentials in 
the parts with `<...>` highlight.

```
credential create --AZURE --description "credential description" --name myazurecredential --subscriptionId <your Azure subscription id> --appId <your Azure application id> --tenantId <your tenant id> --password <your Azure application password> --sshKeyPath id_rsa.pub
credential select --name myazurecredential
template create --AZURE --name azuretemplate --description azure-template --instanceType Standard_D3 --volumeSize 100 
--volumeCount 2
blueprint select --name hdp-small-default
instancegroup configure --instanceGroup host_group_master_1 --nodecount 1 --templateName azuretemplate --securityGroupName all-services-port --ambariServer true
instancegroup configure --instanceGroup host_group_master_2 --nodecount 1 --templateName azuretemplate --securityGroupName all-services-port --ambariServer false
instancegroup configure --instanceGroup host_group_master_3 --nodecount 1 --templateName azuretemplate --securityGroupName all-services-port --ambariServer false
instancegroup configure --instanceGroup host_group_client_1  --nodecount 1 --templateName azuretemplate --securityGroupName all-services-port --ambariServer false
instancegroup configure --instanceGroup host_group_slave_1 --nodecount 3 --templateName azuretemplate --securityGroupName all-services-port --ambariServer false
network select --name default-azure-network
stack create --AZURE --name my-first-stack --region "West US" --wait true
cluster create --description "My first cluster" --wait true
```

**Congratulations!** Your cluster should now be up and running on this way as well. To learn more about Cloudbreak and 
provisioning, we have some [interesting insights](operations.md) for you.
