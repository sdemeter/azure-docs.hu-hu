---
title: "Azure Search-index létrehozása .NET SDK használatával | Microsoft Docs"
description: "Index létrehozása kódban Azure Search .NET SDK használatával."
services: search
documentationcenter: 
author: brjohnstmsft
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 3a851647-fc7b-4fb6-8506-6aaa519e77cd
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 01/13/2017
ms.author: brjohnst
translationtype: Human Translation
ms.sourcegitcommit: 1f06a7197cc1a6dcf7a39c91183a4317bef126bb
ms.openlocfilehash: 3a5131323f438109d94137cb4f577054ec13227f


---
# <a name="create-an-azure-search-index-using-the-net-sdk"></a>Azure Search-index létrehozása .NET SDK használatával
> [!div class="op_single_selector"]
> * [Áttekintés](search-what-is-an-index.md)
> * [Portál](search-create-index-portal.md)
> * [.NET](search-create-index-dotnet.md)
> * [REST](search-create-index-rest-api.md)
> 
> 

Ez a cikk végigvezeti az Azure Search-[index](https://docs.microsoft.com/rest/api/searchservice/Create-Index) [Azure Search .NET SDK](https://aka.ms/search-sdk) használatával történő létrehozásának folyamatán.

Már az útmutató követése és az index létrehozása előtt [létre kell hoznia egy Azure Search szolgáltatást](search-create-service-portal.md).

Vegye figyelembe, hogy a cikkben szereplő összes példakód C# nyelven van megírva. A teljes forráskódot a [GitHub](http://aka.ms/search-dotnet-howto) webhelyén találja.

## <a name="identify-your-azure-search-services-admin-api-key"></a>Azonosítsa az Azure Search szolgáltatás rendszergazdai API-kulcsát
Most, hogy létrehozta az Azure Search szolgáltatást, csaknem készen áll arra, hogy a .NET SDK használatával kérelmeket bocsásson ki a szolgáltatásvégponton. Először meg kell szereznie a létrehozott keresőszolgáltatáshoz generált adminisztrációs API-kulcsok egyikét. A .NET SDK minden kérelemnél elküldi ezt az API-kulcsot a szolgáltatásának. Érvényes kulcs birtokában kérelmenként létesíthető megbízhatósági kapcsolat a kérést küldő alkalmazás és az azt kezelő szolgáltatás között.

1. A szolgáltatás API-kulcsainak megkereséséhez jelentkezzen be az [Azure Portalra](https://portal.azure.com/).
2. Nyissa meg az Azure Search szolgáltatáspaneljét
3. Kattintson a „Kulcsok” ikonra

A szolgáltatás *rendszergazdai kulcsokkal* és *lekérdezési kulcsokkal* fog rendelkezni.

* Az elsődleges és másodlagos *rendszergazdai kulcsok* teljes jogosultságot biztosítanak az összes művelethez, beleértve a szolgáltatás felügyeletének, valamint az indexek, indexelők és adatforrások létrehozásának és törlésének képességét. Két kulcs létezi, tehát ha az elsődleges kulcs újbóli létrehozása mellett dönt, a másodlagos kulcsot továbbra is használhatja (ez fordítva is igaz).
* A *lekérdezési kulcsok* csak olvasási hozzáférést biztosítanak az indexekhez és dokumentumokhoz, és általában a keresési kérelmeket kibocsátó ügyfélalkalmazások kapják meg őket.

Index létrehozása céljából az elsődleges és a másodlagos adminisztrációs kulcsot is használhatja.

<a name="CreateSearchServiceClient"></a>

## <a name="create-an-instance-of-the-searchserviceclient-class"></a>A SearchServiceClient osztály példányának létrehozása
Az Azure Search .NET SDK használatához létre kell hoznia a `SearchServiceClient` osztály egy példányát. Ez az osztály több konstruktorral rendelkezik. Az, amelyiket Ön szeretne, a keresőszolgáltatása nevét és egy `SearchCredentials` objektumot használ paraméterként. A `SearchCredentials` becsomagolja az API-kulcsot.

Az alábbi kód egy új `SearchServiceClient`-példányt hoz létre a keresőszolgáltatás nevének, valamint az API-kulcsnak az alkalmazás konfigurációs fájljában (`app.config` vagy `web.config`) tárolt értékének a felhasználásával.

```csharp
string searchServiceName = ConfigurationManager.AppSettings["SearchServiceName"];
string adminApiKey = ConfigurationManager.AppSettings["SearchServiceAdminApiKey"];

SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
```

`SearchServiceClient` `Indexes`tulajdonsággal rendelkezik. Ez a tulajdonság az Azure Search-indexek létrehozásához, listázásához, frissítéséhez vagy törléséhez szükséges összes módszert biztosítja.

> [!NOTE]
> A `SearchServiceClient` osztály kezeli a keresőszolgáltatása kapcsolatait. A túl sok kapcsolat megnyitásának elkerülése érdekében, ha lehetséges, próbálja meg a `SearchServiceClient` egyetlen példányát megosztani az alkalmazásban. A módszerei szálbiztosak az ilyen megosztás engedélyezéséhez.
> 
> 

<a name="DefineIndex"></a>

## <a name="define-your-azure-search-index"></a>Az Azure Search-index meghatározása
A `Indexes.Create` módszer egyetlen meghívása létrehozza az indexet. Ez a módszer egy `Index` objektumot használ paraméterként, amely meghatározza az Azure Search-indexet. Létre kell hoznia és inicializálnia kell egy `Index` objektumot az alábbiak szerint:

1. Állítsa be az `Index` objektum `Name` tulajdonságát az index nevének.
2. Állítsa be az `Index` objektum `Fields` tulajdonságát a `Field` objektumok tömbjének. A `Field` objektumok létrehozásának legegyszerűbb módja az, ha meghívja a `FieldBuilder.BuildForType` metódust, és a típus paraméternél egy modellosztályt ad meg. A modellosztály olyan tulajdonságokkal rendelkezik, amelyek az index mezőire mutatnak. Ez lehetővé teszi a keresési indexben található dokumentumok modellosztály-példányokhoz kötését is.

> [!NOTE]
> Ha nem tervez modellosztályt használni, közvetlenül `Field` objektumok létrehozásával is meghatározhatja az indexet. A konstruktornak megadhatja a mező nevét és az adattípust (vagy karakterláncmezők esetében az elemzőnek). Más tulajdonságokat is beállíthat, például: `IsSearchable`, `IsFilterable` stb.
>
>

Fontos, hogy az index megtervezésekor a felhasználóként szerzett keresési tapasztalatának és üzleti igényeinek szem előtt tartásával járjon el, és az egyes mezőkhöz a [megfelelő tulajdonságokat](https://docs.microsoft.com/rest/api/searchservice/Create-Index) rendelje. Ezek a tulajdonságok határozzák meg, hogy melyik mezőkre melyik keresési funkciók (szűrés, értékkorlátozás, rendezés, teljes szöveges keresés stb.) vonatkoznak. Azon tulajdonságok esetében, amelyeket külön nem állított be, a `Field` osztály alapértelmezés szerint letiltja a megfelelő keresési funkciót, kivéve, ha Ön kifejezetten engedélyezi.

A fenti példában az indexnek a „hotels” nevet adtuk, és a mezőket egy modellosztály segítségével definiáltuk. A modellosztály minden tulajdonsága olyan attribútumokkal rendelkezik, amelyek meghatározzák a vonatkozó indexmező kereséssel kapcsolatos viselkedéseit. A modellosztály meghatározása a következőképpen történik:

```csharp
[SerializePropertyNamesAsCamelCase]
public partial class Hotel
{
    [Key]
    [IsFilterable]
    public string HotelId { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public double? BaseRate { get; set; }

    [IsSearchable]
    public string Description { get; set; }

    [IsSearchable]
    [Analyzer(AnalyzerName.AsString.FrLucene)]
    [JsonProperty("description_fr")]
    public string DescriptionFr { get; set; }

    [IsSearchable, IsFilterable, IsSortable]
    public string HotelName { get; set; }

    [IsSearchable, IsFilterable, IsSortable, IsFacetable]
    public string Category { get; set; }

    [IsSearchable, IsFilterable, IsFacetable]
    public string[] Tags { get; set; }

    [IsFilterable, IsFacetable]
    public bool? ParkingIncluded { get; set; }

    [IsFilterable, IsFacetable]
    public bool? SmokingAllowed { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public DateTimeOffset? LastRenovationDate { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public int? Rating { get; set; }

    [IsFilterable, IsSortable]
    public GeographyPoint Location { get; set; }

    // ToString() method omitted for brevity...
}
```

Minden tulajdonság esetében annak alapján választottuk ki az attribútumokat, ahogyan szerintünk az alkalmazások használni fogják őket. Valószínű például, hogy a hotelekre kereső felhasználókat érdekelhetik majd a `description` mezőben megadott kulcsszavak, így erre a mezőre vonatkozóan engedélyeztük a teljes szöveges keresést úgy, hogy a `Description` tulajdonsághoz hozzáadtuk az `IsSearchable` attribútumot.

Vegye figyelembe, hogy az indexben csak egy `string` típusú mező lehet kijelölve *kulcsmezőként* a `Key` attribútum hozzáadásával (lásd a fenti példában: `HotelId`).

A fenti indexdefiníció egy nyelvi elemzőt használ a `description_fr` mező esetében, mivel annak francia nyelvű szöveget kell tartalmaznia. A nyelvi elemzőkkel kapcsolatos további információkért lásd a [Nyelvi támogatás című témakört](https://docs.microsoft.com/rest/api/searchservice/Language-support), valamint a vonatkozó [blogbejegyzést](https://azure.microsoft.com/blog/language-support-in-azure-search/).

> [!NOTE]
> Alapértelmezés szerint az index minden mezőjének neve megegyezik a modellosztály megfelelő tulajdonságainak a nevével. Ha minden tulajdonságnevet szóközök nélküli, nagybetűs szavakat (CamelCase) tartalmazó mezőnevekhez szeretne hozzárendelni, akkor jelölje meg az osztályt a `SerializePropertyNamesAsCamelCase` attribútummal. Ha másik névhez szeretné őket hozzárendelni, akkor a fenti `DescriptionFr` tulajdonsághoz hasonlóan a `JsonProperty` attribútumot is használhatja. A `JsonProperty` attribútum előnyt élvez a `SerializePropertyNamesAsCamelCase` attribútummal szemben.
> 
> 

Most, hogy meghatároztuk a modellosztályt, már egyszerűen létrehozhatunk egy indexdefiníciót:

```csharp
var definition = new Index()
{
    Name = "hotels",
    Fields = FieldBuilder.BuildForType<Hotel>()
};
```

## <a name="create-the-index"></a>Az index létrehozása
Most, hogy már rendelkezik egy inicializált `Index` objektummal, a `SearchServiceClient` objektumon lévő `Indexes.Create` meghívásával egyszerűen létrehozhatja az indexet:

```csharp
serviceClient.Indexes.Create(definition);
```

Sikeres kérelem esetén a módszer normálisan tér vissza. Ha probléma merül fel, például érvénytelen egy paraméter, a módszer a `CloudException` kivételt fogja kijelezni.

Miután létrehozta az indexet, és törölni szeretné, csak hívja meg a `SearchServiceClient` objektumon lévő `Indexes.Delete` módszert. A „hotels” nevű index például a következőképpen törölhető:

```csharp
serviceClient.Indexes.Delete("hotels");
```

> [!NOTE]
> Ebben a cikkben a példakód az egyszerűség érdekében az Azure Search .NET SDK szinkron módszereit használja. Azt javasoljuk, hogy a méretezhetőség és a gyors válaszadás érdekében használja saját alkalmazásaiban az aszinkron módszereket. Például a fenti esetekben használhatja a `CreateAsync` és a `DeleteAsync` módszert a `Create` és a `Delete` helyett.
> 
> 

## <a name="next-steps"></a>Következő lépések
Az Azure Search-index létrehozása után készen áll arra, hogy [feltöltse a tartalmát az indexbe](search-what-is-data-import.md), és megkezdje az adatok keresését.




<!--HONumber=Jan17_HO2-->


