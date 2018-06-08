---
title: Projet Settings(Synchronization) (DB2ToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-db2
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
ms.assetid: a5629a72-8c17-46a4-bb4d-19d51a0b98a2
caps.latest.revision: 4
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 2ede393d2edf9c0fa9b2b31e0c6de28c6a2bdf97
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="project-settingssynchronization-db2tosql"></a>Projet Settings(Synchronization) (DB2ToSQL)
La page de synchronisation de la **les paramètres de projet** boîte de dialogue contient des paramètres permettant de personnaliser comment SSMA charge actualisations de la base de données et objets, tels que les tables et procédures stockées, en [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
Les options d’actions par défaut spécifient les paramètres par défaut pour l’actualisation des objets à partir de la base de données DB2 et pour la synchronisation des objets avec la base de données SQL Server. Pour plus d’informations, consultez [Actualiser à partir de la base de données &#40;DB2ToSQL&#41;](../../ssma/db2/refresh-from-database-db2tosql.md).  
  
Vous pouvez accéder à deux pages différentes synchronisation qui contiennent les mêmes paramètres :  
  
-   Pour spécifier les paramètres pour tous les futurs projets SSMA, sur le **outils** menu, cliquez sur **les paramètres de projet par défaut**, puis cliquez sur **synchronisation** en bas du volet gauche.  
  
-   Pour spécifier les paramètres pour le projet actuel, sur le **outils** menu, cliquez sur **les paramètres de projet**, puis cliquez sur **synchronisation** en bas du volet gauche.  
  
## <a name="miscellaneous-options"></a>Options diverses  
**Tentatives**  
Spécifie le nombre de tentatives de SSMA doit lors du chargement d’objets dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Les objets qui ne sont pas chargés dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] lors de la tentative actuelle sera tentée à nouveau jusqu'à ce que SSMA atteint le nombre maximal de tentatives défini dans le processus de synchronisation en cours. Ensemble de valeur par défaut est **2**  
  
## <a name="synchronization-for-db2-options"></a>Synchronisation pour les Options de DB2  
**Action de modification de l’objet locaux et distants**  
Spécifie le paramètre par défaut dans la boîte de dialogue de synchronisation lors de la définition de l’objet change dans SSMA et sur le serveur de base de données. Ensemble de valeur par défaut est **Actualiser à partir de la base de données**.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA charger les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
  
-   Si vous sélectionnez **ignorer**, SSMA n’effectuera pas toutes les actions d’actualisation.  
  
**Action de modification de l’objet local**  
Spécifie le paramètre par défaut dans la boîte de dialogue de synchronisation lorsque l’objet est modifié dans SSMA. Ensemble de valeur par défaut est **ignorer**.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA charger les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
  
-   Si vous sélectionnez **ignorer**, SSMA n’effectuera pas toutes les actions d’actualisation.  
  
**Action de modification de l’objet distant**  
Spécifie le paramètre par défaut dans la boîte de dialogue synchronisation lorsque les objets sont modifiées sur le serveur de base de données. Ensemble de valeur par défaut est **Actualiser à partir de la base de données**.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA charger les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
  
-   Si vous sélectionnez **ignorer**, SSMA n’effectuera pas toutes les actions d’actualisation.  
  
**Action en cas de métadonnées de l’objet local sont manquante**  
Spécifie le paramètre par défaut dans la boîte de dialogue de synchronisation lorsqu’il manque des métadonnées locales. Ensemble de valeur par défaut est **Actualiser à partir de la base de données**.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA SSMA charger les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
  
-   Si vous sélectionnez **ignorer**, SSMA n’effectuera pas toutes les actions d’actualisation.  
  
## <a name="synchronization-for-sql-server-options"></a>Synchronisation pour les Options de SQL Server  
**Action de modification de l’objet locaux et distants**  
Spécifie le paramètre par défaut dans la boîte de dialogue de synchronisation lors de la définition de l’objet change dans SSMA et sur le serveur de base de données. Ensemble de valeur par défaut est **écriture à la base de données**.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA charger les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
  
-   Si vous sélectionnez **écrire dans la base de données**, SSMA mettra à jour les objets dans la base de données en fonction du contenu des métadonnées SSMA lorsque la condition est remplie.  
  
-   Si vous sélectionnez **ignorer**, SSMA n’effectuera pas toutes les actions d’actualisation.  
  
**Action de modification de l’objet local**  
Spécifie le paramètre par défaut dans la boîte de dialogue de synchronisation lorsque l’objet est modifié dans SSMA. Ensemble de valeur par défaut est **écriture à la base de données**.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA charger les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
  
-   Si vous sélectionnez **écrire dans la base de données**, SSMA mettra à jour l’objet dans la base de données en fonction du contenu des métadonnées SSMA lorsque la condition est remplie.  
  
-   Si vous sélectionnez **ignorer**, SSMA n’effectuera pas toutes les actions d’actualisation.  
  
**Action de modification de l’objet distant**  
Spécifie le paramètre par défaut dans la boîte de dialogue synchronisation lorsque les objets sont modifiées sur le serveur de base de données.  Ensemble de valeur par défaut est **Actualiser à partir de la base de données**.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA charger les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
  
-   Si vous sélectionnez **écrire dans la base de données**, SSMA mettra à jour l’objet dans la base de données en fonction du contenu des métadonnées SSMA lorsque la condition est remplie.  
  
-   Si vous sélectionnez **ignorer**, SSMA n’effectuera pas toutes les actions d’actualisation.  
  
**Action en cas de métadonnées de l’objet local sont manquante**  
Spécifie le paramètre par défaut dans la boîte de dialogue de synchronisation lorsqu’il manque des métadonnées locales. Ensemble de valeur par défaut est **Actualiser à partir de la base de données**.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA sélectionne le **Actualiser à partir de la base de données** option lorsque la condition est remplie.  
  
-   Si vous sélectionnez **écrire dans la base de données**, SSMA supprime l’objet à partir de la base de données lorsque la condition est remplie.  
  
-   Si vous sélectionnez **ignorer**, SSMA n’effectuera pas toutes les actions d’actualisation.  
  
