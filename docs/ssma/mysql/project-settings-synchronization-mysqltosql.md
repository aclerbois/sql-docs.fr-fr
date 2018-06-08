---
title: Paramètres (synchronisation) (MySQLToSQL) du projet | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-mysql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
ms.assetid: 42061ff7-e6e7-497b-a0d9-440b9cf5986c
caps.latest.revision: 3
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: d40f9d8fdab09b242143ee859b01a79a0974943b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="project-settings-synchronization-mysqltosql"></a>Paramètres du projet (synchronisation) (MySQLToSQL)
La synchronisation **paramètres du projet** vous permettent de configurer le mode de synchronisation des objets de base de données MySQL avec des objets de base de données SQL Server.  
  
Les actions par défaut spécifient les paramètres par défaut pour l’actualisation des objets à partir de la base de données MySQL et pour la synchronisation des objets avec la base de données SQL Server. Pour plus d’informations, consultez [Actualiser à partir de la base de données &#40;MySQLToSQL&#41;](../../ssma/mysql/refresh-from-database-mysqltosql.md)  
  
Vous pouvez accéder à deux pages différentes synchronisation qui contiennent les mêmes paramètres :  
  
-   Pour spécifier les paramètres pour tous les futurs projets SSMA, sur le **outils** menu, cliquez sur **DefaultProject paramètres**, puis cliquez sur **synchronisation** en bas du volet gauche.  
  
-   Pour spécifier les paramètres pour le projet actuel, sur le **outils** menu, cliquez sur **les paramètres de projet**, puis cliquez sur **synchronisation** en bas du volet gauche.  
  
## <a name="options"></a>Options  
  
##### <a name="misc"></a>Divers  
  
##### <a name="attempts"></a>Tentatives  
Fournit les informations sur le nombre de passages prennent des objets à charger dans SQL Server. Chargement des objets dans SQL Server est généralement effectué en plusieurs passes. Les objets qui ne parviennent pas à charger lors du premier passage, telles que les clés étrangères, peuvent se charger correctement dans l’étape suivante.  
  
Par défaut, la valeur est 2.  
  
## <a name="synchronization-for-mysql"></a>Synchronisation pour MySQL  
  
##### <a name="action-on-local-and-remote-object-change"></a>Action de modification de l’objet locaux et distants  
Spécifie le paramètre par défaut dans la boîte de dialogue de synchronisation lors de la définition de l’objet change dans SSMA et sur le serveur de base de données.  
  
-   Si vous sélectionnez Actualiser à partir de la base de données, SSMA charger les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
  
-   Si vous sélectionnez Ignorer, SSMA n’effectuera pas toutes les actions d’actualisation.  
  
##### <a name="action-on-local-object-change"></a>Action de modification de l’objet local  
Spécifie le paramètre par défaut dans la boîte de dialogue de synchronisation lorsque l’objet est modifié dans SSMA.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA charger les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
  
-   Si vous sélectionnez **ignorer**, SSMA n’effectuera pas toutes les actions d’actualisation.  
  
##### <a name="action-on-remote-object-change"></a>Action de modification de l’objet distant  
Spécifie le paramètre par défaut dans la boîte de dialogue synchronisation lorsque les objets sont modifiées sur le serveur de base de données.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA charger les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
  
-   Si vous sélectionnez **ignorer**, SSMA n’effectuera pas toutes les actions d’actualisation.  
  
##### <a name="action-when-local-object-metadata-is-missing"></a>Action en cas de métadonnées de l’objet local sont manquante  
Spécifie le paramètre par défaut dans la boîte de dialogue synchronisation lorsque les métadonnées locales sont manquante.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA SSMA charger les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
  
-   Si vous sélectionnez **ignorer**, SSMA n’effectuera pas toutes les actions d’actualisation  
  
## <a name="synchronization-for-sql-server"></a>Synchronisation pour SQL Server  
  
##### <a name="action-on-local-and-remote-object-change"></a>Action de modification d’objet locale et à distance  
Spécifie le paramètre par défaut dans la boîte de dialogue de synchronisation lors de la définition de l’objet change dans SSMA et sur le serveur de base de données.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA charger les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
  
-   Si vous sélectionnez **écrire dans la base de données**, SSMA mettra à jour les objets dans la base de données en fonction du contenu des métadonnées SSMA lorsque la condition est remplie.  
  
-   Si vous sélectionnez **ignorer**, SSMA n’effectuera pas toutes les actions d’actualisation.  
  
##### <a name="action-on-local-object-change"></a>Action de modification de l’objet local  
Spécifie le paramètre par défaut dans la boîte de dialogue de synchronisation lorsque l’objet est modifié dans SSMA.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA charger les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
  
-   Si vous sélectionnez **écrire dans la base de données**, SSMA mettra à jour l’objet dans la base de données en fonction du contenu des métadonnées SSMA lorsque la condition est remplie.  
  
-   Si vous sélectionnez **ignorer**, SSMA n’effectuera pas toutes les actions d’actualisation.  
  
##### <a name="action-on-remote-object-change"></a>Action de modification de l’objet distant  
Spécifie le paramètre par défaut dans la boîte de dialogue synchronisation lorsque les objets sont modifiées sur le serveur de base de données.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA charger les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
  
-   Si vous sélectionnez **écrire dans la base de données**, SSMA mettra à jour l’objet dans la base de données en fonction du contenu des métadonnées SSMA lorsque la condition est remplie.  
  
-   Si vous sélectionnez **ignorer**, SSMA n’effectuera pas toutes les actions d’actualisation.  
  
##### <a name="action-when-local-object-metadata-is-missing"></a>Action en cas de métadonnées de l’objet local sont manquante  
Spécifie le paramètre par défaut dans la boîte de dialogue de synchronisation lorsqu’il manque des métadonnées locales.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA sélectionne l’actualisation à partir de l’option de base de données lorsque la condition est remplie.  
  
-   Si vous sélectionnez **écrire dans la base de données**, SSMA supprime l’objet à partir de la base de données lorsque la condition est remplie.  
  
-   Si vous sélectionnez **ignorer**, SSMA n’effectuera pas toutes les actions d’actualisation.  
  
