---
title: "Azure virtuálisgép-méretezési csoport létrehozása a PowerShell használatával | Microsoft Docs"
description: "Azure virtuálisgép-méretezési csoport létrehozása a PowerShell használatával"
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7bb03323-8bcc-4ee4-9a3e-144ca6d644e2
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/21/2017
ms.author: adegeo
translationtype: Human Translation
ms.sourcegitcommit: 1f8e66fac5b82698525794f0486dd0432c7421a7
ms.openlocfilehash: 7286fed39839675eb960b749f3235f83e36c5e9a


---
# <a name="create-a-windows-virtual-machine-scale-set-using-azure-powershell"></a>Windowsos virtuálisgép-méretezési csoport létrehozása az Azure PowerShell-lel
Ezek a lépések behelyettesítésen alapuló megközelítéssel hoznak létre egy Azure-beli virtuálisgép-méretezési csoportot. További információ a méretezési készletekről: [Virtuálisgép-méretezési csoportok áttekintése](virtual-machine-scale-sets-overview.md).

A cikkben található lépések elvégzése nagyjából 30 percet vesz igénybe.

## <a name="step-1-install-azure-powershell"></a>1. lépés: Az Azure PowerShell telepítése
Az Azure PowerShell legfrissebb verziójának telepítésével, a kívánt előfizetés kiválasztásával és a fiókjába való bejelentkezéssel kapcsolatos információkért lásd: [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) (Az Azure PowerShell telepítése és konfigurálása).

## <a name="step-2-create-resources"></a>2. lépés: Erőforrások létrehozása
Hozza létre az új méretezési csoporthoz szükséges erőforrásokat.

### <a name="resource-group"></a>Erőforráscsoport
A virtuálisgép-méretezési csoportoknak egy erőforráscsoporton belül kell lenniük.

1. Szerezzen be egy listát az összes olyan elérhető helyről, ahol erőforrásokat helyezhet el:
   
        Get-AzureLocation | Sort Name | Select Name
2. Válassza ki az Ön számára legalkalmasabb helyet, cserélje le a **$locName** értéket a hely nevére, majd hozza létre a következő változót:
   
        $locName = "location name from the list, such as Central US"
3. Cserélje le a **$rgName** értéket a használni kívánt új erőforráscsoport nevére, majd hozza létre a következő változót: 
   
        $rgName = "resource group name"
4. Hozza létre az erőforráscsoportot:
   
        New-AzureRmResourceGroup -Name $rgName -Location $locName
   
    Ennek nagyjából a következő példához hasonlóan kell kinéznie:
   
        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/########-####-####-####-############/resourceGroups/myrg1

### <a name="virtual-network"></a>Virtuális hálózat
A méretezési csoportban található virtuális gépekhez szükséges egy virtuális hálózat.

1. Cserélje le a **$subnetName** értéket az alhálózat kívánt nevére a virtuális hálózatban, majd hozza létre a következő változót: 
   
        $subnetName = "subnet name"
2. Az alhálózat-konfiguráció létrehozása:
   
        $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
   
    A címelőtag eltérő lehet a virtuális hálózatán belül.
3. Cserélje le a **$netName** értéket a virtuális hálózat kívánt nevére, majd hozza létre a következő változót: 
   
        $netName = "virtual network name"
4. Virtuális hálózat létrehozása:
   
        $vnet = New-AzureRmVirtualNetwork -Name $netName -ResourceGroupName $rgName -Location $locName -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### <a name="configuration-of-the-scale-set"></a>A méretezési csoport konfigurálása
Minden erőforrás rendelkezésre áll a méretezési csoport konfigurálásához, így most már létrehozhatja.  

1. Cserélje le az **$ipName** értéket az IP-konfiguráció kívánt nevére, majd hozza létre a következő változót: 
   
        $ipName = "IP configuration name"
2. Az IP-konfiguráció létrehozása:
   
        $ipConfig = New-AzureRmVmssIpConfig -Name $ipName -LoadBalancerBackendAddressPoolsId $null -SubnetId $vnet.Subnets[0].Id
3. Cserélje le a **$vmssConfig** értéket a méretezésicsoport-konfiguráció kívánt nevére, majd hozza létre a következő változót:   
   
        $vmssConfig = "Scale set configuration name"
4. A méretezésicsoport-konfiguráció létrehozása:
   
        $vmss = New-AzureRmVmssConfig -Location $locName -SkuCapacity 3 -SkuName "Standard_A0" -UpgradePolicyMode "manual"
   
    Ez a példa egy három virtuális géppel létrehozott méretezési csoportot mutat be. A méretezési csoportok kapacitásával kapcsolatos információkért lásd:[Virtuálisgép-méretezési csoportok áttekintése](virtual-machine-scale-sets-overview.md). Ez a lépés a csoportban található virtuális gépek méretének (más néven SkuName) a beállítását is magában foglalja. Az igényeinek megfelelő méret megtalálásához lásd: [Virtuális gépek méretei](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
5. Adja hozzá a hálózatiadapter-konfigurációt a méretezési csoporthoz:
   
        Add-AzureRmVmssNetworkInterfaceConfiguration -VirtualMachineScaleSet $vmss -Name $vmssConfig -Primary $true -IPConfiguration $ipConfig
   
    Ennek nagyjából a következő példához hasonlóan kell kinéznie:
   
        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     :
        OverProvision         :
        Id                    :
        Name                  :
        Type                  :
        Location              : Central US
        Tags                  :

#### <a name="operating-system-profile"></a>Operációs rendszer profilja
1. Cserélje le a **$computerName** értéket a használni kívánt számítógépnév-előtagra, majd hozza létre a következő változót: 
   
        $computerName = "computer name prefix"
2. Cserélje le az **$adminName** értéket a virtuális gépeken található rendszergazdai fiók nevére, majd hozza létre a következő változót:
   
        $adminName = "administrator account name"
3. Cserélje le az **$adminPassword** értéket a fiók jelszavára, majd hozza létre a következő változót:
   
        $adminPassword = "password for administrator accounts"
4. Az operációs rendszer profiljának létrehozása:
   
        Set-AzureRmVmssOsProfile -VirtualMachineScaleSet $vmss -ComputerNamePrefix $computerName -AdminUsername $adminName -AdminPassword $adminPassword

#### <a name="storage-profile"></a>Tárolóprofil
1. Cserélje le a **$storageProfile** értéket a tárterületprofil kívánt nevére, majd hozza létre a változót:  
   
        $storageProfile = "storage profile name"
2. Hozza létre a használni kívánt rendszerképet meghatározó változókat:  
   
        $imagePublisher = "MicrosoftWindowsServer"
        $imageOffer = "WindowsServer"
        $imageSku = "2012-R2-Datacenter"
   
    További információ a használható egyéb rendszerképekről:[Azure virtuális gépek rendszerképeinek keresése és kiválasztása a Windows PowerShell és az Azure CLI használatával](../virtual-machines/virtual-machines-windows-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

3. A tárolóprofil létrehozása:
   
        Set-AzureRmVmssStorageProfile -VirtualMachineScaleSet $vmss -ImageReferencePublisher $imagePublisher -ImageReferenceOffer $imageOffer -ImageReferenceSku $imageSku -ImageReferenceVersion "latest" -OsDiskCreateOption "FromImage" -OsDiskCaching "None"  

### <a name="virtual-machine-scale-set"></a>Virtuálisgép-méretezési csoport
Végül létrehozhatja a méretezési csoportot.

1. Cserélje le a **$vmssName** értéket a virtuálisgép-méretezési csoport nevére, majd hozza létre a következő változót:
   
        $vmssName = "scale set name"
2. A méretezési csoport létrehozása:
   
        New-AzureRmVmss -ResourceGroupName $rgName -Name $vmssName -VirtualMachineScaleSet $vmss
   
    A sikeres üzembe helyezés után a következő példához hasonló tartalom látható:
   
        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     : Updating
        OverProvision         :
        Id                    : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microso
                                ft.Compute/virtualMachineScaleSets/myvmss1
        Name                  : myvmss1
        Type                  : Microsoft.Compute/virtualMachineScaleSets
        Location              : centralus
        Tags                  :

## <a name="step-3-explore-resources"></a>3. lépés: Erőforrások vizsgálata
Ezekkel az erőforrásokkal megvizsgálhatja a létrehozott virtuálisgép-méretezési csoportot:

* Azure Portal – A portál használatával elérhető korlátozott mennyiségű információ.
* [Azure Resource Explorer](https://resources.azure.com/) – Ezzel az eszközzel megtekintheti a méretezési csoport aktuális állapotát.
* Azure PowerShell – A következő paranccsal kérhet le további információkat:
  
        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
  
        Or 
  
        Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

## <a name="next-steps"></a>Következő lépések
* A létrehozott méretezési csoport kezelésével kapcsolatos információkért lásd: [Virtuálisgép-méretezési csoportban lévő virtuális gépek felügyelete](virtual-machine-scale-sets-windows-manage.md)
* Az [Automatikus méretezés és virtuálisgép-méretezési csoportok](virtual-machine-scale-sets-autoscale-overview.md) című dokumentumban foglalt információk alapján beállíthatja a méretezési csoport automatikus méretezését
* További információ a vertikális skálázásáról: [Vertikális automatikus méretezés a virtuálisgép-méretezési csoportokkal](virtual-machine-scale-sets-vertical-scale-reprovision.md)




<!--HONumber=Feb17_HO4-->


