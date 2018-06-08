---
title: Préparer des données Excel pour les rapports mobiles Reporting Services | Microsoft Docs
ms.custom: ''
ms.date: 02/08/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: mobile-reports
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 16698f8d-bfc7-4eca-9e97-82c99d8bc08e
caps.latest.revision: 14
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 76f71b0f284602ae1860fcba5bc69f9221b0be33
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="prepare-excel-data-for-reporting-services-mobile-reports"></a>Préparer les données Excel pour les rapports mobiles Reporting Services
  
Voici quelques éléments à prendre en compte lors de la préparation d'un fichier et de feuilles de calcul Excel à utiliser dans un rapport mobile :  
  
## <a name="do"></a>À faire  
  
- Prévoir une feuille de calcul par jeu de données.  
- Prévoir des en-têtes de colonnes sur la première ligne.  
- Maintenir la cohérence des types de données dans chaque colonne.  
- Formater les cellules avec les types appropriés dans Excel.  
- Prévoir les données dans des feuilles de calcul et non dans le modèle de données Excel.  
- Lorsque vous utilisez des formules, vérifiez que toute la colonne est calculée avec la même formule.  
- Utiliser Excel 2007 ou version ultérieure.  
- Enregistrer les fichiers Excel avec l'extension XLSX.  
          
## <a name="dont"></a>À ne pas faire  
  
- Inclure des images, des graphiques, des tableaux croisés dynamiques ou d’autres objets incorporés dans les feuilles de calcul de jeux de données.  
- Inclure des lignes de total ou calculées.  
- Maintenir le fichier ouvert dans Excel lors de l'importation.  
- Formater manuellement les nombres en ajoutant une devise ou d’autres symboles.  
- Utiliser un classeur avec les données stockées dans le modèle de données.  
  
## <a name="worksheets"></a>Feuilles de calcul  
          
Lorsque vous préparez un fichier Excel comme un jeu de données pour un rapport mobile, assurez-vous que vous disposez d'un jeu de données par feuille de calcul. Chaque feuille de calcul individuelle est importée dans [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)] comme une table distincte. Les feuilles de calcul portant le même nom et provenant de partir de plusieurs sources Excel sont renommées lors de l'importation par incrémentation de leur numéro. Pour exemple, si un classeur contient trois feuilles de calcul intitulées « MyWorksheet », les deuxième et troisième feuilles se nommeront « MyWorksheet0 » et « MyWorksheet1 ». La capture d'écran ci-dessous montre les premières lignes d'une feuille de calcul Excel idéalement préparée pour l'importation.  
  
![SS_MRP_ExcelDataSheet](../../reporting-services/mobile-reports/media/ss-mrp-exceldatasheet.png)  
          
## <a name="column-headers"></a>En-têtes de colonnes  
  
Comme vous pouvez le voir dans l'exemple ci-dessus, la première ligne contient le nom de la mesure de cette colonne. [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)] conserve ces en-têtes de colonnes pour faciliter la référence dans les paramètres des éléments de la galerie. Cependant les en-têtes de colonne ne sont pas nécessaires. S’ils sont manquants, [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)] génère les en-têtes en utilisant la convention Excel A,B,C,...,AA,BB,...  
  
[!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)] détecte automatiquement les en-têtes de première ligne lors de l’importation des feuilles de calcul Excel en comparant les types de données des deux premières cellules de chaque colonne. Si les types de données des deux premières cellules de n'importe quelle colonne ne correspondent pas, la première ligne est configurée pour contenir des en-têtes de colonnes. Par conséquent, si une table comporte des en-têtes de colonnes numériques, faites précéder les noms d'en-têtes avec une chaîne afin qu'ils soient détectés en tant qu'en-têtes dans le processus d'importation.  
  
## <a name="cells"></a>Cellules  
  
Les données des cellules de chaque colonne d'un jeu de données d’une feuille de calcul doivent être homogènes. Un type de données est affecté à chaque colonne lors de l'importation. [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)] détecte automatiquement les types de données en tant que chaîne, double (numérique), valeur booléenne (true/false) ou datetime. Des types de données mixtes dans la même colonne peuvent fausser cette détection ou la faire échouer. Dans cette détection, les en-têtes de colonnes peuvent représenter un type de chaîne. Les cellules doivent être formatées avec le type approprié dans Excel pour s’assurer que [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)] détecte les types souhaités. Dans l'exemple ci-dessus, les six colonnes se présentent ainsi :  
*  Une colonne datetime  
*  Une colonne string  
*  Quatre colonnes double  
  
Si une feuille de calcul contient des cellules ou des formules calculées, seule la valeur d'affichage qui en résulte est importée dans [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)].  
  
## <a name="file-location-and-refreshing-excel-data"></a>Emplacement du fichier et actualisation des données Excel  
  
Il n'existe aucune restriction concernant l’emplacement où vous stockez les fichiers Excel que vous importez dans [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)]. Toutefois, si vous déplacez ou renommez le fichier après l'importation, vous ne pourrez pas actualiser ces données via la commande **Actualiser toutes les données** disponible dans la vue Données.   
  
>**Remarque**: [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)] n'actualise pas automatiquement les données Excel. Vous pouvez actualiser les données via la commande [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)] **de** , mais uniquement si le fichier n'a pas été déplacé.  
  
## <a name="dates"></a>Dates  
  
Les champs de date étant indispensables à de nombreux rapports mobiles, vous devez veiller à formater correctement les cellules en tant que dates dans Excel. Dans certains cas, cela signifie qu'une conversion est nécessaire. Voici des exemples de formules permettant de convertir des cellules de texte en dates dans Excel.  
  
    Week 24-2013=DATE(MID(A2,9,4),1,-2)-WEEKDAY(DATE(MID(A2,9,4),1,3))+MID(A2,6,2)*7  
  
    2013/03/21=DATEVALUE(A1)  
  
    2013-mar-12=DATEVALUE(RIGHT(A1,2)&"-"&MID(A1,6,3)&"-"&LEFT(A1,4))  
  
Après avoir converti les cellules, vous devez les mettre en forme en tant que dates en les sélectionnant ou en sélectionnant toute la colonne, puis menu **contextuel** > **Format de cellule** > **Date** dans la liste **Catégorie**. Vous pouvez également utiliser l’assistant Excel Convertir Texte en Colonne afin de convertir les cellules texte en dates correctement formatées.  
  
## <a name="unsupported"></a>Non pris en charge  
  
L’utilisation de formats de données de feuilles de calcul autres que ceux décrits ci-dessus peut entraîner des résultats imprévisibles lors de l'importation. Il est recommandé de limiter les feuilles de calcul d’un fichier Excel aux formats appropriés afin de les utiliser dans un rapport mobile.  
  
Les objets personnalisés dans des feuilles de calcul Excel, notamment les tableaux croisés dynamiques, les visualisations et les images, ne sont pas importés dans [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)].  
  
### <a name="see-also"></a>Voir aussi  
- [Prepare data for Reporting Services mobile reports](../../reporting-services/mobile-reports/prepare-data-for-reporting-services-mobile-reports.md)  
- [Créer et publier des rapports mobiles avec l’Éditeur de rapports mobiles SQL Server](../../reporting-services/mobile-reports/create-mobile-reports-with-sql-server-mobile-report-publisher.md)  
-  Affichez les [rapports mobiles SQL Server et les indicateurs de performance clés dans l’application iPad](https://pbiwebprod-docs.azurewebsites.net/en-us/documentation/powerbi-mobile-ipad-kpis-mobile-reports)  (Power BI pour iOS)  
-  Affichez les [rapports mobiles SQL Server et les indicateurs de performance clés dans l’application iPhone](https://pbiwebprod-docs.azurewebsites.net/en-us/documentation/powerbi-mobile-iphone-kpis-mobile-reports) (Power BI pour iOS)  
  
  
  
  
  
  
  

