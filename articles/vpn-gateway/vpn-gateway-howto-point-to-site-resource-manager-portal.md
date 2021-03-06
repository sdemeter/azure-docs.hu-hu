---
title: "Számítógép csatlakoztatása Azure-beli virtuális hálózathoz pont–hely kapcsolat használatával: Portal | Microsoft Docs"
description: "A Resource Managerrel és az Azure Portallal létrehozott pont–hely VPN Gateway-kapcsolattal biztonságosan csatlakozhat az Azure Virtual Networkhöz."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a15ad327-e236-461f-a18e-6dbedbf74943
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/17/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: cf72197aba2c6e6c7a51f96d1161cf1fbe88a0c5
ms.openlocfilehash: 61ef9523739ebd8bd105c6faccf42405c7c62567


---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal"></a>Pont–hely kapcsolat konfigurálása virtuális hálózat számára az Azure Portalon
> [!div class="op_single_selector"]
> * [Resource Manager – Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Resource Manager – PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Klasszikus – Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 

A pont–hely (P2S) konfiguráció lehetővé teszi biztonságos kapcsolat létesítését a virtuális hálózattal egy különálló ügyfélszámítógépről. A pont–hely kapcsolat akkor hasznos, ha távoli helyről szeretne csatlakozni a virtuális hálózathoz, például otthonról vagy konferenciáról, vagy akkor, ha csak néhány ügyfelet kíván csatlakoztatni a virtuális hálózathoz. 

A pont–hely kapcsolatok nem igényelnek VPN-eszközt vagy nyilvános IP-címet a működéshez. VPN-kapcsolat létesítéséhez manuálisan kell kezdeményezni a kapcsolatot az ügyfélszámítógépről. A Pont–hely kapcsolatokról további információt a cikk végén, a [Pont–hely kapcsolatok – gyakori kérdések](#faq) című részben talál.

Ez a cikk lépésről lépésre bemutatja, hogyan hozható létre virtuális hálózat pont–hely kapcsolattal az Azure Portalon. Ezek a lépések a Resource Manager-alapú üzemi modellre vonatkoznak.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Üzemi modellek és módszerek a pont–hely (P2S) kapcsolatokhoz
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Az alábbi táblázatban látható a két üzemi modell és a pont–hely konfigurációkhoz elérhető üzembe helyezési módszerek. Amint elérhetővé válik a konfigurációs lépéseket ismertető cikk, egy arra mutató közvetlen hivatkozás szerepel majd a táblázatban.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="basic-workflow"></a>Alapvető munkafolyamat
![Pont–hely diagram](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/point-to-site-connection-diagram.png)

### <a name="a-nameexampleaexample-values"></a><a name="example"></a>Példaértékek
* **Név: VNet1**
* **Címtér: 192.168.0.0/16**<br>Ebben a példában csak egy címteret használunk. Azonban a virtuális hálózatához több címteret is használhat.
* **Alhálózat neve: FrontEnd**
* **Alhálózati címtartomány: 192.168.1.0/24**
* **Előfizetés:**Ha több előfizetése is van, ellenőrizze, hogy a megfelelőt használja-e.
* **Erőforráscsoport: TestRG**
* **Hely: East US**
* **Átjáró-alhálózat: 192.168.200.0/24**
* **Virtuális hálózati átjáró neve: VNet1GW**
* **Átjáró típusa: VPN**
* **VPN típusa: Útvonalalapú**
* **Nyilvános IP-cím: VNet1GWpip**
* **Kapcsolat típusa: pont–hely**
* **Ügyfélcímkészlet: 172.16.201.0/24**<br>Azok a VPN-ügyfelek, amelyek ezzel a pont–hely kapcsolattal csatlakoznak a virtuális hálózathoz, az ügyfélcímkészletből kapnak IP-címet.

## <a name="before-beginning"></a>Mielőtt hozzálát
* Győződjön meg arról, hogy rendelkezik Azure-előfizetéssel. Ha még nincs Azure-előfizetése, aktiválhatja [MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details), vagy regisztrálhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial).

## <a name="a-namecreatevnetapart-1---create-a-virtual-network"></a><a name="createvnet"></a>1. rész – Virtuális hálózat létrehozása
Ha gyakorlatként hozza létre ezt a konfigurációt, használja a [példaértékeket](#example).

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="a-nameaddressapart-2---specify-address-space-and-subnets"></a><a name="address"></a>2. rész – Címterek és alhálózatok megadása
Miután létrehozta a virtuális hálózatot, további címtereket és alhálózatokat adhat hozzá.

[!INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="a-namegatewaysubnetapart-3---add-a-gateway-subnet"></a><a name="gatewaysubnet"></a>3. rész – Átjáróalhálózat hozzáadása

Mielőtt csatlakoztatja virtuális hálózatát egy átjáróhoz, létre kell hoznia az átjáróalhálózatot ahhoz a virtuális hálózathoz, amelyhez csatlakozni szeretne. Az átjárószolgáltatások az átjáróalhálózatban megadott IP-címeket használják. Ha lehetséges, hozzon létre egy átjáróalhálózatot /28 vagy /27 CIDR-blokk használatával annak érdekében, hogy a jövőbeli további konfigurációs követelmények számára elegendő IP-címet biztosíthasson.

Az ebben a részben szereplő képernyőképek példa referenciaként szolgálnak. Ügyeljen arra, hogy azt az átjáróalhálózati címtartományt használja, amelyik a konfigurációhoz szükséges értékeknek megfelelő.

###<a name="to-create-a-gateway-subnet"></a>Átjáróalhálózat létrehozása

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="a-namednsapart-4---specify-a-dns-server-optional"></a><a name="dns"></a>4. rész – DNS-kiszolgáló megadása (nem kötelező)
[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="a-namecreategwapart-5---create-a-virtual-network-gateway"></a><a name="creategw"></a>5. rész – Virtuális hálózati átjáró létrehozása
A pont–hely kapcsolatokhoz a következő beállításokra van szükség:

* Átjáró típusa: VPN
* VPN típusa: Útvonalalapú

### <a name="to-create-a-virtual-network-gateway"></a>Virtuális hálózati átjáró létrehozása
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="a-namegeneratecertapart-6---generate-certificates"></a><a name="generatecert"></a>6. rész – Tanúsítványok előállítása
A tanúsítványokat az Azure használja a VPN-ügyfelek hitelesítésére a pont–hely VPN-kapcsolatokban. A nyilvános tanúsítványadatokat (nem a titkos kulcsot) egy Base-64 kódolású X.509 .cer fájlként exportálja egy vállalati tanúsítványmegoldás által létrehozott főtanúsítványból vagy egy önaláírt főtanúsítványból. Ezután importálja a nyilvános tanúsítványadatokat a főtanúsítványból az Azure-ba. Ezenfelül létre kell hoznia egy ügyféltanúsítványt az ügyfelek főtanúsítványából. Minden egyes ügyfélen, amelynek pont–hely kapcsolattal kell csatlakoznia a virtuális hálózathoz, telepíteni kell egy, a főtanúsítványból létrehozott ügyféltanúsítványt.

### <a name="a-namegetcerastep-1---obtain-the-cer-file-for-the-root-certificate"></a><a name="getcer"></a>1. lépés – A .cer fájl beszerzése a főtanúsítványhoz

A főtanúsítványhoz be kell szereznie a .cer fájlt. Használhat vállalati megoldásból származó főtanúsítványt, vagy [a makecert használatával létrehozhat egy önaláírt főtanúsítványt](vpn-gateway-certificates-point-to-site.md). Bár a PowerShell használatával is lehet önaláírt tanúsítványt létrehozni, az ilyen tanúsítványok nem tartalmazzák a P2S-kapcsolatokhoz szükséges mezőket.

1. A .cer fájl tanúsítványból való beszerzéséhez nyissa meg a **certmgr.msc** fájlt, és keresse meg a főtanúsítványt. Kattintson a jobb gombbal az önaláírt főtanúsítványra, és kattintson a **minden tevékenység**, majd az **exportálás** elemre. Megnyílik a **Tanúsítványexportáló varázsló**.
2. A varázslóban kattintson a **Tovább** gombra, válassza a **Nem, nem akarom exportálni a titkos kulcsomat** lehetőségre, majd kattintson a **Tovább** gombra.
3. Az **Exportfájlformátum** lapon válassza a **Base-64 kódolású X.509 (.CER)** lehetőséget. Ezután kattintson a **Tovább** gombra. 
4. Az **Exportálandó fájl** lapon a **Tallózás** gombra kattintva keresse meg azt a helyet, ahová exportálni szeretné a tanúsítványt. A **Fájlnév** mezőben nevezze el a tanúsítványfájlt. Ezután kattintson a **Next** (Tovább) gombra.
5. Kattintson a **Befejezés** gombra a tanúsítvány exportálásához.

### <a name="a-namegenerateclientcertastep-2---generate-a-client-certificate"></a><a name="generateclientcert"></a>2. lépés – Ügyféltanúsítvány létrehozása
Létrehozhat egy egyedi tanúsítványt minden, a virtuális hálózathoz csatlakozó ügyfél számára, vagy használhatja ugyanazt a tanúsítványt több ügyfél esetén. Az egyedi ügyféltanúsítványok előállításának előnye az, hogy szükség esetén visszavonhat egyetlen tanúsítványt. Ha azonban mindenki ugyanazt az ügyféltanúsítványt használja, és úgy találja, hogy egyetlen ügyféltől vissza kell vonnia a tanúsítványt, az összes olyan ügyfél számára elő kell állítania és telepítenie kell új tanúsítványokat, amelyek az adott tanúsítványt használják a hitelesítéshez.

####<a name="enterprise-certificate"></a>Vállalati tanúsítvány
- Ha vállalati tanúsítványmegoldást használ, az általános 'name@yourdomain.com', formátumban hozza létre az ügyféltanúsítványokat a „tartománynév\felhasználónév” formátuma helyett.
- Ellenőrizze, hogy a kiadott ügyféltanúsítvány azon a „felhasználói” tanúsítványsablonon alapul-e, amely használati listájának első helyén az „ügyfél-hitelesítés” áll, nem az intelligens kártyás bejelentkezés vagy egyebek. A tanúsítvány ellenőrzéséhez kattintson duplán az ügyféltanúsítványra, és tekintse meg a **Részletek > Kibővített kulcshasználat** részt.

####<a name="self-signed-certificate"></a>Önaláírt tanúsítvány 
Ha önaláírt tanúsítványt használ, az ügyféltanúsítvány létrehozásával kapcsolatban tekintse meg a [Working with self-signed root certificates for Point-to-Site configurations](vpn-gateway-certificates-point-to-site.md) (Önaláírt főtanúsítványok használata pont–hely konfigurációk esetében).

### <a name="a-nameexportclientcertastep-3---export-the-client-certificate"></a><a name="exportclientcert"></a>3. lépés – Az ügyféltanúsítvány exportálása
A hitelesítéshez ügyféltanúsítványra van szükség. Az ügyféltanúsítvány létrehozása után exportálja azt. Az exportált ügyféltanúsítvány később telepítve lesz mindegyik ügyfélszámítógépen.

1. Az ügyféltanúsítványok exportálásához a *certmgr.msc* fájlt használhatja. Kattintson a jobb gombbal az exportálni kívánt ügyféltanúsítványra, majd a **minden feladat** és az **exportálás** elemre.
2. Exportálja az ügyféltanúsítványt a titkos kulccsal. Ez egy *.pfx* fájl. Jegyezze fel vagy jegyezze meg a jelszót (kulcsot), amelyet beállított a tanúsítványhoz.

## <a name="a-nameaddresspoolapart-7---add-the-client-address-pool"></a><a name="addresspool"></a>7. rész – Az ügyfélcímkészlet hozzáadása
1. Miután létrehozta a virtuális hálózati átjárót, navigáljon a virtuális hálózati átjáró paneljének **Beállítások** részéhez. A **Beállítások** részben kattintson a **Pont–hely konfiguráció** elemre a **Konfiguráció** panel megnyitásához.
   
    ![Pont–hely panel](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/configuration.png)
2. A **Címkészlet** azon IP-címek készletét jelenti, amelyek közül a csatlakozó ügyfelek IP-címet kapnak. Adja hozzá a címkészletet, majd kattintson a **Mentés** gombra.
   
    ![Ügyfélcímkészlet](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/ipaddresspool.png)

## <a name="a-nameuploadfileapart-8---upload-the-root-certificate-cer-file"></a><a name="uploadfile"></a>8. rész – A főtanúsítvány .cer fájljának feltöltése
Miután létrehozta az átjárót, feltöltheti a megbízható főtanúsítványhoz tartozó .cer fájlt az Azure-ba. Legfeljebb 20 főtanúsítványhoz tölthet fel fájlokat. A főtanúsítvány titkos kulcsát ne töltse fel az Azure-ba. Miután feltöltötte a .cer fájlt, az Azure használhatja azt azon ügyfelek hitelesítéséhez, amelyek a virtuális hálózathoz csatlakoznak.

1. A tanúsítványokat a rendszer hozzáadja a **Főtanúsítvány** szakasz **Pont–hely konfiguráció** paneléhez.  
2. Győződjön meg arról, hogy Base-64 kódolású X.509 (.cer) fájlként exportálta a főtanúsítványt. Ebben a formátumban kell exportálnia, hogy szövegszerkesztővel meg tudja nyitni a tanúsítványt.
3. Nyissa megy a tanúsítványt egy szövegszerkesztővel, például a Jegyzettömbbel. Csak a következő szakaszt másolja egy folyamatos sorként:
   
    ![Tanúsítványadatok](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/copycert.png)

    > [!NOTE]
    > A tanúsítványadatok másolásakor a szöveget egy folyamatos sorként másolja kocsivissza vagy új sor nélkül. A kocsivisszák és az új sorok megjelenítéséhez lehet, hogy módosítania kell a nézetet a szövegszerkesztőben a „Szimbólum megjelenítése/Minden karakter megjelenítése” beállításra.                                                                                                                                                                            
    >
    >

4. Illessze be a tanúsítványadatokat a **Nyilvános tanúsítványadatok** mezőbe. **Nevezze el** a tanúsítványt, majd a kattintson a **Mentés** gombra. Legfeljebb 20 megbízható főtanúsítványt adhat hozzá.
   
    ![Tanúsítvány feltöltése](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/rootcertupload.png)

## <a name="a-nameclientconfigapart-9---download-and-install-the-vpn-client-configuration-package"></a><a name="clientconfig"></a>9. rész – A VPN-ügyfél konfigurációs csomagjának letöltése és telepítése
Az Azure-hoz pont–hely kapcsolattal csatlakozó ügyfeleken az ügyféltanúsítványon kívül telepíteni kell egy VPN-ügyfélkonfigurációs csomagot is. A windowsos ügyfelekhez érhetők el VPN-ügyfélkonfigurációs csomagok. 

A VPN-ügyfélcsomag tartalmazza a Windows beépített VPN-ügyfélszoftverének konfigurálásához szükséges információkat. A konfiguráció arra a VPN-re jellemző, amelyhez csatlakozni szeretne. A csomag nem telepít további szoftvert.

1. A **Pont–hely konfiguráció** panelen kattintson a **VPN-ügyfél letöltése** elemre a **VPN-ügyfél letöltése** panel megnyitásához.
   
    ![VPN-ügyfél letöltése, 1](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/downloadvpnclient1.png)
2. Válassza ki az ügyfélnek megfelelő csomagot, majd kattintson a **Letöltés** gombra. 64 bites ügyfelek esetén válassza az **AMD64** lehetőséget. 32 bites ügyfelek esetén válassza az **x86** lehetőséget.

    ![VPN-ügyfél letöltése, 2](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/client.png)
3. Telepítse a csomagot az ügyfélszámítógépen. Ha egy SmartScreen előugró ablak jelenik meg, kattintson a **További információ**, majd a **Futtatás mindenképpen** elemre a csomag telepítéséhez.
4. Nyissa meg az ügyfélszámítógépen a **Hálózati beállítások** eszközt, és kattintson a **VPN** elemre. A listán láthatja a kapcsolatot. Annak a virtuális hálózatnak a nevét mutatja, amellyel kapcsolatot létesít, és következő példára hasonlít: 
   
    ![VPN-ügyfél](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpn.png)


## <a name="a-nameinstallclientcertapart-10---install-the-client-certificate"></a><a name="installclientcert"></a>10. rész – Az ügyféltanúsítvány telepítése
Minden ügyfélszámítógépnek rendelkeznie kell egy ügyféltanúsítvánnyal a hitelesítéshez. Az ügyféltanúsítvány telepítésekor szükség lesz az ügyféltanúsítvány exportálásakor létrehozott jelszóra.

1. Másolja a .pfx fájlt az ügyfélszámítógépre.
2. Kattintson duplán a .pfx fájlra a telepítéshez. Ne módosítsa a telepítés helyét.

## <a name="a-nameconnectapart-11---connect-to-azure"></a><a name="connect"></a>11. rész – Csatlakozás az Azure-hoz
1. Csatlakozzon a virtuális hálózathoz. Ehhez navigáljon az ügyfélszámítógépen a VPN-kapcsolatokhoz, és keresse meg a létrehozott VPN-kapcsolatot. Ugyanaz a neve, mint a virtuális hálózatnak. Kattintson a **Connect** (Csatlakozás) gombra. Megjelenhet egy előugró üzenet, amely a tanúsítvány használatára utal. Ilyen esetében kattintson a **Folytatás** gombra emelt szintű jogosultságok használatához. 
2. A csatlakozás megkezdéséhez a **Kapcsolat** állapotlapon kattintson a **Csatlakozás** gombra. Ha megjelenik a **Tanúsítvány kiválasztása** képernyő, ellenőrizze, hogy az a csatlakozáshoz használni kívánt ügyféltanúsítványt mutatja-e. Ha nem, kattintson a legördülő nyílra, válassza ki a helyes tanúsítványt, majd kattintson az **OK** gombra.
   
    ![VPN-ügyfél csatlakoztatása az Azure-hoz](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/clientconnect.png)

    
3. Ekkor létre kell jönnie a kapcsolatnak.
   
    ![Az Azure-hoz csatlakoztatott VPN-ügyfél](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/connected.png)
                                                                                                                                                                           

> [!NOTE]
> Ha vállalati hitelesítésszolgáltatói megoldás használatával kiadott tanúsítványt használ, és problémák merülnek fel a hitelesítés során, ellenőrizze a hitelesítési sorrendet az ügyféltanúsítványon. A hitelesítési lista sorrendjének ellenőrzéséhez kattintson duplán az ügyféltanúsítványra, és lépjen a **Részletek > Kibővített kulcshasználat** részre. Ellenőrizze, hogy a listán az „ügyfél-hitelesítés” jelenik-e meg első helyen. Ha nem, ki kell adnia egy ügyféltanúsítványt, amely az ügyfél-hitelesítést a lista első helyén tartalmazó felhasználói sablonon alapul. 
>
>

## <a name="a-nameverifyapart-12---verify-your-connection"></a><a name="verify"></a>12. lépés – A kapcsolat ellenőrzése
1. Annak ellenőrzéséhez, hogy a VPN-kapcsolat aktív-e, nyisson meg egy rendszergazda jogú parancssort, és futtassa az *ipconfig/all* parancsot.
2. Tekintse meg az eredményeket. Figyelje meg, hogy a kapott IP-cím azok közül a címek közül való, amelyeket a pont–hely VPN-ügyfél konfigurációjának címkészletében megadott. Az eredmények a következőhöz hasonlóak:
   
        PPP adapter VNet1:
            Connection-specific DNS Suffix .:
            Description.....................: VNet1
            Physical Address................:
            DHCP Enabled....................: No
            Autoconfiguration Enabled.......: Yes
            IPv4 Address....................: 172.16.201.3(Preferred)
            Subnet Mask.....................: 255.255.255.255
            Default Gateway.................:
            NetBIOS over Tcpip..............: Enabled

## <a name="a-nameaddato-add-or-remove-trusted-root-certificates"></a><a name="add"></a>Megbízható főtanúsítványok hozzáadása vagy eltávolítása
A megbízható főtanúsítványt eltávolíthatja az Azure-ból. Ha eltávolít egy megbízható tanúsítványt, akkor az abból a főtanúsítványból generált ügyféltanúsítványok nem tudnak többé pont–hely kapcsolattal csatlakozni az Azure-hoz. Ha azt szeretné, hogy az ügyfelek csatlakozni tudjanak, telepíteniük kell egy olyan új ügyféltanúsítvány, amelyet az Azure által megbízhatónak tartott tanúsítványból generáltak.

A visszavont ügyféltanúsítványok listáját a**Pont–hely konfiguráció** panelen kezelheti. Ez az a panel, amelyet a [megbízható főtanúsítvány feltöltéséhez](#uploadfile) használt.

## <a name="a-namerevokeclientato-manage-the-list-of-revoked-client-certificates"></a><a name="revokeclient"></a>A visszavont ügyféltanúsítványok listájának kezelése
Az ügyféltanúsítványokat vissza lehet vonni. A visszavont tanúsítványok listájával az egyes ügyféltanúsítványok alapján, szelektíven tagadhatja meg a pont–hely kapcsolódás lehetőségét. Ha töröl egy .cer formátumú főtanúsítványt az Azure-ból, azzal megvonja a hozzáférést minden olyan ügyféltanúsítványtól, amelyet a visszavont főtanúsítvánnyal generáltak/írtak alá. Ha egy konkrét ügyféltanúsítványt szeretne visszavonni, és nem a főtanúsítványt, erre is lehetősége nyílik. Ily módon továbbra is érvényesek marad a főtanúsítványból generált többi tanúsítvány. 

A szokásos gyakorlat az, hogy a főtanúsítvánnyal kezelik a hozzáférést a munkacsoport vagy a szervezet szintjén, az egyes felhasználókra vonatkozó részletesebb szabályozást pedig visszavont ügyféltanúsítványokkal oldják meg.

A visszavont ügyféltanúsítványok listáját a**Pont–hely konfiguráció** panelen kezelheti. Ez az a panel, amelyet a [megbízható főtanúsítvány feltöltéséhez](#uploadfile) használt. Adja hozzá a tanúsítvány nevét és ujjlenyomatát, majd mentse azt.

## <a name="a-namefaqapoint-to-site-faq"></a><a name="faq"></a>Pont–hely kapcsolatok – gyakori kérdések

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Következő lépések
Miután a kapcsolat létrejött, hozzáadhat virtuális gépeket a virtuális hálózataihoz. További információkért lásd: [Virtuális gépek](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).




<!--HONumber=Feb17_HO3-->


