---
title: Propriétés de l’étape du travail - Nouvelle étape du travail (page Général) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-agent
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.ag.job.stepgeneral.f1
ms.assetid: 8d1885ba-4386-4528-8f2b-68c16852720c
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: d284bd4cd7dc81ebf1e5915854613b41680033b6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="job-step-properties---new-job-step-general-page"></a>Propriétés de l’étape du travail - Nouvelle étape du travail (page Général)
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance), la plupart des fonctionnalités SQL Server Agent sont prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Database Managed Instance et SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Utilisez cette page pour afficher et modifier les propriétés d’une étape d’un travail de [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent ou pour définir une nouvelle étape de travail.  
  
Pour naviguer vers cette page, dans l’Explorateur d’objets [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)] , développez [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent, cliquez avec le bouton droit sur **Travaux**, cliquez sur **Nouvelles tâches**, sélectionnez la page **Étapes** et cliquez sur **Nouveau**. Vous pouvez également accéder à cette page en cliquant avec le bouton droit sur un travail dans l’Explorateur d’objets, en cliquant sur **Propriétés**, en sélectionnant la page **Étapes** et en cliquant sur **Nouveau**, **Insérer**ou **Modifier**.  
  
## <a name="options"></a>Options  
**Nom de l’étape**  
Définissez le nom de l'étape du travail.  
  
**Type**  
Définissez le sous-système utilisé par l'étape du travail. En fonction du sous-système sélectionné, les options affichées pour définir l'étape du travail changent.  
  
**Exécuter en tant que**  
Définissez le compte proxy pour l'étape du travail. Les membres du rôle serveur fixe sysadmin peuvent également spécifier le **Compte de service SQL Server Agent**.  
  
**Sauvegarde de la base de données**  
Définissez la base de données où est exécutée l'étape du travail. Cette option est disponible pour tous les types d'étapes de travail.  
  
**Command**  
Définissez la commande exécutée par l'étape du travail.  
  
## <a name="options-for-transact-sql-job-steps"></a>Options des étapes de travail Transact-SQL  
**Ouvrir**  
Charge la commande à partir d'un fichier.  
  
**Tout sélectionner**  
Sélectionne le texte de la commande.  
  
**Copier**  
Copie le texte sélectionné dans le Presse-papiers.  
  
**Coller**  
Colle le contenu du Presse-papiers.  
  
**Analyser**  
Vérifie la syntaxe de la commande.  
  
## <a name="options-for-activex-script-job-steps"></a>Options pour les étapes de travail Script ActiveX  
  
> [!IMPORTANT]  
> Le sous-système de création de scripts ActiveX sera supprimé de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent dans une version future de [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité.  
  
**VBScript**  
Définit [!INCLUDE[msCoName](../../includes/msconame_md.md)] Visual Basic Scripting Edition en tant que langage des étapes de travail.  
  
**JScript**  
Définit JScript en tant que langage des étapes de travail.  
  
**Autres**  
Tapez le nom du langage des étapes de travail écrites dans un autre langage de script.  
  
**Ouvrir**  
Charge la commande à partir d'un fichier.  
  
**Tout sélectionner**  
Sélectionne le texte de la commande.  
  
**Copier**  
Copie le texte sélectionné.  
  
**Coller**  
Colle le contenu du Presse-papiers.  
  
## <a name="options-for-operating-system-cmdexec-job-steps"></a>Options des étapes de travail du système d'exploitation (CmdExec)  
**Traiter le code de sortie d'une commande réussie**  
Tapez le code de sortie renvoyé par la commande pour indiquer la réussite de son exécution.  
  
**Ouvrir**  
Charge la commande à partir d'un fichier.  
  
**Tout sélectionner**  
Sélectionne le texte de la commande.  
  
**Copier**  
Copie le texte sélectionné.  
  
**Coller**  
Colle le contenu du Presse-papiers.  
  
## <a name="options-for-powershell-job-steps"></a>Options des étapes de travail PowerShell  
**Ouvrir**  
Charge le script à partir d'un fichier.  
  
**Tout sélectionner**  
Sélectionne le texte du script.  
  
**Copier**  
Copie le texte sélectionné.  
  
**Coller**  
Colle le contenu du Presse-papiers.  
  
## <a name="options-for-replication-distributor-job-steps"></a>Options pour les étapes de travail Serveur de distribution de réplication  
**Tout sélectionner**  
Sélectionne le texte de la commande.  
  
**Copier**  
Copie le texte sélectionné.  
  
**Coller**  
Colle le contenu du Presse-papiers.  
  
## <a name="options-for-replication-merge-job-steps"></a>Options pour les étapes de travail Fusion de réplication  
**Tout sélectionner**  
Sélectionne le texte de la commande.  
  
**Copier**  
Copie le texte sélectionné.  
  
**Coller**  
Colle le contenu du Presse-papiers.  
  
## <a name="options-for-replication-queue-reader-job-steps"></a>Options des étapes de travail de l'Agent de lecture de file d'attente de la réplication  
**Sauvegarde de la base de données**  
Base de données à utiliser pour l'étape du travail.  
  
**Tout sélectionner**  
Sélectionne le texte de la commande.  
  
**Copier**  
Copie le texte sélectionné.  
  
**Coller**  
Colle le contenu du Presse-papiers.  
  
## <a name="options-for-replication-snapshot-job-steps"></a>Options pour les étapes de travail d'instantané de réplication  
**Tout sélectionner**  
Sélectionne le texte de la commande.  
  
**Copier**  
Copie le texte sélectionné.  
  
**Coller**  
Colle le contenu du Presse-papiers.  
  
## <a name="options-for-replication-transaction-log-reader-job-steps"></a>Options pour les étapes de travail Lecteur du journal des transactions de réplication  
**Tout sélectionner**  
Sélectionne le texte de la commande.  
  
**Copier**  
Copie le texte sélectionné.  
  
**Coller**  
Colle le contenu du Presse-papiers.  
  
## <a name="options-for-sql-server-analysis-services-command-job-steps"></a>Options pour les étapes de travail Commande SQL Server Analysis Services  
**Server**  
Sélectionnez le serveur sur lequel vous souhaitez exécuter l'étape du travail.  
  
**Ouvrir**  
Charge la commande à partir d'un fichier.  
  
**Tout sélectionner**  
Sélectionne le texte de la commande.  
  
**Copier**  
Copie le texte sélectionné.  
  
**Coller**  
Colle le contenu du Presse-papiers.  
  
## <a name="options-for-sql-server-analysis-services-query-job-steps"></a>Options pour les étapes de travail Requête SQL Server Analysis Services  
**Server**  
Sélectionnez le serveur sur lequel vous souhaitez exécuter l'étape du travail.  
  
**Base de données**  
Base de données à utiliser pour l'étape du travail.  
  
**Ouvrir**  
Charge la commande à partir d'un fichier.  
  
**Tout sélectionner**  
Sélectionne le texte de la commande.  
  
**Copier**  
Copie le texte sélectionné.  
  
**Coller**  
Colle le contenu du Presse-papiers.  
  
## <a name="options-for-integration-services-package-execution-job-steps"></a>Options pour les étapes de travail Exécution du package Integration Services  
  
### <a name="general-tab"></a>Onglet Général  
Spécifiez l'emplacement du package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion_md.md)] ([!INCLUDE[ssIS](../../includes/ssis_md.md)]) et la méthode d'authentification à utiliser. Les options suivantes sont disponibles quand vous sélectionnez cet onglet :  
  
**Source du package**  
Spécifiez l'emplacement où est stocké le package [!INCLUDE[ssIS](../../includes/ssis_md.md)] . Choisissez une des options suivantes :  
  
-   **SQL Server**  
  
-   **Système de fichiers**  
  
-   **Magasin de packages SSIS**  
  
**Server**  
Tapez le nom du serveur où est stocké le package [!INCLUDE[ssIS](../../includes/ssis_md.md)] . Cette option n’est disponible que si **SQL Server** ou **Magasin de packages SSIS** est spécifié dans **Source du package**.  
  
**Utiliser l'authentification Windows**  
Les connexions à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] utilisent l'authentification [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows.  
  
**Utiliser l'authentification SQL Server**  
Les connexions à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] utilisent l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] . Si vous sélectionnez cette méthode d’authentification, entrez le **Nom d’utilisateur** et le **Mot de passe**appropriés.  
  
> [!IMPORTANT]  
> [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] L’authentification est fournie pour la compatibilité descendante. Pour une sécurité renforcée, utilisez l'authentification Windows dans la mesure du possible.  
  
**Package**  
Tapez l'emplacement du package.  
  
> [!IMPORTANT]  
> Pour les packages [!INCLUDE[ssIS](../../includes/ssis_md.md)] protégés par mot de passe, cliquez sur l’onglet **Configurations** pour entrer le mot de passe dans la boîte de dialogue **Mot de passe du package** . Sinon, le travail de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent chargé d'exécuter le package protégé par mot de passe échouera.  
  
### <a name="configurations-tab"></a>Onglet Configurations  
Spécifiez les options de configuration du package [!INCLUDE[ssIS](../../includes/ssis_md.md)] . Lorsque vous sélectionnez cet onglet, les options suivantes sont disponibles.  
  
**Fichiers de configuration**  
Répertorie les fichiers de configuration pour le package.  
  
**Ajouter**  
Ajoute un fichier de configuration pour le package.  
  
**Supprimer**  
Supprime un fichier de configuration pour le package.  
  
**Monter**  
Déplace le fichier de configuration sélectionné vers le haut.  
  
**Descendre**  
Déplace le fichier de configuration sélectionné vers le bas.  
  
### <a name="command-files-tab"></a>Onglet Fichiers de commandes  
Sélectionnez les fichiers de commandes pour le package. Les fichiers de commandes sont traités dans l'ordre dans lequel ils apparaissent dans la liste. Les options suivantes sont disponibles quand vous sélectionnez cet onglet :  
  
**Fichiers de commandes**  
Répertorie les fichiers de commandes pour le package.  
  
**Ajouter**  
Ajoute un fichier de commandes.  
  
**Supprimer**  
Supprime le fichier de commandes sélectionné.  
  
**Monter**  
Déplace le fichier de commandes sélectionné vers le haut.  
  
**Descendre**  
Déplace le fichier de commandes sélectionné vers le bas.  
  
### <a name="data-sources-tab"></a>Onglet Sources de données  
Sous cet onglet, vous pouvez consulter les sources de données spécifiées dans le package.  
  
**Gestionnaire de connexions**  
Affichez le nom de la source de données.  
  
**Description**  
Affichez la description de la source de données.  
  
**Chaîne de connexion**  
Affiche la chaîne de connexion de la source de données.  
  
### <a name="execution-options-tab"></a>Onglet Options d'exécution  
Sous cet onglet, vous pouvez consulter ou modifier les options d'exécution pour le package.  
  
**Mettre le package en échec en cas d'avertissements de validation**  
Sélectionnez cette option pour faire échouer l'exécution du package en cas d'avertissements de validation.  
  
**Valider le package sans l'exécuter**  
Sélectionnez cette option pour que l'étape du travail valide le package, mais ne l'exécute pas.  
  
**Maximum d'exécutables simultanés**  
Nombre maximal de fichiers exécutables pouvant être exécutés en même temps.  
  
**Activer les points de contrôle de package**  
Sélectionnez cette option pour que l'étape du travail utilise les points de vérification du package.  
  
**Fichier de point de contrôle**  
Tapez le nom du fichier de point de vérification du package.  
  
**...**  
Naviguez jusqu'au fichier de point de vérification du package.  
  
**Substituer les options de redémarrage**  
Sélectionnez cette option pour spécifier les options de redémarrage de cette étape de travail qui sont différentes des options de redémarrage spécifiées dans le package.  
  
**Option de redémarrage**  
Sélectionnez l'action à appliquer lorsque le package redémarre.  
  
### <a name="logging-tab"></a>Onglet Enregistrement  
Sous cet onglet, vous pouvez consulter ou modifier les modules fournisseur d'informations du package.  
  
**Module fournisseur d’informations**  
Sélectionnez le ClassID du module fournisseur d'informations.  
  
**Chaîne de configuration**  
Tapez la chaîne de configuration du module fournisseur d'informations.  
  
**Supprimer**  
Supprime le module fournisseur d'informations.  
  
### <a name="set-values-tab"></a>Onglet Valeurs définies  
Sous cet onglet, vous pouvez consulter ou modifier les valeurs des propriétés du package.  
  
**Chemin de la propriété**  
Permet de consulter ou de modifier le chemin d'accès de la propriété.  
  
**Value**  
Permet de consulter ou de modifier la valeur de la propriété.  
  
**Supprimer**  
Supprime la propriété.  
  
### <a name="verification-tab"></a>Onglet Vérification  
Sous cet onglet, vous pouvez sélectionner les options de vérification pour l'étape du travail.  
  
**Exécuter uniquement les packages signés**  
Exécute uniquement les packages qui ont été signés. Lorsque cette option est sélectionnée, l'étape du travail échoue si le package n'est pas signé.  
  
**Vérifier la build du package**  
Exécute uniquement les packages dotés d'un numéro de build spécifique. Lorsque cette option est sélectionnée, l'étape du travail échoue si le package n'a pas le numéro de build spécifié.  
  
**Build**  
Tapez le numéro de build du package.  
  
**Vérifier l'ID de package**  
Exécute uniquement les packages dotés d'un ID spécifique. Lorsque cette option est sélectionnée, l'étape du travail échoue si le package n'a pas l'ID spécifié.  
  
**ID du package**  
Tapez l'ID du package.  
  
**Vérifier l'ID de version**  
Exécute uniquement les packages dotés d'un ID de version spécifique. Lorsque cette option est sélectionnée, l'étape du travail échoue si le package n'a pas l'ID de version spécifié.  
  
**ID de version**  
Tapez l'ID de version.  
  
### <a name="command-line-tab"></a>Onglet Ligne de commande  
Spécifiez les options de ligne de commande du package. Lorsque vous sélectionnez cet onglet, les options suivantes sont disponibles.  
  
**Restaurer les options d'origine**  
Utilise les options de ligne de commande définies dans cette boîte de dialogue.  
  
**Modifier la ligne de commande manuellement**  
Spécifiez les options dans la fenêtre de ligne de commande.  
  
**Ligne de commande**  
Tapez les options de ligne de commande à utiliser pour ce package.  
  
## <a name="see-also"></a> Voir aussi  
[Gérer les étapes de travail](../../ssms/agent/manage-job-steps.md)  
[Travaux de SQL Server Agent pour les packages](http://msdn.microsoft.com/en-us/ecf7a5f9-b8a7-47f1-9ac0-bac07cb89e31)  
[Administration des agents de réplication](http://msdn.microsoft.com/en-us/f27186b8-b1b2-4da0-8b2b-91f632c2ab7e)  
  
