---
title: Paramètres (synchronisation) (SybaseToSQL) du projet | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-sybase
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
ms.assetid: 2cd6bc01-b8e5-4312-83a4-eac66dc1d460
caps.latest.revision: 3
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 2dd0db2f5767c6d9a41f7a2c920139e3e324e3c0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="project-settings-synchronization-sybasetosql"></a>Paramètres du projet (synchronisation) (SybaseToSQL)
La page de synchronisation de la **les paramètres de projet** boîte de dialogue contient des paramètres permettant de personnaliser comment SSMA charge des objets de base de données, telles que les tables et procédures stockées, [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure.  
  
Vous pouvez accéder à deux pages différentes synchronisation qui contiennent les mêmes paramètres :  
  
-   Si vous souhaitez spécifier des paramètres pour tous les futurs projets SSMA, sur le **outils** menu, sélectionnez **les paramètres de projet par défaut**, sélectionnez le type de projet de migration pour lequel les paramètres sont requis pour être affichées ou modifiées à partir de **Version cible de la Migration** liste déroulante, puis sélectionnez **synchronisation** en bas du volet gauche.  
  
-   Pour spécifier les paramètres pour le projet actuel, sur le **outils** menu, sélectionnez **les paramètres de projet**, puis sélectionnez **synchronisation** en bas du volet gauche.  
  
## <a name="options"></a>Options  
**Tentatives**  
Spécifie le nombre de tentatives de SSMA doit lors du chargement d’objets dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Les objets qui ne sont pas chargés dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] lors de la tentative actuelle sera tentée à nouveau jusqu'à ce que SSMA atteint le nombre maximal de tentatives défini dans le processus de synchronisation en cours.  
  
## <a name="synchronization-for-sql-server"></a>Synchronisation pour SQL Server  
**Actualiser l’objet local de la modification de l’objet locaux et distants**  
Spécifie si SSMA doit remplacer les métadonnées de l’objet local avec les métadonnées de l’objet distant si les objets locaux et distants changent.  
Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA charger les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
Si vous sélectionnez **écrire dans la base de données**, SSMA mettra à jour les objets dans la base de données en fonction du contenu des métadonnées SSMA lorsque la condition est remplie.  
Si vous sélectionnez **ignorer**, SSMA n’effectuera pas toutes les actions d’actualisation.   
Ensemble d’option par défaut est **écrire dans la base de données.**  
  
**Actualiser l’objet local de la modification de l’objet local**  
Spécifie si SSMA doit remplacer les métadonnées de l’objet local avec les métadonnées de l’objet distant si les modifications de l’objet distant.  
Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA charger les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
Si vous sélectionnez **écrire dans la base de données**, SSMA mettra à jour l’objet dans la base de données en fonction du contenu des métadonnées SSMA lorsque la condition est remplie.  
Si vous sélectionnez **ignorer**, SSMA n’effectuera pas toutes les actions d’actualisation.   
Ensemble d’option par défaut est **écrire dans la base de données**.  
  
**Actualiser l’objet local de la modification de l’objet distant**  
Spécifie si SSMA doit remplacer les métadonnées de l’objet local avec les métadonnées de l’objet distant si les modifications de l’objet distant.  
Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA charger les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
Si vous sélectionnez **écrire dans la base de données**, SSMA mettra à jour l’objet dans la base de données en fonction du contenu des métadonnées SSMA lorsque la condition est remplie.  
Si vous sélectionnez **ignorer**, SSMA n’effectuera pas toutes les actions d’actualisation.   
Ensemble d’option par défaut est **Actualiser à partir de la base de données**.  
  
**Actualisation des métadonnées de l’objet local sont manquante**  
Spécifie si SSMA doit créer les métadonnées locales si un objet existe sur la base de données cible, mais pas localement.  
Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA sélectionne l’actualisation à partir de l’option de base de données lorsque la condition est remplie.  
Si vous sélectionnez **écrire dans la base de données**, SSMA supprime l’objet à partir de la base de données lorsque la condition est remplie.  
Si vous sélectionnez **ignorer**, SSMA n’effectuera pas toutes les actions d’actualisation.   
Ensemble d’option par défaut est **Actualiser à partir de la base de données**.  
  
