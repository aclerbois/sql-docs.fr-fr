---
title: Utilisation d’IMultipleResults pour traiter plusieurs jeux de résultats | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-ole-db-commands
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- multiple rowsets
- rowsets [OLE DB], multiple
- IMultipleResults interface
- multiple-rowset results
ms.assetid: 754d3f30-7d94-4b67-8dac-baf2699ce9c6
caps.latest.revision: 40
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: f3ab7376bcbb6b8237aa2600ac19fb5a13b6304f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-imultipleresults-to-process-multiple-result-sets"></a>Utilisation d'IMultipleResults pour traiter plusieurs jeux de résultats
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Les consommateurs utilisent le **IMultipleResults** interface pour traiter les résultats retournés par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’exécution de commande du fournisseur OLE DB Native Client. Lorsque le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client soumet une commande pour l’exécution, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] exécute les instructions et renvoie les résultats.  
  
 Un client doit traiter tous les résultats d'exécution de commande. Étant donné que la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’exécution de commande du fournisseur OLE DB Native Client peut générer des objets de plusieurs ensembles de lignes de résultats, utilisez le **IMultipleResults** interface pour vous assurer que la récupération de données d’application termine l’aller-retour initié par le client.  
  
 Les éléments suivants [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction génère plusieurs ensembles de lignes, certaines données de ligne contenant le **OrderDetails** table et autres les résultats de la clause COMPUTE BY :  
  
```  
SELECT OrderID, FullPrice = (UnitPrice * Quantity), Discount,  
    Discounted = UnitPrice * (1 - Discount) * Quantity  
FROM OrderDetails  
ORDER BY OrderID  
COMPUTE  
    SUM(UnitPrice * Quantity), SUM(UnitPrice * (1 - Discount) * Quantity)  
    BY OrderID  
```  
  
 Si un consommateur exécute une commande contenant ce texte et demande un ensemble de lignes comme interface de résultats retournée, seul le premier jeu de lignes est retourné. Le consommateur peut traiter toutes les lignes dans l'ensemble de lignes retourné. Cependant, si la propriété de source de données DBPROP_MULTIPLECONNECTIONS a la valeur VARIANT_FALSE et MARS n’est pas activé sur la connexion, aucune autre commande ne peut être exécutée sur l’objet de session (la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Client fournisseur OLE DB natif ne crée pas une autre connexion) jusqu'à ce que la commande est annulée. Si MARS n’est pas activé sur la connexion, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Client fournisseur OLE DB natif retourne une erreur DB_E_OBJECTOPEN si DBPROP_MULTIPLECONNECTIONS a la valeur VARIANT_FALSE et retourne E_FAIL s’il existe une transaction active.  
  
 Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client retourne également DB_E_OBJECTOPEN lors de l’utilisation transmis en continu les paramètres de sortie et l’application n’a pas utilisé toutes les valeurs de paramètre de sortie retournées avant d’appeler **IMultipleResults::GetResults** pour obtenir le jeu de résultats suivant. Si MARS n'est pas activé et que la connexion est occupée à exécuter une commande qui ne produit pas un ensemble de lignes ou produit un ensemble de lignes qui n'est pas un curseur côté serveur, et que la propriété de la source de données DBPROP_MULTIPLECONNECTIONS a la valeur VARIANT_TRUE, le fournisseur OLE DB [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client crée des connexions supplémentaires pour prendre en charge les objets de commande concurrentiels à moins qu'une transaction ne soit active, auquel cas une erreur est retournée. Les transactions et le verrouillage sont gérés par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion par connexion. Si une deuxième connexion est générée, la commande sur les connexions séparées ne partage pas les verrous. Prenez soin de vérifier qu'une commande n'en bloque pas une autre en maintenant les verrous sur les lignes demandées par l'autre commande. Si MARS est activé, plusieurs commandes peuvent être actives sur les connexions et si les transactions explicites sont utilisées, les commandes partagent toutes une transaction commune.  
  
 Le consommateur peut annuler la commande à l’aide de [ISSAbort::Abort](../../relational-databases/native-client-ole-db-interfaces/issabort-abort-ole-db.md) ou en libérant toutes les références détenues sur l’objet de commande et l’ensemble de lignes dérivé.  
  
 À l’aide de **IMultipleResults** dans toutes les instances permet au consommateur d’obtenir tous les ensembles de lignes générés par l’exécution de commande et permet aux consommateurs de manière appropriée déterminer à quel moment annuler l’exécution de commande et libérer un objet de session pour une utilisation par d’autres commandes.  
  
> [!NOTE]  
>  Lorsque vous utilisez les curseurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l'exécution de commande crée le curseur. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne une valeur indiquant si la création du curseur a réussi ou échoué ; par conséquent, l'aller-retour vers l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se termine au retour de l'exécution de commande. Chaque **GetNextRows** appel devient alors un aller-retour. De cette façon, plusieurs objets de commande actifs peuvent exister, chacun traitant un ensemble de lignes qui est le résultat d'une extraction du curseur côté serveur. Pour plus d’informations, consultez [ensembles de lignes et curseurs SQL Server](../../relational-databases/native-client-ole-db-rowsets/rowsets-and-sql-server-cursors.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Commandes](../../relational-databases/native-client-ole-db-commands/commands.md)  
  
  
