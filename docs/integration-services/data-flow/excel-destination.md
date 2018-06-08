---
title: Destination Excel | Microsoft Docs
ms.custom: ''
ms.date: 04/02/2018
ms.prod: sql
ms.prod_service: integration-services
ms.component: data-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.exceldest.f1
- sql13.dts.designer.exceldestadapter.connection.f1
- sql13.dts.designer.exceldestadapter.mappings.f1
- sql13.dts.designer.exceldestadapter.erroroutput.f1
helpviewer_keywords:
- destinations [Integration Services], Excel
- Excel [Integration Services]
ms.assetid: 37c07446-1264-4814-b4f5-9c66d333bb24
caps.latest.revision: 49
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: c77f42be3a3d4d2ac1cf2e77ff0adab9d32e7f0f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="excel-destination"></a>Destination Excel
  La destination Excel charge les données dans des feuilles de calcul ou des plages au sein de classeurs [!INCLUDE[msCoName](../../includes/msconame-md.md)] Excel.  

> [!IMPORTANT]
> Pour obtenir des informations détaillées sur la connexion à des fichiers Excel, et sur les limitations et les problèmes connus liés au chargement de données depuis ou vers des fichiers Excel, consultez [Charger des données depuis ou vers Excel avec SQL Server Integration Services (SSIS)](../load-data-to-from-excel-with-ssis.md).
  
## <a name="access-modes"></a>Modes d'accès  
 La destination Excel propose trois modes d'accès différents pour charger les données :  
  
-   Une table ou une vue.  
  
-   Une table ou une vue spécifiée dans une variable.  
  
-   Les résultats d'une instruction SQL. La requête peut être une requête paramétrable.  
  
## <a name="configure-the-excel-destination"></a>Configurer la destination Excel  
 La destination Excel utilise un gestionnaire de connexions Excel pour se connecter à une source de données et le gestionnaire de connexions indique le fichier de feuille de calcul à utiliser. Pour plus d'informations, consultez [Excel Connection Manager](../../integration-services/connection-manager/excel-connection-manager.md).  
  
 La destination Excel comporte une entrée normale et une sortie d'erreur.  
  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 La boîte de dialogue **Éditeur avancé** reflète toutes les propriétés qui peuvent être définies par programmation. Pour plus d'informations sur les propriétés définissables dans la boîte de dialogue **Éditeur avancé** ou par programmation, cliquez sur l'une des rubriques suivantes :  
  
-   [Propriétés communes](http://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796)  
  
-   [Propriétés personnalisées d’Excel](../../integration-services/data-flow/excel-custom-properties.md)  
  
 Pour plus d’informations sur la façon de définir les propriétés, consultez [Définir les propriétés d’un composant de flux de données](../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md).  
  
## <a name="excel-destination-editor-connection-manager-page"></a>Éditeur de destination Excel (page Gestionnaire de connexions)
  Utilisez la page **Gestionnaire de connexions** de la boîte de dialogue **Éditeur de destination Excel** pour spécifier des informations sur la source de données et afficher un aperçu des résultats. La destination Excel charge les données dans une feuille de calcul ou dans une plage (de cellules) nommée d'un classeur [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] .  
  
> [!NOTE]  
>  La propriété **CommandTimeout** de la destination Excel n'est pas disponible dans l' **Éditeur de destination Excel**, mais peut être définie à l'aide de l' **Éditeur avancé**. De plus, certaines options de chargement rapide sont uniquement disponibles dans l' **Éditeur avancé**. Pour plus d'informations sur ces propriétés, consultez la section sur la destination Excel dans [Excel Custom Properties](../../integration-services/data-flow/excel-custom-properties.md).  
  
### <a name="static-options"></a>Options statiques  
 **Gestionnaire de connexions Excel**  
 Sélectionnez un gestionnaire de connexions Excel existant dans la liste ou créez une connexion en cliquant sur **Nouveau**.  
  
 **Nouveau**  
 Créez un gestionnaire de connexions à l’aide de la boîte de dialogue **Gestionnaire de connexions Excel** .  
  
 **Mode d'accès aux données**  
 Spécifiez la méthode de sélection des données dans la source.  
  
|Option|Description|  
|------------|-----------------|  
|Table ou vue|Charge les données dans une feuille de calcul ou dans une plage nommée de la source de données Excel.|  
|Variable de nom de table ou de vue|Spécifiez le nom de la feuille de calcul ou de la plage dans une variable.<br /><br /> **Informations connexes**: [Utiliser des variables dans des packages](http://msdn.microsoft.com/library/7742e92d-46c5-4cc4-b9a3-45b688ddb787)|  
|Commande SQL|Charge les données dans la destination Excel à l'aide d'une requête SQL.|  
  
 **Nom de la feuille Excel**  
 Sélectionnez la destination Excel dans la liste déroulante. Si la liste est vide, cliquez sur **Nouveau**.  
  
 **Nouveau**  
 Cliquez sur **Nouveau** pour ouvrir la boîte de dialogue **Créer une table** . Lorsque vous cliquez sur **OK**, la boîte de dialogue crée le fichier Excel vers lequel le **Gestionnaire de connexions Excel** pointe.  
  
 **Afficher les données existantes**  
 Affichez un aperçu des résultats à l’aide de la boîte de dialogue **Visualiser les résultats de la requête** . L'aperçu peut afficher jusqu'à 200 lignes.  
  
### <a name="data-access-mode-dynamic-options"></a>Options dynamiques du mode d'accès aux données  
  
#### <a name="data-access-mode--table-or-view"></a>Mode d'accès aux données = Table ou vue  
 **Nom de la feuille Excel**  
 Sélectionnez le nom de la feuille de calcul ou de la plage nommée dans une liste des feuilles ou plages disponibles dans la source de données.  
  
#### <a name="data-access-mode--table-name-or-view-name-variable"></a>Mode d'accès aux données = Variable de nom de table ou de vue  
 **Nom de la variable**  
 Sélectionnez la variable contenant le nom de la feuille de calcul ou de la plage nommée.  
  
#### <a name="data-access-mode--sql-command"></a>Mode d'accès aux données = Commande SQL  
 **Texte de la commande SQL**  
 Entrez le texte de la requête SQL, générez la requête en cliquant sur **Générer la requête**ou cliquez sur **Parcourir**pour accéder au fichier contenant le texte de la requête.  
  
 **Générer la requête**  
 Utilisez la boîte de dialogue **Générateur de requêtes** pour construire la requête SQL visuellement.  
  
 **Parcourir**  
 Dans la boîte de dialogue **Ouvrir** , localisez le fichier qui contient le texte de la requête SQL.  
  
 **Analyser la requête**  
 Vérifiez la syntaxe du texte de la requête.  
  
## <a name="excel-destination-editor-mappings-page"></a>Éditeur de destination Excel (page Mappages)
  Utilisez la page **Mappages** de la boîte de dialogue **Éditeur de destination Excel** pour mapper des colonnes d'entrée à des colonnes de destination.  
  
### <a name="options"></a>Options  
 **Colonnes d'entrée disponibles**  
 Affichez la liste des colonnes d'entrée disponibles. Au moyen du glisser-déplacer, mappez les colonnes d'entrée disponibles dans la table sur des colonnes de destination.  
  
 **Colonnes de destination disponibles**  
 Affichez la liste des colonnes de destination disponibles. Utilisez une opération de glisser-déplacer pour mapper les colonnes de destination disponibles dans la table aux colonnes d'entrée.  
  
 **Colonne d'entrée**  
 Affiche les colonnes d'entrée sélectionnées dans le tableau ci-dessus. Vous pouvez modifier les mappages au moyen de la liste **Colonnes d'entrée disponibles**.  
  
 **Colonne de destination**  
 Affiche chaque colonne de destination disponible, qu'elle soit mappée ou non.  
  
## <a name="excel-destination-editor-error-output-page"></a>Éditeur de destination Excel (page Sortie d'erreur)
  La page **Avancé** de la boîte de dialogue **Éditeur de destination Excel** vous permet de spécifier les options de gestion des erreurs.  
  
### <a name="options"></a>Options  
 **Entrée ou Sortie**  
 Affichez le nom de la source de données.  
  
 **Colonne**  
 Indique les colonnes (sources) externes que vous avez sélectionnées dans le nœud **Gestionnaire de connexions** de la boîte de dialogue **Éditeur de source Excel**.  
  
 **Erreur**  
 Indiquez ce qui doit se produire lorsqu'une erreur se produit : ignorer l'échec, rediriger la ligne ou faire échouer le composant.  
  
 **Rubriques connexes :** [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md)  
  
 **Troncation**  
 Indiquez ce qui doit se produire lorsqu'une troncation se produit : ignorer l'échec, rediriger la ligne ou faire échouer le composant.  
  
 **Description**  
 Affiche la description de l'erreur.  
  
 **Définir cette valeur sur les cellules sélectionnées**  
 Indiquez ce qui doit se produire pour l'ensemble des cellules sélectionnées lorsqu'une erreur ou une troncation se produit : ignorer l'échec, rediriger la ligne ou faire échouer le composant.  
  
 **Appliquer**  
 Appliquez l'option de gestion des erreurs aux cellules sélectionnées.  
  
## <a name="see-also"></a> Voir aussi  
 [Charger des données depuis ou vers Excel avec SQL Server Integration Services (SSIS)](../load-data-to-from-excel-with-ssis.md)  
 [Source Excel](../../integration-services/data-flow/excel-source.md)   
[Gestionnaire de connexions Excel](../connection-manager/excel-connection-manager.md)