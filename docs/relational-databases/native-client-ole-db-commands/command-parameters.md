---
title: Commande paramètres | Documents Microsoft
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
- parameters [SQL Server Native Client]
- SQL Server Native Client OLE DB provider, parameters
- SQL Server Native Client OLE DB provider, commands
- parameters [SQL Server Native Client], OLE DB
- commands [OLE DB]
ms.assetid: 072ead49-ebaf-41eb-9a0f-613e9d990f26
caps.latest.revision: 40
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: cafa0f1eacd1b574454fbeaee6edead796c86398
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="command-parameters"></a>Paramètres de commande
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Les paramètres sont marqués dans le texte de la commande avec le caractère de point d'interrogation. Par exemple, l'instruction SQL suivante est marquée pour un paramètre d'entrée unique :  
  
```  
{call SalesByCategory('Produce', ?)}  
```  
  
 Pour améliorer les performances en réduisant le trafic réseau, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Client fournisseur OLE DB natif ne dérive pas automatiquement les informations de paramètre, sauf si **ICommandWithParameters::GetParameterInfo** ou **ICommandPrepare::Prepare** est appelé avant d’exécuter une commande. Cela signifie que le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Client fournisseur OLE DB natif n’effectue pas automatiquement :  
  
-   Vérifiez l’exactitude du type de données spécifié avec **ICommandWithParameters::SetParameterInfo**.  
  
-   Mappage du DBTYPE spécifié dans les informations de liaison d'accesseur au type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] correct pour le paramètre.  
  
 Les applications recevront des erreurs ou présenteront une perte de précision avec l'une ou l'autre de ces méthodes si elles spécifient des types de données qui ne sont pas compatibles avec le type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] du paramètre.  
  
 Pour éviter cela, l'application doit :  
  
-   Vérifiez que *pwszDataSourceType* correspond à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour le paramètre de type de données si le codage en dur **ICommandWithParameters::SetParameterInfo**.  
  
-   s'assurer que la valeur DBTYPE qui est liée au paramètre est du même type que le type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour le paramètre en cas de codage effectué de manière irréversible d'un accesseur ;  
  
-   Code de l’application d’appeler **ICommandWithParameters::GetParameterInfo** afin que le fournisseur puisse obtenir le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] des types de données des paramètres dynamiquement. Notez que cela provoque une boucle réseau supplémentaire au serveur.  
  
> [!NOTE]  
>  Le fournisseur ne prend pas en charge l’appel **ICommandWithParameters::GetParameterInfo** pour toute [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mettre à jour ou supprimer une instruction contenant une clause FROM ; pour toute instruction SQL en fonction d’une sous-requête contenant des paramètres ; pour SQL instructions contenant des marqueurs de paramètre dans les deux expressions d’une comparaison, like, ou quantifié ; ou un prédicat des requêtes où l’un des paramètres est un paramètre à une fonction. Lors du traitement d’un lot d’instructions SQL, le fournisseur également ne prend pas en charge l’appel **ICommandWithParameters::GetParameterInfo** des marqueurs de paramètre dans les instructions après la première instruction du lot. Commentaires (/ * \*/) ne sont pas autorisés dans les [!INCLUDE[tsql](../../includes/tsql-md.md)] commande.  
  
 Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client prend en charge les paramètres d’entrée dans les commandes d’instruction SQL. Sur les commandes de l’appel de procédure, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Client fournisseur OLE DB natif prend en charge l’entrée, sortie et les paramètres d’entrée/sortie. Les valeurs de paramètre de sortie sont retournées à l'application lors de l'exécution (uniquement si aucun ensemble de lignes n'est retourné) ou lorsque tous les ensembles de lignes retournés sont épuisés par l'application. Pour vous assurer que les valeurs renvoyées sont valides, utilisez **IMultipleResults** pour forcer la consommation de l’ensemble de lignes.  
  
 Les noms des paramètres de procédure stockée n'ont pas besoin d'être spécifiés dans une structure DBPARAMBINDINFO. Utilisez NULL pour la valeur de la *pwszName* membre pour indiquer que le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client doit ignorer le nom du paramètre et utiliser uniquement l’ordinal spécifié dans le *rgParamOrdinals* membre **ICommandWithParameters::SetParameterInfo**. Si le texte de la commande contient à la fois des paramètres nommés et sans nom, tous les paramètres sans nom doivent être spécifiés avant les paramètres nommés.  
  
 Si le nom d’un paramètre de procédure stockée est spécifié, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Client fournisseur OLE DB natif vérifie le nom pour vous assurer qu’il est valide. Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Client fournisseur OLE DB natif retourne une erreur lorsqu’il reçoit un nom de paramètre erroné du consommateur.  
  
> [!NOTE]  
>  Pour exposer la prise en charge de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] XML et les types définis par l’utilisateur (UDT), le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client implémente une nouvelle [ISSCommandWithParameters](../../relational-databases/native-client-ole-db-interfaces/isscommandwithparameters-ole-db.md) interface.  
  
## <a name="see-also"></a>Voir aussi  
 [Commandes](../../relational-databases/native-client-ole-db-commands/commands.md)  
  
  
