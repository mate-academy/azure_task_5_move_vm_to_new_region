\# Task 5 — Azure Resource Mover Evidence



\## Resource Move



Azure Resource Mover was used to move the VM and its networking resources.



\* Source region: `swedencentral`

\* Target region: `denmarkeast`

\* Target resource group: `mate-azure-task-5`

\* VM: `mate-vm-sweden`

\* VM size: `Standard\_B2s\_v2`



The required UK West region was initially considered, but `Standard\_B2s\_v2` was reported as `NotAvailableForSubscription` for this subscription in UK West. Therefore, the alternative region permitted by the task instructions was used.



\## Resource Mover operation results



\* Dependency resolution: `Succeeded`

\* Prepare: `Succeeded`

\* Initiate Move: `Succeeded`

\* Commit: `Succeeded`

\* Target VM location after move: `denmarkeast`

\* Target resource group: `mate-azure-task-5`



\## Post-move web application check



The web application was tested after the move.



\* URL: `http://9.205.148.176:8080`

\* HTTP status: `200 OK`

\* TCP port 8080: `TcpTestSucceeded : True`



\## Artifact validation



Command:



`.\\scripts\\validate-artifacts.ps1`



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



\## Artifacts



`artifacts.json` contains the generated `resourcesTemplate` URL.



The SAS token is valid until `2026-10-26`.



