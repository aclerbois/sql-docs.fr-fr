---
title: Le Mode DirectQuery | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 0e0c462c8ae4183889b9c75f5fe30203042a02a0
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="directquery-mode"></a>Mode DirectQuery
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Cet article décrit *mode DirectQuery* pour les modèles tabulaires Analysis Services sur les niveaux de compatibilité 1200 et supérieurs. Vous pouvez activer le mode DirectQuery pour les modèles que vous concevez dans SSDT. Pour les modèles tabulaires qui ont déjà été déployés, vous pouvez basculer en mode DirectQuery dans SSMS. Avant de choisir le mode DirectQuery, vous devez comprendre quels sont ses avantages et ses restrictions.
  
##  <a name="bkmk_Benefits"></a> Avantages
 Par défaut, les modèles tabulaires utilisent un cache mémoire pour stocker et interroger les données. Quand les modèles tabulaires interrogent des données en mémoire, même les requêtes complexes peuvent être extrêmement rapides. Toutefois, l’utilisation de données en mémoire cache présente des limitations. En effet, des jeux de données de grande taille peuvent dépasser la capacité mémoire disponible, et il peut s’avérer difficile, voire impossible, de répondre aux exigences d’actualisation des données avec une planification de traitement régulière.  
  
 DirectQuery permet de passer outre ces limites tout en exploitant les fonctionnalités SGBDR qui peuvent rendre l’exécution des interrogations plus efficace. Avec DirectQuery :  
  
-   Les données sont à jour et il n’y a aucune charge de gestion supplémentaire liée à la conservation d’une copie distincte des données (dans le cache mémoire). Les modifications apportées aux données sources sous-jacentes peuvent être immédiatement reflétées dans des requêtes sur le modèle de données.  
  
-   Les datasets peuvent être plus volumineux que la capacité mémoire d’un serveur Analysis Services.  
  
-   DirectQuery peut tirer parti de l’accélération des requêtes côté fournisseur, telle que celle fournie par les index de colonne optimisés en mémoire.  
  
-   La sécurité peut être appliquée par la base de données principale, à l’aide des fonctionnalités de sécurité au niveau des lignes de la base de données (vous pouvez également utiliser la sécurité au niveau des lignes dans le modèle par le biais de DAX).  
  
-   Si le modèle contient des formules complexes qui peuvent nécessiter plusieurs requêtes, Analysis Services peut procéder à une optimisation pour garantir que le plan de requête pour la requête exécutée sur la base de données principale est aussi efficace que possible.  
  
##  <a name="bkmk_prereq"></a>Restrictions
Les modèles tabulaires en mode DirectQuery ont certaines restrictions. Avant de changer de mode, déterminez si les avantages de l’exécution des requêtes sur le serveur principal l’emportent sur toute réduction des fonctionnalités.  
  
 Si vous changez le mode d’un modèle existant dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], le concepteur de modèles vous informera de toute fonctionnalité de votre modèle incompatible avec le mode DirectQuery.  
  
 La liste suivante résume les principales restrictions de fonctionnalités à garder à l’esprit :  
  
|||  
|-|-|  
|**Domaine de fonctionnalité**|**Restriction**|  
|**Sources de données**|Les modèles DirectQuery peuvent uniquement utiliser des données d’une base de données relationnelle des types suivants : SQL Server, Azure SQL Database, Oracle et Teradata.  Pour plus d’informations sur les versions et les fournisseurs, consultez Sources de données prises en charge pour DirectQuery plus loin dans cet article.| 
|**Procédures stockées SQL**|Pour les modèles DirectQuery, les procédures stockées ne peuvent pas être spécifiées dans une instruction SQL pour définir les tables quand vous utilisez l’Assistant Importation de données. |   
|**Tables calculées**|Les tables calculées ne sont pas prises en charge dans les modèles DirectQuery, au contraire des colonnes calculées. Si vous essayez de convertir un modèle tabulaire qui contient une table calculée, une erreur se produit indiquant que le modèle ne peut pas contenir de données collées.|  
|**Limites de requêtes**|La limite de ligne par défaut est d’un million de lignes. Vous pouvez l’augmenter en spécifiant **MaxIntermediateRowSize** dans le fichier msmdsrv.ini. Pour plus d’informations, consultez [Propriétés DAX](../../analysis-services/server-properties/dax-properties.md) .
|**Formules DAX**|Lors de l’interrogation d’un modèle tabulaire en mode DirectQuery, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] convertit toutes les formules DAX et les définitions de mesure en instructions SQL. Les formules DAX contenant des éléments qui ne peuvent pas être convertis en syntaxe SQL retournent des erreurs de validation sur le modèle.<br /><br /> Cette restriction est principalement limitée à certaines fonctions DAX. Pour les mesures, les formules DAX sont converties en opérations de basées sur des jeux par rapport à la base de données relationnelle. Cela signifie que toutes les mesures créées implicitement sont prises en charge. <br /><br /> Quand une erreur de validation se produit, vous devez réécrire la formule en utilisant une autre fonction, ou utiliser des colonnes dérivées dans la source de données comme solution de contournement.  Si un modèle tabulaire comprend des formules contenant des fonctions incompatibles, cela est signalé quand vous basculez en mode DirectQuery dans le concepteur. <br /><br />**Remarque**  :  Certaines formules du modèle peuvent être validées quand vous basculez le modèle en mode DirectQuery, mais retourner des résultats différents une fois exécutées sur le cache et la banque de données relationnelle. Cela est dû au fait que les calculs basés sur le cache utilisent la sémantique du moteur d’analyse en mémoire, qui contient des fonctionnalités permettant d’émuler le comportement d’Excel, alors que les requêtes sur des données stockées dans la base de données relationnelle utilisent obligatoirement la sémantique de SQL Server.<br /><br /> SQL stocké  <br /><br /> Pour plus d’informations, consultez [compatibilité des formules DAX en mode DirectQuery](../../analysis-services/tabular-models/dax-formula-compatibility-in-directquery-mode-ssas-2016.md).|  
|**Cohérence des formules**|Dans certains cas, la même formule peut retourner différents résultats dans un modèle mis en cache comparé à un modèle DirectQuery qui utilise uniquement la banque de données relationnelle. Ces différences sont une conséquence des différences sémantiques entre le moteur d'analyse en mémoire et SQL Server.<br /><br /> Pour obtenir la liste complète des problèmes de compatibilité, y compris les fonctions qui peuvent retourner des résultats différents lorsque le modèle est déployé en temps réel, consultez [compatibilité des formules DAX dans le mode DirectQuery (SQL Server Analysis Services)](http://msdn.microsoft.com/en-us/981b6a68-434d-4db6-964e-d92f8eb3ee3e).|  
|**Limitations de MDX**|Aucun nom d’objet relatif. Tous les noms d’objet doivent être complets.<br /><br /> Aucune instruction MDX pour une session (jeux nommés, membres calculés, cellules calculées, valeur totale affichée, membres par défaut, etc.), mais vous pouvez utiliser des constructions à l’échelle de la requête, telles que la clause ’WITH’.<br /><br /> Aucun tuple avec des membres de différents niveaux dans ses clauses de sous-sélection MDX.<br /><br /> Aucune hiérarchie définie par l’utilisateur.<br /><br /> Aucune requête SQL native des (en règle générale, Analysis Services prend en charge un sous-ensemble T-SQL, mais pas pour des modèles DirectQuery).|  

## <a name="data-sources-supported-for-directquery"></a>Sources de données prises en charge pour DirectQuery
Les modèles tabulaires DirectQuery au niveau de compatibilité 1200 et les versions ultérieures sont compatibles avec les fournisseurs et les sources de données suivantes :

Source de données   |Versions  |Fournisseurs
---------|---------|---------
Microsoft SQL Server    |  2008 et versions ultérieures      |       Fournisseur OLE DB pour SQL Server, fournisseur SQL Server Native Client OLE DB, fournisseur de données .NET Framework pour SQL Client  
Base de données SQL Microsoft Azure    |   Tous      |  Fournisseur OLE DB pour SQL Server, fournisseur SQL Server Native Client OLE DB, fournisseur de données .NET Framework pour SQL Client            
Microsoft Azure SQL Data Warehouse     |   Tous     |  Fournisseur de données .NET Framework pour SQL Client       
Microsoft SQL Analytics Platform System (APS)     |   Tous      |  Fournisseur OLE DB pour SQL Server, fournisseur SQL Server Native Client OLE DB, fournisseur de données .NET Framework pour SQL Client       
Bases de données relationnelles Oracle     |  Oracle 9i et versions ultérieures       |  Fournisseur OLE DB Oracle       
Bases de données relationnelles Teradata    |  Teradata V2R6 et versions ultérieures     | Fournisseur de données .Net pour Teradata        

## <a name="connecting-to-a-data-source"></a>Connexion à une source de données
Quand vous concevez un modèle DirectQuery dans SSDT, le processus de connexion à une source de données et de sélection des tables et des champs à inclure dans votre modèle ressemble fortement à celui applicable aux modèles en mémoire. 

Si vous avez déjà activé DirectQuery mais que vous ne vous êtes pas encore connecté à une source de données, vous pouvez utiliser l’Assistant Importation de table pour vous connecter à votre source de données, sélectionner les tables et les champs, spécifier une requête SQL et ainsi de suite. La différence sera qu’une fois terminé, aucune donnée ne sera réellement importée vers le cache en mémoire. 

![Réussite de l’importation DirectQuery](../../analysis-services/tabular-models/media/directquery-import-success.png)

Si vous avez déjà utilisé l’Assistant Importation de table pour importer des données mais que vous n’avez pas encore activé le mode DirectQuery, le cache en mémoire est effacé quand vous procédez à l’activation.

  
## <a name="additional-articles-in-this-section"></a>Autres articles de cette section
[Activer le mode DirectQuery dans SSDT](../../analysis-services/tabular-models/enable-directquery-mode-in-ssdt.md)

[Activer le mode DirectQuery dans SSMS](../../analysis-services/tabular-models/enable-directquery-mode-in-ssms.md)

[Ajouter des exemples de données à un modèle DirectQuery en mode Création](../../analysis-services/tabular-models/add-sample-data-to-a-directquery-model-in-design-mode.md)

[Définition de partitions dans les modèles DirectQuery](../../analysis-services/tabular-models/define-partitions-in-directquery-models-ssas-tabular.md)
  
[Tester un modèle en mode DirectQuery](../../analysis-services/tabular-models/test-a-model-in-directquery-mode.md)

[Compatibilité des formules DAX en mode DirectQuery](../../analysis-services/tabular-models/dax-formula-compatibility-in-directquery-mode-ssas-2016.md)
  
