---
title: Configuration avancée pour SQL Server Machine Learning Services | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 5fc4e661f68a23ff2a954b832463eb60c7449057
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="advanced-configuration-options-for-sql-server-machine-learning-services"></a>Options de configuration avancée pour les Services de SQL Server Machine Learning
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article décrit les modifications que vous pouvez effectuer après l’installation, pour modifier la configuration de l’exécution du script externe et d’autres services associés d’apprentissage dans SQL Server.

**S’applique à :** SQL Server 2016 R Services, SQL Server 2017 d’apprentissage automatique Services

##  <a name="bkmk_Provisioning"></a> Comptes d’utilisateurs supplémentaires de disposition pour la machine apprentissage

Processus de script externe dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s’exécutent dans le contexte des comptes d’utilisateur local à faible privilège. Ces processus en cours d’exécution dans des comptes individuels à faible privilège présente les avantages suivants :

+ Réduit les privilèges des processus d’exécution de script externe sur le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ordinateur
+ Fournit une isolation entre les sessions d’un runtime externe tels que R ou Python.

Dans le cadre de l’installation, une nouvelle fenêtre *pool de comptes d’utilisateur* est créé qui contient les comptes d’utilisateur local requis pour l’exécution du processus du runtime externes, tels que R ou Python. Vous pouvez modifier le nombre d’utilisateurs en fonction des besoins pour prendre en charge les tâches d’apprentissage automatique. 

En outre, votre administrateur de base de données doit permettre à ce groupe l’autorisation pour se connecter à n’importe quelle instance où l’apprentissage a été activée. Pour plus d’informations, consultez [modifier le pool de comptes d’utilisateur pour SQL Server Machine Learning Services](../../advanced-analytics/r/modify-the-user-account-pool-for-sql-server-r-services.md).

À des ressources sensibles protext sur la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez définir une liste de contrôle d’accès (ACL) pour ce groupe. En spécifiant l’accès est refusé par le groupe de ressources, vous pouvez empêcher l’accès par des processus externes, tels que les runtimes R ou Python.

+ Le pool de comptes d’utilisateurs est lié à une instance particulière. Un pool distinct de comptes de travail est nécessaire pour chaque instance sur lequel machine learning a été activée. Les comptes ne peuvent pas être partagés par plusieurs instances.

+ Les noms de comptes d’utilisateur dans le pool sont au format SQLInstanceName*nn*. Par exemple, si vous utilisez l’instance par défaut pour l’apprentissage, le pool de comptes d’utilisateur prend en charge les noms de compte tels que MSSQLSERVER01, MSSQLSERVER02 et ainsi de suite.

+ La taille du pool de comptes d’utilisateur est statique et sa valeur par défaut est 20. Le nombre de sessions de runtime externe qui peuvent s’exécuter simultanément est limité par la taille de ce pool de comptes d’utilisateur. Pour modifier cette limite, un administrateur doit utiliser le Gestionnaire de Configuration SQL Server.

Pour plus d’informations sur la façon d’apporter des modifications pour le pool de comptes d’utilisateur, consultez [modifier le pool de comptes d’utilisateur pour SQL Server Machine Learning Services](../../advanced-analytics/r/modify-the-user-account-pool-for-sql-server-r-services.md).

##  <a name="bkmk_ManagingMemory"></a> Gérer la mémoire utilisée par les processus de script externe

Par défaut, les exécutions de script externe pour l’apprentissage ne servent pas plus de 20 % de mémoire. Il dépend de votre système, mais en général, vous pouvez trouver cette limite inadéquates pour les tâches d’apprentissage automatique graves telles que l’apprentissage d’un modèle ou de prévision sur plusieurs lignes de données. 

Pour prendre en charge d’apprentissage automatique, un administrateur peut augmenter cette limite. Lorsque vous procédez ainsi, vous devrez peut-être réduire la quantité de mémoire réservée pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou pour d’autres services. Vous devez également envisager d’à l’aide du gouverneur de ressources pour définir un pool de ressources externes ou des pools, afin que vous pouvez allouer les pools de ressources spécifiques aux travaux R ou Python.

Pour plus d’informations, consultez [la gouvernance de ressources pour l’apprentissage](../../advanced-analytics/r/resource-governance-for-r-services.md).


## <a name="bkmk_Launchpad"></a>Modifier le compte de service Launchpad

Distinct [!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] service est créé pour chaque instance sur laquelle vous avez configuré les services d’apprentissage automatique.

Par défaut, le service Launchpad est configuré pour s’exécuter avec le compte NT Service\MSSQLLaunchpad, qui dispose de toutes les autorisations nécessaires pour exécuter des scripts R. Toutefois, si vous modifiez ce compte, la zone de lancement peut ne pas pouvoir démarrer ou accéder à l’instance de SQL Server où les scripts externes doivent être exécutés.

Si vous modifiez le compte de service, veillez à utiliser l’application de **stratégie de sécurité locale** et mettre à jour les autorisations sur chaque compte de service afin d’inclure ces autorisations :

+ Changer les quotas de mémoire d'un processus (SeIncreaseQuotaPrivilege)
+ Outrepasser le contrôle de parcours (SeChangeNotifyPrivilege)
+ Ouvrir une session en tant que service (SeServiceLogonRight)
+ Remplacer un jeton de niveau processus (SeAssignPrimaryTokenPrivilege)

Pour plus d’informations sur les autorisations requises pour exécuter les services SQL Server, consultez [Configurer les comptes de service Windows et les autorisations](https://msdn.microsoft.com/library/ms143504.aspx#Windows).

##  <a name="bkmk_ChangingConfig"></a> Modifier les options de service avancées

Dans les versions antérieures de SQL Server 2016 R Services, vous pourriez modifier certaines propriétés du service en modifiant le [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] fichier de configuration. 

Toutefois, ce fichier est n’est plus utilisé pour la modification des configurations. Nous vous recommandons d’utiliser le Gestionnaire de Configuration SQL Server pour les modifications à la configuration du service, telles que le compte de service et le nombre d’utilisateurs.

**Pour modifier la configuration de Launchpad**

1. Ouvrez le [Gestionnaire de configuration SQL Server](../../relational-databases/sql-server-configuration-manager.md). 
2. SQL Server Launchpad d’avec le bouton droit et sélectionnez **propriétés**.

    + Pour modifier le compte de service, cliquez sur le **session** onglet.

    + Pour augmenter le nombre d’utilisateurs, cliquez sur le **avancé** onglet.


**Pour modifier les paramètres de débogage**

Quelques propriétés peuvent uniquement être modifiées à l’aide du fichier de configuration de la zone de lancement qui peut-être être utile dans certains cas limités, tels que le débogage. Le fichier de configuration est créé au cours de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le programme d’installation et par défaut est enregistré comme un fichier texte brut à l’emplacement suivant : `<instance path>\binn\rlauncher.config`

Pour pouvoir apporter des modifications à ce fichier, vous devez être administrateur sur l’ordinateur exécutant [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si vous modifiez le fichier, nous vous recommandons de faire une copie de sauvegarde avant d’enregistrer les modifications.

Le tableau suivant répertorie les paramètres avancés de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], avec les valeurs autorisées. 

|**Nom du paramètre**|**Type**|**Description**|
|----|----|----|
|TRAVAIL\_NETTOYAGE\_ON\_QUITTER|Entier |Il s’agit d’un paramètre exclusivement interne : ne modifiez pas cette valeur. </br></br>Spécifie si le dossier de travail temporaire créé pour chaque session de runtime externe doit être nettoyé une fois la session est terminée. Ce paramètre est utile pour le débogage. </br></br>Valeurs prises en charge sont **0** (désactivé) ou **1** (activé). </br></br>La valeur par défaut est 1, la signification de fichiers journaux est supprimée à la sortie.|
|TRACE\_NIVEAU|Entier |Configure le niveau de détail de trace de MSSQLLAUNCHPAD à des fins de débogage. Cela affecte les fichiers de trace dans le chemin d’accès spécifié par le paramètre LOG_DIRECTORY. </br></br>Valeurs prises en charge sont : **1** (erreur), **2** (performances), **3** (avertissement), **4** (informations). </br></br>La valeur par défaut est 1, ce qui signifie seulement les avertissements de sortie.|

Tous les paramètres prennent la forme d’une paire clé-valeur, chaque paramètre figurant sur une ligne distincte. Par exemple, pour modifier le niveau de suivi, vous devez ajouter la ligne `Default: TRACE_LEVEL=4`.

## <a name="see-also"></a>Voir aussi

[Considérations sur la sécurité](security-considerations-for-the-r-runtime-in-sql-server.md)
