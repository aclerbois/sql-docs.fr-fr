---
title: Inscrire une base de données en tant que DAC | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
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
- sql13.swb.registerdacwizard.summary.f1
- sql13.swb.registerdacwizard.introduction.f1
- sql13.swb.registerdacwizard.setproperties.f1
- sql13.swb.registerdacwizard.registerdac.f1
helpviewer_keywords:
- wizard [DAC], register
- How to [DAC], register
- register DAC
- data-tier application [SQL Server], register
ms.assetid: 08e52aa6-12f3-41dd-a793-14b99a083fd5
caps.latest.revision: 22
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: d14994b4e90429cdd4d3e8a9b58c2b3998492b3d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="register-a-database-as-a-dac"></a>Inscrire une base de données en tant que DAC
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Utilisez l’ **Assistant Inscrire l’application de la couche Données** ou un script Windows PowerShell pour générer une définition d’application de la couche Données (DAC) qui décrit les objets d’une base de données existante et inscrivez la définition de la DAC dans la base de données système **msdb** (**master** dans [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]).  
  
-   **Before you begin:**  [Limitations and Restrictions](#LimitationsRestrictions), [Permissions](#Permissions)  
  
-   **Pour mettre à niveau une DAC, en utilisant :**  [L’Assistant Inscrire l’application de la couche Données](#UsingRegisterDACWizard), [PowerShell](#RegisterDACPowerShell)  
  
## <a name="before-you-begin"></a>Avant de commencer  
 Le processus d'inscription crée une définition de la DAC qui définit les objets de la base de données. La combinaison de la définition de la DAC et de la base de données forme une instance DAC. Si vous inscrivez une base de données comme une DAC sur une instance gérée du moteur de base de données, la DAC inscrite est incorporée dans l'utilitaire SQL Server la prochaine fois que le jeu d'éléments de collecte de l'utilitaire est envoyé de l'instance au point de contrôle de l'utilitaire. La DAC sera ensuite présente dans le nœud **Applications de la couche Données déployées** dans l’ [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] **Utility Explorer** and reported in the **Applications de la couche Données déployées** details page.  
  
###  <a name="LimitationsRestrictions"></a> Limitations et restrictions  
 L'inscription de la DAC ne peut être exécutée que sur une base de données dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)]ou [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 4 (SP4) ou version ultérieure. L'inscription de la DAC ne peut pas être effectuée si une DAC est déjà inscrite pour la base de données. Par exemple, si la base de données a été créée en déployant une DAC, vous ne pouvez pas exécuter l’ **Assistant Inscrire l’application de la couche Données**.  
  
 Vous ne pouvez pas inscrire de DAC si la base de données a des objets qui ne sont pas pris en charge dans une DAC, ou des utilisateurs à relation contenant-contenu. Pour plus d'informations sur les types d'objets pris en charge dans une DAC, consultez [DAC Support For SQL Server Objects and Versions](../../relational-databases/data-tier-applications/dac-support-for-sql-server-objects-and-versions.md).  
  
###  <a name="Permissions"></a> Permissions  
 L’inscription d’une DAC dans une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] nécessite au moins les autorisations ALTER ANY LOGIN et VIEW DEFINITION de l’étendue de la base de données, les autorisations SELECT sur **sys.sql_expression_dependencies**, et l’appartenance au rôle serveur fixe **dbcreator** . Les membres du rôle serveur fixe **sysadmin** ou le compte d’administrateur système intégré de SQL Server nommé **sa** peuvent également inscrire une DAC. L'inscription d'une DAC qui ne contient pas de connexions dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)] requiert l'appartenance aux rôles **dbmanager** ou **serveradmin** . L'inscription d'une DAC comportant des connexions dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)] requiert l'appartenance aux rôles **loginmanager** ou **serveradmin** .  
  
##  <a name="UsingRegisterDACWizard"></a> Utilisation de l'Assistant Inscrire l'application de la couche Données  
 **Pour inscrire une DAC à l'aide d'un Assistant**  
  
1.  Dans l' **Explorateur d'objets**, développez le nœud pour l'instance qui contient la base de données à inscrire en tant que DAC.  
  
2.  Développez le nœud **Bases de données** .  
  
3.  Cliquez avec le bouton droit sur la base de données à inscrire, pointez sur **Tâches**, puis sélectionnez **Inscrire en tant qu’application de la couche Données**.  
  
4.  Renseignez les boîtes de dialogue de l'Assistant :  
  
    1.  [Page Introduction](#Introduction)  
  
    2.  [Page Définir les propriétés](#Set_properties)  
  
    3.  [Page Validation et résumé](#Summary)  
  
    4.  [Page Inscrire la DAC](#Register)  
  
##  <a name="Introduction"></a> Page Introduction  
 Cette page décrit les étapes de l'inscription d'une application de la couche Données.  
  
 **Ne plus afficher cette page.** - Cochez la case pour ne plus afficher la page à l'avenir.  
  
 **Suivant >** : passe à la page **Définir les propriétés**.  
  
 **Annuler** : termine l’Assistant sans inscrire une DAC.  
  
 [Utilisation de l'Assistant Inscrire l'application de la couche Données](#UsingRegisterDACWizard)  
  
##  <a name="Set_properties"></a> Page Définir les propriétés  
 Utilisez cette page pour spécifier des propriétés au niveau de la DAC telles que le nom de l'application et sa version.  
  
 **Nom de l'application.** - Chaîne qui spécifie le nom utilisé pour identifier la définition de la DAC ; le champ est rempli par le nom de la base de données.  
  
 **Version.** - Valeur numérique qui identifie la version de la DAC. La version de la DAC est utilisée dans Visual Studio pour identifier la version de la DAC sur laquelle les développeurs travaillent. Lors du déploiement d’une DAC, la version est stockée dans la base de données **msdb** et peut être affichée ultérieurement sous le nœud **Applications de la couche Données** dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 **Description.** - Facultatif. Texte qui explique l'objectif de la DAC. Lors du déploiement d’une DAC, la description est stockée dans la base de données **msdb** et peut être affichée ultérieurement sous le nœud **Applications de la couche Données** dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
 **< Précédent** : vous renvoie à la page **Introduction**.  
  
 **Suivant >** : vérifie qu’une DAC peut être générée à partir des objets de la base de données et affiche les résultats dans la page **Validation et résumé**.  
  
 **Annuler** : termine l’Assistant sans inscrire la DAC.  
  
 [Utilisation de l'Assistant Inscrire l'application de la couche Données](#UsingRegisterDACWizard)  
  
##  <a name="Summary"></a> Page Validation et résumé  
 Utilisez cette page pour examiner les mesures que l'Assistant prendra lors de l'inscription de la DAC. La page passe par trois états alors qu'elle vérifie qu'une DAC peut être générée à partir des objets de la base de données.  
  
 [Utilisation de l'Assistant Inscrire l'application de la couche Données](#UsingRegisterDACWizard)  
  
### <a name="retrieving-objects"></a>Récupération d'objets  
 **Récupération d'objets de bases de données et de serveurs.** - Affiche une barre de progression au fur et à mesure que l'Assistant récupère tous les objets requis de la base de données et de l'instance du moteur de base de données.  
  
 **< Précédent** : vous renvoie à la page **Définir les propriétés** pour modifier vos entrées.  
  
 **Suivant >** : inscrit la DAC et affiche les résultats dans la page **Inscrire la DAC**.  
  
 **Annuler** : termine l’Assistant sans inscrire la DAC.  
  
 [Utilisation de l'Assistant Inscrire l'application de la couche Données](#UsingRegisterDACWizard)  
  
### <a name="validating-objects"></a>Validation d'objets  
 **Vérification de**  *Nom_schéma* **.** *Nom_objet* **.** - Affiche une barre de progression au fur et à mesure que l'Assistant vérifie les dépendances des objets récupérés, et vérifie que ces objets sont tous valides pour une DAC. *nom_schéma ***.*** nom_objet* identifie l’objet en cours de vérification.  
  
 **< Précédent** : vous renvoie à la page **Définir les propriétés** pour modifier vos entrées.  
  
 **Suivant >** : inscrit la DAC et affiche les résultats dans la page **Inscrire la DAC**.  
  
 **Annuler** : termine l’Assistant sans inscrire la DAC.  
  
 [Utilisation de l'Assistant Inscrire l'application de la couche Données](#UsingRegisterDACWizard)  
  
### <a name="summary"></a>Résumé  
 **Le paramètre suivant sera utilisé pour inscrire votre DAC.** - Affiche un rapport des propriétés et objets qui seront inclus dans la DAC.  
  
 **Enregistrer le rapport** : sélectionnez ce bouton pour enregistrer une copie du rapport de validation dans un fichier HTML. Le dossier par défaut est un dossier **SQL Server Management Studio\DAC Packages** dans le dossier Documents de votre compte Windows.  
  
 **< Précédent** : vous renvoie à la page **Définir les propriétés** pour modifier vos entrées.  
  
 **Suivant >** : inscrit la DAC et affiche les résultats dans la page **Inscrire la DAC**.  
  
 **Annuler** : termine l’Assistant sans inscrire la DAC.  
  
 [Utilisation de l'Assistant Inscrire l'application de la couche Données](#UsingRegisterDACWizard)  
  
##  <a name="Register"></a> Page Inscrire la DAC  
 Cette page signale la réussite ou l'échec de l'inscription.  
  
 **Inscription de la DAC** : signale la réussite ou l’échec de chaque action entreprise pour inscrire la DAC. Examinez les informations pour déterminer la réussite ou l'échec de chaque action. Toute action pour laquelle une erreur s'est produite aura un lien dans la colonne **Résultat** . Sélectionnez le lien pour consulter le rapport de d'erreur de cette action.  
  
 **Enregistrer le rapport** : sélectionnez ce bouton pour enregistrer le rapport d’inscription dans un fichier HTML. Le fichier signale l'état de chaque action, notamment toutes les erreurs générées par chacune des actions. Le dossier par défaut est un dossier **SQL Server Management Studio\DAC Packages** dans le dossier Documents de votre compte Windows. Le nom de fichier est au format \<Nom_package_DAC>_RegisterDACReport_aaaammjj.html, où \<*Nom_package_DAC*> est le nom du package déployé, *aaaa* = l’année en cours, *mm* = le mois en cours et *jj* = la date du jour.  
  
 **Terminer** : met fin à l’Assistant.  
  
 [Utilisation de l'Assistant Inscrire l'application de la couche Données](#UsingRegisterDACWizard)  
  
##  <a name="RegisterDACPowerShell"></a> Inscrire une DAC à l'aide de PowerShell  
 **Pour inscrire une base de données comme DAC à l’aide de la méthode Register() dans un script PowerShell**  
  
1.  Créez un objet serveur SMO et définissez-le sur l'instance qui contient la base de données à inscrire comme une DAC.  
  
2.  Ajoutez une variable qui spécifie le nom de la base de données.  
  
3.  Spécifiez les métadonnées pour la DAC, par exemple le nom de la DAC, la version et la description.  
  
4.  Exécutez la méthode Register avec les informations spécifiées ci-dessus.  
  
### <a name="example-powershell"></a>Exemple (PowerShell)  
 L'exemple suivant inscrit une base de données nommée MyDB comme une DAC.  
  
```  
## Set a SMO Server object to the default instance on the local computer.  
CD SQLSERVER:\SQL\localhost\DEFAULT  
$srv = get-item .  
  
## Specify the database to register as a DAC.  
$dbname = "MyDB"  
  
## Specify the DAC metadata.  
$applicationname = "MyApplication"  
$version = "1.0.0.0"  
$description = "This DAC defines the database used by my application."  
  
## Register the DAC.  
$registerunit = New-Object Microsoft.SqlServer.Management.Dac.DacExtractionUnit($srv, $dbname, $applicationname, $version)  
$registerunit.Description = $description  
$registerunit.Register()  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Applications de la couche Données](../../relational-databases/data-tier-applications/data-tier-applications.md)  
  
  
