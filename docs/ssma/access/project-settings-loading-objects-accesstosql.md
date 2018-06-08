---
title: Paramètres (chargement d’objets) du projet (AccessToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-access
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
ms.assetid: 9ec1c1e8-a3e1-4e81-bf49-631f87daa209
caps.latest.revision: 4
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: a12bdcd556504eaed7e0472b6e20e70f876b4275
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="project-settings-loading-objects-accesstosql"></a>Paramètres (chargement d’objets) du projet (AccessToSQL)
Les paramètres du projet du chargement des objets vous permettent de configurer le mode de synchronisation des objets de base de données Access avec des objets de base de données SQL Server.  
  
Les actions par défaut spécifient les paramètres par défaut pour l’actualisation des objets à partir de la base de données Access et pour la synchronisation des objets avec la base de données SQL Server. Pour plus d’informations, consultez [Actualiser à partir de la base de données &#40;AccessToSQL&#41;](../../ssma/access/refresh-from-database-accesstosql.md)  
  
Vous pouvez accéder à deux pages différentes synchronisation qui contiennent les mêmes paramètres :  
  
-   Pour spécifier les paramètres pour tous les futurs projets SSMA, sur le **outils** menu, cliquez sur **DefaultProject paramètres**, puis cliquez sur **le chargement des objets** au bas du volet gauche.  
  
-   Pour spécifier les paramètres pour le projet actuel, sur le **outils** menu, cliquez sur **les paramètres de projet**, puis cliquez sur **le chargement des objets** au bas du volet gauche.  
  
## <a name="options"></a>Options  
  
##### <a name="misc"></a>Divers  
  
##### <a name="attempts"></a>Tentatives  
Fournit les informations sur le nombre de passages prennent des objets à charger dans SQL Server. Chargement des objets dans SQL Server est généralement effectué en plusieurs passes. Les objets qui ne parviennent pas à charger lors du premier passage, telles que les clés étrangères, peuvent se charger correctement dans l’étape suivante.  
  
Par défaut, la valeur est 2.  
  
## <a name="synchronization-for-sql-server"></a>Synchronisation pour SQL Server  
  
##### <a name="default-action-on-local-and-remote-object-change"></a>Action par défaut sur la modification d’objet locale et à distance  
Spécifie le paramètre par défaut dans la boîte de dialogue de synchronisation lors de la définition de l’objet change dans SSMA et sur le serveur de base de données.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA charger les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
  
-   Si vous sélectionnez **écrire dans la base de données**, SSMA mettra à jour les objets dans la base de données en fonction du contenu des métadonnées SSMA lorsque la condition est remplie.  
  
-   Si vous sélectionnez **ignorer**, SSMA n’effectuera pas toutes les actions d’actualisation.  
  
##### <a name="default-action-on-local-object-change"></a>Action par défaut sur la modification de l’objet local  
Spécifie le paramètre par défaut dans la boîte de dialogue de synchronisation lorsque l’objet est modifié dans SSMA.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA charger les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
  
-   Si vous sélectionnez **écrire dans la base de données**, SSMA mettra à jour l’objet dans la base de données en fonction du contenu des métadonnées SSMA lorsque la condition est remplie.  
  
-   Si vous sélectionnez **ignorer**, SSMA n’effectuera pas toutes les actions d’actualisation.  
  
##### <a name="default-action-on-remote-object-change"></a>Action par défaut sur la modification de l’objet distant  
Spécifie le paramètre par défaut dans la boîte de dialogue synchronisation lorsque les objets sont modifiées sur le serveur de base de données.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA charger les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
  
-   Si vous sélectionnez **écrire dans la base de données**, SSMA mettra à jour l’objet dans la base de données en fonction du contenu des métadonnées SSMA lorsque la condition est remplie.  
  
-   Si vous sélectionnez **ignorer**, SSMA n’effectuera pas toutes les actions d’actualisation.  
  
##### <a name="default-action-when-local-object-metadata-is-missing"></a>Action par défaut lorsque les métadonnées de l’objet local sont manquante  
Spécifie le paramètre par défaut dans la boîte de dialogue de synchronisation lorsqu’il manque des métadonnées locales.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA sélectionne l’actualisation à partir de l’option de base de données lorsque la condition est remplie.  
  
-   Si vous sélectionnez **écrire dans la base de données**, SSMA supprime l’objet à partir de la base de données lorsque la condition est remplie.  
  
-   Si vous sélectionnez **ignorer**, SSMA n’effectuera pas toutes les actions d’actualisation.  
  
