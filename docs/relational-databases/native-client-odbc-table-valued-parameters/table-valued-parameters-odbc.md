---
title: Paramètres table (ODBC) | Documents Microsoft
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-table-valued-parameters
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- table-valued parameters (ODBC)
- ODBC, table-valued parameters
ms.assetid: ef06cd13-18e2-4c65-8ede-c3955d820e54
caps.latest.revision: 28
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 8f8c0f95a894b92c3522fa98d4022d28540d4a29
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34707297"
---
# <a name="table-valued-parameters-odbc"></a>Paramètres table (ODBC)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  La prise en charge des paramètres table par ODBC permet à une application cliente d'envoyer plus efficacement des données paramétrables au serveur par l'envoi de plusieurs lignes au serveur en un seul appel.  
  
 Pour plus d’informations sur les paramètres table sur le serveur, consultez [utiliser des paramètres &#40;moteur de base de données&#41;](../../relational-databases/tables/use-table-valued-parameters-database-engine.md).  
  
 Dans ODBC, vous pouvez envoyer des paramètres table au serveur de deux manières :  
  
-   Toutes les données de paramètre table peuvent être dans la mémoire à la fois SQLExecDirect ou SQLExecute est appelée. Ces données sont stockées dans des tableaux si la valeur de table contient plusieurs lignes.  
  
-   Une application peut spécifier data-at-execution pour un paramètre table lorsque SQLExecDirect ou SQLExecute est appelée. Dans ce cas, les lignes de données de la valeur de table peuvent être fournies par lots, ou une par une pour réduire la mémoire requise.  
  
 La première option permet aux procédures stockées de renfermer une logique métier plus importante. Par exemple, une même procédure stockée peut encapsuler une transaction d'enregistrement de commande entière lorsque les éléments de la commande sont passés sous la forme de paramètre table. Cette option est très efficace, car un seul aller-retour sur le serveur est requis. Vous pouvez également utiliser différentes procédures pour gérer séparément l'en-tête de commande et les éléments de commande, ce qui nécessiterait davantage de code et un contrat plus complexe entre le client et le serveur.  
  
 La seconde méthode propose un mécanisme efficace pour les opérations en bloc portant sur de grandes quantités de données. Une application peut en effet transmettre en continu des lignes de données au serveur sans devoir commencer par toutes les mettre en mémoire tampon.  
  
 Vous pouvez créer des contraintes et des clés primaires lorsque vous créez la variable de table. Les contraintes sont un bon moyen de veiller à ce que les données d'une table répondent à des spécifications particulières.  
  
## <a name="in-this-section"></a>Dans cette section  
 [Scénarios d’utilisation des paramètres table ODBC](../../relational-databases/native-client-odbc-table-valued-parameters/uses-of-odbc-table-valued-parameters.md)  
 Décrit les principaux scénarios utilisateur pour les paramètres table et ODBC.  
  
 [Type ODBC SQL pour les paramètres table](../../relational-databases/native-client-odbc-table-valued-parameters/odbc-sql-type-for-table-valued-parameters.md)  
 Décrit le type SQL_SS_TABLE. ll s'agit d'un nouveau type SQL ODBC qui prend en charge les paramètres table.  
  
 [Champs de descripteur de paramètres table](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameter-descriptor-fields.md)  
 Décrit les champs de descripteur qui prennent en charge les paramètres table.  
  
 [Champs de descripteur pour les colonnes constituantes des paramètres table](../../relational-databases/native-client-odbc-table-valued-parameters/descriptor-fields-for-table-valued-parameter-constituent-columns.md)  
 Décrit des champs de descripteur qui ont une signification pour les paramètres table.  
  
 [Champs d’enregistrement de diagnostic de paramètres table](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameter-diagnostic-record-fields.md)  
 Décrit deux champs de diagnostic qui ont été ajoutés aux enregistrements de diagnostic pour prendre en charge les paramètres table.  
  
 [Attributs d’instruction qui affectent des paramètres table](../../relational-databases/native-client-odbc-table-valued-parameters/statement-attributes-that-affect-table-valued-parameters.md)  
 Décrit un nouveau champ d'en-tête de descripteur qui permet aux colonnes de paramètres table d'être adressés.  
  
 [Liaison et transfert de paramètres table et de valeurs de colonnes](../../relational-databases/native-client-odbc-table-valued-parameters/binding-and-data-transfer-of-table-valued-parameters-and-column-values.md)  
 Décrit la liaison de paramètre et comment passer un paramètre table au serveur.  
  
 [Métadonnées de paramètres table pour les instructions préparées](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameter-metadata-for-prepared-statements.md)  
 Décrit comment une application peut obtenir des métadonnées pour un appel de procédure préparé.  
  
 [Métadonnées de paramètres table supplémentaires](../../relational-databases/native-client-odbc-table-valued-parameters/additional-table-valued-parameter-metadata.md)  
 Décrit comment utiliser SQLProcedureColumns SQLTables et SQLColumns pour récupérer des métadonnées pour un paramètre table.  
  
 [Conversion des données des paramètres table, et autres erreurs et avertissements](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameter-data-conversion-and-other-errors-and-warnings.md)  
 Décrit comment traiter les erreurs sur les valeurs de colonne de paramètre table.  
  
 [Compatibilité des versions](../../relational-databases/native-client-odbc-table-valued-parameters/cross-version-compatibility.md)  
 Décrit les conflits qui peuvent se produire lorsque des paramètres table sont utilisés par un client ou un serveur utilisant une version antérieure de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)].  
  
 [Récapitulatif des API de paramètre table ODBC](../../relational-databases/native-client-odbc-table-valued-parameters/odbc-table-valued-parameter-api-summary.md)  
 Répertorie les fonctions ODBC qui prennent en charge les paramètres table.  
  
 [Exemples de programmation du paramètre table ODBC](http://msdn.microsoft.com/library/3f52b7a7-f2bd-4455-b79e-d015fb397726)  
 Décrit comment réaliser des tâches courantes.  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server Native Client &#40;ODBC&#41;](../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)   
 [Paramètres table &#40;SQL Server Native Client&#41;](../../relational-databases/native-client/features/table-valued-parameters-sql-server-native-client.md)  
  
  
