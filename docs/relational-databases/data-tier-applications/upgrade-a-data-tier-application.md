---
title: Mettre à niveau une application de la couche Données | Microsoft Docs
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
- sql13.swb.upgradedacwizard.summary.f1
- sql13.swb.upgradedacwizard.reviewplan.f1
- sql13.swb.upgradedacwizard.upgradedac.f1
- sql13.swb.upgradedacwizard.selectpackage.f1
- sql13.swb.upgradedacwizard.reviewpolicy.f1
- sql13.swb.upgradedacwizard.selectoptions.f1
- sql13.swb.upgradedacwizard.checkdrift.f1
- sql13.swb.upgradedacwizard.introduction.f1
helpviewer_keywords:
- upgrade DAC
- data-tier application [SQL Server], upgrade
- wizard [DAC], upgrade
- How to [DAC], upgrade
ms.assetid: c117df94-f02b-403f-9383-ec5b3ac3763c
caps.latest.revision: 35
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: fe2d3027feada8c7140822dde49fe6d1b4dcbf46
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="upgrade-a-data-tier-application"></a>Upgrade a Data-tier Application
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Utilisez l'Assistant Mise à niveau de l'application de la couche Données ou bien un script Windows PowerShell pour modifier le schéma et les propriétés d'une application de la couche Données (DAC) actuellement déployée pour qu'ils correspondent à ceux définis dans la nouvelle version de la DAC.  
  
-   **Avant de commencer :**  [Choix des options de mise à niveau de la DAC](#ChoseDACUpgOptions), [Limitations et restrictions](#LimitationsRestrictions), [Conditions préalables](#Prerequisites), [Sécurité](#Security), [Autorisations](#Permissions)  
  
-   **Pour mettre à niveau une DAC en utilisant :**  [L’Assistant Mise à niveau de l’application de la couche Données](#UsingDACUpgradeWizard), [PowerShell](#UpgradeDACPowerShell)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
 La mise à niveau d'une DAC est un processus sur place qui modifie le schéma de la base de données existante de manière à ce qu'il corresponde au schéma défini dans une nouvelle version de la DAC. La nouvelle version de la DAC est fournie dans un fichier de package DAC. Pour plus d’informations sur la création d’un package DAC, consultez [Applications de la couche Données](../../relational-databases/data-tier-applications/data-tier-applications.md).  
  
###  <a name="ChoseDACUpgOptions"></a> Choix des options de mise à niveau de la DAC  
 Il existe quatre options de mise à niveau dans le cadre d'une mise à niveau sur place :  
  
-   **Ignorer les pertes de données** - Si sa valeur est **True**, la mise à niveau est effectuée, même si certaines opérations mènent à une perte des données. Si sa valeur est **False**, ces opérations mettent fin à la mise à niveau. Par exemple, si l’une des tables de la base de données actuelle n’est pas présente dans le schéma de la nouvelle DAC, la table est supprimée si la valeur **True** est spécifiée. Le paramètre par défaut est **True**.  
  
-   **Bloquer en cas de modifications** - Si la valeur est **True**, la mise à niveau est terminée si le schéma de la base de données est différent de celui défini dans la DAC précédente. Si la valeur est **False**, la mise à niveau continue, même si des modifications sont détectées. Le paramètre par défaut est **False**.  
  
-   **Restauration en cas d’échec** - Si la valeur est **True**, la mise à niveau est englobée dans une transaction, et une restauration est tentée en cas d’erreurs. Si la valeur est **False**, toutes les modifications sont validées à mesure qu’elles sont effectuées. Par conséquent, si des erreurs se produisent, vous pourrez avoir à restaurer une sauvegarde précédente de la base de données. Le paramètre par défaut est **False**.  
  
-   **Ignorer la validation de la stratégie** - Si la valeur est **True**, la stratégie de sélection du serveur de DAC n’est pas évaluée. Si la valeur est **False**, la stratégie est évaluée et la mise à niveau s’arrête en cas d’erreur de validation. Le paramètre par défaut est **False**.  
  
###  <a name="LimitationsRestrictions"></a> Limitations et restrictions  
 Les mises à niveau d'une DAC ne peuvent être effectuées que dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)]ou dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 4 (SP4) ou version ultérieure.  
  
###  <a name="Prerequisites"></a> Conditions préalables  
 Par mesure de prudence, il est recommandé d'effectuer une sauvegarde complète de la base de données avant de la mettre à niveau. Si une erreur se produit lors d'une mise à niveau et que toutes ses modifications ne peuvent pas être restaurées, vous devrez peut-être restaurer la sauvegarde.  
  
 Avant de démarrer la mise à niveau, plusieurs actions peuvent être entreprises pour valider le package DAC et les actions de mise à niveau. Pour plus d'informations sur la façon de procéder à ces vérifications, consultez [Validate a DAC Package](../../relational-databases/data-tier-applications/validate-a-dac-package.md).  
  
-   Nous vous recommandons de pas effectuer de mise à niveau en utilisant un package DAC provenant de sources inconnues ou non approuvées. De tels packages peuvent contenir du code malveillant susceptible d'exécuter un code Transact-SQL indésirable ou de provoquer des erreurs en modifiant le schéma. Avant d'utiliser un package provenant d'une source inconnue ou non approuvée, décompressez la DAC et vérifiez le code, par exemple les procédures stockées ou autre code défini par l'utilisateur.  
  
-   Si des modifications ont été apportées à la base de données actuelle après le déploiement de la dernière version de la DAC, certaines d'entre elles risquent d'empêcher la réussite de la mise à niveau, ou bien risquent d'être supprimées par la mise à niveau. Vous devez d'abord générer un rapport afin de passer en revue toutes les modifications apportées à la base de données.  
  
-   Par mesure de prudence, il est recommandé de générer une liste des modifications qui seront apportées au schéma lors de la mise à niveau, et de passer en revue la liste à la recherche d'éventuels problèmes.  
  
 Le nom d'application dans le package DAC doit correspondre au nom d'application de la DAC actuellement déployée. Par exemple, si la DAC actuelle a pour nom d'application **GeneralLedger**, vous pouvez effectuer la mise à niveau uniquement à l'aide d'un package DAC ayant également pour nom d'application **GeneralLedger**.  
  
 Vérifiez que le journal des transactions contient suffisamment d'espace pour enregistrer toutes les modifications.  
  
###  <a name="Security"></a> Sécurité  
 Pour améliorer la sécurité, les connexions d'authentification SQL Server sont stockées dans un package DAC sans mot de passe. Lorsque le package est déployé ou mis à niveau, la connexion est créée en tant que connexion désactivée avec un mot de passe généré. Pour activer les connexions, connectez-vous à l'aide d'une connexion qui possède l'autorisation ALTER ANY LOGIN et utilisez ALTER LOGIN pour activer la connexion et affecter un nouveau mot de passe pouvant être communiqué à l'utilisateur. Cela n'est pas nécessaire pour les connexions d'authentification Windows car leurs mots de passe ne sont pas gérés par SQL Server.  
  
####  <a name="Permissions"></a> Permissions  
 Une DAC ne peut être mise à niveau que par les membres des rôles serveur fixes **sysadmin** ou **serveradmin** , ou par les connexions situées dans le rôle serveur fixe **dbcreator** et disposant d'autorisations ALTER ANY LOGIN. La connexion doit être propriétaire de la base de données existante. Le compte d’administrateur système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intégré nommé **sa** peut aussi mettre à niveau une DAC.  
  
##  <a name="UsingDACUpgradeWizard"></a> Utilisation de l'Assistant Mise à niveau de l'application de la couche Données  
 **Pour mettre à niveau une DAC à l'aide d'un Assistant**  
  
1.  Dans l' **Explorateur d'objets**, développez le nœud pour l'instance qui contient la DAC à mettre à niveau.  
  
2.  Développez le nœud **Gestion** , puis le nœud **Applications de la couche Données** .  
  
3.  Cliquez avec le bouton droit sur le nœud de la DAC à mettre à niveau, puis sélectionnez **Mettre à niveau l’application de la couche Données…**.  
  
4.  Renseignez les boîtes de dialogue de l'Assistant :  
  
    1.  [Page Introduction](#Introduction)  
  
    2.  [Page Sélectionner un package](#Select_dac_package)  
  
    3.  [Page Vérifier la stratégie](#Review_policy)  
  
    4.  [Page Détecter les modifications](#Detect_change)  
  
    5.  [Réviser le plan de mise à niveau](#ReviewUpgPlan)  
  
    6.  [Page Résumé](#Summary)  
  
    7.  [Page Mettre à niveau la DAC](#Upgrade)  
  
##  <a name="Introduction"></a> Page Introduction  
 Cette page décrit les étapes de mise à niveau d'une application de couche Données.  
  
 **Ne plus afficher cette page.** - Cochez la case pour ne plus afficher la page à l'avenir.  
  
 **Suivant >** : passe à la page **Sélectionner un package**.  
  
 **Annuler** : met fin à l’Assistant sans mettre à niveau la DAC.  
  
##  <a name="Select_dac_package"></a> Page Sélectionner un package  
 Utilisez cette page pour spécifier le package DAC qui contient la nouvelle version de l'application de couche Données. La page passe par deux états.  
  
### <a name="select-the-dac-package"></a>Sélectionner le package DAC  
 Utilisez l'état initial de la page pour choisir le package DAC à déployer. Le package de la DAC doit être un fichier de package de DAC valide et doit avoir une extension .dacpac. Le nom d'application de la DAC dans le package de DAC doit être le même que le nom d'application de la DAC actuelle.  
  
 **Package de DAC** : spécifie le chemin et le nom de fichier du package DAC qui contient la nouvelle version de l’application de couche Données. Vous pouvez sélectionner le bouton **Parcourir** à droite de la zone pour accéder à l'emplacement du package de DAC.  
  
 **Nom de l’application** : zone en lecture seule qui affiche le nom d’application de la DAC, attribué au moment de sa création ou de son extraction d’une base de données.  
  
 **Version** : zone en lecture seule qui affiche la version affectée au moment où la DAC a été créée ou extraite d’une base de données.  
  
 **Description** : zone en lecture seule qui affiche la description écrite au moment où la DAC a été créée ou extraite d’une base de données.  
  
 **< Précédent** : renvoie à la page **Introduction**.  
  
 **Suivant >** : affiche une barre de progression dès que l’Assistant confirme que le fichier sélectionné est un package DAC valide.  
  
 **Annuler** : met fin à l’Assistant sans mettre à niveau la DAC.  
  
### <a name="validating-the-dac-package"></a>Validation du package DAC  
 Affiche une barre de progression quand l'Assistant confirme que le fichier sélectionné est un package DAC valide. Si le package de DAC est validé, l'Assistant passe à la page **Vérifier la stratégie** . Si le fichier n'est pas un package DAC valide, l'Assistant reste sur la page **Sélectionner un package DAC** . Sélectionnez un autre package DAC valide ou annulez l'Assistant et générez un nouveau package DAC.  
  
 **Validation du contenu de la DAC** : barre de progression qui indique l’état actuel de la validation.  
  
 **< Précédent** : retourne à l’état initial de la page **Sélectionner un package**.  
  
 **Suivant >** : passe à la dernière version de la page **Sélectionner un package**.  
  
 **Annuler** : met fin à l’Assistant sans déployer la DAC.  
  
##  <a name="Review_policy"></a> Page Vérifier la stratégie  
 Utilisez cette page pour vérifier les résultats de l'évaluation de la stratégie de sélection du serveur de la DAC, si la DAC a une stratégie. La stratégie de sélection du serveur de la DAC est facultative et est affectée à une DAC créée dans Microsoft Visual Studio. La stratégie utilise les facettes de la stratégie de sélection du serveur pour spécifier les conditions que doit remplir une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] pour héberger la DAC.  
  
 **Résultats de l’évaluation des conditions de stratégie** : rapport en lecture seule indiquant si les évaluations des conditions de la stratégie de sélection du serveur de la DAC sont remplies. Les résultats de l'évaluation de chaque condition sont indiqués sur des lignes distinctes.  
  
 **Ignorez les violations de stratégie** : utilisez cette case à cocher pour poursuivre la mise à niveau en cas d’échec d’une ou plusieurs conditions de stratégie. Ne sélectionnez cette option que si vous êtes sûr que toutes les conditions qui ont échoué n'empêcheront pas l'opération de la DAC de réussir.  
  
 **< Précédent** : renvoie à la page **Sélectionner un package**.  
  
 **Suivant >** : passe à la page **Détecter les modifications**.  
  
 **Annuler** : met fin à l’Assistant sans mettre à niveau la DAC.  
  
##  <a name="Detect_change"></a> Page Détecter les modifications  
 Cette page indique les résultats de recherche des Assistants portant sur les modifications effectuées dans la base de données qui font différer le schéma de sa définition enregistrée dans les métadonnées de la DAC, dans **msdb**. Par exemple, si les instructions CREATE, ALTER ou DROP ont été utilisées pour ajouter, modifier ou supprimer des objets de la base de données, après le déploiement d'origine de la DAC. La page affiche d'abord une barre de progression, puis signale les résultats de l'analyse.  
  
 **Détection des modifications. Cette opération peut prendre quelques minutes** : affiche une barre de progression pendant que l’Assistant recherche des différences entre le schéma actuel de la base de données et les objets dans la définition de la DAC.  
  
 **Résultats de la détection des modifications:** : indique que l’analyse est terminée et affiche les résultats au-dessous.  
  
 **La base de données DatabaseName n’a pas changé** : l’Assistant n’a pas détecté de différences entre les objets définis dans la base de données et leurs équivalents dans la définition de la DAC.  
  
 **La base de données DatabaseName a changé** : l’Assistant a détecté des modifications entre les objets dans la base de données et leurs équivalents dans la définition de la DAC.  
  
 **Poursuivre malgré la perte possible des modifications** : indique que vous êtes conscient que certains objets ou données dans la base de données actuelle ne seront pas présents dans la nouvelle base de données, et que vous souhaitez poursuivre la mise à niveau. Vous ne devez sélectionner ce bouton que si vous avez analysé le rapport des modifications et si vous connaissez la procédure à effectuer pour transférer manuellement tous les objets ou données nécessaires dans la nouvelle base de données. Si vous n'en êtes pas sûr, cliquez sur le bouton **Enregistrer le rapport** pour enregistrer le rapport des modifications, puis sur **Annuler**. Analysez le rapport, organisez le transfert de tous les objets et données nécessaires une fois la mise à niveau terminée, puis redémarrez l'Assistant.  
  
 **Enregistrer le rapport** : cliquez sur ce bouton pour enregistrer un rapport des modifications détectées par l’Assistant entre les objets de la base de données et leurs équivalents dans la définition de la DAC. Vous pouvez ensuite vérifier le rapport pour déterminer si vous devez prendre des mesures, une fois la mise à niveau terminée, pour incorporer dans la nouvelle base de données tout ou partie des objets répertoriés dans le rapport.  
  
 **< Précédent** : renvoie à la page **Sélectionner le package DAC**.  
  
 **Suivant >** : passe à la page **Options**.  
  
 **Annuler** : met fin à l’Assistant sans déployer la DAC.  
  
## <a name="options-page"></a>Page Options  
 Utilisez cette page pour sélectionner l'option de restauration en cas de échec de la mise à niveau.  
  
 **Restauration en cas d'échec** - Sélectionnez cette option pour inclure la mise à niveau dans une transaction que l'Assistant peut tenter de restaurer en cas de erreur. Pour plus d'informations sur l'option, consultez [Choix des options de mise à niveau de la DAC](#ChoseDACUpgOptions).  
  
 **Paramètres par défaut** : rétablit la valeur par défaut (False) de l’option.  
  
 **< Précédent** : renvoie à la page **Détecter les modifications**.  
  
 **Suivant >** : passe à la page **Vérifier le plan de mise à niveau**.  
  
 **Annuler** : met fin à l’Assistant sans déployer la DAC.  
  
##  <a name="ReviewUpgPlan"></a> Page Vérifier le plan de mise à niveau  
 Utilisez cette page pour vérifier les actions qui sont effectuées par le processus de mise à niveau. Ne poursuivez l'opération que si vous êtes sûr que la mise à niveau s'effectuera sans créer de problèmes.  
  
 **Les actions suivantes permettront de mettre à niveau la DAC.** - Vérifiez les informations affichées pour vous assurer que les actions prises seront correctes. La colonne **Action** affiche les actions, telles que les instructions Transact-SQL, qui seront exécutées pour effectuer la mise à niveau. La colonne **Perte de données** contiendra un avertissement si l'action associée risque de supprimer des données.  
  
 **Actualiser** – Actualise la liste des actions.  
  
 **Enregistrer le rapport d'action** - Enregistre le contenu de la fenêtre d'action dans un fichier HTML.  
  
 **Poursuivre malgré la perte possible des modifications** : indique que vous êtes conscient que certains objets ou données dans la base de données actuelle ne seront pas présents dans la nouvelle base de données, et que vous souhaitez poursuivre la mise à niveau. Vous ne devez sélectionner ce bouton que si vous avez analysé le rapport des modifications et si vous connaissez la procédure à effectuer pour transférer manuellement tous les objets ou données nécessaires dans la nouvelle base de données. Si vous n’en êtes pas sûr, cliquez sur le bouton **Enregistrer le rapport d’action** pour enregistrer le rapport des modifications et sur le bouton **Enregistrer les scripts** pour enregistrer le script Transact-SQL, puis cliquez sur **Annuler**. Analysez le rapport et le script, organisez le transfert de tous les objets et données nécessaires une fois la mise à niveau terminée, puis redémarrez l'Assistant.  
  
 **Enregistrer les scripts** : enregistre les instructions Transact-SQL qui permettront d’effectuer la mise à niveau vers un fichier texte.  
  
 **Paramètres par défaut** : rétablit la valeur par défaut (False) de l’option.  
  
 **< Précédent** : renvoie à la page **Détecter les modifications**.  
  
 **Suivant >** : passe à la page **Résumé**.  
  
 **Annuler** : met fin à l’Assistant sans déployer la DAC.  
  
##  <a name="Summary"></a> Page Résumé  
 Utilisez cette page pour effectuer une vérification finale des actions que l'Assistant prendra lors de la mise à niveau de la DAC.  
  
 **Les paramètres suivants seront utilisés pour mettre à niveau votre DAC.** - Vérifiez les informations affichées pour vous assurer que les actions prises seront correctes. La fenêtre affiche la DAC que vous avez sélectionnée pour être mise à niveau, et le package de DAC qui contient la nouvelle version de la DAC. La fenêtre s'affiche également si la version actuelle de la base de données est identique à la définition de la DAC actuelle, ou si la base de données a été modifiée.  
  
 **< Précédent** : vous renvoie à la page **Vérifier le plan de mise à niveau**.  
  
 **Suivant >** : déploie la DAC et affiche les résultats dans la page **Mettre à niveau la DAC**.  
  
 **Annuler** : met fin à l’Assistant sans déployer la DAC.  
  
##  <a name="Upgrade"></a> Page Mettre à niveau la DAC  
 Cette page signale la réussite ou l'échec de l'opération de mise à niveau.  
  
 **Mise à niveau de la DAC** : signale la réussite ou l’échec de chaque action entreprise pour mettre à niveau la DAC. Examinez les informations pour déterminer la réussite ou l'échec de chaque action. Toute action pour laquelle une erreur s'est produite aura un lien dans la colonne **Résultat** . Sélectionnez le lien pour consulter le rapport de d'erreur de cette action.  
  
 **Enregistrer le rapport** : sélectionnez ce bouton pour enregistrer le rapport de mise à niveau dans un fichier HTML. Le fichier signale l'état de chaque action, notamment toutes les erreurs générées par chacune des actions. Le dossier par défaut est un dossier SQL Server Management Studio\DAC Packages dans le dossier Documents de votre compte Windows.  
  
 **Terminer** : met fin à l’Assistant.  
  
##  <a name="UpgradeDACPowerShell"></a> Utilisation de PowerShell  
 **Pour mettre à niveau une DAC à l’aide de la méthode IncrementalUpgrade() dans un script PowerShell**  
  
1.  Créez un objet serveur SMO et affectez-lui l'instance qui contient la DAC à mettre à niveau.  
  
2.  Ouvrez un objet **ServerConnection** et connectez-vous à la même instance.  
  
3.  Utilisez **System.IO.File** pour charger le fichier de package DAC.  
  
4.  Utilisez **add_DacActionStarted** et **add_DacActionFinished** pour vous abonner aux événements de mise à niveau de la DAC.  
  
5.  Définissez **DacUpgradeOptions**.  
  
6.  Utilisez la méthode **IncrementalUpgrade** pour mettre à niveau la DAC.  
  
7.  Fermez le flux de fichier utilisé pour lire le fichier de package DAC.  
  
### <a name="example-powershell"></a>Exemple (PowerShell)  
 L’exemple suivant met à niveau une application DAC nommée MyApplication sur une instance par défaut du [!INCLUDE[ssDE](../../includes/ssde-md.md)] en utilisant une nouvelle version de DAC dans un package MyApplication2017.dacpac.  
  
```  
## Set a SMO Server object to the default instance on the local computer.  
CD SQLSERVER:\SQL\localhost\DEFAULT  
$srv = get-item .  
  
## Open a Common.ServerConnection to the same instance.  
$serverconnection = New-Object Microsoft.SqlServer.Management.Common.ServerConnection($srv.ConnectionContext.SqlConnectionObject)  
$serverconnection.Connect()  
$dacstore = New-Object Microsoft.SqlServer.Management.Dac.DacStore($serverconnection)  
  
## Load the DAC package file.  
$dacpacPath = "C:\MyDACs\MyApplication2017.dacpac"  
$fileStream = [System.IO.File]::Open($dacpacPath,[System.IO.FileMode]::OpenOrCreate)  
$dacType = [Microsoft.SqlServer.Management.Dac.DacType]::Load($fileStream)  
  
## Subscribe to the DAC upgrade events.  
$dacstore.add_DacActionStarted({Write-Host `n`nStarting at $(get-date) :: $_.Description})  
$dacstore.add_DacActionFinished({Write-Host Completed at $(get-date) :: $_.Description})  
  
## Upgrade the DAC and close the package.  
$dacName  = "MyApplication"  
  
## Set the upgrade options.  
$upgradeProperties = New-Object Microsoft.SqlServer.Management.Dac.DacUpgradeOptions  
$upgradeProperties.blockonchanges = $true  
$upgradeProperties.ignoredataloss = $false  
$upgradeProperties.rollbackonfailure = $true  
$ upgradeProperties.skippolicyvalidation = $false  
  
## Upgrade the DAC  
$dacstore.IncrementalUpgrade($dacName, $dacType, $upgradeProperties)  
## Close the package file.  
$fileStream.Close()  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Applications de la couche Données](../../relational-databases/data-tier-applications/data-tier-applications.md)   
 [SQL Server PowerShell](../../relational-databases/scripting/sql-server-powershell.md)  
  
  
