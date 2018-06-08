---
title: Commande OLE DB, transformation | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
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
- sql13.dts.designer.oledbcommandtrans.f1
helpviewer_keywords:
- statements [Integration Services]
- OLE DB Command transformation
ms.assetid: baa6735c-5acf-4759-b077-1216aca16c6c
caps.latest.revision: 55
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 1f0f455371c8ce7c5a2f5b7a02b9dc9e5d712f2d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ole-db-command-transformation"></a>transformation de commande OLE DB
  La transformation de commande OLE DB exécute une instruction SQL pour chaque ligne d'un flux de données. Par exemple, vous pouvez exécuter une instruction SQL qui insère, met à jour ou supprime des lignes d'une table de base de données.  
  
 Vous pouvez configurer la transformation de commande OLE DB de plusieurs manières :  
  
-   Spécifiez l'instruction SQL exécutée par la transformation pour chaque ligne.  
  
-   Spécifiez le nombre de secondes accordées pour l'exécution de l'instruction SQL.  
  
-   Spécifiez la page de codes par défaut.  
  
 En général, l'instruction SQL inclut des paramètres. Les valeurs des paramètres sont stockées dans des colonnes externes dans l'entrée de transformation et le mappage d'une colonne d'entrée à une colonne externe mappe une colonne d'entrée à un paramètre. Par exemple, pour rechercher des lignes dans la table **DimProduct** par la valeur de leur colonne **ProductKey** et pour les supprimer, vous pouvez mapper la colonne externe nommée **Param_0** à la colonne d’entrée nommée **ProductKey** , puis exécuter l’instruction SQL `DELETE FROM DimProduct WHERE ProductKey = ?`. La transformation de commande OLE DB fournit les noms des paramètres, que vous ne pouvez pas modifier. Les noms des paramètres sont **Param_0**, **Param_1**et ainsi de suite.  
  
 Si vous configurez la transformation de commande OLE DB à l'aide de la boîte de dialogue **Éditeur avancé** , les paramètres de l'instruction SQL peuvent être mappés automatiquement à des colonnes externes dans l'entrée de transformation (et les caractéristiques de chaque paramètre définis) en cliquant sur le bouton **Actualiser** . Toutefois, si le fournisseur OLE DB utilisé par la transformation de commande OLE DB ne prend pas en charge la dérivation d'informations de paramètres à partir du paramètre, vous devez configurer les colonnes externes manuellement. Cela signifie que vous devez ajouter une colonne pour chaque paramètre à l’entrée externe de la transformation, mettre à jour les noms de colonnes de façon à utiliser des noms tels que **Param_0**, spécifier la valeur de la propriété DBParamInfoFlags, puis mapper les colonnes d’entrée qui contiennent des valeurs de paramètres aux colonnes externes.  
  
 La valeur de DBParamInfoFlags représente les caractéristiques du paramètre. Par exemple, la valeur **1** indique qu'il s'agit d'un paramètre d'entrée et la valeur **65** indique qu'il s'agit d'un paramètre d'entrée pouvant contenir une valeur nulle. Les valeurs doivent correspondre aux valeurs de l'énumération OLE DB DBPARAMFLAGSENUM. Pour plus d'informations, consultez la documentation de référence OLE DB.  
  
 La transformation de commande OLE DB inclut la propriété personnalisée **SQLCommand** . La propriété peut être mise à jour par une expression de propriété lors du chargement du package. Pour plus d’informations, consultez [Expressions Integration Services &#40;SSIS&#41; ](../../../integration-services/expressions/integration-services-ssis-expressions.md), [Expressions de propriété dans des packages](../../../integration-services/expressions/use-property-expressions-in-packages.md) et [Propriétés personnalisées des transformations](../../../integration-services/data-flow/transformations/transformation-custom-properties.md).  
  
 Cette transformation a une entrée, une sortie standard et une sortie d'erreur.  
  
## <a name="logging"></a>Journalisation  
 Vous pouvez consigner les appels émis par la transformation de commande OLE DB vers les fournisseurs de données externes. Vous pouvez utiliser cette fonctionnalité de journalisation pour résoudre des problèmes liés aux connexions et aux commandes vers des sources de données externes effectuées par la transformation de commande OLE DB. Pour consigner les appels que la transformation de commande OLE DB adresse à des fournisseurs de données externes, activez la journalisation des packages et sélectionnez l'événement **Diagnostic** au niveau du package. Pour plus d’informations, consultez [Outils de dépannage pour l’exécution des packages](../../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md).  
  
## <a name="related-tasks"></a>Related Tasks  
 Vous pouvez configurer la transformation à l'aide du concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] ou du modèle objet. Pour plus d'informations sur la configuration par programmation de cette transformation, consultez le Guide du développeur.  
  
## <a name="configure-the-ole-db-command-transformation"></a>Configurer la transformation de commande OLE DB
  Pour ajouter et configurer une transformation de commande OLE DB, le package doit déjà contenir au moins une tâche de flux de données et une source telle qu'une source de fichier plat ou une source OLE DB. Cette transformation est en général utilisée pour l'exécution de requêtes paramétrables.  
  
#### <a name="to-configure-the-ole-db-command-transformation"></a>Pour configurer la transformation de commande OLE DB  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir.  
  
3.  Cliquez sur l'onglet **Flux de données** puis, à partir de la **Boîte à outils**, faites glisser la transformation de commande OLE DB sur la surface de dessin.  
  
4.  Connectez la transformation de commande OLE DB au flux de données en faisant glisser un connecteur (la flèche verte ou rouge) à partir d'une source de données ou d'une transformation précédente vers la transformation de commande OLE DB.  
  
5.  Cliquez avec le bouton droit sur le composant et sélectionnez Modifier ou Afficher l’ **éditeur avancé**.  
  
6.  Sous l'onglet **Gestionnaires de connexions** , sélectionnez un gestionnaire de connexions OLE DB dans la liste **Gestionnaires de connexions** . Pour plus d’informations, consultez [OLE DB Connection Manager](../../../integration-services/connection-manager/ole-db-connection-manager.md).  
  
7.  Cliquez sur l’onglet **Propriétés du composant** , puis sur les points de suspension **(…)** dans la zone **SqlCommand** .  
  
8.  Dans l’ **Éditeur de valeur de chaîne**, tapez l’instruction SQL paramétrable avec un point d’interrogation (?) comme marqueur de paramètre pour chaque paramètre.  
  
9. Cliquez sur **Actualiser**. Quand vous cliquez sur **Actualiser**, la transformation crée une colonne pour chaque paramètre de la collection Colonnes externes et définit la propriété DBParamInfoFlags.  
  
10. Cliquez sur l'onglet **Propriétés d'entrée et de sortie** .  
  
11. Développez **Entrée de commande OLE DB**, puis **Colonnes externes**.  
  
12. Vérifiez que **Colonnes externes** contient une colonne pour chaque paramètre de l'instruction SQL. Les noms des colonnes sont **Param_0**, **Param_1**et ainsi de suite.  
  
     Vous ne devez pas modifier les noms des colonnes. Si vous modifiez les noms des colonnes, [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] génère une erreur de validation pour la transformation de commande OLE DB.  
  
     De même, vous ne devez pas modifier le type de données. La propriété DataType de chaque colonne est définie avec le type de données correct.  
  
13. Si **Colonnes externes** ne répertorie aucune colonne, vous devez les ajouter manuellement.  
  
    -   Cliquez une fois sur **Ajouter une colonne** pour chaque paramètre de l'instruction SQL.  
  
    -   Mettez à jour les noms des colonnes comme suit : **Param_0**, **Param_1**et ainsi de suite.  
  
    -   Spécifiez une valeur dans la propriété DBParamInfoFlags. La valeur doit correspondre à une valeur de l'énumération OLE DB DBPARAMFLAGSENUM. Pour plus d'informations, consultez la documentation de référence OLE DB.  
  
    -   Spécifiez le type de données de la colonne et, en fonction du type de données, spécifiez la page de codes, la longueur, la précision et l'échelle de la colonne.  
  
    -   Pour supprimer un paramètre inutilisé, sélectionnez-le dans **Colonnes externes**, puis cliquez sur **Supprimer une colonne**.  
  
    -   Cliquez sur **Mappage de colonnes** et mappez les colonnes de la liste **Colonnes d'entrée disponibles** à des paramètres de la liste **Colonnes de destination disponibles** .  
  
14. Cliquez sur **OK**.  
  
15. Pour enregistrer le package mis à jour, cliquez sur **Enregistrer** dans le menu **Fichier** .  
  
## <a name="see-also"></a> Voir aussi  
 [Flux de données](../../../integration-services/data-flow/data-flow.md)   
 [Transformations Integration Services](../../../integration-services/data-flow/transformations/integration-services-transformations.md)  
  
  
