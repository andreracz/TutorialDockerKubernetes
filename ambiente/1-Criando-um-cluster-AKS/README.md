Comandos para montagem de cluster AKS e registro ACR
====================================================

Windows
-------
Para montar o ambiente, os seguinte comandos devem ser rodados no powershell, com o Azure Cli instalado:

1. Obter o id da subscrion atual: ```$subscription = (az account show | ConvertFrom-Json).id```
2. Escolha um nome único para o seu ACR: ```$acrName="meuacrunico"```
3. Escolha um nome para o seu resource group: ```$resouceGroupName="tutorial-kube"```
4. Escolha um nome para o seu cluster AKS: ```$aksName="tutorial-kube-aks"```
5. Criar resource group: ```az group create --location eastus --name $resouceGroupName```
6. Criar ACR: ```az acr create -g $resouceGroupName --name $acrName --sku basic```
7. Criar AKS: ```az aks create -g $resouceGroupName --name $aksName --node-count 1 --attach-acr $acrName --generate-ssh-keys```
8. Conectar com o AKS localmente: ```az aks get-credentials -g $resourceGroupName --name $aksName```
9. Conectar com o ACR localmente: ```az acr login --name $acrName```
10. Testar a conexão com o AKS: ```kubectl get namespace```, deverá listar os namespaces


Linux (ou WSL)
--------------
Para montar o ambiente, os seguinte comandos devem ser rodados no bash, com o Azure Cli e o jq instalado:

1. Obter o id da subscrion atual: ```SUBSCRIPTION=$(az account show | jq -r .id)```
2. Escolha um nome único para o seu ACR: ```ACRNAME=meuacrunico```
3. Escolha um nome para o seu resource group: ```RESOURCEGROUPNAME=tutorial-kube```
4. Escolha um nome para o seu cluster AKS: ```AKSNAME=tutorial-kube-aks```
5. Criar resource group: ```az group create --location eastus --name $RESOURCEGROUPNAME```
6. Criar ACR: ```az acr create -g $RESOURCEGROUPNAME --name $ACRNAME --sku basic --admin-enabled true```
7. Criar AKS: ```az aks create -g $RESOURCEGROUPNAME --name $AKSNAME --node-count 1 --attach-acr $ACRNAME --generate-ssh-keys```
8. Conectar com o AKS localmente: ```az aks get-credentials -g $RESOURCEGROUPNAME --name $AKSNAME```
9. Obter o token do ACR: ACRTOKEN=$(az acr credential show -n $ACRNAME | jq -r .passwords[0].value)
10. Logar no docker: sudo docker login $ACRNAME.azurecr.io --username $ACRNAME --password $ACRTOKEN
Conectar com o ACR localmente: ```az acr login --name $ACRNAME```
11. Testar a conexão com o AKS: ```kubectl get namespace```, deverá listar os namespaces
