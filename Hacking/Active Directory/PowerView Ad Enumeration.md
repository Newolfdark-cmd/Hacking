
Para ver el funcionamiento de la herramienta pasamos directamente a un laboratorio de ejemplo:

Nos situamos en el directorio de PowerView ``C:\tools``

```PowerShell
#ejecutamos el siguiente comando
powershell -ep bypass
. .\PowerView.ps1

#Diferentes comandos para conseguir información del dominio actual
Get-Domain
Get-DomainSID
Get-DomainPolicy
(Get-DomainPolicy)."SystemAccess"
(Get-DomainPolicy)."kerberospolicy"
Get-DomainController
Get-DomainController -Domain SECURITY.local
Get-DomainUser
Get-DomainUser | select samaccountname, objectsid
Get-DomainUser -Identity user -Properties Displayname,MemberOf,Objectsid,useraccountcontrol | Format-list
Get-NetComputer | select name
Get-NetComputer -Domain SECURITY.local | select cn, operatingsystem 
Get-NetGroup
Get-NetGroup 'Domain Admins'
Get-NetGroupMembet "Domain Admins" | select MemberName
Get-NetGroup -userName name @ select name
Find-DomainShare -ComputerName prod.research.SECURITY.local -verbose
Find-DomainShare -ComputerName prod.research.SECURITY.local -checkshakeAccess -verbose
Get-NetGPO | select displayname
Get-NetOU | select name, distinguishedname
Get-NetDomainTrust
Get-NetForest
Get-NetForestTrust
Get-NetForestTrust -Forest tech.local
Get-NetForest -Foret tech.local
Get-NetForestDomain
Get-NetForestDomain -Forest tech.local
Get-DomaintrustMapping
Get-objectAcl -SamAccountName "users" -ResolveGUIDs
FInd-InterestingDoaminAcl -ResolveGUIDs
FInd-InterestingDoaminAcl -ResolveGUIDs | select IdentityReferenceName, objectDn, ActiveDirectoryRights
Get-NetUser -sPN | select samaccountname, servicepriuncipalname
Get-NetUser -PreauthNotRequired | select samaccountname useraccountcontrol
```