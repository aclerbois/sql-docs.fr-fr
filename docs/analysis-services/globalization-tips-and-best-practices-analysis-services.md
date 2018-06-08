---
title: Globalisation conseils et meilleures pratiques (Analysis Services) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 12b771c40e6c17f1da41f1636b785b7dfc719941
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="globalization-tips-and-best-practices-analysis-services"></a>Conseils et meilleures pratiques en matière de globalisation (Analysis Services)
[!INCLUDE[ssas-appliesto-sqlas-aas](../includes/ssas-appliesto-sqlas-aas.md)]

  [!INCLUDE[applies](../includes/applies-md.md)] Multidimensionnel uniquement  
  
 Ces conseils et astuces peuvent vous aider à augmenter la portabilité de vos solutions Business Intelligence et à éviter les erreurs qui sont directement liées aux paramètres de langue et de classement.  
  
##  <a name="bkmk_sameColl"></a> Utilisation de classements similaires dans toute la pile  
 Si possible, essayez d'utiliser les mêmes paramètres de classement dans [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] que ceux que vous utilisez pour le moteur de base de données, avec une cohérence dans le respect de la largeur, le respect de la casse et la sensibilité aux accents.  
  
 Chaque service a ses propres paramètres de classement, la valeur par défaut pour le moteur de base de données étant SQL_Latin1_General_CP1_CI_AS et celle pour Analysis Services étant Latin1_General_AS. Les valeurs par défaut sont compatibles en termes de cas, de largeur et de respect des accents. Sachez que si vous variez les paramètres d'un classement, vous pouvez rencontrer des problèmes quand les propriétés de classement divergent de manière fondamentale.  
  
 Même lorsque les paramètres de classement sont fonctionnellement équivalents, vous pouvez faire face à un cas particulier où un espace dans une chaîne vide est interprété différemment par chaque service.  
  
 Le caractère espace est un « cas spécial », car il peut être représenté comme un jeu de caractères codé sur un octet (SBCS) ou sur deux octets (DBCS) au format Unicode. Dans le moteur relationnel, deux chaînes composées séparées par un espace (une en SBCS et l'autre en DBCS) sont considérées comme identiques. Dans Analysis Services, au cours du traitement, les deux mêmes chaînes composées ne sont pas identiques et la deuxième instance est signalée comme un doublon.  
  
 Pour plus d'informations et pour obtenir des suggestions de solutions de contournement, consultez [Les vides dans une chaîne Unicode sont traités différemment selon le classement](http://social.technet.microsoft.com/wiki/contents/articles/23979.ssas-processing-error-blanks-in-a-unicode-string-have-different-processing-outcomes-based-on-collation-and-character-set.aspx).  
  
##  <a name="bkmk_recos"></a> Recommandations courantes en matière de classement  
 Analysis Services présente toujours la liste complète de toutes les langues et tous les classements disponibles. Il ne filtre pas les classements en fonction de la langue sélectionnée. Veillez à choisir une combinaison exploitable.  
  
 La liste suivante contient les classements les plus couramment utilisés.  
  
 Vous devez considérer cette liste comme un point de départ pour un examen approfondi, plutôt que comme une recommandation définitive qui exclut d'autres options. Vous constaterez peut-être qu'un classement qui n'est pas spécifiquement recommandé est celui qui convient le mieux à vos données. Un test approfondi est la seule façon de vérifier si les valeurs de données sont triées et comparées correctement. Comme toujours, veillez à exécuter à la fois les charges de travail de traitement et de requête quand vous testez le classement.  
  
-   Latin1_General_100_AS est souvent utilisé pour les applications qui utilisent les 26 caractères de [l’alphabet latin de base ISO](http://en.wikipedia.org/wiki/ISO_basic_Latin_alphabet).  
  
-   Les langues d'Europe du Nord qui comptent des lettres scandinaves (par exemple, ø) peuvent utiliser Finnish_Swedish_100.  
  
-   Les langues d'Europe orientale, comme le russe, utilisent souvent Cyrillic_General_100.  
  
-   La langue et les classements chinois varient selon la région, mais sont généralement Chinois simplifié ou Chinois traditionnel.  
  
     En République populaire de Chine et à Singapour, le Support technique Microsoft considère plutôt le Chinois simplifié avec Pinyin comme l'ordre de tri par défaut. Les classements recommandés sont Chinese_PRC (pour SQL Server 2000), Chinese_PRC_90 (pour SQL Server 2005) ou Chinese_Simplified_Pinyin_100 (pour SQL Server 2008 et versions ultérieures).  
  
     À Taïwan, il est plus courant de voir le Chinois traditionnel avec l'ordre de tri recommandé basé sur le nombre de traits : Chinese_Taiwan_Stroke (pour SQL Server 2000), Chinese_Taiwan_Stroke_90 (pour SQL Server 2005) ou Chinese_Traditional_Stroke_Count_100 (pour SQL Server 2008 et versions ultérieures).  
  
     Les autres régions (comme Macao (R.A.S.) et Macao) utilisent également le Chinois traditionnel. Pour les classements, à Macao (R.A.S.) il n'est pas rare de voir Chinese_Hong_Kong_Stroke_90 (sur SQL Server 2005). À Macao, Chinese_Traditional_Stroke_Count_100 (SQL Server 2008 et versions ultérieures) est utilisé relativement souvent.  
  
-   Pour le japonais, le classement le plus courant est Japanese_CI_AS. Japanese_XJIS_100 est utilisé dans les installations prenant en charge [JIS2004](http://en.wikipedia.org/wiki/JIS_X_0213). On utilise généralement Japanese_BIN2 dans les projets de migration de données, avec des données provenant de plateformes non-Windows ou de sources de données autres que le moteur de base de données relationnelle de SQL Server.  
  
     Japanese_Bushu_Kakusu_100 est rarement observé sur les serveurs qui exécutent des charges de travail Analysis Services.  
  
-   Korean_100 est recommandé pour le coréen. Bien que Korean_Wansung_Unicode figure encore dans la liste, il est déconseillé.  
  
##  <a name="bkmk_objid"></a> Respect de la casse des identificateurs d'objets  
 À compter de SQL Server 2012 SP2, le respect de la casse des ID d'objets est appliqué indépendamment du classement, mais le comportement varie selon la langue :  
  
|Script de langue|Respect de la casse|  
|---------------------|----------------------|  
|**Alphabet latin de base**|Les identificateurs d'objets exprimés dans le script Latin (les 26 lettres majuscules ou minuscules françaises) sont traités comme ne respectant pas la casse, quel que soit le classement. Par exemple, les ID d'objet suivants sont considérés comme identiques : 54321**abcdef**, 54321**ABCDEF**, 54321**AbCdEf**. En interne, Analysis Services traite les caractères de la chaîne comme s'ils étaient tous en majuscule, puis il effectue une comparaison d'octets simple qui est indépendante de la langue.<br /><br /> Notez que seuls les 26 caractères sont affectés. S'il s'agit d'une langue d'Europe de l'Ouest qui utilise des caractères scandinaves, le caractère supplémentaire n'est pas mis en majuscule.|  
|**Cyrillique, grec, copte, arménien**|Les identificateurs d'objets en script bicaméral non latin, tel que le cyrillique, respectent toujours la casse. Par exemple, Измерение et измерение sont considérés comme deux valeurs distinctes, même si la seule différence est la casse de la première lettre.|  
  
 **Impact du respect de la casse pour les identificateurs d'objets**  
  
 Seuls les identificateurs d'objets, et non les noms d'objets, sont soumis aux comportements de respect de la casse décrits dans le tableau. Si vous constatez un changement dans le fonctionnement de votre solution (comparaison avant/après après l'installation de SQL Server 2012 SP2 ou version ultérieure), il s'agit très probablement d'un problème de traitement. Les requêtes ne sont pas affectées par les identificateurs d'objets. Pour les deux langages de requête (DAX et MDX), le moteur de formule utilise le nom d'objet (et non l'identificateur).  
  
> [!NOTE]  
>  Les modifications de code liées à la casse sont une modification avec rupture pour certaines applications. Pour plus d’informations, consultez [Modifications avec rupture dans les fonctionnalités Analysis Services de SQL Server 2016](../analysis-services/breaking-changes-to-analysis-services-features-in-sql-server-2016.md) .  
  
##  <a name="bkmk_test"></a> Test des paramètres régionaux à l'aide d'Excel, de SQL Server Profiler et de SQL Server Management Studio  
 Lors des tests de traductions, la connexion doit spécifier le LCID de la traduction. Comme décrit dans [Get Different Language from SSAS into Excel](http://extremeexperts.com/sql/Tips/ExcelDiffLocale.aspx), vous pouvez utiliser Excel pour tester vos traductions.  
  
 Pour cela, modifiez manuellement le fichier .odc pour qu'il contienne la propriété de chaîne de connexion d'identificateur de paramètres régionaux. Essayez cette procédure avec l'exemple de base de données multidimensionnelle Adventure Works.  
  
-   Recherchez les fichiers .odc existants. Une fois que vous avez trouvé celui correspondant à la base de données multidimensionnelle Adventure Works, cliquez dessus avec le bouton droit pour l'ouvrir dans le Bloc-notes.  
  
-   Ajoutez `Locale Identifier=1036` à la chaîne de connexion. Enregistrez et fermez le fichier.  
  
-   Ouvrez Excel | **Données** | **Connexions existantes**. Filtrez la liste pour afficher uniquement les fichiers de connexions sur cet ordinateur. Recherchez la connexion à Adventure Works (examinez soigneusement le nom ; il peut y en avoir plusieurs). Ouvrez la connexion.  
  
     Vous devriez voir les traductions en français de l'exemple de base de données Adventure Works.  
  
     ![Tableau croisé dynamique Excel avec traductions Français](../analysis-services/media/ssas-localetest-excel.png "tableau croisé dynamique Excel avec traductions Français")  
  
 Vous pouvez utiliser SQL Server Profiler pour confirmer les paramètres régionaux. Cliquez sur un événement `Session Initialize` et examinez la liste des propriétés dans la zone de texte ci-dessous pour trouver `<localeidentifier>1036</localeidentifier>`.  
  
 Dans Management Studio, vous pouvez spécifier Locale Identifier sur une connexion de serveur.  
  
-   Dans l’Explorateur d’objets | **Connecter** | **Analysis Services** | **Options**, cliquez sur l’onglet **Paramètres de connexion supplémentaires** .  
  
-   Entrez `Local Identifier=1036` , puis cliquez sur **Connexion**.  
  
-   Exécutez une requête MDX sur la base de données Adventure Works. Les résultats de la requête doivent être les traductions en français.  
  
     ![Requête MDX avec traductions Français dans SSMS](../analysis-services/media/ssas-localetest-ssms.png "requête MDX avec traductions Français dans SSMS")  
  
##  <a name="bkmk_mdx"></a> Écriture de requêtes MDX dans une solution contenant des traductions  
 Les données affichées pour les noms des objets [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] correspondent à des traductions, mais les identificateurs de ces mêmes objets ne sont pas traduits. Chaque fois que possible, utilisez les identificateurs et les clés pour les objets [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] à la place des légendes et des noms traduits. Par exemple, utilisez les clés de membres à la place des noms de membres pour les instructions et les scripts MDX (Multidimensional Expressions) pour garantir la portabilité dans plusieurs langues.  
  
> [!NOTE]  
>  Rappelez-vous que les noms d'objets tabulaires ne respectent jamais la casse, quel que soit le classement. Les noms d'objets multidimensionnels, en revanche, suivent le respect de la casse du classement. Comme seuls les noms d'objets multidimensionnels respectent la casse, assurez-vous que toutes les requêtes MDX faisant référence à des objets multidimensionnels ont la casse correcte.  
  
##  <a name="bkmk_datetime"></a> Écriture de requêtes MDX contenant des valeurs de date et d'heure  
 Voici quelques suggestions pour rendre vos requêtes MDX de date et d'heure plus portables entre différentes langues :  
  
1.  **Utiliser des parties numériques pour les comparaisons et les opérations**  
  
     Quand vous effectuez des opérations et des comparaisons de mois et de jours de la semaine, utilisez les parties numériques de date et d'heure plutôt que les chaînes équivalentes (par exemple, utilisez MonthNumberofYear plutôt que MonthName). Les valeurs numériques sont moins affectées par les différences dans les traductions.  
  
2.  **Utiliser des chaînes équivalentes dans un jeu de résultats**  
  
     Lors de la création de jeux de résultats vus par les utilisateurs finaux, utilisez de préférence la chaîne (par exemple MonthName) pour que votre audience multilingue puisse tirer parti des traductions que vous avez fournies.  
  
3.  **Utiliser des formats de date ISO pour les informations de date et d'heure universelles**  
  
     Un [expert Analysis Services](http://geekswithblogs.net/darrengosbell/Default.aspx) fait la recommandation suivante : « J’utilise toujours le format de date ISO aaaa-mm-jj pour toutes les chaînes de date que je passe aux requêtes SQL ou MDX, car il est sans équivoque et fonctionne quels que soient les paramètres régionaux du serveur ou du client. Je pense que le serveur doit s'en remettre à ses paramètres régionaux lors de l'analyse d'un format de date ambigu, mais je pense également que si vous avez une option qui n'est pas ouverte à l'interprétation, mieux vaut choisir cela de toutes façons ».  
  
4.  **Utiliser la fonction Format pour appliquer un format spécifique, quels que soient les paramètres régionaux de langue**  
  
     La requête MDX suivante, tirée d'un billet de forum, illustre l'utilisation de Format pour retourner des dates dans un format spécifique, quels que soient les paramètres régionaux sous-jacents.  
  

    ```  
    WITH MEMBER [LinkTimeAdd11Date_Manual] as Format(dateadd("d",15,"2014-12-11"), "mm/dd/yyyy")  
    member [LinkTimeAdd15Date_Manual] as Format(dateadd("d",11,"2014-12-13"), "mm/dd/yyyy")  
    SELECT  
    { [LinkTimeAdd11Date_Manual]  
    ,[LinkTimeAdd15Date_Manual]  
    }  
    ON COLUMNS   
    FROM [Adventure Works]  
  
    ```  
  
## <a name="see-also"></a>Voir aussi  
 [Scénarios de globalisation pour Analysis Services](../analysis-services/globalization-scenarios-for-analysis-services.md)   
 [Rédiger des instructions Transact-SQL internationales](../relational-databases/collations/write-international-transact-sql-statements.md)  
  
  
