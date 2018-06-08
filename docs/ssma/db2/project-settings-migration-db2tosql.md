---
title: Paramètres (Migration) (DB2ToSQL) du projet | Documents Microsoft
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
ms.assetid: 48aaa8e6-a9cb-487d-9ba5-fc3f1c4786ae
caps.latest.revision: 5
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 6db66377f894754371d664c6e9b03abe12a8479f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="project-settings-migration-db2tosql"></a>Paramètres du projet (Migration) (DB2ToSQL)
La page de la Migration de le **les paramètres de projet** boîte de dialogue contient des paramètres permettant de personnaliser comment SSMA migre les données à partir de DB2 pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
Le volet de la Migration est disponible à la fois dans le **les paramètres de projet** et **les paramètres de projet par défaut** boîtes de dialogue.  
  
-   Pour spécifier les paramètres pour tous les projets SSMA, sur le **outils** menu, sélectionnez **les paramètres de projet par défaut**, sélectionnez le type de projet de migration pour lequel les paramètres sont requis pour être affichées ou modifiées à partir de **Version cible de la Migration** déroulante cliquez sur **général** au bas de la gauche, puis cliquez sur **Migration**.  
  
-   Pour spécifier les paramètres pour le projet actuel, sur le **outils** menu, sélectionnez **les paramètres de projet**, cliquez sur **général** au bas de la gauche, puis cliquez sur **Migration**.  
  
## <a name="migration-engine"></a>Moteur de migration  
  
|Terme|Définition|  
|--------|--------------|  
|**Moteur de migration**|Spécifie le moteur de base utilisé lors de la Migration de données. Migration des données côté client fait référence au client SSMA récupérer les données de la source et l’insertion en bloc ces données dans SQL Server. Migration des données côté serveur fait référence au moteur de migration de données SSMA (programme de copie en bloc) en cours d’exécution dans la boîte de SQL Server en tant qu’un travail de l’Agent SQL, la récupération des données à partir de la source et insérant directement dans SQL Server, ce qui évite un supplémentaire client-saut (performances).<br /><br />**Mode par défaut**: moteur de Migration de données côté Client<br /><br />**Mode optimisé**: moteur de Migration de données côté Client<br /><br />**Mode plein**: moteur de Migration de données côté Client|  
  
> [!IMPORTANT]  
> Lorsque le **moteur de Migration** option est définie sur **moteur de Migration de données côté serveur**, un nouveau projet de définition d’option **moteur de Migration de données côté serveur utilisez 32 bits** s’affiche. Il spécifie si l’utilitaire de programme de copie en bloc (BCP) 32 bits ou 64 bits est utilisé pour migrer des données.  
  
## <a name="miscellaneous-options"></a>Options diverses  
  
|Terme|Définition|  
|--------|--------------|  
|**Taille de lot**|Spécifie le lot taille utilisée pendant la migration des données.<br /><br />**Mode par défaut**: 10000<br /><br />**Mode optimisé**: 10000<br /><br />**Mode plein**: 10000|  
|**Contraintes de validation**|Spécifie si SSMA doit vérifier les contraintes lorsqu’il insère des données dans des tables SQL Server.<br /><br />**Mode par défaut**: False<br /><br />**Mode optimisé**: False<br /><br />**Mode plein**: False|  
|**Délai d’expiration de la Migration de données**|Spécifie le délai d’attente utilisé lors de la migration de données<br /><br />**Mode par défaut**: 15<br /><br />**Mode optimisé**: 15<br /><br />**Mode plein**: 15|  
|**Options de Migration des données étendues**|Affiche les options de migration des données supplémentaires pour chaque table dans l’onglet Détail distinct.<br /><br />**Mode par défaut**: masquer<br /><br />**Mode optimisé**: masquer<br /><br />**Mode plein**: masquer|  
|**Exécuter les déclencheurs**|Spécifie si SSMA doit exécuter les déclencheurs d’insertion lorsqu’il ajoute des données dans des tables SQL Server.<br /><br />**Mode par défaut**: False<br /><br />**Mode optimisé**: False<br /><br />**Mode plein**: False|  
|**Conserver l'identité**|Spécifie si SSMA conserve les valeurs null dans la source de données lorsqu’il ajoute des données à SQL Server, quel que soit les valeurs par défaut qui sont spécifiés dans SQL Server.<br /><br />**Mode par défaut**: True<br /><br />**Mode optimisé**: True<br /><br />**Mode plein**: False|  
|**Conserver les valeurs NULL**|Spécifie si SSMA conserve les valeurs null dans la source de données lorsqu’il ajoute des données à SQL Server, quel que soit les valeurs par défaut qui sont spécifiés dans SQL Server.<br /><br />**Mode par défaut**: True<br /><br />**Mode optimisé**: True<br /><br />**Mode plein**: True|  
|**Marquer l’opération de suppression de chaîne avec l’erreur**|Si la taille de la colonne cible est inférieure à la longueur de la chaîne source, la valeur sera tronquée et marquée comme une erreur.<br /><br />**Mode par défaut**: Oui<br /><br />**Mode optimisé**: Oui<br /><br />**Mode plein**: Oui|  
|**En cas d’erreur**|Arrête la migration des données lorsqu’une erreur se produit. Il comporte trois options :<br /><br />**Arrêter la migration :** arrête l’opération de migration de données<br /><br />**Passez à la table suivante :** cesse de migration des données à la table actuelle et passe à la suivante<br /><br />**Atteindre le lot suivant :** cesse de migration des données vers le lot en cours et passe à la suivante<br /><br />**Mode par défaut**: procéder au lot suivant<br /><br />**Mode optimisé**: procéder au lot suivant<br /><br />**Mode plein**: procéder au lot suivant|  
|**Remplacez les dates non pris en charge**|Spécifie si SSMA devrait corriger les dates antérieures à la plus ancienne [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] **datetime** date (01 janvier 1753).<br /><br />Pour conserver les valeurs de date en cours, sélectionnez **ne rien faire**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] n’accepte pas les dates antérieures au 01 janvier 1753 dans une colonne datetime. Si vous utilisez des dates antérieures, vous devez convertir les valeurs datetime aux valeurs de caractère.<br /><br />Pour convertir les dates antérieures au 01 janvier 1753 avec la valeur NULL, sélectionnez **remplacer par NULL**.<br /><br />Pour remplacer les dates antérieures au 01 janvier 1753 avec une date de prise en charge, sélectionnez **remplacer la plus proche de la date de prise en charge**.<br /><br />**Mode par défaut**: ne rien faire<br /><br />**Mode optimisé**: ne rien faire<br /><br />**Mode plein**: Remplacez par le plus proche de date pris en charge|  
|**Verrou de table**|Spécifie si SSMA verrouille les tables lorsqu’il ajoute des données aux tables lors de la migration de données. Obtient un verrou de mise à jour en bloc pour la durée de l’opération de copie en bloc. Si la valeur est False, un verrou est défini au niveau de la ligne.<br /><br />**Mode par défaut**: True<br /><br />**Mode optimisé**: True<br /><br />**Mode plein**: True|  
  
## <a name="parallel-data-migration"></a>Migration de données en parallèle  
  
|Terme|Définition|  
|--------|--------------|  
|**Mode de Migration de données en parallèle**|Spécifie le mode utilisé pour les threads de branchement pour permettre la migration des données en parallèle. En mode Auto, SSMA choisit le nombre de threads (10 par défaut), dupliquée pour migrer les données. En mode personnalisé, utilisateur peut spécifier le nombre de threads dupliquée pour migrer des données (valeur minimale est 1 et la valeur maximale est 100). Actuellement, moteur migration client uniquement des données de côté prend en charge la migration des données en parallèle.<br /><br />**Mode par défaut**: automatique<br /><br />**Mode optimisé**: automatique<br /><br />**Mode plein**: automatique|  
  
> [!IMPORTANT]  
> Lorsque le **parallèles en Mode Migration de données** option est définie sur **personnalisé**, un nouveau projet de définition d’option **le nombre de threads** s’affiche. Il spécifie le nombre de threads utilisés pour la migration de données.  
  
