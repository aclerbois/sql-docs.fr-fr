---
title: Index des propriétés dynamiques ADO | Documents Microsoft
ms.prod: sql
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- dynamic properties [ADO], index
ms.assetid: 80d389dd-46ef-459f-b0d4-6f712fc4f32d
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 306b4ecf404afef9e7e6ed2c523c2ca362fab23b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ado-dynamic-property-index"></a>Index des propriétés dynamiques ADO
Fournisseurs de données, les fournisseurs de services et les composants de service peuvent ajouter des propriétés dynamiques pour la **propriétés** collections de non ouverts [connexion](../../../ado/reference/ado-api/connection-object-ado.md) et [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objets. Un fournisseur donné peut également insérer des propriétés supplémentaires lors de l’ouverture de ces objets. Certaines de ces propriétés sont répertoriées dans le [propriétés dynamiques ADO](../../../ado/reference/ado-api/ado-dynamic-properties.md) section. D’autres sont répertoriées sous les fournisseurs spécifiques dans le [annexe a : fournisseurs](../../../ado/guide/appendixes/appendix-a-providers.md) section.  
  
 Les tableaux suivants sont cross-indexes des noms ADO et OLE DB pour chaque propriété dynamique standard de fournisseur OLE DB. Vos fournisseurs peuvent ajouter d’autres propriétés non répertoriées ici. Pour obtenir des informations spécifiques sur les propriétés dynamiques spécifiques au fournisseur, consultez la documentation du fournisseur.  
  
 Référence du programmeur OLE DB fait référence à un nom de la propriété ADO par le terme « Description ». Pour plus d’informations sur ces propriétés standards, rechercher ou parcourir l’index dans le [documentation OLE DB](https://msdn.microsoft.com/library/windows/desktop/ms722784.aspx)pour la propriété OLE DB par son nom.  
  
## <a name="connection-dynamic-properties"></a>Propriétés dynamiques de connexion  
  
|Nom de la propriété ADO|Nom de la propriété OLE DB|  
|-----------------------|--------------------------|  
|Sessions actives|DBPROP_ACTIVESESSIONS|  
|Annulation gérable en asynchrone|DBPROP_ASYNCTXNABORT|  
|Validation gérable en asynchrone|DBPROP_ASYNCTNXCOMMIT|  
|Niveaux d’Isolation de validation automatique|DBPROP_SESS_AUTOCOMMITISOLEVELS|  
|Emplacement du catalogue|DBPROP_CATALOGLOCATION|  
|Terme du catalogue|DBPROP_CATALOGTERM|  
|Définition de colonne|DBPROP_COLUMNDEFINITION|  
|Connect Timeout|DBPROP_INIT_TIMEOUT|  
|Catalogue actuel|DBPROP_CURRENTCATALOG|  
|Source de données|DBPROP_INIT_DATASOURCE|  
|Nom de la source de données|DBPROP_DATASOURCENAME|  
|Objet de Source de données modèle de thread|DBPROP_DSOTHREADMODEL|  
|Nom du SGBD|DBPROP_DBMSNAME|  
|Version du SGBD|DBPROP_DBMSVER|  
|Extended Properties|DBPROP_INIT_PROVIDERSTRING|  
|Regrouper par prise en charge|DBPROP_GROUPBY|  
|Prise en charge de Table hétérogène|DBPROP_HETEROGENEOUSTABLES|  
|Identificateur de respect de la casse|DBPROP_IDENTIFIERCASE|  
|Catalogue initial|DBPROP_INIT_CATALOG|  
|Niveaux d’isolation|DBPROP_SUPPORTEDTXNISOLEVELS|  
|Conservation d’isolement|DBPROP_SUPPORTEDTXNISORETAIN|  
|Identificateur de paramètres régionaux|DBPROP_INIT_LCID|  
|Emplacement|DBPROP_INIT_LOCATION|  
|Taille d’Index maximale|DBPROP_MAXINDEXSIZE|  
|Taille de ligne maximale|DBPROP_MAXROWSIZE|  
|Taille de ligne maximale inclut les BLOB|DBPROP_MAXROWSIZEINCLUDESBLOB|  
|Nombre maximal de Tables dans SELECT|DBPROP_MAXTABLESINSELECT|  
|Mode|DBPROP_INIT_MODE|  
|Plusieurs jeux de paramètres|DBPROP_MULTIPLEPARAMSETS|  
|Plusieurs résultats|DBPROP_MULTIPLERESULTS|  
|Plusieurs objets de stockage|DBPROP_MULTIPLESTORAGEOBJECTS|  
|Mise à jour de plusieurs Table|DBPROP_MULTITABLEUPDATE|  
|Ordre de classement NULL|DBPROP_NULLCOLLATION|  
|Comportement de la concaténation des valeurs NULL|DBPROP_CONCATNULLBEHAVIOR|  
|Services OLE DB|DBPROP_INIT_OLEDBSERVICES|  
|OLE DB Version|DBPROP_PROVIDEROLEDBVER|  
|Prise en charge des objets OLE|DBPROP_OLEOBJECTS|  
|Ouvrez la prise en charge de l’ensemble de lignes|DBPROP_OPENROWSETSUPPORT|  
|Colonnes ORDER BY dans la liste de sélection|DBPROP_ORDERBYCOLUMNSINSELECT|  
|Disponibilité du paramètre de sortie|DBPROP_OUTPUTPARAMETERAVAILABILITY|  
|Passer par les accesseurs Ref|DBPROP_BYREFACCESSORS|  
|Mot de passe|DBPROP_AUTH_PASSWORD|  
|Persist Security Info|DBPROP_AUTH_PERSIST_SENSITIVE_AUTHINFO|  
|Type d’identificateur persistant|DBPROP_PERSISTENTIDTYPE|  
|Préparation d’annulation|DBPROP_PREPAREABORTBEHAVIOR|  
|Préparation de validation|DBPROP_PREPARECOMMITBEHAVIOR|  
|Terme de procédure|DBPROP_PROCEDURETERM|  
|Demander|DBPROP_INIT_PROMPT|  
|Nom convivial du fournisseur|DBPROP_PROVIDERFRIENDLYNAME|  
|Provider Name|DBPROP_PROVIDERFILENAME|  
|Version du fournisseur|DBPROP_PROVIDERVER|  
|Source de données en lecture seule|DBPROP_DATASOURCEREADONLY|  
|Conversions d’ensemble de lignes de commande|DBPROP_ROWSETCONVERSIONSONCOMMAND|  
|Terme du schéma|DBPROP_SCHEMATERM|  
|Utilisation de schéma|DBPROP_SCHEMAUSAGE|  
|Prise en charge SQL|DBPROP_SQLSUPPORT|  
|Stockage structuré|DBPROP_STRUCTUREDSTORAGE|  
|Prise en charge de sous-requête|DBPROP_SUBQUERIES|  
|Terme de table|DBPROP_TABLETERM|  
|DDL de transaction|DBPROP_SUPPORTEDTXNDDL|  
|ID d'utilisateur|DBPROP_AUTH_USERID|  
|Nom d'utilisateur|DBPROP_USERNAME|  
|Handle de fenêtre|DBPROP_INIT_HWND|  
  
## <a name="recordset-dynamic-properties"></a>Propriétés dynamiques du jeu d’enregistrements  
 Notez que la **propriétés dynamiques** de la **Recordset** objet accédez hors de portée (deviennent indisponible) lorsque le **Recordset** est fermé.  
  
|Nom de la propriété ADO|Nom de la propriété OLE DB|  
|-----------------------|--------------------------|  
|IAccessor|DBPROP_IACCESSOR|  
|IChapteredRowset||  
|IColumnsInfo|DBPROP_ICOLUMNSINFO|  
|IColumnsRowset|DBPROP_ICOLUMNSROWSET|  
|IConnectionPointContainer|DBPROP_ICONNECTIONPOINTCONTAINER|  
|IConvertType||  
|ILockBytes|DBPROP_ILOCKBYTES|  
|IRowset|DBPROP_IROWSET|  
|IDBAsynchStatus|DBPROP_IDBASYNCHSTATUS|  
|IParentRowset||  
|IRowsetChange|DBPROP_IROWSETCHANGE|  
|IRowsetExactScroll||  
|IRowsetFind|DBPROP_IROWSETFIND|  
|IRowsetIdentity|DBPROP_IROWSETIDENTITY|  
|IRowsetInfo|DBPROP_IROWSETINFO|  
|IRowsetLocate|DBPROP_IROWSETLOCATE|  
|IRowsetRefresh|DBPROP_IROWSETREFRESH|  
|IRowsetResynch||  
|IRowsetScroll|DBPROP_IROWSETSCROLL|  
|IRowsetUpdate|DBPROP_IROWSETUPDATE|  
|IRowsetView|DBPROP_IROWSETVIEW|  
|IRowsetIndex|DBPROP_IROWSETINDEX|  
|ISequentialStream|DBPROP_ISEQUENTIALSTREAM|  
|IStorage|DBPROP_ISTORAGE|  
|IStream|DBPROP_ISTREAM|  
|ISupportErrorInfo|DBPROP_ISUPPORTERRORINFO|  
|Ordre d’accès|DBPROP_ACCESSORDER|  
|Ensemble de lignes en mode Append-Only|DBPROP_APPENDONLY|  
|Traitement de l’ensemble de lignes asynchrone|DBPROP_ROWSET_ASYNCH|  
|De recalcul automatique|DBPROP_ADC_AUTORECALC|  
|Taille de l’extraction en arrière-plan|DBPROP_ASYNCHFETCHSIZE|  
|Priorité de thread en arrière-plan|DBPROP_ASYNCHTHREADPRIORITY|  
|Taille de lot|DBPROP_ADC_BATCHSIZE|  
|Blocage des objets de stockage|DBPROP_BLOCKINGSTORAGEOBJECTS|  
|Type de signet|DBPROP_BOOKMARKTYPE|  
|Signet|DBPROP_IROWSETLOCATE|  
|Signets ordonnés|DBPROP_ORDEREDBOOKMARKS|  
|Mettre en cache les lignes enfants|DBPROP_ADC_CACHECHILDROWS|  
|Mettre en cache les colonnes différées|DBPROP_CACHEDEFERRED|  
|Modifier les lignes insérées|DBPROP_CHANGEINSERTEDROWS|  
|Privilèges de colonne|DBPROP_COLUMNRESTRICT|  
|Notification de jeu de colonnes|DBPROP_NOTIFYCOLUMNSET|  
|Colonne accessible en écriture|DBPROP_MAYWRITECOLUMN|  
|Délai de commande|DBPROP_COMMANDTIMEOUT|  
|Version du moteur de curseur|DBPROP_ADC_CEVER|  
|Différer la colonne|DBPROP_DEFERRED|  
|Mises à jour des objets de stockage délai|DBPROP_DELAYSTORAGEOBJECTS|  
|Récupérer vers l’arrière|DBPROP_CANFETCHBACKWARDS|  
|Opérations de filtre|DBPROP_FILTERCOMPAREOPS|  
|Les opérations de recherche|DBPROP_FINDCOMPAREOPS|  
|Colonnes masquées (nombre)|DBPROP_HIDDENCOLUMNS|  
|Contenant des lignes|DBPROP_CANHOLDROWS|  
|Lignes immobiles|DBPROP_IMMOBILEROWS|  
|Taille de l’extraction initiale|DBPROP_ASYNCHPREFETCHSIZE|  
|Signets littéral|DBPROP_LITERALBOOKMARKS|  
|Identité de ligne littérale|DBPROP_LITERALIDENTITY|  
|Mettre à jour d’état de modification|DBPROP_ADC_MAINTAINCHANGESTATUS|  
|Nombre maximal de lignes ouvrir|DBPROP_MAXOPENROWS|  
|Nombre maximal de lignes en attente|DBPROP_MAXPENDINGROWS|  
|Nombre maximal de lignes|DBPROP_MAXROWS|  
|Utilisation de la mémoire|DBPROP_MEMORYUSAGE|  
|Granularité de notification|DBPROP_NOTIFICATIONGRANULARITY|  
|Phases de notification|DBPROP_NOTIFICATIONPHASES|  
|Objets traités|DBPROP_TRANSACTEDOBJECT|  
|Autres modifications visibles|DBPROP_OTHERUPDATEDELETE|  
|Autres Inserts Visible|DBPROP_OTHERINSERT|  
|Propres Changes visibles|DBPROP_OWNUPDATEDELETE|  
|Propres Inserts visibles|DBPROP_OWNINSERT|  
|Conserver sur Abandon|DBPROP_ABORTPRESERVE|  
|Conserver lors de la validation|DBPROP_COMMITPRESERVE|  
|Private1||  
|Redémarrage rapide|DBPROP_QUICKRESTART|  
|Événements réentrants|DBPROP_REENTRANTEVENTS|  
|Supprimer les lignes supprimées|DBPROP_REMOVEDELETED|  
|Signaler les changements multiples|DBPROP_REPORTMULTIPLECHANGES|  
|Modifier la forme nom|DBPROP_ADC_RESHAPENAME|  
|Resynchronisation des commandes|DBPROP_ADC_CUSTOMRESYNCH|  
|Retourner les insertions en attente|DBPROP_RETURNPENDINGINSERTS|  
|Notification de suppression de ligne|DBPROP_NOTIFYROWDELETE|  
|Notification de modification de première ligne|DBPROP_NOTIFYROWFIRSTCHANGE|  
|Notification d’insertion de ligne|DBPROP_NOTIFYROWINSERT|  
|Privilèges de ligne|DBPROP_ROWRESTRICT|  
|Notification de resynchronisation de ligne|DBPROP_NOTIFYROWRESYNCH|  
|Modèle de thread de ligne|DBPROP_ROWTHREADMODEL|  
|Notification de modification de ligne Annuler|DBPROP_NOTIFYROWUNDOCHANGE|  
|Notification de suppression de ligne Annuler|DBPROP_NOTIFYROWUNDODELETE|  
|Notification d’annulation Insert ligne|DBPROP_NOTIFYROWUNDOINSERT|  
|Notification de mise à jour de ligne|DBPROP_NOTIFYROWUPDATE|  
|Notification de modification de Position extraction de l’ensemble de lignes|DBPROP_NOTIFYROWSETFETCHPOSITIONCHANGE|  
|Notification de mise en production de l’ensemble de lignes|DBPROP_NOTIFYROWSETRELEASE|  
|Faites défiler vers l’arrière|DBPROP_CANSCROLLBACKWARDS|  
|Curseur côté serveur|DBPROP_SERVERCURSOR|  
|Ignorer les signets supprimés|DBPROP_BOOKMARKSKIPPED|  
|Forte identité de ligne|DBPROP_STRONGIDENTITY|  
|Catalogue unique|DBPROP_ADC_UNIQUECATALOG|  
|Lignes uniques|DBPROP_UNIQUEROWS|  
|Schéma unique|DBPROP_ADC_UNIQUESCHEMA|  
|Table unique|DBPROP_ADC_UNIQUETABLE|  
|Mise à jour|DBPROP_UPDATABILITY|  
|Critères de mise à jour|DBPROP_ADC_UPDATECRITERIA|  
|Resynchronisation de la mise à jour|DBPROP_ADC_UPDATERESYNC|  
|Utiliser des signets|DBPROP_BOOKMARKS|