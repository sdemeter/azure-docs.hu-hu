---
title: "Service Bus-névtér létrehozása az Azure Portal használatával | Microsoft Docs"
description: "A Service Bus használatának megkezdéséhez szükség van egy névtérre. Az alábbiakban leírtak szerint hozhat létre egyet az Azure Portal használatával."
services: service-bus-messaging
documentationcenter: .net
author: jtaubensee
manager: timlt
editor: 
ms.assetid: fbb10e62-b133-4851-9d27-40bd844db3ba
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 11/30/2016
ms.author: jotaub
translationtype: Human Translation
ms.sourcegitcommit: 0b1f6f7ec47e47f39407cdbfd5efef2a18944ecc
ms.openlocfilehash: 89610512c264cef4bd52728240ddc0a34091edea


---
# <a name="create-a-service-bus-namespace-using-the-azure-portal"></a>Service Bus-névtér létrehozása az Azure Portal használatával
A névtér egy közös tároló az üzenetkezelés összes összetevője számára. Egyetlen névtér több üzenetsort és témakört tartalmazhat, és a névterek gyakran alkalmazástárolókként is szolgálnak. A Service Bus-névterek létrehozásának jelenleg különböző két módja van.

1. Azure Portal (ez a cikk)
2. [Resource Manager-sablonok][create-namespace-using-arm]

## <a name="create-a-namespace-in-the-azure-portal"></a>Névtér létrehozása az Azure Portalon
[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Gratulálunk! Létrehozott egy Service Bus üzenetkezelési névteret.

## <a name="next-steps"></a>Következő lépések
Tekintse meg a [GitHub-mintáinkat][github-samples], ahol további példákat talál, amelyek az Azure Service Bus üzenetkezelési szolgáltatásának speciális funkcióit mutatják be.

[create-namespace-using-arm]: service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples



<!--HONumber=Dec16_HO1-->


