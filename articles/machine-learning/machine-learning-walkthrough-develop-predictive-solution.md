---
title: "Prediktív megoldás a hitelkockázat kiszámításához a Machine Learning használatával | Microsoft Docs"
description: "Részletes útmutató, amely azt ismerteti, hogyan hozható létre a hitelkockázat értékelésére szolgáló prediktív elemzési megoldás az Azure Machine Learning Studio eszközben."
keywords: "hitelkockázat, prediktív elemzési megoldás,kockázatértékelés"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 43300854-a14e-4cd2-9bb1-c55c779e0e93
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/15/2017
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: b652c0f817e5e56d4ff50701345b03db634f616c
ms.openlocfilehash: 043c54094f53ae539c833eb8f167201781c677a6


---
# <a name="walkthrough-develop-a-predictive-analytics-solution-for-credit-risk-assessment-in-azure-machine-learning"></a>Részletes útmutató: A hitelkockázat értékelésére szolgáló prediktív elemzési megoldás fejlesztése az Azure Machine Learning Studio használatával

Ebben az útmutatóban a prediktív elemzési megoldások Machine Learning Studióban való fejlesztési folyamatát tekintjük át részleteiben. Egy egyszerű modellt fejlesztünk ki a Machine Learning Studióban, majd üzembe helyezzük azt Azure Machine Learning-webszolgáltatásként, ahol a modell új adatokkal végezhet előrejelzéseket. 

Az útmutatóban leírtak abból indulnak ki, hogy legalább egyszer használta már a Machine Learning Studiót, és hogy valamennyire tisztában van a gépi tanulás fogalmaival. Az útmutató azonban nem feltételezi, hogy a fent említett területeken szakértő lenne.

Ha még soha nem használta az **Azure Machine Learning Studiót**, érdemes lehet [az első adatelemzési kísérlet az Azure Machine Learning Studióban történő létrehozását](machine-learning-create-experiment.md) ismertető oktatóanyaggal kezdenie. Az oktatóanyag végigvezeti a Machine Learning Studio használatának megkezdésén. Bemutatja az alapokat, azt, hogy hogyan húzhat be modulokat a kísérletbe és kapcsolhatja össze azokat, és hogyan futtathatja a kísérletet és tekintheti meg az eredményeket. Egy másik eszköz, amely hasznos lehet az első lépések során, egy diagram, amely áttekintést nyújt a Machine Learning Studio képességeiről. Ezt a következő helyről töltheti le és nyomtathatja ki: [Az Azure Machine Learning Studio képességeit áttekintő diagram](machine-learning-studio-overview-diagram.md).
 
Ha csak most ismerkedik a gépi tanulás területével általánosságban, akkor az alábbi videósorozat hasznos lehet az Ön számára. A videósorozat címe [Adatelemzés kezdőknek](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md), és remek bevezetőt kínál hétköznapi nyelven és elterjedt fogalmak használatával a gépi tanulás témakörébe.


[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
 

## <a name="the-problem"></a>A probléma

Tegyük fel, hogy előrejelzést kell készíteni egy személy hitelkockázatáról az általa kitöltött hitelkérelemben megadott adatok alapján.  

A hitelkockázat-értékelés összetett probléma, de az útmutató kedvéért leegyszerűsítjük egy kicsit. Ezután ezt példaként használjuk annak bemutatására, hogyan hozható létre prediktív elemzési megoldás a Microsoft Azure Machine Learning segítségével. Ehhez az Azure Machine Learning Studiót és egy Machine Learning-webszolgáltatást használunk majd.  

## <a name="the-solution"></a>A megoldás

Ebben a részletes útmutatóban nyilvánosan elérhető hitelkockázati adatokkal fogunk dolgozni, amelyek alapján kifejlesztünk és betanítunk egy prediktív modellt. Ezután üzembe helyezzük a modellt webszolgáltatásként, hogy mások is használhassák hitelkockázat-értékeléshez.

A hitelkockázat-értékelési megoldás létrehozásához az alábbi lépéseket fogjuk követni:  

1. [Machine Learning-munkaterület létrehozása](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [Meglévő adatok feltöltése](machine-learning-walkthrough-2-upload-data.md)
3. [Kísérlet létrehozása](machine-learning-walkthrough-3-create-new-experiment.md)
4. [A modellek betanítása és kiértékelése](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [A webszolgáltatás üzembe helyezése](machine-learning-walkthrough-5-publish-web-service.md)
6. [Hozzáférés a webszolgáltatáshoz](machine-learning-walkthrough-6-access-web-service.md)

> [!TIP] 
> Az ebben az útmutatóban kifejlesztett kísérlet működő példányát megtalálja a [Cortana Intelligence Galleryben](https://gallery.cortanaintelligence.com). Lépjen **[a hitelkockázat előrejelzésével kapcsolatos útmutatóra](https://gallery.cortanaintelligence.com/Experiment/Walkthrough-Credit-risk-prediction-1)**, és kattintson az **Open in Studio** (Megnyitás a Studióban) lehetőségre a kísérlet egy példányának a Machine Learning Studio-munkaterületre való letöltéséhez.
> 
> Ez az útmutató [a hitelkockázat bináris osztályozás útján való előrejelzésével](http://go.microsoft.com/fwlink/?LinkID=525270) kapcsolatos mintakísérlet egyszerűsített verzióján alapul, amely szintén a [Galleryben](http://gallery.cortanaintelligence.com/) érhető el.



<!--HONumber=Feb17_HO3-->


