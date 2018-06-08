---
title: Copie de données vers SQL Server sur Linux | Documents Microsoft
description: ''
author: rothja
ms.author: jroth
manager: craigg
ms.date: 01/30/2018
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.assetid: 7b93d0d7-7946-4b78-b33a-57d6307cdfa9
ms.openlocfilehash: d792a7dfd4481d5c5cce3e2517559dcb0b3e22be
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="bulk-copy-data-with-bcp-to-sql-server-on-linux"></a>Copie de données en bloc avec bcp vers/depuis SQL Server sous Linux

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Cet article explique comment utiliser le [bcp](../tools/bcp-utility.md) utilitaire de ligne de commande pour copier en bloc des données entre une instance de 2017 du serveur SQL sur Linux et un fichier de données dans un format spécifié par l’utilisateur.

Vous pouvez utiliser `bcp` pour importer un grand nombre de lignes dans des tables SQL Server ou pour exporter des données à partir de tables SQL Server dans des fichiers de données. Sauf lorsqu’il est utilisé avec l’option queryout, `bcp` ne nécessite aucune connaissance de Transact-SQL. L'utilitaire de ligne de commande `bcp` fonctionne avec Microsoft SQL Server en local ou dans le cloud, sur Linux, Windows ou Docker et sur Azure SQL Database et Azure SQL Data Warehouse.

Cet article vous montre comment à :
- Importer des données dans une table à l’aide de la commande `bcp in`.
- Exporter les données d’une table à l’aide de la `bcp out` commande

## <a name="install-the-sql-server-command-line-tools"></a>Installer les outils de ligne de commande SQL Server

`bcp` fait partie des outils de ligne de commande SQL Server, qui ne sont pas installés automatiquement avec SQL Server sur Linux. Si vous n’avez pas déjà installé les outils de ligne de commande de SQL Server sur l’ordinateur Linux, vous devez les installer. Pour plus d’informations sur la façon d’installer les outils, sélectionnez votre distribution Linux à partir de la liste suivante :

- [Red Hat Enterprise Linux (RHEL)](sql-server-linux-setup-tools.md#RHEL)
- [Ubuntu](sql-server-linux-setup-tools.md#ubuntu)
- [SUSE Linux Enterprise Server (SLES)](sql-server-linux-setup-tools.md#SLES)

## <a name="import-data-with-bcp"></a>Importer des données avec bcp

Dans ce didacticiel, vous créez une base de données exemple et une table sur l’instance de SQL Server locale (**localhost**), puis utilisez `bcp` à charger dans la table d’exemple à partir d’un fichier texte sur le disque.

### <a name="create-a-sample-database-and-table"></a>Créer un exemple de base de données et de table

Commençons par la création d’une base de données exemple avec un simple tableau qui est utilisé dans le reste de ce didacticiel.

1. Ouvrez un terminal de commande sur votre ordinateur Linux.

2. Copiez et collez les commandes suivantes dans la fenêtre de Terminal Server. Ces commandes utilisent le **sqlcmd** utilitaire de ligne de commande pour créer une base de données exemple (**BcpSampleDB**) et une table (**TestEmployees**) sur l’instance de SQL Server locale (**localhost**). N’oubliez pas de remplacer le `username` et `<your_password>` si nécessaire avant d’exécuter les commandes.

Créer la base de données **BcpSampleDB**:
```bash 
sqlcmd -S localhost -U sa -P <your_password> -Q "CREATE DATABASE BcpSampleDB;"
```
Créer la table **TestEmployees** dans la base de données **BcpSampleDB**:
```bash 
sqlcmd -S localhost -U sa -P <your_password> -d BcpSampleDB -Q "CREATE TABLE TestEmployees (Id INT IDENTITY(1,1) NOT NULL PRIMARY KEY, Name NVARCHAR(50), Location NVARCHAR(50));"
```
### <a name="create-the-source-data-file"></a>Créez le fichier de données source
Copiez et collez la commande suivante dans votre fenêtre de Terminal Server. Nous utilisons la fonction intégrée `cat` commande pour créer un fichier de données texte avec trois enregistrements enregistrez le fichier dans votre répertoire de base en tant que **~/test_data.txt**. Les champs dans les enregistrements sont délimités par une virgule.

```bash
cat > ~/test_data.txt << EOF
1,Jared,Australia
2,Nikita,India
3,Tom,Germany
EOF
```

Vous pouvez vérifier que le fichier de données a été correctement créé en exécutant la commande suivante dans la fenêtre de Terminal Server :
```bash 
cat ~/test_data.txt
```

Les éléments suivants doivent s'afficher dans la fenêtre de terminal :
```bash
1,Jared,Australia
2,Nikita,India
3,Tom,Germany
```

### <a name="import-data-from-the-source-data-file"></a>Importer des données à partir du fichier de données source
Copiez et collez les commandes suivantes dans la fenêtre de Terminal Server. Cette commande utilise `bcp` pour se connecter à l’instance de SQL Server locale (**localhost**) et importer les données à partir du fichier de données (**~/test_data.txt**) dans la table (**TestEmployees**) dans la base de données (**BcpSampleDB**). N’oubliez pas de remplacer le nom d’utilisateur et `<your_password>` si nécessaire avant d’exécuter les commandes.

```bash 
bcp TestEmployees in ~/test_data.txt -S localhost -U sa -P <your_password> -d BcpSampleDB -c -t  ','
```

Voici une brève vue d’ensemble des paramètres de ligne de commande que nous avons utilisé avec `bcp` dans cet exemple :
- `-S`: Spécifie l’instance de SQL Server à laquelle se connecter
- `-U`: Spécifie l’ID de connexion utilisé pour se connecter à SQL Server
- `-P`: Spécifie le mot de passe pour l’ID de connexion
- `-d`: Spécifie la base de données à laquelle se connecter
- `-c`: Effectue l'opération en utilisant un type de données caractères
- `-t`: Spécifie le séparateur en fin de champ. Nous utilisons `comma` comme indicateur de fin de champ pour les enregistrements dans notre fichier de données

> [!NOTE]
> Nous ne spécifions pas une marque de fin de ligne personnalisée dans cet exemple. Les lignes dans le fichier de données de texte ont été terminées correctement avec un `newline` lorsque nous avons utilisé la commande `cat` pour créer précédemment le fichier de données.

Vous pouvez vérifier que les données n’a été importées en exécutant la commande suivante dans la fenêtre de Terminal Server. N’oubliez pas de remplacer le `username` et `<your_password>` si nécessaire avant d’exécuter la commande.
```bash 
sqlcmd -S localhost -d BcpSampleDB -U sa -P <your_password> -I -Q "SELECT * FROM TestEmployees;"
```

Les résultats suivants doivent s'afficher :
```bash
Id          Name                Location
----------- ------------------- -------------------
          1 Jared               Australia
          2 Nikita              India
          3 Tom                 Germany

(3 rows affected)
```

## <a name="export-data-with-bcp"></a>Exporter des données avec bcp

Dans ce didacticiel, vous utilisez `bcp` pour exporter des données à partir de la table d’exemple que nous avons créées précédemment dans un fichier de données.

Copiez et collez les commandes suivantes dans la fenêtre de Terminal Server. Ces commandes utilisent le `bcp` utilitaire de ligne de commande pour exporter des données à partir de la table **TestEmployees** dans la base de données **BcpSampleDB** vers un nouveau fichier de données appelé **~/test_export.txt** .  N’oubliez pas de remplacer le nom d’utilisateur et `<your_password>` si nécessaire avant d’exécuter la commande.

```bash 
bcp TestEmployees out ~/test_export.txt -S localhost -U sa -P <your_password> -d BcpSampleDB -c -t ','
```

Vous pouvez vérifier que les données ont été correctement exportées en exécutant la commande suivante dans la fenêtre de Terminal Server :
```bash 
cat ~/test_export.txt
```

Les éléments suivants doivent s'afficher dans la fenêtre de terminal :
```
1,Jared,Australia
2,Nikita,India
3,Tom,Germany
```

## <a name="see-also"></a>Voir aussi
- [bcp (utilitaire)](../tools/bcp-utility.md)
- [Formats de données pour la compatibilité lors de l’utilisation de bcp](../relational-databases/import-export/specify-data-formats-for-compatibility-when-using-bcp-sql-server.md)
- [Importer des données en bloc à l’aide de BULK INSERT](../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md)
- [INSERTION en bloc (Transact-SQL)](../t-sql/statements/bulk-insert-transact-sql.md)
