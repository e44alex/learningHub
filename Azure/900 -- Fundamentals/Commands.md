create VM
```Bash
az vm create \
  --resource-group learn-50e8707f-645c-4dac-9dcf-b22c6731beac \
  --name my-vm \
  --public-ip-sku Standard \
  --image Ubuntu2204 \
  --admin-username azureuser \
  --generate-ssh-keys
```

Setup Nginx
```Bash
az vm extension set \
  --resource-group learn-50e8707f-645c-4dac-9dcf-b22c6731beac \
  --vm-name my-vm \
  --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --version 2.1 \
  --settings '{"fileUris":["https://raw.githubusercontent.com/MicrosoftDocs/mslearn-welcome-to-azure/master/configure-nginx.sh"]}' \
  --protected-settings '{"commandToExecute": "./configure-nginx.sh"}'
```

show list of Network Security Groups
```Bash
az network nsg list \
  --resource-group learn-50e8707f-645c-4dac-9dcf-b22c6731beac \
  --query '[].name' \
  --output tsv
```

Show list of NSG rules

```Bash
az network nsg rule list \
  --resource-group learn-50e8707f-645c-4dac-9dcf-b22c6731beac \
  --nsg-name my-vmNSG \
  --query '[].{Name:name, Priority:priority, Port:destinationPortRange, Access:access}' \
  --output table
```

Create NSG rule
```Bash
az network nsg rule list \
  --resource-group learn-50e8707f-645c-4dac-9dcf-b22c6731beac \
  --nsg-name my-vmNSG \
  --query '[].{Name:name, Priority:priority, Port:destinationPortRange, Access:access}' \
  --output table
```