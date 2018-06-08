---
title: Préparation des commandes | Documents Microsoft
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
- SQL Server Native Client OLE DB provider, commands
- prepared statements [SQL Server Native Client]
- commands [OLE DB]
- command preparation [SQL Server Native Client]
ms.assetid: 09ec0c6c-0a44-4766-b9b7-5092f676ee54
caps.latest.revision: 31
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: c8eb50b7d5cb34fcc7611fb57a5d42d7f37b1396
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="preparing-commands"></a>Préparation des commandes
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Le fournisseur OLE DB [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client prend en charge la préparation de commande pour l'exécution multiple optimisée d'une commande unique ; toutefois, la préparation de commande génère une charge mémoire et un consommateur n'a pas besoin de préparer une commande pour l'exécuter plus d'une fois. En général, la préparation de commande est nécessaire si celle-ci doit être exécutée plus de trois fois.  
  
 Pour des raisons de performance, la préparation de commande est différée jusqu'à ce que la commande soit exécutée. Il s'agit du comportement par défaut. Toute erreur dans la commande en cours de préparation est inconnue tant que la commande n'a pas été exécutée ou qu'une opération de métapropriété n'a pas été effectuée. L'affectation de la valeur FALSE à la propriété [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SSPROP_DEFERPREPARE peut désactiver ce comportement par défaut.  
  
 Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], lorsqu'une commande est exécutée directement (sans la préparer au préalable), un plan d'exécution est créé et mis en cache. Si l'instruction SQL est réexécutée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dispose d'un algorithme efficace pour faire correspondre la nouvelle instruction au plan d'exécution existant dans le cache et réutilise le plan d'exécution pour cette instruction.  
  
 Pour les commandes préparées, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit la prise en charge native de la préparation et de l'exécution des instructions de commande. Lorsque vous préparez une instruction, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crée un plan d'exécution, le met en cache et retourne un handle de ce plan d'exécution au fournisseur. Le fournisseur utilise ensuite ce handle pour exécuter l'instruction à plusieurs reprises. Aucune procédure stockée n'est créée. Étant donné que le handle identifie directement le plan d'exécution pour une instruction SQL au lieu de faire correspondre l'instruction au plan d'exécution dans le cache (comme c'est le cas pour l'exécution directe), il est plus efficace de préparer une instruction que de l'exécuter directement, si vous savez que l'instruction sera exécutée plusieurs fois.  
  
 Dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], les instructions préparées ne peuvent pas être utilisées pour créer des objets temporaires et ne peut pas faire référence à des procédures stockées système qui créent des objets temporaires, tels que des tables temporaires. Ces procédures doivent être exécutées directement.  
  
 Certaines commandes ne doivent jamais être préparées. Par exemple, les commandes qui spécifient l'exécution de procédure stockée ou incluent du texte non valide pour la création de procédure stockée [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne doivent pas être préparées.  
  
 Si une procédure stockée temporaire est créée, le fournisseur OLE DB [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client exécute la procédure stockée temporaire et retourne des résultats comme si l'instruction elle-même avait été exécutée.  
  
 La création de procédure stockée temporaire est contrôlée par la propriété d'initialisation spécifique au fournisseur OLE DB [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client SSPROP_INIT_USEPROCFORPREP. Si la valeur de propriété est SSPROPVAL_USEPROCFORPREP_ON ou SSPROPVAL_USEPROCFORPREP_ON_DROP, le fournisseur OLE DB [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client essaie de créer une procédure stockée lorsqu'une commande est préparée. La création de procédure stockée réussit si l'utilisateur d'application a des autorisations [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suffisantes.  
  
 Pour les consommateurs qui se déconnectent rarement, la création de procédures stockées temporaires peut nécessiter d’importantes ressources de **tempdb**, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de données système dans lequel les objets temporaires sont créés. Lorsque la valeur de SSPROP_INIT_USEPROCFORPREP est SSPROPVAL_USEPROCFORPREP_ ON, les procédures stockées temporaires créées par le fournisseur OLE DB [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client sont supprimées uniquement lorsque la session qui a créé la commande perd sa connexion à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si cette connexion est la connexion par défaut créée lors de l'initialisation de la source de données, la procédure stockée temporaire est supprimée uniquement lorsque la source de données devient non initialisée.  
  
 Lorsque la valeur de SSPROP_INIT_USEPROCFORPREP est SSPROPVAL_USEPROCFORPREP_ON_DROP, les procédures stockées temporaires du fournisseur OLE DB [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client sont supprimées lorsque l'un des événements suivants se produit :  
  
-   Le consommateur utilise **ICommandText::SetCommandText** pour indiquer une nouvelle commande.  
  
-   Le consommateur utilise **ICommandPrepare::Unprepare** pour indiquer qu’il ne requiert plus le texte de commande.  
  
-   Le consommateur libère toutes les références à l'objet de commande à l'aide de la procédure stockée temporaire.  
  
 Un objet de commande a au plus une procédure stockée temporaire **tempdb**. Toute procédure stockée temporaire existante représente le texte de commande actuel d'un objet de commande spécifique.  
  
## <a name="see-also"></a>Voir aussi  
 [Commandes](../../relational-databases/native-client-ole-db-commands/commands.md)  
  
  
