PS D:\pet\mate_academy\azure\azure_task_5_move_vm_to_new_region\azure_task_5_move_vm_to_new_region> Connect-AzAccount                                        
Please select the account you want to login with.

Retrieving subscriptions for the selection...

Subscription name Tenant
----------------- ------
Free              Каталог по умолчанию

PS D:\pet\mate_academy\azure\azure_task_5_move_vm_to_new_region\azure_task_5_move_vm_to_new_region> New-AzResourceGroup -Name mate-azure-task-5 -Location "UK West"

Confirm
Provided resource group already exists. Are you sure you want to update it?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y

ResourceGroupName : mate-azure-task-5
Location          : ukwest
ProvisioningState : Succeeded
Tags              : 
ResourceId        : /subscriptions/7c43b80f-286d-4af9-9c5f-c34a65078107/resourceGroups/mate-azure-task-5
