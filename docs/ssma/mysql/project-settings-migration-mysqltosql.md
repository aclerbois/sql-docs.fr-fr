---
title: Paramètres (Migration) (MySQLToSQL) du projet | Documents Microsoft
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
ms.assetid: 2a3cba9e-cd54-4a8b-b858-8fc4cf2580d9
caps.latest.revision: 14
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: b540d746440dd699821c2105b8d1f50ac5a3a831
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="project-settings-migration-mysqltosql"></a>Paramètres du projet (Migration) (MySQLToSQL)
La page de la Migration de le **les paramètres de projet** boîte de dialogue contient des paramètres permettant de personnaliser comment SSMA migre les données de MySQL vers SQL Server.  
  
Le volet de la Migration est disponible dans le **les paramètres de projet** et **les paramètres de projet par défaut** boîtes de dialogue.  
  
-   Pour spécifier les paramètres pour tous les projets SSMA, sur le **outils** menu, sélectionnez **les paramètres de projet par défaut**, sélectionnez le type de projet dans **Version cible de la Migration** liste déroulante de laquelle vous souhaitez accéder aux paramètres, cliquez sur **général** au bas de la gauche, puis cliquez sur **Migration**.  
  
-   Pour spécifier les paramètres pour le projet actuel, sur le **outils** menu, sélectionnez **les paramètres de projet**, cliquez sur **général** au bas de la gauche, puis cliquez sur **Migration**.  
  
## <a name="options"></a>Options  
  
### <a name="bulk-copy"></a>Copie en bloc  
  
|Terme|Définition|  
|--------|--------------|  
|**Taille de lot**|Spécifie le lot taille utilisée pendant la migration des données.<br /><br />**Mode par défaut**: 1000<br /><br />**Mode optimisé**: 1000<br /><br />**Mode plein**: 1000|  
|**Contraintes de validation**|Spécifie si SSMA doit vérifier les contraintes lorsqu’il insère des données dans des tables SQL Server.<br /><br />**Mode par défaut**: False<br /><br />**Mode optimisé**: False<br /><br />**Mode plein**: False|  
|**Exécuter les déclencheurs**|Spécifie si SSMA doit exécuter les déclencheurs d’insertion lorsqu’il ajoute des données dans des tables SQL Server.<br /><br />**Mode par défaut**: False<br /><br />**Mode optimisé**: False<br /><br />**Mode plein**: False|  
|**Conserver l'identité**|Spécifie si SSMA conserve les valeurs d’identité MySQL lorsqu’il ajoute des données à SQL Server. La valeur False entraîne des valeurs d’identité être affectée par la destination.<br /><br />**Mode par défaut**: True<br /><br />**Mode optimisé**: True<br /><br />**Mode plein**: True|  
|**Conserver les valeurs NULL**|Spécifie si SSMA conserve les valeurs null dans la source de données lorsqu’il ajoute des données à SQL Server, quel que soit les valeurs par défaut qui sont spécifiés dans SQL Server.<br /><br />**Mode par défaut**: True<br /><br />**Mode optimisé**: True<br /><br />**Mode plein**: True|  
|**Verrou de table**|Spécifie si SSMA verrouille les tables lorsqu’il ajoute des données aux tables lors de la migration de données. Obtient un verrou de mise à jour en bloc pour la durée de l’opération de copie en bloc. Si la valeur est False, un verrou est défini au niveau de la ligne.<br /><br />**Mode par défaut**: False<br /><br />**Mode optimisé**: False<br /><br />**Mode plein**: False|  
  
### <a name="data-modification"></a>Modification de données  
  
|Terme|Définition|  
|--------|--------------|  
|**Migration de Dates non valides**|Spécifie comment migrer les dates non valides avec comme ' 2007-04-23' ou ' 2000-06-31 10:00:00 » dans les formats de DATE et DATETIME.<br /><br />**Mode par défaut**: valeur NULL<br /><br />**Mode optimisé**: valeur NULL<br /><br />**Mode plein**: valeur NULL|  
|**Valeurs d’heure négatives Migration**|Spécifie comment migrer les valeurs négatives comme «-30:11:00 » dans les colonnes de temps.<br /><br />**Mode par défaut**: valeur NULL<br /><br />**Mode optimisé**: valeur NULL<br /><br />**Mode plein**: valeur NULL|  
|**Valeurs d’heure plus de 24 heures de Migration**|Spécifie comment migrer les valeurs d’heure de plus de « 23 : 59:59 » dans les colonnes de temps.<br /><br />**Mode par défaut**: valeur NULL<br /><br />**Mode optimisé**: valeur NULL<br /><br />**Mode plein**: valeur NULL|  
|**Tronquer les valeurs binaires pour tenir dans la colonne**|Si Oui, SSMA tronque les valeurs binaires de MySQL qui ne tiennent pas dans les colonnes de la table SQL et génère le message d’erreur approprié. Si non, la ligne provoque une erreur<br /><br />**Mode par défaut**: non<br /><br />**Mode optimisé**: non<br /><br />**Mode plein**: non|  
|**Tronquer les valeurs de caractère pour tenir dans la colonne**|SSMA tronque les valeurs de caractère à partir de MySQL qui ne tiennent pas dans les colonnes de la table SQL et génère le message d’erreur approprié.<br /><br />**Mode par défaut**: non<br /><br />**Mode optimisé**: non<br /><br />**Mode plein**: non|  
|**Aucune Migration de Dates**|Spécifie comment migrer zéro dates comme « 0000-00-00' ou ' 0000-00-00 00:00:00 » dans les colonnes DATE et DATETIME.<br /><br />**Mode par défaut**: valeur NULL<br /><br />**Mode optimisé**: valeur NULL<br /><br />**Mode plein**: valeur NULL|  
|**Un zéro dans la Migration de Dates**|Spécifie comment migrer les dates avec zéro parties comme « 2009-01-00' ou ' 2000-00-00 11:00:00 » dans les colonnes DATE et DATETIME.<br /><br />**Mode par défaut**: valeur NULL<br /><br />**Mode optimisé**: valeur NULL<br /><br />**Mode plein**: valeur NULL|  
  
### <a name="migration-engine"></a>Moteur de migration  
  
|Terme|Définition|  
|--------|--------------|  
|**Moteur de migration**|Spécifie le moteur de base utilisé lors de la Migration de données. Migration des données côté client fait référence au client SSMA récupérer les données de la source et l’insertion en bloc ces données dans SQL Server. Migration des données côté serveur fait référence au moteur de migration de données SSMA (programme de copie en bloc) en cours d’exécution dans la boîte de SQL Server en tant qu’un travail de l’Agent SQL, la récupération des données à partir de la source et insérant directement dans SQL Server, ce qui évite un supplémentaire client-saut (performances).<br /><br />**Mode par défaut**: moteur de Migration de données côté Client<br /><br />**Mode optimisé**: moteur de Migration de données côté Client<br /><br />**Mode plein**: moteur de Migration de données côté Client|  
  
> [!IMPORTANT]  
> Lorsque le **moteur de Migration** option est définie sur **moteur de Migration de données côté serveur**, un nouveau projet de définition d’option **moteur de Migration de données côté serveur utilisez 32 bits** s’affiche. Il spécifie si l’utilitaire de programme de copie en bloc (BCP) 32 bits ou 64 bits est utilisé pour migrer des données.  
  
### <a name="misc"></a>Divers  
  
|Terme|Définition|  
|--------|--------------|  
|**Options de Migration des données étendues**|Affiche les options de migration des données supplémentaires pour chaque table dans l’onglet Détail distinct.<br /><br />**Mode par défaut**: masquer<br /><br />**Mode optimisé**: masquer<br /><br />**Mode plein**: masquer|  
|**En cas d’erreur**|Arrête la migration des données lorsqu’une erreur se produit. Il comporte trois options :<br /><br />**Arrêter la migration :** arrête l’opération de migration de données<br /><br />**Passez à la table suivante :** cesse de migration des données à la table actuelle et passe à la suivante<br /><br />**Atteindre le lot suivant :** cesse de migration des données vers le lot en cours et passe à la suivante<br /><br />**Mode par défaut**: procéder au lot suivant<br /><br />**Mode optimisé**: procéder au lot suivant<br /><br />**Mode plein**: procéder au lot suivant|  
  
### <a name="parallel-data-migration"></a>Migration de données en parallèle  
  
|Terme|Définition|  
|--------|--------------|  
|**Mode de Migration de données en parallèle**|Spécifie le mode utilisé pour les threads de branchement pour permettre la migration des données en parallèle. En mode Auto, SSMA choisit le nombre de threads (10 par défaut), dupliquée pour migrer les données. En mode personnalisé, utilisateur peut spécifier le nombre de threads dupliquée pour migrer des données (valeur minimale est 1 et la valeur maximale est 100). Actuellement, moteur migration client uniquement des données de côté prend en charge la migration des données en parallèle.<br /><br />**Mode par défaut**: automatique<br /><br />**Mode optimisé**: automatique<br /><br />**Mode plein**: automatique|  
  
> [!IMPORTANT]  
> Lorsque le **parallèles en Mode Migration de données** option est définie sur **personnalisé**, un nouveau projet de définition d’option **le nombre de threads** s’affiche. Il spécifie le nombre de threads utilisés pour la migration de données.  
  
### <a name="spatial-data"></a>Données spatiales  
  
|Terme|Définition|  
|--------|--------------|  
|**Gestion des erreurs**|Spécifie comment gérer les erreurs de migration de valeurs de types de données spatiales. Si 'Remplacer par la valeur NULL' est spécifié, toutes les valeurs spatiales et provoquent des erreurs seront remplacés par la valeur NULL. Aucun remplacement n’est effectuée dans le cas contraire.<br /><br />**Mode par défaut**: générer une erreur<br /><br />**Mode optimisé**: générer une erreur<br /><br />**Mode plein**: générer une erreur|  
|**Validation de la valeur**|Spécifie comment gérer les valeurs spatiales non valides. Si la valeur est « Essayez rendre valide » est spécifié, une tentative est effectuée pour modifier des valeurs non valides pour les rendre valide.<br /><br />**Mode par défaut**: rendre valide<br /><br />**Mode optimisé**: ne modifiez pas<br /><br />**Mode plein**: rendre valide|  
  
