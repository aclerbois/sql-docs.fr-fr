---
title: Exporter une application de la couche Données | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: data-tier-applications
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-data-tier-apps
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.exportdac.progress.f1
- sql13.swb.exportdac.summary.f1
- sql13.swb.exportdac.results.f1
- sql13.swb. exportdac.results.f1
- sql13.swb. exportdac.summary.f1
- sql13.swb. exportdac.settings.f1
- sql13.swb.exportdac.welcome.f1
- sql13.swb.exportdac.settings.f1
helpviewer_keywords:
- How to [DAC], export
- wizard [DAC], export
- export DAC
- data-tier application [SQL Server], export
ms.assetid: 61915bc5-0f5f-45ac-8cfe-3452bc185558
caps.latest.revision: 20
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: b744e81c80a1e5c817a0a7b9964007e9aec521d2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="export-a-data-tier-application"></a>Exporter une application de la couche Données
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  L'exportation d'une application de la couche Données (DAC) déployée ou d'une base de données crée un fichier d'exportation qui inclut les définitions des objets de la base de données et toutes les données contenues dans les tables. Le fichier d'exportation peut ensuite être importé dans une autre instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)]ou dans [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]. Les opérations d’exportation-importation peuvent être combinées pour migrer une DAC entre différentes instances, pour créer une archive ou pour créer une copie sur site d’une base de données déployée dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
## <a name="before-you-begin"></a>Avant de commencer  
 Le processus d'exportation génère un fichier d'exportation DAC en deux étapes.  
  
1.  L'exportation génère une définition de la DAC dans le fichier d'exportation (fichier BACPAC), de la même manière que l'extraction d'une DAC génère une définition de la DAC dans un fichier de package DAC. La définition de la DAC exportée inclut tous les objets de la base de données active. Si le processus d'exportation est exécuté sur une base de données à l'origine déployée à partir de la DAC et si des modifications ont été apportées directement à la base de données après le déploiement, la définition exportée correspond au jeu d'objets dans la base de données, pas à ce qui a été défini dans la DAC d'origine.  
  
2.  L'exportation copie en bloc les données de toutes les tables dans la base de données et les incorpore dans le fichier d'exportation.  
  
 Le processus d'exportation définit la version de la DAC sur 1.0.0.0 et la description de la DAC dans le fichier d'exportation sur une chaîne vide. Si la base de données a été déployée à partir de la DAC, la définition de la DAC dans le fichier d'exportation contient le nom donné à la DAC d'origine, sinon le nom de la DAC est défini sur le nom de la base de données.  
  

###  <a name="LimitationsRestrictions"></a> Limitations et restrictions  
 Une DAC ou une base de données peut uniquement être exportée à partir d'une base de données dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)]ou [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 4 (SP4) ou version ultérieure.  
  
 Vous ne pouvez pas exporter une base de données contenant des objets qui ne sont pas pris en charge dans une DAC ou contenant des utilisateurs à relation contenant-contenu. Pour plus d'informations sur les types d'objets pris en charge dans une DAC, consultez [DAC Support For SQL Server Objects and Versions](../../relational-databases/data-tier-applications/dac-support-for-sql-server-objects-and-versions.md).  
  
###  <a name="Permissions"></a> Permissions  
 L’exportation d’une DAC requiert au minimum des autorisations ALTER ANY LOGIN et VIEW DEFINITION de la portée de la base de données, ainsi que des autorisations SELECT sur **sys.sql_expression_dependencies**. L'exportation d'une DAC peut être réalisée par les membres du rôle serveur fixe securityadmin également membres du rôle de base de données fixe database_owner dans la base de données à partir de laquelle est extraite la DAC. Les membres du rôle serveur fixe sysadmin ou le compte d’administrateur système intégré de SQL Server nommé **sa** peuvent également exporter une DAC.  
  
##  <a name="UsingDeployDACWizard"></a> Utilisation de l'Assistant Exporter l'application de la couche Données  
 **Pour exporter une DAC à l'aide d'un Assistant**  
  
1.  Connectez-vous à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], sur site ou dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
2.  Dans l' **Explorateur d'objets**, développez le nœud de l'instance à partir de laquelle vous voulez exporter la DAC.  
  
3.  Cliquez avec le bouton droit sur le nom de la base de données.  
  
4.  Cliquez sur **Tâches** , puis sélectionnez **Exporter une application de la couche Données…**  
  
5.  Renseignez les boîtes de dialogue de l'Assistant :  
  
    -   [Page Introduction](#Introduction)  
  
    -   [Page Paramètres d'exportation](#Export_settings)  
  
    -   [Page Validation](#Validation)  
  
    -   [Page Résumé](#Summary)  
  
    -   [Page Progression](#Progress)  
  
    -   [Page Résultats](#Results)  
  
##  <a name="Introduction"></a> Page Introduction  
 Cette page décrit les étapes de l'Assistant Exporter l'application de la couche Données.  
  
 **Options**  
  
 **Ne plus afficher cette page.** - Activez la case à cocher pour ne plus afficher la page Introduction à l'avenir.  
  
 **Suivant** - Passe à la page **Sélectionner le package DAC** .  
  
 **Annuler** : annule l'opération et ferme l'Assistant.  
  
##  <a name="Export_settings"></a> Page Paramètres d'exportation  
 Utilisez cette page pour indiquer l'emplacement où vous souhaitez créer le fichier BACPAC.  
  
-   **Enregistrer sur le disque local** - Crée un fichier de BACPAC dans un répertoire sur l’ordinateur local. Cliquez sur **Parcourir...** pour explorer l'ordinateur local, ou spécifiez le chemin d'accès dans l'espace fourni. Le chemin d'accès doit inclure un nom de fichier et l'extension .bacpac.  
  
-   **Enregistrer dans Windows Azure** - Crée un fichier BACPAC dans un conteneur Windows Azure. Vous devez vous connecter à un conteneur Windows Azure afin de valider cette option. Notez que cette option requiert également que vous spécifiiez un répertoire local pour le fichier temporaire. Notez que le fichier temporaire est créé à l'emplacement spécifié et qu'il y reste une fois l'opération terminée.  
  
 Pour spécifier un sous-ensemble de tables à exporter, utilisez l'option **Avancé** .  
  
##  <a name="Validation"></a> Page Validation  
 Utilisez la page de validation pour passer en revue tous les problèmes qui empêchent l'opération. Pour continuer, résolvez les problèmes bloquants, puis cliquez sur **Réexécuter la validation** pour vous assurer que la validation est réussie.  
  
 Pour continuer, cliquez sur **Suivant**.  
  
##  <a name="Summary"></a> Page Résumé  
 Utilisez cette page pour passer en revue la source spécifiée et les paramètres cibles de l'opération. Pour terminer l'exportation à l'aide des paramètres spécifiés, cliquez sur **Terminer**. Pour annuler l'exportation et quitter l'Assistant, cliquez sur **Annuler**.  
  
##  <a name="Progress"></a> Page Progression  
 Cette page affiche une barre de progression indiquant l'état de l'opération. Pour afficher l'état détaillé, cliquez sur l'option **Afficher les détails** .  
  
##  <a name="Results"></a> Page Résultats  
 Cette page signale la réussite ou l'échec de l'exportation et affiche les résultats de chaque action. Toute action pour laquelle une erreur s'est produite aura un lien dans la colonne **Résultat** . Cliquez sur le lien pour consulter le rapport d'erreur de cette action.  
  
 Cliquez sur **Terminer** pour fermer l'Assistant.  
  
##  <a name="NetApp"></a> Utilisation d'une application .Net Framework  
 **Pour exporter une DAC à l’aide de la méthode Export() dans une application .Net Framework.**  
  
 Pour afficher un exemple de code, téléchargez l'exemple d'application DAC sur [Codeplex](http://go.microsoft.com/fwlink/?LinkId=219575)  
  
1.  Créez un objet serveur SMO et définissez-le sur l'instance qui contient la DAC à exporter.  
  
2.  Ouvrez un objet **ServerConnection** et connectez-vous à la même instance.  
  
3.  Utilisez la méthode **Export** de type **Microsoft.SqlServer.Management.Dac.DacStore** pour exporter la DAC. Spécifiez le nom de la DAC à exporter, ainsi que le chemin d'accès au dossier où le fichier d'exportation doit être placé.  
  
## <a name="see-also"></a> Voir aussi  
 [Applications de la couche Données](../../relational-databases/data-tier-applications/data-tier-applications.md)   
 [Extraire une DAC d'une base de données](../../relational-databases/data-tier-applications/extract-a-dac-from-a-database.md)  
  
  
