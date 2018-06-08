---
title: Nouveautés de SSMA pour DB2 (DB2ToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-db2
ms.custom: ''
ms.date: 03/01/2018
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
ms.assetid: 1cc38f85-3caa-42d0-8c76-a380c1d15c67
caps.latest.revision: 8
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: b394ff1a8d97fcc91d81a122e620f3e1db1cf243
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="whats-new-in-ssma-for-db2-db2tosql"></a>Nouveautés de SSMA pour DB2 (DB2ToSQL)
Cette rubrique répertorie SSMA pour DB2 les modifications dans chaque version.  

## <a name="ssma-v77"></a>SSMA v7.7
La version v7.7 de SSMA pour DB2 contient les modifications suivantes :
- SSMA pour DB2 a été améliorée avec des correctifs ciblés améliorent les mesures de qualité et de conversion.
- En fonction de la forte demande, la version 32 bits de SSMA pour DB2 est de retour. Par rapport à l’implémentation précédente (avant v7.4), il existe deux packages de programme d’installation, mais ils ne peuvent pas être installés côte à côte. Par conséquent, vous devez choisir la version la plus appropriée en fonction des composants de connectivité que vous avez. Il est toujours préférable d’utiliser la version 64 bits, si possible.

> [!IMPORTANT]
> SSMA v7.4 et versions ultérieures, .net 4.5.2 est un préalable de l’installation.

## <a name="ssma-v76"></a>SSMA v7.6
La version v7.6 de SSMA pour DB2 a été améliorée avec des correctifs ciblés améliorent les mesures de qualité et de conversion et prise en charge de SQL Server 2017 (version préliminaire publique). Prise en charge de 2017 du serveur SQL sur Windows et Linux en version préliminaire publique et ne doit pas être utilisé pour les migrations de production.

> [!IMPORTANT]
> SSMA v7.4 et versions ultérieures, .net 4.5.2 est un préalable de l’installation, et la version 32 bits de l’outil a été supprimée.

## <a name="ssma-v75"></a>Versions 7.5 SSMA
La version de versions 7.5 de SSMA pour DB2 a été améliorée avec plusieurs améliorations pour garantir une accessibilité supérieure aux personnes handicapées.

> [!IMPORTANT]
> .NET 4.5.2 est requis pour l’installation de versions 7.5 SSMA. En outre, à partir de v7.4, la version 32 bits de SSMA est interrompue.

## <a name="ssma-v74"></a>SSMA v7.4
La version v7.4 de SSMA pour DB2 contient les modifications suivantes :
- Le **délai de requête** option est désormais disponible pendant la détection des objets de schéma à la source et cible.
![option de délai d’attente de requête](../media/query-timeout_red.png)

- Les mesures de qualité et de conversion a été améliorée avec des correctifs ciblés, en fonction des commentaires des clients.

> [!IMPORTANT]
> .NET 4.5.2 est requis pour l’installation de SSMA v7.4. En outre, à partir de v7.4, la version 32 bits de SSMA est interrompue.

## <a name="ssma-v73"></a>SSMA v7.3
La version v7.3 de SSMA pour DB2 contient les modifications suivantes :
- Métrique de qualité et de conversion améliorée avec des correctifs ciblés en fonction des commentaires des clients.
- Infrastructure d’extensibilité SSMA exposé via les éléments suivants :
  - Fonctionnalité d’exportation à un projet SQL Server Data Tools (SSDT).
    -   Vous pouvez désormais exporter des scripts de schéma à partir de SSMA pour un projet SSDT. Vous pouvez utiliser les scripts de schéma pour apporter des modifications de schéma supplémentaires et de déployer votre base de données.
![Enregistrer en tant que commande de projet SSDT](../media/export-schema-scripts_red.png)
  - Bibliothèques peuvent être consommées par SSMA pour effectuer des conversions personnalisées.
    - Vous pouvez maintenant construire le code qui peut gérer les conversions de syntaxe personnalisée et les conversions qui n’ont pas été précédemment traitées par SSMA.
      - Obtenir des instructions sur la création d’un convertisseur personnalisé sont disponibles dans ce billet de blog, [fonctions de conversion d’extension Assistant Migration SQL Server](https://blogs.msdn.microsoft.com/datamigration/2017/02/21/2185/).
      - Exemple de projet pour la conversion peut être le télécharger [billet de blog](https://blogs.msdn.microsoft.com/datamigration/ssmafororacleconversionsample/).

## <a name="ssma-v72"></a>SSMA v7.2
La version v7.2 de SSMA pour DB2 contient les modifications suivantes :
- Métrique de qualité et de conversion améliorée avec des correctifs ciblés en fonction des commentaires des clients.
- Améliorations de télémétrie pour fournir une meilleure points de données pour résoudre les problèmes de client et d’améliorer le taux de conversion de SSMA.

## <a name="ssma-v71"></a>SSMA v7.1
La version de v7.1 de SSMA pour Access contient les modifications suivantes :
- 2017 du serveur SQL sur Windows et Linux CTP1 est désormais une plateforme cible prise en charge pour la migration. Cette fonctionnalité est dans technical preview et permet le déplacement de schéma et les données pour les serveurs de SQL Server cible.
- SSMA prend désormais en charge les mises à jour automatiques pour télécharger la dernière version de SSMA dès qu’elles sont disponibles.
- Fichiers binaires installables de SSMA sont maintenant remis via des fichiers de package Windows installer (.msi).

**Ressources**

[Extension des capacités de SQL Server Migration Assistant conversion](https://blogs.msdn.microsoft.com/datamigration/2017/02/21/2185/)

[Évaluer et migrer des données à partir de plateformes de données non Microsoft pour SQL Server *(avec un exemple d’Oracle)*](https://blogs.msdn.microsoft.com/datamigration/2016/11/16/sql-server-migration-assistant-how-to-assess-and-migrate-databases-from-non-microsoft-data-platforms-to-sql-server/) 

## <a name="may-2016"></a>Mai 2016  
La version de mai 2016 de SSMA pour DB2 contient les modifications suivantes :  

-  Ajout de la prise en charge pour SQL Server 2016.
-  Conversion ajoutée des tables en mémoire et régulières de DB2 aux fonctionnalités en mémoire et hekaton de SQL Server.
-  Conversion ajoutée DB2 des contrôles d’accès aux objets de stratégie de SQL Server (sécurité de niveau ligne pour DB2).
-  Ajouté une conversion des tables avec version système DB2 aux tables temporelles de SQL Server.
-  Améliorée d’analyseur de DB2 et de programme de résolution.
-  Supprimé vérification du programme d’installation pour .net 2.0.
-  Supprimé *.dll inutiles de programme d’installation de Db2.
-  Fixe « enregistrer le projet » et « ouvrir le projet » des commandes pour la Console de SSMA.
-  Commande securepassword « fixe » pour la Console de SSMA.
-  Correction de comptage des objets pour le chargement initial.
-  Résolution du bogue dans les paramètres globaux.
  
## <a name="march-2016"></a>Mars 2016  
La version préliminaire de mars 2016 de SSMA pour DB2 contient les modifications suivantes :  
  
-  Prise en charge supplémentaire pour la migration vers SQL Server 2016.  
  
## <a name="january-2016"></a>Janvier 2016  
La version de maintenance de janvier 2016 de SSMA pour DB2 contient les modifications suivantes :  
  
-  Prise en charge supplémentaire pour un nombre de fonctions standard.  
-  Correction des erreurs de l’analyseur DB2.  
-  Fixe DB2 v9 zOS prise en charge (RFC 5690920).  
-  DB2 fixe non résolue à des erreurs de l’identificateur lors de la conversion.  
-  Élément de Menu ajouté vue journal de SSMA (RFC 5706203).  
-  Ajout de télémétrie.
  
## <a name="november-2014"></a>Novembre 2014  
La version de novembre 2014 de SSMA pour DB2 était la version initiale.
