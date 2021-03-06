---
title: "A helyszíni identitás-infrastruktúra megfigyelése a felhőben."
description: "Leírását és esetleges használatának okát lásd az Azure AD Connect Health vonatkozó lapján."
services: active-directory
documentationcenter: 
author: karavar
manager: samueld
editor: curtand
ms.assetid: 82798ea6-5cd3-4f30-93ae-d56536f8d8e3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/06/2017
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: c9ebe21acbe06aa25bcadb3b500c34cf89154388
ms.openlocfilehash: 9abdfa7349bee74b89a23663969904110c730395


---
# <a name="monitor-your-on-premises-identity-infrastructure-and-synchronization-services-in-the-cloud"></a>A helyszíni identitás-infrastruktúra és a szinkronizálási szolgáltatások megfigyelése a felhőben
Az Azure AD Connect Health segít megfigyelni a helyszíni identitás-infrastruktúrát és a szinkronizálási szolgáltatásokat, és további betekintést nyújt a működésükbe.  Megbízható Office 365- és Microsoft Online Services-kapcsolat fenntartását teszi lehetővé olyan fontos identitás-összetevők megfigyelésével, mint például az AD FS-kiszolgálók, az Azure AD Connect-kiszolgálók (más néven a szinkronizálási motor) vagy az Active Directory-tartományvezérlők. Ezen összetevők legfontosabb adatpontjait is könnyen hozzáférhetővé teszi, így könnyen lekérdezhetővé válnak a használatra és egyéb lényeges jellemzőkre vonatkozó adatok, hogy megalapozott döntéseket hozhasson.

Ez az információ az [Azure AD Connect Health portálon](https://aka.ms/aadconnecthealth) található meg. Az Azure AD Connect Health portálon riasztásokat, teljesítményfigyelési adatokat, használatelemzési információkat és sok más tartalmat tekinthet meg. Az Azure AD Connect Health egyetlen helyre gyűjti a legfontosabb identitás-összetevők állapotadatait.

![Mi az az Azure AD Connect Health?](./media/active-directory-aadconnect-health/aadconnecthealth2.png)

Az Azure AD Connect Health jövőbeni frissítései további figyelési szolgáltatásokat és további identitás-összetevőkkel kapcsolatos elemzési lehetőségeket biztosítanak majd. A szolgáltatás identitásalapú irányítópultja egy nagyteljesítményű, kifogástalan állapotú és integrált környezetet biztosít, ahol a felhasználók még hatékonyabb munkavégzésre lesznek képesek.

## <a name="why-use-azure-ad-connect-health"></a>Miért érdemes az Azure AD Connect Health eszközt használni?
A helyszíni címtárak és az Azure AD integrálása révén a felhasználók munkája hatékonyabbá válik, mivel a felhőalapú és a helyszíni erőforrások hozzáféréséhez közös identitás áll a rendelkezésükre. Ez az integráció azonban komoly kihívásokat támaszt a környezet megfelelő állapotával szemben, hiszen a felhasználók számára kizárólag ilyen körülmények között garantálható mind a helyszíni, mind a felhőerőforrások megbízható hozzáférése, bármilyen eszközről. Az Azure AD Connect Health egyszerű felhőalapú megközelítése révén segít megfigyelni az Office 365 és más Azure AD-alkalmazások hozzáféréséhez használt helyszíni identitás-infrastruktúrát, és további betekintést nyújt annak működésébe. Használata éppolyan egyszerű, mintha egy-egy ügynököt telepítene a helyszíni identitás-kiszolgálókra.

## <a name="azure-ad-connect-health-for-ad-fsactive-directory-aadconnect-health-adfsmd"></a>[Azure AD Connect Health for AD FS](active-directory-aadconnect-health-adfs.md)
Az Azure AD Connect Health for AD FS a következő verziókat támogatja: AD FS 2.0 Windows Server 2008 R2-ben, AD FS Windows Server 2012-ben és Windows Server 2012 R2-ben. Ezen kívül támogatja az AD FS proxy- vagy webalkalmazás-proxykiszolgálók figyelését is, amelyek az extranetes hozzáféréshez biztosítanak hitelesítési támogatást. Az állapotügynök egyszerű és alacsony költséggel végrehajtható telepítésével elérhetővé válnak az Azure AD Connect Health for AD FS alábbi képességei:

* riasztásokkal kiegészített megfigyelés az AD FS és az AD FS-proxykiszolgálók nem megfelelő állapotának észleléséhez;
* kritikus riasztásokhoz kapcsolódó e-mail értesítések;
* teljesítményadat-tendenciák megtekintése, amely hasznosan alkalmazható az AD FS kapacitástervezéséhez;
* az AD FS használatának különböző területekre (például alkalmazásokra, felhasználókra vagy hálózati helyekre) kiterjedő elemzései, amelyekkel felmérhetők az AD FS használatának módjai;
* AD FS-jelentések, például a rossz felhasználónév/jelszó párossal legtöbbet próbálkozó 50 felhasználó, az utolsó IP-címmel együtt

Az alábbi videó az Azure AD Connect Health for AD FS működéséről nyújt áttekintést

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD-Connect-Health--Monitor-you-identity-bridge/player]
>
>

## <a name="azure-ad-connect-health-for-syncactive-directory-aadconnect-health-syncmd"></a>[Azure AD Connect Health szinkronizálási szolgáltatás](active-directory-aadconnect-health-sync.md)
Az Azure AD Connect Health for Sync figyelemmel kíséri a helyszíni Active Directory és az Azure Active Directory közötti szinkronizálásokat, és rájuk vonatkozó információkat biztosít. Az Azure AD Connect Health for Sync legfontosabb képességei a következők:

* riasztásokkal kiegészített megfigyelés az Azure AD Connect-kiszolgálók (más néven a szinkronizálási motor) nem megfelelő állapotának észleléséhez;
* kritikus riasztásokhoz kapcsolódó e-mail értesítések;
* a szinkronizálási műveletek elemzései, beleértve a rájuk vonatkozó késési diagramokat és olyan különböző műveletek tendenciáit, mint például a hozzáadások, a frissítések vagy a törlések;
* a szinkronizálási tulajdonságok és az Azure AD-re történt legutóbbi sikeres exportálási művelet gyors áttekintése.
* Az objektumszintű szinkronizációs hibákról való jelentésekhez \(nem szükséges Prémium szintű Microsoft Azure AD\)

Az alábbi videó az Azure AD Connect Health szinkronizálás szolgáltatás működéséről nyújt áttekintést

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-Active-Directory-Connect-Health-Monitoring-the-sync-engine/player]
>
>

## <a name="azure-ad-connect-health-for-ad-ds-previewactive-directory-aadconnect-health-addsmd"></a>[Azure AD Connect Health for AD DS (előzetes verzió)](active-directory-aadconnect-health-adds.md)
Az Azure AD Connect Health for AD DS a Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 vagy Windows Server 2016 rendszeren telepített tartományvezérlők figyelését teszi lehetővé. A könnyen kezelhető és alacsony költségű állapotügynök telepítése lehetővé teszi, hogy a helyszíni AD DS-környezetet közvetlenül a felhőből figyelje meg. Az Azure AD Connect Health for AD DS legfontosabb képességei a következők:

* Riasztások figyelése annak észlelése érdekében, hogy a tartományvezérlő esetleg nem megfelelő állapotú, valamint e-mailes értesítések küldése kritikus riasztásokkor.
* A tartományvezérlő irányítópultja gyors betekintést biztosít a tartományvezérlők állapotába és üzemeltetési állapotába.
* A Replikáció állapota irányítópult a legújabb replikációs adatokkal, a hibák észlelésekor pedig hibaelhárítási útmutatókra mutató hivatkozásokkal szolgál.
* Gyors és helyfüggetlen hozzáférést biztosít a népszerű teljesítményszámlálók teljesítményadat-grafikonjaihoz, melyek hibaelhárítási és figyelési célból szükségesek.

Az alábbi videó az Azure AD Connect Health for AD DS működéséről nyújt áttekintést

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD-Connect-Health-monitors-on-premises-AD-Domain-Services/player]
>
>

## <a name="get-started-with-azure-ad-connect-health"></a>Az Azure AD Connect Health használatának első lépései
Az Azure AD Connect Health használatának megkezdése nagyon egyszerű. Kövesse az alábbi lépéseket:

1. [Szerezze be a Prémium szintű Azure AD-t](../active-directory-get-started-premium.md) vagy [indítson egy próbaverziót](https://azure.microsoft.com/trial/get-started-active-directory/).
2. [Töltse le és telepítse az Azure AD Connect Health-ügynököket](#download-and-install-azure-ad-connect-health-agent) az identitás-kiszolgálókra.
3. Az Azure AD Connect Health irányítópultját a következő címen tekintheti meg: [https://aka.ms/aadconnecthealth](https://aka.ms/aadconnecthealth)

> [!NOTE]
> Ne feledje, hogy mielőtt bármilyen adatot megtekinthetne az Azure AD Connect Health irányítópultján, telepítenie kell az Azure AD Connect Health-ügynököket a célkiszolgálókon.
>
>

## <a name="download-and-install-azure-ad-connect-health-agent"></a>Az Azure AD Connect Health-ügynök letöltése és telepítése
* Győződjön meg róla, hogy teljesülnek az Azure AD Connect Health követelményei
* Az Azure AD Connect Health for AD FS használatának megkezdéséhez az ügynök legfrissebb verzióját ide kattintva töltheti le: [Az Azure AD Connect Health Agent for AD FS letöltése.](http://go.microsoft.com/fwlink/?LinkID=518973)
  [](active-directory-aadconnect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-fs)
* A Azure AD Connect Health Agent for Sync használatának megkezdéséhez töltse le és telepítse az [Azure AD Connect legfrissebb verzióját](http://go.microsoft.com/fwlink/?linkid=615771).  Az állapotügynök az (1.0.9125.0-s verzió vagy újabb) Azure AD Connect részeként telepíthető.  Az Azure AD Connect támogatja az előző verziókról, helyben végzett frissítést.
* Az Azure AD Connect Health for AD DS használatának megkezdéséhez az ügynök legfrissebb verzióját ide kattintva töltheti le: [Az Azure AD Connect Health Agent for AD DS letöltése.](http://go.microsoft.com/fwlink/?LinkID=820540)
  [](active-directory-aadconnect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-fs)

## <a name="azure-ad-connect-health-portal"></a>Az Azure AD Connect Health portál
Az Azure AD Connect Health portálon riasztásokat, teljesítményfigyelési adatokat és használatelemzési információkat tekinthet meg. Az Azure AD Connect Health fő panelje a https://aka.ms/aadconnecthealth címen található.  A panelek az ablakoknak megfelelő funkciót töltenek be. A fő panelen a Gyors üzembe helyezés, az Azure AD Connect Health szolgáltatásai és további konfigurációs lehetőségek láthatók. A képernyőfelvételek alatt ezekről olvashat rövid magyarázatot.  Az ügynökök üzembe helyezését követően az állapotfigyelő szolgáltatás automatikusan azonosítja azokat a szolgáltatásokat, amelyeket az Azure AD Connect Health figyel.

![Az Azure AD Connect Health portál](./media/active-directory-aadconnect-health/portal4.png)

* **Quick Start** (Gyors üzembe helyezés) – az elem kiválasztásával megnyílik a gyors üzembe helyezési panel. Itt a Get Tools (Eszközök beszerzése) lehetőség kiválasztásával letöltheti az Azure AD Connect Health-ügynököt, hozzáférhet a dokumentációkhoz, és visszajelzést is küldhet.
* **Active Directory Federation Services** (Active Directory összevonási szolgáltatások) – az Azure AD Connect Health által aktuálisan figyelt összes AD FS-szolgáltatás. Ha kiválaszt egy példányt, megnyílik egy, a szolgáltatáspéldányra vonatkozó információkat tartalmazó panel.  Az információk között tulajdonságokat, riasztásokat, megfigyelési adatokat, használatelemzést és egy áttekintést talál. A képességekről [itt](active-directory-aadconnect-health-adfs.md) talál további információkat.
* **Azure Active Directory Connect (Sync)** ((Szinkronizálási) Azure Active Directory Connect) – az Azure AD Connect Health által aktuálisan figyelt Azure AD Connect-kiszolgálók. Ha kiválaszt egy bejegyzést, megnyílik egy, az Azure AD Connect-kiszolgálókra vonatkozó információkat tartalmazó panel. A képességekről [itt](active-directory-aadconnect-health-sync.md) talál további információkat.
* **Active Directory Domain Services** (Active Directory-tartományi szolgáltatások) – az Azure AD Connect Health által aktuálisan figyelt összes AD DS-erdő. Ha kiválaszt egy erdőt, megnyílik egy, az erdőre vonatkozó adatokat tartalmazó panel.  Ezek az adatok a következők áttekintését teszik lehetővé: lényeges információk, tartományvezérlő irányítópult, replikáció állapota irányítópult, riasztások és figyelés. A képességekről [itt](active-directory-aadconnect-health-adds.md) talál további információkat.
* **Configure** (Konfigurálás) – itt az alábbi funkciókat kapcsolhatja be vagy ki:

  1. Az automatikus frissítés engedélyezésével az Azure AD Connect Health-ügynök mindig a legfrissebb verzióban érhető el. Ez az jelenti, hogy az Azure AD Connect Health-ügynök automatikusan frissül a legfrissebb verzióra, amint az megjelenik. Ez e beállítás alapértelmezés szerint engedélyezve van.
  2. Engedélyezheti a Microsoft számára az Azure AD-címtár állapotadataihoz való hozzáférést hibaelhárítási céllal. Ez azt jelenti, hogy amikor a beállítás engedélyezve van, a Microsoft ugyanazokat az adatokat láthatja, amelyeket Ön. Ez a hibaelhárítás és a problémakezelés során jelent külön segítséget. A beállítás alapértelmezés szerint le van tiltva.

## <a name="related-links"></a>Kapcsolódó hivatkozások
* [Az Azure AD Connect Health-ügynök telepítése](active-directory-aadconnect-health-agent-install.md)
* [Az Azure AD Connect Health műveletei](active-directory-aadconnect-health-operations.md)
* [Az Azure AD Connect Health használata az AD FS szolgáltatással](active-directory-aadconnect-health-adfs.md)
* [Az Azure AD Connect Health szinkronizálási szolgáltatás használata](active-directory-aadconnect-health-sync.md)
* [Az Azure AD Connect Health használata az AD DS szolgáltatással](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health – gyakori kérdések](active-directory-aadconnect-health-faq.md)
* [Az Azure AD Connect Health verzióelőzményei](active-directory-aadconnect-health-version-history.md)



<!--HONumber=Jan17_HO3-->


