---
title: "Szerepköralapú hozzáférés-vezérlés a klasszikus Azure portálon | Microsoft Docs"
description: "Megismerheti a hozzáférés kezelését az Azure Portal szerepköralapú hozzáférés-vezérlése segítségével. Szerepkör-hozzárendelésekkel rendelhet engedélyeket az erőforrásokhoz."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: 8078f366-a2c4-4fbb-a44b-fc39fd89df81
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/17/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: bb894c38de63d0eac4066eeabaed7ba791021cc4
ms.openlocfilehash: 05da6cd034a387b54eff0790996662223e4b8bab


---
# <a name="use-role-assignments-to-manage-access-to-your-azure-subscription-resources"></a>Az Azure-előfizetések erőforrásaihoz való hozzáférés kezelése szerepkör-hozzárendelésekkel
> [!div class="op_single_selector"]
> * [Hozzáférés kezelése felhasználó vagy csoport alapján](role-based-access-control-manage-assignments.md)
> * [Hozzáférés kezelése erőforrás alapján](role-based-access-control-configure.md)

Az Azure Szerepköralapú hozzáférés-vezérlés (RBAC) részletes hozzáférés-vezérlést biztosít az Azure-hoz. Az RBAC használata lehetővé teszi, hogy csak olyan mértékű hozzáférést biztosítson, ami a felhasználóknak a feladataik elvégzéséhez szükséges. Ez a cikk segít az RBAC beállításában és használatában az Azure Portalon. Ha további részleteket szeretne arról, hogy hogyan segít az RBAC a hozzáférések kezelésében, tekintse meg [a szerepköralapú hozzáférés-vezérlést](role-based-access-control-what-is.md) ismertető szakaszt.

## <a name="view-access"></a>Hozzáférés megtekintése
Az [Azure Portal](https://portal.azure.com) fő paneljén láthatja, hogy kinek van hozzáférése egy adott erőforráshoz, erőforráscsoporthoz vagy előfizetéshez. Tegyük fel, hogy szeretnénk megnézni, hogy kinek van hozzáférése egy erőforráscsoporthoz:

1. Válassza az **Erőforráscsoportok** elemet a bal oldali navigációs sávban.  
    ![Erőforráscsoportok – ikon](./media/role-based-access-control-configure/resourcegroups_icon.png)
2. Válassza ki az erőforráscsoport nevét az **Erőforráscsoportok** panelen.
3. A bal oldali menüben válassza az **Access control (IAM)** lehetőséget.  
4. Az Access control panel az összes olyan felhasználót, csoportot és alkalmazást felsorolja, amely hozzáféréssel rendelkezik az erőforráscsoporthoz.  
   
    ![Képernyőfelvétel a felhasználók panelről – örökölt vagy hozzárendelt hozzáférés](./media/role-based-access-control-configure/view-access.png)

Megfigyelheti, hogy egyes felhasználók **hozzárendelt** mások pedig **örökölt** hozzáféréssel rendelkeznek. A hozzáférés lehet kifejezetten az erőforráscsoporthoz rendelt, vagy a szülő előfizetés egyik hozzárendeléséből örökölt.

> [!NOTE]
> Az új RBAC-modellben a hagyományos előfizetés-adminisztrátorok és társadminisztrátorok minősülnek tulajdonosoknak.

## <a name="add-access"></a>Hozzáférés felvétele
A hozzáférés a szerepkör-hozzárendelés hatókörébe tartozó erőforrásból, erőforráscsoportból vagy előfizetésből biztosítható.

1. Kattintson a **Hozzáadás** gombra az Access control panelen.  
2. A **Szerepkör kiválasztása** panelen válassza ki a felvenni kívánt szerepkört.
3. Válassza ki azt a felhasználót, csoportot vagy alkalmazást a címtárában, amely számára hozzáférést kíván biztosítani. A címtárban rákereshet megjelenítendő nevekre, e-mail-címekre és objektumazonosítókra.  
   
    ![Felhasználók hozzáadása panel – keresés – képernyőfelvétel](./media/role-based-access-control-configure/grant-access2.png)
4. A hozzárendelés létrehozásához kattintson az **OK** gombra. A **Felhasználó felvétele** előugró ablak nyomon követi a folyamatot.  
    ![Felhasználó felvétele folyamatjelző sáv – képernyőfelvétel](./media/role-based-access-control-configure/addinguser_popup.png)

A szerepkör-hozzárendelés a sikeres felvétel után megjelenik a **Felhasználók** panelen.

## <a name="remove-access"></a>Hozzáférés megszüntetése
1. A Hozzáférés-vezérlés panel jelölőnégyzetei segítségével válasszon ki egy vagy több szerepkör-hozzárendelést.
2. Válassza az **Eltávolítás** lehetőséget.  
3. Egy előugró ablakban a rendszer a művelet megerősítését kéri. A szerepkör-hozzárendelések eltávolításához válassza az **Igen** lehetőséget.

Az örökölt hozzárendeléseket nem lehet eltávolítani. Ha egy örökölt hozzárendelést kell eltávolítania, azt abban a hatókörben kell megtennie, ahol a szerepkör-hozzárendelés létrejött. A **Hatókör** oszlopban az **Örökölt** elem melletti hivatkozás azokra az erőforrásokra mutat, ahol az adott szerepkör hozzárendelése megtörtént. Lépjen az ott szereplő erőforrásra a szerepkör-hozzárendelés eltávolításához.

![Felhasználók panel – az örökölt hozzáférés letiltja az eltávolítás gombot – képernyőfelvétel](./media/role-based-access-control-configure/remove-access2.png)

## <a name="other-tools-to-manage-access"></a>Más eszközök a hozzáférés kezelésére
A szerepkörök hozzárendelését és a hozzáférések kezelését más eszközökön is elvégezheti az Azure RBAC-parancsokkal, nem csak az Azure Portalon.  Az alábbi hivatkozásokat követve további információkat szerezhet az előfeltételekről, és megismerkedhet az Azure RBAC-parancsok használatával.

* [Azure PowerShell](role-based-access-control-manage-access-powershell.md)
* [Azure parancssori felület](role-based-access-control-manage-access-azure-cli.md)
* [REST API](role-based-access-control-manage-access-rest.md)

## <a name="next-steps"></a>Következő lépések
* [Jelentés létrehozása a hozzáférés-módosítások előzményeiről](role-based-access-control-access-change-history-report.md)
* Lásd: [Beépített RBAC-szerepkörök](role-based-access-built-in-roles.md)
* Saját [egyéni szerepkörök az Azure RBAC-ben](role-based-access-control-custom-roles.md)




<!--HONumber=Feb17_HO3-->


