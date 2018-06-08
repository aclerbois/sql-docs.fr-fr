---
title: Messages d’erreur - Parallel Data Warehouse | Documents Microsoft
description: Les messages d’erreur Parallel Data Warehouse (PDW) signalent les erreurs et problèmes rencontrent par les composants PDW et peuvent également inclure des erreurs de SQL Server exposés via PDW. Ces messages d’erreur utilisent une syntaxe cohérente pour présenter des informations. Cette syntaxe vous permettra d’identifier et de corriger les problèmes.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 33bdf11388ae52959d264e2df091e9c9669b159b
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="error-messages-in-parallel-data-warehouse"></a>Messages d’erreur dans Parallel Data Warehouse

Les messages d’erreur Parallel Data Warehouse (PDW) signalent les erreurs et problèmes rencontrent par les composants PDW et peuvent également inclure des erreurs de SQL Server exposés via PDW. Ces messages d’erreur utilisent une syntaxe cohérente pour présenter des informations. Cette syntaxe vous permettra d’identifier et résoudre les problèmes sur SQL Server PDW.  
  
## <a name="Basics"></a>Bases de Message d’erreur  
Messages d’erreur retournés suivent la même syntaxe.  
  
`Error_Indicator [SQL_State_Code] [Driver_Details] [QueryID] Message_String`  
  
Voici les valeurs possibles pour chaque champ :  
  
|Champ| Description|Exemple|  
|---------|---------------|-----------|  
|*Error_Indicator*|Le mot « Erreur » ou tout autre texte d’alerte de l’utilisateur à un problème.|ERROR|  
|*SQL_State_Code*|Le code d’état SQL, en fonction de la spécification ODBC. Le pilote génère le code d’état SQL approprié chaque fois qu’il renvoie un message à une application. Le texte « Microsoft » indique la source de l’erreur.|42000|  
|*Driver_Details*|Dépendant du pilote plus d’informations, telles que le type de pilote utilisé.|Pilotes ODBC SQL Server 2008 R2 Parallel Data Warehouse|  
|*QueryID*|Identificateur unique pour la requête. Cette valeur permet de rechercher des informations supplémentaires relatives au traitement de la requête. Par exemple, les détails de l’exécution de requête sont accessibles dans la Console d’administration à l’aide de l’ID de requête. Pour plus d’informations, consultez [contrôler le matériel à l’aide de la Console d’administration](monitor-the-appliance-by-using-the-admin-console.md).<br /><br />Si un QueryID n’est pas applicable, le texte « Interne » est renvoyé à l’utilisateur.|QID2377|  
|*Message_String*|Description explicite de l’erreur ou le problème. Lors du retour d’erreurs de SQL Server, il s’agit du texte du message SQL Server.|Seuls une assignation égale peut apparaître dans la liste set d’une instruction UPDATE.|  
  
Ces valeurs de l’exemple sont présentés à l’utilisateur comme suit :  
  
`ERROR [42000] [Microsoft][ODBC SQL Server 2008 R2 Parallel Data Warehouse driver][QID2380]Only equal assignment can appear in the set list of an UPDATE statement.`  
  
## <a name="see-also"></a>Voir aussi  
<!-- MISSING LINKS 
[Common Metadata Query Examples &#40;SQL Server PDW&#41;](../sqlpdw/common-metadata-query-examples-sql-server-pdw.md)  
-->
[Présentation des alertes de la Console d’administration](understanding-admin-console-alerts.md)  
  
