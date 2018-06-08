---
title: Propriétés personnalisées OLE DB | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: data-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 13a82d41-dd7a-4708-bc84-4407a536c877
caps.latest.revision: 8
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: a9060dfbc8a5dfd7732bdd29c6bec37c5a877069
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ole-db-custom-properties"></a>Propriétés personnalisées OLE DB
  **Propriétés personnalisées des sources**  
  
 La source OLE DB comporte des propriétés personnalisées et les propriétés communes à l'ensemble des composants de flux de données.  
  
 Le tableau suivant décrit les propriétés personnalisées de la source OLE DB. Toutes les propriétés sont en lecture/écriture.  
  
|Nom de la propriété|Type de données|Description|  
|-------------------|---------------|-----------------|  
|AccessMode|Entier|Mode utilisé pour accéder à la base de données. Les valeurs possibles sont **Open Rowset**, **OpenRowset à partir de Variable**, **Commande SQL**et **Commande SQL à partir d'une variable**. La valeur par défaut est **Open Rowset**.|  
|AlwaysUseDefaultCodePage|Booléen|Valeur qui indique s'il faut utiliser la valeur de la propriété **DefaultCodePage** pour chaque colonne ou tenter de dériver la page de codes à partir des paramètres régionaux de chaque colonne. La valeur par défaut de cette propriété est **False**.|  
|CommandTimeout|Entier|Nombre de secondes accordées comme délai d'exécution d'une commande. Une valeur égale à 0 indique un délai illimité.<br /><br /> Remarque : cette propriété n’est pas disponible dans **l’Éditeur de source OLE DB**, mais vous pouvez la définir à l’aide de **l’éditeur avancé**.|  
|DefaultCodePage|Entier|Page de codes à utiliser lorsque les informations de page de codes ne sont pas disponibles depuis la source de données.|  
|OpenRowset|String|Nom de l'objet de base de données utilisé pour ouvrir un ensemble de lignes.|  
|OpenRowsetVariable|String|Variable qui contient le nom de l'objet de base de données utilisé pour ouvrir un ensemble de lignes.|  
|ParameterMapping|String|Mappage des paramètres de la commande SQL en variables.|  
|SqlCommand|String|Commande SQL à exécuter.|  
|SqlCommandVariable|String|Variable qui contient la commande SQL à exécuter.|  
  
 La sortie et les colonnes de sortie de la source OLE DB ne disposent pas de propriétés personnalisées.  
  
 Pour plus d’informations, consultez [OLE DB Source](../../integration-services/data-flow/ole-db-source.md).  
  
 **Propriétés personnalisées des destinations**  
  
 La destination OLE DB comporte à la fois des propriétés personnalisées et les propriétés communes à l'ensemble des composants de flux de données.  
  
 Le tableau suivant décrit les propriétés personnalisées de la destination OLE DB. Toutes les propriétés sont en lecture/écriture.  
  
> [!NOTE]  
>  Les options FastLoad répertoriées ici (FastLoadKeepIdentity, FastLoadKeepNulls et FastLoadOptions) correspondent aux propriétés qui portent des noms similaires et sont présentées dans l’interface **IRowsetFastLoad** implémentée par le fournisseur Microsoft OLE DB pour SQL Server (SQLOLEDB). Pour plus d'informations, effectuez une recherche sur IRowsetFastLoad dans MSDN Library.  
  
|Nom de la propriété|Type de données|Description|  
|-------------------|---------------|-----------------|  
|AccessMode|Integer (énumération)|Valeur qui spécifie la manière dont la destination accède à sa base de données de destination.<br /><br /> Cette propriété peut prendre les valeurs suivantes :<br /><br /> <br /><br /> **OpenRowset** (0) : vous devez fournir le nom d’une table ou d’une vue.<br /><br /> **OpenRowset à partir de Variable** (1) : vous devez fournir le nom d’une variable qui contient le nom d’une table ou d’une vue.<br /><br /> **OpenRowset à l’aide de FastLoad** (3) : vous devez fournir le nom d’une table ou d’une vue.<br /><br /> **OpenRowset à l’aide de FastLoad à partir de Variable** (4) : vous devez fournir le nom d’une variable qui contient le nom d’une table ou d’une vue.<br /><br /> **Commande SQL** (2) : vous devez fournir une instruction SQL.|  
|AlwaysUseDefaultCodePage|Booléen|Valeur qui indique s'il faut utiliser la valeur de la propriété **DefaultCodePage** pour chaque colonne ou tenter de dériver la page de codes à partir des paramètres régionaux de chaque colonne. La valeur par défaut de cette propriété est **False**.|  
|CommandTimeout|Entier|Nombre maximal de secondes pendant lesquelles la commande SQL peut être exécutée avant d'arriver à expiration. La valeur 0 indique un délai infini. La valeur par défaut de cette propriété est 0.<br /><br /> Remarque : cette propriété n’est pas disponible dans **l’Éditeur de destination OLE DB**, mais vous pouvez la définir à l’aide de **l’éditeur avancé**.|  
|DefaultCodePage|Entier|Page de codes par défaut associée à la destination OLE DB.|  
|FastLoadKeepIdentity|Booléen|Valeur spécifiant si les valeurs d'identité doivent être copiées lors du chargement des données. Cette propriété est disponible uniquement avec l'une des options de chargement rapide. La valeur par défaut de cette propriété est **False**. Cette propriété correspond à la propriété OLE DB [IRowsetFastLoad &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/irowsetfastload-ole-db.md) **SSPROP_FASTLOADKEEPIDENTITY**.|  
|FastLoadKeepNulls|Booléen|Valeur spécifiant si les valeurs NULL doivent être copiées lors du chargement des données. Cette propriété est disponible uniquement avec l'une des options de chargement rapide. La valeur par défaut de cette propriété est **False**. Cette propriété correspond à la propriété OLE DB [IRowsetFastLoad &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/irowsetfastload-ole-db.md) **SSPROP_FASTLOADKEEPNULLS**.|  
|FastLoadMaxInsertCommitSize|Entier|Valeur qui spécifie la taille du lot que la destination OLE DB tente de valider au cours des opérations de chargement rapide. La valeur par défaut ( **0**) indique une opération de validation simple après le traitement de toutes les lignes.|  
|FastLoadOptions|String|Collection d'options de chargement rapide. Les options de chargement rapide incluent le verrouillage des tables et la vérification des contraintes. Vous pouvez spécifier une de ces options, les deux ou ni l'une ni l'autre. Cette propriété correspond à la propriété OLE DB IRowsetFastLoad **SSPROP_FASTLOADOPTIONS** et accepte les options de chaîne telles que **CHECK_CONSTRAINTS** et **TABLOCK**.<br /><br /> Remarque : certaines options de cette propriété ne sont pas disponibles dans **l’Éditeur de destination Excel**, mais vous pouvez les définir à l’aide de **l’éditeur avancé**.|  
|OpenRowset|String|Quand AccessMode est défini sur **OpenRowset**, nom de la table ou de la vue à laquelle la destination OLE DB a accès.|  
|OpenRowsetVariable|String|Quand AccessMode est défini sur **OpenRowset à partir de Variable**, nom de la variable contenant le nom de la table ou de la vue à laquelle la destination OLE DB a accès.|  
|SqlCommand|String|Quand AccessMode est défini sur **Commande SQL**, instruction Transact-SQL que la destination OLE DB utilise pour spécifier les colonnes de destination pour les données.|  
  
 L'entrée et les colonnes d'entrée de la destination OLE DB ne disposent pas de propriétés personnalisées.  
  
 Pour plus d’informations, consultez [OLE DB Destination](../../integration-services/data-flow/ole-db-destination.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Propriétés communes](http://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796)  
  
  
