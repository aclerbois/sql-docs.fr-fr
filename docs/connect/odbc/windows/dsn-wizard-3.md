---
title: Écran 3 (le pilote ODBC pour SQL Server) de l’Assistant Source de données | Documents Microsoft
ms.custom: ''
ms.date: 09/27/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 76326eeb-1144-4b9f-85db-50524c655d30
caps.latest.revision: 22
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: bbbedb76089ffb508f6ce521bd831b213ba90fc5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-source-wizard-screen-3"></a>3 de l’écran de l’Assistant Source de données

Spécifiez la base de données par défaut, différentes options ANSI à utiliser par le pilote, et le nom d'un serveur miroir.

## <a name="options"></a>Options

### <a name="change-the-default-database-to"></a>Modifier la base de données par défaut

Indique le nom de la base de données par défaut pour toute connexion faite à l'aide de cette source de données. Lorsque cette zone est désactivée, les connexions utilisent la base de données par défaut définie pour l'ID de connexion sur le serveur. Lorsque cette zone est activée, la base de données nommée dans la zone remplace la base de données définie par défaut pour l'ID de connexion. Si le **joindre le nom de fichier de base de données** zone porte le nom d’un fichier primaire, la base de données décrite par le fichier primaire est attaché en tant qu’une base de données en utilisant le nom de la base de données spécifié dans le **remplacer la base de données par défaut**boîte.

Il est plus efficace d'utiliser la base de données par défaut comme ID de connexion que de spécifier une base de données par défaut dans la source de données ODBC.

### <a name="mirror-server"></a>Serveur miroir

Indique le nom du partenaire de basculement de la base de données à mettre en miroir. Si un nom de base de données n’est pas affiché dans le **changer la base de données par défaut** zone ou le nom affiché de la base de données par défaut est **serveur miroir** est grisée.

Facultativement, vous pouvez spécifier un nom de principal du serveur (SPN) pour le serveur miroir. Le SPN du serveur miroir est utilisé pour l'authentification mutuelle entre client et serveur.

### <a name="attach-database-filename"></a>Joindre le nom de fichier de base de données

Indique le nom du fichier primaire d'une base de données joignable. Cette base de données est jointe et utilisée comme base de données par défaut pour la source de données. Spécifiez le chemin d'accès complet du fichier primaire. Le nom de la base de données spécifié dans le **changer la base de données par défaut** boîte est utilisée comme nom de la base de données attachée.

### <a name="use-ansi-quoted-identifiers"></a>Utiliser des identificateurs ANSI entre guillemets

Spécifie que QUOTED_IDENTIFIERS est activé lorsque le pilote ODBC pour SQL Server se connecte. Lorsque cette case à cocher est activée, SQL Server applique les règles ANSI concernant les guillemets. Les guillemets doubles ne doivent être utilisés que pour les identificateurs, tels que les noms de colonne et de table. Les chaînes de caractères doivent apparaître entre guillemets.

```
SELECT "LastName"
FROM "Person.Contact"
WHERE "LastName" = 'O''Brien'
```

Lorsque cette case à cocher est désactivée, les applications qui utilisent des identificateurs entre guillemets, tels que l'utilitaire Microsoft Query qui est fourni avec Microsoft Excel, rencontrent des erreurs lorsqu'ils génèrent des instructions SQL avec des identificateurs entre guillemets.

### <a name="use-ansi-nulls-paddings-and-warnings"></a>Utiliser les nulls, remplissages et avertissements ANSI

Spécifie que les options ANSI_NULLS, ANSI_WARNINGS et ANSI_PADDINGS doivent être définie sur lorsque le pilote ODBC pour SQL Server se connecte.

Avec l'option ANSI_NULLS activée, le serveur met en vigueur les règles ANSI concernant la comparaison des colonnes pour NULL. La syntaxe ANSI "IS NULL" ou "IS NOT NULL" doit être utilisée pour toutes les comparaisons NULL. La syntaxe Transact-SQL "= NULL" n'est pas prise en charge.

Avec l’option ANSI_WARNINGS est activée, SQL Server génère des messages d’avertissement pour les conditions qui violent les règles ANSI mais qui ne violent pas les règles Transact-SQL. Les erreurs de ce type peuvent être des données tronquées lors de l'exécution d'une instruction INSERT ou UPDATE, ou la rencontre d'une valeur NULL pendant une fonction d'agrégation. 

Avec l’option ANSI_PADDING est activée, les espaces à droite sur **varchar** les valeurs et les zéros de fin sur **varbinary** valeurs ne sont pas supprimés automatiquement.

### <a name="application-intent"></a>Intention de l’application

Déclare le type de la charge de travail de l'application lors de la connexion à un serveur. Les valeurs possibles sont **ReadOnly** et **ReadWrite**.

### <a name="multi-subnet-failover"></a>Basculement de sous-réseaux multiples.

Si votre application se connecte à un groupe de disponibilité des récupération (groupes de disponibilité AlwaysOn) de d’urgence haute disponibilité (AG) sur des sous-réseaux différents, l’activation **basculement de sous-réseaux multiples.** Configure le pilote ODBC pour SQL Server pour fournir une détection plus rapide et une connexion au serveur (actuellement) actif.

### <a name="transparent-network-ip-resolution"></a>Résolution IP de réseau transparent.

Modifie le comportement de **basculement de sous-réseaux multiples** pour permettre une reconnexion plus rapide au cours du basculement. Consultez [à l’aide de résolution IP de réseau Transparent](../../../connect/odbc/using-transparent-network-ip-resolution.md) pour plus d’informations.

### <a name="column-encryption"></a>Chiffrement de colonne.

Permet le déchiffrement automatique et le chiffrement des transferts de données vers et à partir de colonnes chiffrées avec la [Always Encrypted](../../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md) fonctionnalité disponible dans SQL Server 2016 et versions ultérieure.

### <a name="use-fmtonly-metadata-discovery"></a>Utilisez la découverte des métadonnées FMTONLY :

Utilisez la méthode de découverte de métadonnées SET FMTONLY héritée lors de la connexion à SQL Server 2012 ou version ultérieure. Activer cette option uniquement lorsque vous utilisez des requêtes non pris en charge par [sp_describe_first_result_set](../../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md), telles que celles contenant des tables temporaires. 

### <a name="next"></a>Suivant

Passe à l’écran suivant de l’Assistant.

### <a name="back"></a>Précédent

Retourne à l’écran précédent de l’Assistant.

## <a name="next-steps"></a>Étapes suivantes

[2 de l’écran de l’Assistant Source de données](../../../connect/odbc/windows/dsn-wizard-2.md)

[Écran Assistant Source de données 4](../../../connect/odbc/windows/dsn-wizard-4.md)
