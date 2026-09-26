# Task 5 — Azure Resource Mover Evidence

## Resource Move

Azure Resource Mover was used to move the VM and its networking resources.

* Source region: `swedencentral`
* Target region: `denmarkeast`
* Target resource group: `mate-azure-task-5`
* VM: `mate-vm-sweden`
* VM size: `Standard_B2s_v2`

The required UK West region was initially considered, but `Standard_B2s_v2` was reported as `NotAvailableForSubscription` for this subscription in UK West. Therefore, the alternative region permitted by the task instructions was used.

## Resource Mover operation results

* Dependency resolution: `Succeeded`
* Prepare: `Succeeded`
* Initiate Move: `Succeeded`
* Commit: `Succeeded`
* Target VM location after move: `denmarkeast`
* Target resource group: `mate-azure-task-5`

## Post-move web application check

The web application was tested after the move.

* URL: `http://9.205.148.176:8080`
* HTTP status: `200 OK`
* TCP port 8080: `TcpTestSucceeded : True`

## Artifact validation

Command:

```powershell
.\scripts\validate-artifacts.ps1
```

Result:

```text
Checked if Virtual Machine exists - OK
Checked Virtual Machine location - OK
Checked if the Public IP resource exists - OK
Checked Public IP DNS label - OK
Checked if the Network Interface resource exists - OK
Checked if Public IP assigned to the VM - OK
Checked if the Network Security Group resource exists - OK
Checked if NSG has SSH network security rule configured - OK
Checked if NSG has HTTP network security rule configured - OK
Checked if the web application is running - OK
🥳 Congratulations! All tests passed!
```

## Post-move cleanup

### 1. Stop source VM

The source VM was stopped after validation.

Command:

```powershell
Stop-AzVM `
    -ResourceGroupName "mate-azure-task-2" `
    -Name "mate-vm-sweden" `
    -Force
```

Result:

```text
OperationId : 99d2825c-28f7-4771-871d-a517ec74a189
Status      : Succeeded
StartTime   : 27-Sep-26 1:01:07 AM
EndTime     : 27-Sep-26 1:01:19 AM
Error       :
```

### 2. Delete source resource group

The source resource group was deleted after the source VM was stopped.

Command:

```powershell
Remove-AzResourceGroup `
    -Name "mate-azure-task-2" `
    -Force
```

Result:

```text
True
```

A subsequent check returned no resource group:

```powershell
Get-AzResourceGroup -Name "mate-azure-task-2" -ErrorAction SilentlyContinue |
    Select-Object ResourceGroupName, Location, ProvisioningState
```

No output was returned, confirming that `mate-azure-task-2` no longer exists.

### 3. Stop moved VM

The moved VM was stopped but not deleted.

Command:

```powershell
Stop-AzVM `
    -ResourceGroupName "mate-azure-task-5" `
    -Name "mate-vm-sweden" `
    -Force
```

Result:

```text
OperationId : 9fcf3e20-895d-4bac-9c00-470344072e61
Status      : Succeeded
StartTime   : 27-Sep-26 1:05:30 AM
EndTime     : 27-Sep-26 1:06:12 AM
Error       :
```

Final VM state check:

```powershell
$vm = Get-AzVM `
    -ResourceGroupName "mate-azure-task-5" `
    -Name "mate-vm-sweden" `
    -Status

$vm.Statuses |
    Select-Object Code, DisplayStatus, Level, Time |
    Format-Table -AutoSize
```

Result:

```text
Code                        DisplayStatus          Level Time
----                        -------------          ----- ----
ProvisioningState/succeeded Provisioning succeeded  Info 26-Sep-26 11:05:41 PM
PowerState/deallocated      VM deallocated          Info
```

The moved VM remains in `mate-azure-task-5` in `denmarkeast` and was stopped/deallocated as required. It was not deleted.

## Artifacts

`artifacts.json` contains the generated `resourcesTemplate` URL.

The SAS token is valid until `2026-10-26`.
