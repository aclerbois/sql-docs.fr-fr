---
title: Problèmes courants avec le service Launchpad et de l’exécution du script externe dans SQL Server | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/31/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 08ab6e2db6d6cde5e41f7ddb88c8afd1da241df7
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34706837"
---
# <a name="common-issues-with-launchpad-service-and-external-script-execution-in-sql-server"></a>Problèmes courants avec le service Launchpad et de l’exécution du script externe dans SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

 Service SQL Server approuvée Launchpad prend en charge l’exécution du script externe pour R et Python. Sur SQL Server 2016 R Services SP1 fournit le service. SQL Server 2017 inclut le service Launchpad en tant que partie de l’installation intitial.

Plusieurs problèmes peuvent empêcher le Launchpad à partir de démarrage, y compris les problèmes de configuration ou de modifications ou manquant des protocoles réseau. Cet article fournit des conseils de dépannage pour nombreux problèmes. Pour les nous manquées, vous pouvez publier des questions à la [forum Machine Learning Server](https://social.msdn.microsoft.com/Forums/home?category=MicrosoftR).

**S’applique à :** SQL Server 2016 R Services, SQL Server 2017 d’apprentissage automatique Services

## <a name="determine-whether-launchpad-is-running"></a>Déterminer si le Launchpad est en cours d’exécution

1. Ouvrez le **Services** Panneau de configuration (Services.msc). Ou, à partir de la ligne de commande, tapez **SQLServerManager13.msc** ou **SQLServerManager14.msc** pour ouvrir [Gestionnaire de Configuration SQL Server](https://docs.microsoft.com/sql/relational-databases/sql-server-configuration-manager).

2. Prenez note de Launchpad s’exécute sous le compte de service. Chaque instance d’où R ou Python est activée doit avoir sa propre instance du service Launchpad. Par exemple, le service pour une instance nommée peut être un nom tel que _MSSQLLaunchpad$ InstanceName_.

3. Si le service est arrêté, redémarrez-le. Sur le redémarrage, s’il existe des problèmes avec la configuration, un message est publié dans le journal des événements système et le service est arrêté à nouveau. Vérifiez le journal des événements système pour plus d’informations sur l’arrêt du service.

4. Passez en revue le contenu de RSetup.log et assurez-vous qu’il n’y a pas d’erreurs dans le programme d’installation. Par exemple, le message *sortie avec le code 0* indique l’échec de démarrage du service.

5. Pour rechercher les autres erreurs, examinez le contenu de rlauncher.log.

## <a name="check-the-launchpad-service-account"></a>Vérifier le compte de service Launchpad

Le compte de service par défaut peut être « Service NT\$SQL2016 » ou « Service NT\$SQL2017 ». La dernière partie peut varier selon le nom de votre instance SQL.

Le service Launchpad (Launchpad.exe) s’exécute à l’aide d’un compte de service de faibles. Toutefois, pour démarrer R et Python et communiquer avec l’instance de base de données, le compte de service Launchpad nécessite des droits d’utilisateur suivants :

- Ouvrir une session en tant que service (SeServiceLogonRight)
- Remplacer un jeton de niveau processus (SeAssignPrimaryTokenPrivilege)
- Outrepasser le contrôle de parcours (SeChangeNotifyPrivilege)
- Ajuster les quotas de mémoire pour un processus (SeIncreaseQuotaSizePrivilege)

Pour plus d’informations sur ces droits d’utilisateur, consultez la section « privilèges et droits Windows » dans [Windows de configurer les comptes de service et autorisations](../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).

> [!TIP]
> Si vous êtes familiarisé avec l’utilisation de l’outil de prise en charge des Diagnostics plate-forme (SDP) pour les diagnostics de SQL Server, vous pouvez utiliser SDP pour passer en revue le fichier de sortie portant le nom MachineName_UserRights.txt.

## <a name="user-group-for-launchpad-cannot-log-on-locally"></a>Groupe d’utilisateurs pour Launchpad ne peut pas ouvrir une session localement

Pendant l’installation des services d’apprentissage automatique, SQL Server crée le groupe d’utilisateurs Windows **SQLRUserGroup** et il configure ensuite avec tous les droits nécessaires pour Launchpad pour vous connecter à SQL Server et exécuter des tâches de script externe. Si ce groupe d’utilisateurs est activé, il est également utilisé pour exécuter des scripts Python.

Toutefois, dans les organisations où les stratégies de sécurité restrictives sont appliquées, les droits requis par ce groupe peuvent avoir été supprimés manuellement ou ils peuvent être automatiquement révoqués par la stratégie. Si les droits ont été supprimées, Launchpad peut ne plus se connecter à SQL Server et SQL Server ne peut pas appeler le runtime externe.

Pour corriger ce problème, vérifiez que le groupe **SQLRUserGroup** dispose du droit système **Permettre l’ouverture d’une session locale**.

Pour plus d’informations, consultez [Windows de configurer les comptes de service et autorisations](https://msdn.microsoft.com/library/ms143504.aspx#Windows).

## <a name="permissions-to-run-external-scripts"></a>Autorisations pour exécuter des scripts externes

Même si le Launchpad est correctement configuré, il retourne une erreur si l’utilisateur n’est pas autorisé à exécuter des scripts R ou Python.

Si vous avez installé SQL Server en tant qu’un administrateur de base de données ou que vous êtes propriétaire d’une base de données, vous sont accordées automatiquement cette autorisation. Toutefois, les autres utilisateurs généralement aient plus d’autorisations limitées. S’ils essaient d’exécuter un script R, ils obtiennent une erreur de Launchpad.

Pour corriger le problème, dans SQL Server Management Studio, un administrateur de sécurité peut modifier le compte de connexion SQL ou un compte d’utilisateur Windows en exécutant le script suivant :

```SQL
GRANT EXECUTE ANY EXTERNAL SCRIPT TO <username>
```

Pour plus d’informations, consultez [GRANT (Transact-SQL](../t-sql/statements/grant-transact-sql.md).

## <a name="common-launchpad-errors"></a>Erreurs courantes de Launchpad

Cette section répertorie les messages d’erreur courants qui renvoie du Launchpad.

## <a name="unable-to-launch-runtime-for-r-script"></a>« Impossible de lancer le runtime pour le script R »

Si le groupe Windows pour les utilisateurs de R (également utilisé pour Python) ne peut pas se connecter à l’instance qui est en cours d’exécution R Services, vous pouvez voir les erreurs suivantes :

- Erreurs sont générées lorsque vous essayez d’exécuter des scripts R :

    * *Impossible de lancer le runtime pour le script « R ». Vérifiez la configuration du runtime « R ».*

    * *Une erreur de script externe s’est produite. Impossible de lancer le runtime.*

- Les erreurs générées par le [!INCLUDE[rsql_launchpad](../includes/rsql-launchpad-md.md)] service :

    * *Impossible d’initialiser le lanceur RLauncher.dll*

    * *Aucune DLL de lanceur n’a été enregistrée.*

    * *Journaux de sécurité indiquent que le compte de SERVICE NT a été incapable de se connecter*

Pour plus d’informations sur la façon d’accorder à ce groupe d’utilisateurs les autorisations nécessaires, consultez [installer SQL Server 2016 R Services](install/sql-r-services-windows-install.md).

> [!NOTE]
> Cette limitation ne s’applique pas si vous utilisez des connexions SQL pour exécuter des scripts R à partir d’une station de travail distante.

## <a name="logon-failure-the-user-has-not-been-granted-the-requested-logon-type"></a>« Échec de la connexion : l’utilisateur ne dispose pas du type d’accès demandé »

Par défaut, [!INCLUDE[rsql_launchpad_md](../includes/rsql-launchpad-md.md)] utilise le compte suivant lors du démarrage : `NT Service\MSSQLLaunchpad`. Le compte est configuré par [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)] le programme d’installation pour que toutes les autorisations nécessaires.

Si vous affectez un compte différent à Launchpad, ou le droit est supprimé par une stratégie sur l’ordinateur SQL Server, le compte n’a ne peut-être pas les autorisations nécessaires, et vous pouvez voir cette erreur :

>*Échec d’ouverture de session ERROR_LOGON_TYPE_NOT_GRANTED 1385 (0x569) : l’utilisateur ne bénéficie pas du type d’ouverture de session demandé sur cet ordinateur.*

Pour accorder des autorisations nécessaires pour le nouveau compte de service, utilisez l’application de la stratégie de sécurité locale et mettre à jour les autorisations sur le compte à inclure les autorisations suivantes :

+ Changer les quotas de mémoire d'un processus (SeIncreaseQuotaPrivilege)
+ Outrepasser le contrôle de parcours (SeChangeNotifyPrivilege)
+ Ouvrir une session en tant que service (SeServiceLogonRight)
+ Remplacer un jeton de niveau processus (SeAssignPrimaryTokenPrivilege)

## <a name="unable-to-communicate-with-the-launchpad-service"></a>« Impossible de communiquer avec le service Launchpad »

Si vous avez installé, puis apprentissage, mais que vous obtenez cette erreur lorsque vous essayez d’exécuter un script R ou Python, le Launchpad service pour l’instance est peut-être arrêté en cours d’exécution.

1. À partir d’une invite de commandes Windows, ouvrez le Gestionnaire de configuration SQL Server. Pour plus d’informations, consultez [SQL Server Configuration Manager](https://docs.microsoft.com/sql/relational-databases/sql-server-configuration-manager).

2. Avec le bouton droit de Launchpad de SQL Server pour l’instance, puis sélectionnez **propriétés**.

3. Sélectionnez le **Service** onglet et vérifiez que le service est en cours d’exécution. Si elle n’est pas en cours d’exécution, modifiez le **le Mode de démarrage** à **automatique**, puis sélectionnez **appliquer**.

4. Le redémarrage du service généralement de résoudre le problème afin que d’exécuter des scripts d’apprentissage machine. Si le redémarrage ne résout pas le problème, notez le chemin d’accès et les arguments dans le **chemin d’accès binaire** propriété et procédez comme suit :

    A. Vérifier le fichier .config de l’utilitaire de chargement et assurez-vous que le répertoire de travail est valide.

    B. Vérifiez que le groupe Windows qui est utilisé par le Launchpad peut se connecter à l’instance de SQL Server, comme décrit dans la [section précédente](#bkmk_LaunchpadTS).

    c. Si vous modifiez les propriétés de service, redémarrez le service Launchpad.

## <a name="fatal-error-creation-of-tmpfile-failed"></a>« Échec de la création d’une erreur irrécupérable de tmpFile »

Dans ce scénario, vous avez correctement installé les fonctionnalités de machine learning et Launchpad est en cours d’exécution. Vous essayez d’exécuter du code R ou Python simple, mais Launchpad échoue avec une erreur semblable à celui-ci : 

>*Impossible de communiquer avec le runtime pour le script R. Vérifiez la configuration requise du runtime R.*

En même temps, l’exécution du script externe écrit le message suivant en tant que partie du message STDERR : 

>*Erreur irrécupérable : la création de tmpfile a échoué.*

Cette erreur indique que le compte qui tente d’utiliser Launchpad ne dispose pas d’autorisation de se connecter à la base de données. Cette situation peut se produire lors de l’implémentation des stratégies de sécurité stricte. Pour déterminer si c’est le cas, passez en revue les journaux SQL Server et vérifiez pour voir si le compte MSSQLSERVER01 a été refusé lors de la connexion. Les mêmes informations sont fournies dans les fichiers journaux qui sont spécifiques à R\_SERVICES ou PYTHON\_SERVICES. Recherchez ExtLaunchError.log.

Par défaut, 20 comptes sont définies et associés au processus Launchpad.exe, avec les noms MSSQLSERVER01 via MSSQLSERVER20. Si vous faites un usage intensif de R ou Python, vous pouvez augmenter le nombre de comptes.

Pour résoudre ce problème, assurez-vous que le groupe a *autoriser la connexion localement* autorisations à l’instance locale où les fonctionnalités de machine learning ont été installées et activées. Dans certains environnements, ce niveau d’autorisation peut nécessiter une exception de la stratégie de groupe à partir de l’administrateur réseau.

## <a name="not-enough-quota-to-process-this-command"></a>« Pas de suffisamment de quota pour traiter cette commande »

Cette erreur peut indiquer plusieurs possibilités :

- LaunchPad peut avoir des utilisateurs externes insuffisantes pour exécuter la requête externe. Par exemple, si vous exécutez simultanément plus de 20 requêtes externes, et il existe uniquement 20 utilisateurs par défaut, une ou plusieurs requêtes risque d’échouer.

- Mémoire disponible est insuffisante pour traiter la tâche de R. Cette erreur se produit le plus souvent dans un environnement par défaut, où SQL Server peut être à l’aide de 70 pour cent des ressources de l’ordinateur. Pour plus d’informations sur la façon de modifier la configuration du serveur pour prendre en charge une plus grande utilisation de ressources par R, consultez [Opérationnalisation de votre code R](r/operationalizing-your-r-code.md).

## <a name="cant-find-package"></a>« Impossible de trouver le package »

Si vous exécutez le code R dans SQL Server et obtenez ce message, mais n’avez pas reçu le message lorsque vous avez exécuté le même code en dehors de SQL Server, cela signifie que le package n’est pas installé à l’emplacement de bibliothèque par défaut utilisé par SQL Server.

Cette erreur peut se produire de différentes façons :

- Vous avez installé un nouveau package sur le serveur, mais l’accès a été refusé, R installé le package dans une bibliothèque de l’utilisateur.

- Vous installé R Services installé un autre outil de R ou ensemble de bibliothèques, y compris Microsoft R Server (autonome), le Client Microsoft R, RStudio et ainsi de suite.

Pour déterminer l’emplacement de la bibliothèque de package de R qui est utilisée par l’instance, ouvrez SQL Server Management Studio (ou tout autre outil de requête de base de données), connectez-vous à l’instance, puis exécutez la procédure stockée suivante :

```SQL
EXEC sp_execute_external_script @language = N'R',  
@script = N' print(normalizePath(R.home())); print(.libPaths());'; 
```

#### <a name="sample-results"></a>Exemples de résultats

*Message(s) STDOUT provenant du script externe :*

*[1] » C:\\fichiers programme\\Microsoft SQL Server\\MSSQL13. SQL2016\\R_SERVICES »*

*[1] » C:/Program Files/Microsoft SQL Server/MSSQL13. R_SERVICES/SQL2016/bibliothèque »*

Pour résoudre ce problème, vous devez réinstaller le package à la bibliothèque d’instance de SQL Server.

>[!NOTE]
>Si vous avez mis à niveau une instance de SQL Server 2016 à utiliser la dernière version de Microsoft R, l’emplacement de la bibliothèque par défaut est différent. Pour plus d’informations, consultez [SqlBindR utilisé pour mettre à niveau une instance de R Services](r/use-sqlbindr-exe-to-upgrade-an-instance-of-sql-server.md).

## <a name="launchpad-shuts-down-due-to-mismatched-dlls"></a>LaunchPad s’arrête en raison d’une DLL incompatibles

Si vous installez le moteur de base de données avec d’autres fonctionnalités, le correctif logiciel du serveur et puis ajoutez ultérieurement la fonctionnalité d’apprentissage à l’aide du support d’origine, une version incorrecte des composants d’apprentissage peut être installée. Lorsque Launchpad détecte une incompatibilité de version, il s’arrête et crée un fichier dump.

Pour éviter ce problème, veillez à installer les nouvelles fonctionnalités au même niveau de correctif logiciel en tant que l’instance de serveur.

**Mauvaise méthode pour mettre à niveau :**

1. Installer SQL Server 2016 sans R Services.
2. Mise à niveau de la mise à jour Cumulative de SQL Server 2016 2.
3. Installer R Services (de-de base de données) à l’aide du support RTM.

**La méthode correcte pour mettre à niveau :**

1. Installer SQL Server 2016 sans R Services.
2. Mise à niveau de SQL Server 2016 vers le niveau de correctif souhaité. Par exemple, installer le Service Pack 1, puis Cumulative Update 2.
3. Pour ajouter la fonctionnalité au niveau du correctif approprié, réexécutez le programme d’installation de SP1 et CU2 et appuyez sur R Services (de-de base de données). 

## <a name="launchpad-fails-to-start-if-8dot3-notation-is-required"></a>LaunchPad ne démarre pas si la notation de modèle 8.3 est requise

> [!NOTE] 
> Sur les anciens systèmes, Launchpad peut échouer démarrer s’il existe une exigence de notation modèle 8.3. Cette exigence a été supprimée dans les versions ultérieures. Les clients SQL Server 2016 R Services doivent installer une des opérations suivantes :
> * SQL Server 2016 SP1 et CU1 : [mise à jour Cumulative 1 pour SQL Server](https://support.microsoft.com/help/3208177/cumulative-update-1-for-sql-server-2016-sp1).
> * SQL Server 2016 RTM, Cumulative Update 3 et cela [correctif](https://support.microsoft.com/help/3210110/on-demand-hotfix-update-package-for-sql-server-2016-cu3), qui est disponible à la demande.

Pour la compatibilité avec R, SQL Server 2016 R Services (de-de base de données) requis le lecteur où la fonctionnalité est installée pour prendre en charge la création de noms de fichiers courts à l’aide de *modèle 8.3 notation*. Un nom de fichier au format 8.3 est également appelé un *nom de fichier court*, et il est utilisé pour la compatibilité avec les versions antérieures de Microsoft Windows ou en guise d’alternative à des noms de fichier longs.

Si le volume sur lequel vous installez R ne prend pas en charge les noms de fichiers courts, les processus qui lancent R à partir de SQL Server ne peuvent pas être en mesure de localiser le fichier exécutable correct et Launchpad ne démarrera pas.

Pour résoudre ce problème, vous pouvez activer la notation de modèle 8.3 sur le volume où SQL Server est installé et sur lequel les Services de R est installé. Vous devez ensuite fournir le nom court pour le répertoire de travail dans le fichier de configuration de R Services.

1. Pour activer la notation de modèle 8.3, exécutez l’utilitaire fsutil avec le *8dot3name* argument comme décrit ici : [fsutil 8dot3name](https://technet.microsoft.com/library/ff621566(v=ws.11).aspx).

2. Après l’activation de la notation de modèle 8.3, ouvrez le fichier RLauncher.config et notez la propriété de `WORKING_DIRECTORY`. Pour plus d’informations sur la façon de trouver ce fichier, consultez [collecte des données pour la résolution des problèmes de Machine Learning](data-collection-ml-troubleshooting-process.md).

3. Utiliser l’utilitaire fsutil avec le *fichier* argument pour spécifier un chemin d’accès de fichier court pour le dossier qui est spécifié dans WORKING_DIRECTORY.

4. Modifiez le fichier de configuration pour spécifier le même répertoire de travail que vous avez entré dans la propriété WORKING_DIRECTORY. Vous pouvez également spécifier un autre répertoire de travail et choisissez un chemin d’accès existant qui est déjà compatible avec la notation de modèle 8.3.


## <a name="next-steps"></a>Étapes suivantes

[Problèmes connus et dépannage de machine Learning Services](machine-learning-troubleshooting-faq.md)

[Collecte de données pour la résolution des problèmes d’apprentissage](data-collection-ml-troubleshooting-process.md)

[FAQ sur la mise à niveau et l’installation](r/upgrade-and-installation-faq-sql-server-r-services.md)

[Résoudre les problèmes de connexions du moteur de base de données](../database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine.md)
