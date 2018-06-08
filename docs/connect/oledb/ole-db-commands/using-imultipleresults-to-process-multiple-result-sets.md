---
title: Utilisation d’IMultipleResults pour traiter plusieurs jeux de résultats | Documents Microsoft
description: Utilisation d’IMultipleResults pour traiter les résultats multiples définit
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: ole-db-commands
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- multiple rowsets
- rowsets [OLE DB], multiple
- IMultipleResults interface
- multiple-rowset results
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: 162b706391e2128cb715396fd836bf446a625b17
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-imultipleresults-to-process-multiple-result-sets"></a>Utilisation d'IMultipleResults pour traiter plusieurs jeux de résultats
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Les consommateurs utilisent le **IMultipleResults** interface pour traiter les résultats renvoyés par le pilote OLE DB pour l’exécution de commande de SQL Server. Lorsque le pilote OLE DB pour SQL Server soumet une commande pour l’exécution, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] exécute les instructions et renvoie les résultats.  
  
 Un client doit traiter tous les résultats d'exécution de commande. Étant donné que le pilote OLE DB pour l’exécution de la commande SQL Server peut générer des objets de plusieurs ensembles de lignes de résultats, utilisez le **IMultipleResults** interface pour vous assurer que la récupération de données d’application termine l’aller-retour initié par le client.  
  
 Les éléments suivants [!INCLUDE[tsql](../../../includes/tsql-md.md)] instruction génère plusieurs ensembles de lignes, certaines données de ligne contenant le **OrderDetails** table et autres les résultats de la clause COMPUTE BY :  
  
```  
SELECT OrderID, FullPrice = (UnitPrice * Quantity), Discount,  
    Discounted = UnitPrice * (1 - Discount) * Quantity  
FROM OrderDetails  
ORDER BY OrderID  
COMPUTE  
    SUM(UnitPrice * Quantity), SUM(UnitPrice * (1 - Discount) * Quantity)  
    BY OrderID  
```  
  
 Si un consommateur exécute une commande contenant ce texte et demande un ensemble de lignes comme interface de résultats retournée, seul le premier jeu de lignes est retourné. Le consommateur peut traiter toutes les lignes dans l'ensemble de lignes retourné. Cependant, si la propriété de source de données DBPROP_MULTIPLECONNECTIONS a la valeur VARIANT_FALSE et MARS n’est pas activé sur la connexion, aucune autre commande ne peut être exécutée sur l’objet de session (le pilote OLE DB pour SQL Server ne crée pas une autre connexion) jusqu'à ce que le commande est annulée. Si MARS n’est pas activé sur la connexion, le pilote OLE DB pour SQL Server retourne une erreur DB_E_OBJECTOPEN si DBPROP_MULTIPLECONNECTIONS a la valeur VARIANT_FALSE et retourne E_FAIL s’il existe une transaction active.  
  
 Le pilote OLE DB pour SQL Server retourne également DB_E_OBJECTOPEN lors de l’utilisation transmis en continu les paramètres de sortie et l’application n’a pas utilisé toutes les valeurs de paramètre de sortie retournées avant d’appeler **IMultipleResults::GetResults** à obtenir le jeu de résultats suivant. Si MARS n’est pas activé et que la connexion est occupée à exécuter une commande qui ne produit pas un ensemble de lignes ou qui produit un ensemble de lignes n’est pas un curseur côté serveur et si la propriété de source de données DBPROP_MULTIPLECONNECTIONS a la valeur VARIANT_TRUE, le pilote OLE DB pour SQL Server crée des connexions supplémentaires pour prendre en charge les objets de commande simultanées, sauf si une transaction est active, auquel cas il retourne une erreur. Les transactions et le verrouillage sont gérés par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] connexion par connexion. Si une deuxième connexion est générée, la commande sur les connexions séparées ne partage pas les verrous. Prenez soin de vérifier qu'une commande n'en bloque pas une autre en maintenant les verrous sur les lignes demandées par l'autre commande. Si MARS est activé, plusieurs commandes peuvent être actives sur les connexions et si les transactions explicites sont utilisées, les commandes partagent toutes une transaction commune.  
  
 Le consommateur peut annuler la commande à l’aide de [ISSAbort::Abort](../../oledb/ole-db-interfaces/issabort-abort-ole-db.md) ou en libérant toutes les références détenues sur l’objet de commande et l’ensemble de lignes dérivé.  
  
 À l’aide de **IMultipleResults** dans toutes les instances permet au consommateur d’obtenir tous les ensembles de lignes générés par l’exécution de commande et permet aux consommateurs de manière appropriée déterminer à quel moment annuler l’exécution de commande et libérer un objet de session pour une utilisation par d’autres commandes.  
  
> [!NOTE]  
>  Lorsque vous utilisez les curseurs [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], l'exécution de commande crée le curseur. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] retourne une valeur indiquant si la création du curseur a réussi ou échoué ; par conséquent, l'aller-retour vers l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] se termine au retour de l'exécution de commande. Chaque **GetNextRows** appel devient alors un aller-retour. De cette façon, plusieurs objets de commande actifs peuvent exister, chacun traitant un ensemble de lignes qui est le résultat d'une extraction du curseur côté serveur. Pour plus d’informations, consultez [ensembles de lignes et curseurs SQL Server](../../oledb/ole-db-rowsets/rowsets-and-sql-server-cursors.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Commandes](../../oledb/ole-db-commands/commands.md)  
  
  
