Comandos para montagem de cluster AKS e registro ACR
====================================================

Para montar o ambiente, os seguinte comandos devem ser rodados no powershell, com o Azure Cli instalado:

1. Obter o id da subscrion atual: ```$subscription = (az account show | ConvertFrom-Json).id```
2. Escolha um nome único para o seu ACR: ```$acrName="meuacrunico"```
3. Escolha um nome para o seu resource group: ```$resouceGroupName="tutorial-kube"```
4. Escolha um nome para o seu cluster AKS: ```$aksName="tutorial-kube-aks"```
5. Criar resource group: ```az group create --location eastus --name $resouceGroupName```
6. Criar ACR: ```az acr create -g $resouceGroupName --name $acrName --sku basic```
7. Criar AKS: ```az aks create -g $resouceGroupName --name $aksName --node-count 1 --attach-acr $acrName```
8. Criar service principal: ```$servicePrincipal = az ad sp create-for-rbac --sdk-auth --skip-assignment```
9. Converter o Service principal em objeto para uso nos proximos comandos: ```$sp = $servicePrincipal |ConvertFrom-Json```
10. Atribuir o papel de Contributor para o Service Principal poder fazer deploy no AKS: ```az role assignment create --assignee $sp.clientId --scope /subscriptions/$subscription/resourcegroups/$resouceGroupName/providers/Microsoft.ContainerService/managedClusters/$aksName --role Contributor```
11. Atribuir o papel de AcrPush para o Service Principal poder fazer Push do Container no ACR:  ```az role assignment create --assignee $sp.clientId  --scope /subscriptions/$subscription/resourceGroups/$resourceGroupName/providers/Microsoft.ContainerRegistry/registries/$acrName --role AcrPush```
12. Conectar com o AKS localmente: ```az aks get-credentials -g $resourceGroupName --name $aksName```
13. Conectar com o ACR localmente: ```az acr login --name $acrName```
14. Testar a conexão com o AKS: ```kubectl get namespace```, deverá listar os namespaces
