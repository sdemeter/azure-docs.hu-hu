---
title: "Alkalmazás- vagy felhasználóspecifikus Marathon-szolgáltatás | Microsoft Docs"
description: "Alkalmazás- vagy felhasználóspecifikus Marathon-szolgáltatás létrehozása"
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Tárolók, Marathon, mikroszolgáltatások, DC/OS, Azure"
ms.assetid: 16ecc16e-e504-480e-8dc3-cac14e9e1561
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/12/2016
ms.author: rogardle
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: befd26888b653c660cc614221684732771496aed


---
# <a name="create-an-application-or-userspecific-marathon-service"></a>Alkalmazás- vagy felhasználóspecifikus Marathon-szolgáltatás létrehozása
Az Azure tárolószolgáltatás előkonfigurált Apache Mesos és Marathon rendszerű főkiszolgálókat biztosít. Ezek képesek ugyan az alkalmazások hangolására a fürtön, de jobb, ha erre a célra nem a főkiszolgálókat használja. A Marathon konfigurációjának finomhangolásához például magukra a főkiszolgálókra történő bejelentkezésre, illetve módosításokra van szükség. Így könnyen létrejöhetnek a szabványostól eltérő, egyedi főkiszolgálók, amelyeket emiatt függetlenül kell kezelni és felügyelni. Ezenkívül az is előfordulhat, hogy az egyik csapat által igényelt konfiguráció nem optimális megoldás egy másik csapat számára.

Ebben a cikkben elmagyarázzuk, hogyan adhat meg alkalmazás- vagy felhasználóspecifikus Marathon-szolgáltatást.

Mivel ez a szolgáltatás egyetlen felhasználóhoz vagy csapathoz fog tartozni, tetszés szerint konfigurálható. Az Azure Container Service gondoskodik arról, hogy a szolgáltatás futása ne szakadjon meg. Ha a szolgáltatás meghiúsul, az Azure Container Service újraindul. Az esetek többségében nem is lehet észrevenni a kiesést.

## <a name="prerequisites"></a>Előfeltételek
[Helyezze üzembe az Azure Container Service egy példányát](container-service-deployment.md) DC/OS típusú vezénylővel, és [győződjön meg róla, hogy az ügyfél képes csatlakozni a fürthöz](container-service-connect.md). Végezze el a következő lépéseket is.

[!INCLUDE [install the DC/OS CLI](../../includes/container-service-install-dcos-cli-include.md)]

## <a name="create-an-application-or-userspecific-marathon-service"></a>Alkalmazás- vagy felhasználóspecifikus Marathon-szolgáltatás létrehozása
Először hozzon létre egy JSON-konfigurációs fájlt, amely meghatározza a létrehozni kívánt alkalmazásszolgáltatás nevét. A jelen példában használjuk a `marathon-alice` megnevezést a keretrendszer neveként. Mentse a fájlt például a `marathon-alice.json` néven:

```json
{"marathon": {"framework-name": "marathon-alice" }}
```

Ezután a DC/OS parancssori felülettel telepítse a Marathon példányát a konfigurációs fájlban megadott beállításokkal:

```bash
dcos package install --options=marathon-alice.json marathon
```

Ekkor a DC/OS felhasználói felületének Services (Szolgáltatások) lapján már látnia kell, hogy fut a `marathon-alice` szolgáltatás. Ha közvetlenül szeretne csatlakozni, a felhasználói felületet a `http://<hostname>/service/marathon-alice/` címen találja.

## <a name="set-the-dcos-cli-to-access-the-service"></a>A DC/OS parancssori felület beállítása a szolgáltatás eléréséhez
A DC/OS parancssori felület igény szerint konfigurálható ezen új szolgáltatás elérésére a `marathon.url` tulajdonságnak `marathon-alice` példányra mutató beállításával, a következőképpen:

```bash
dcos config set marathon.url http://<hostname>/service/marathon-alice/
```

A `dcos config show` paranccsal ellenőrizheti, hogy a Marathon melyik példányához működik a parancssori felület. A `dcos config unset marathon.url` paranccsal visszaállhat a fő Marathon szolgáltatás használatára.




<!--HONumber=Nov16_HO2-->


