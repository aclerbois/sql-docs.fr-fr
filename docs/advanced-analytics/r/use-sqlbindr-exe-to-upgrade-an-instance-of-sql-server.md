---
title: Mise à niveau des composants R et Python dans les instances de SQL Server R (Machine Learning Services) | Documents Microsoft
description: Mise à niveau R et Python dans SQL Server 2016 R Services ou SQL Server 2017 Machine Learning Services à l’aide de sqlbindr.exe à lier à un serveur de Machine Learning.
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/05/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 11b9e58c583712d8ee5ae70f4dbb98b6c175239c
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34707687"
---
# <a name="upgrade-machine-learning-r-and-python-components-in-sql-server-instances"></a>Mise à niveau machine learning (R et Python) des composants dans les instances de SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

L’intégration R et Python dans SQL Server inclut les packages open source et les propriétaires de Microsoft. Sous maintenance de SQL Server standard, les packages R et Python sont mis à jour en fonction du cycle de version de SQL Server, avec des correctifs pour les packages existants à la version actuelle. 

La plupart des chercheurs de données sont habitués à l’utilisation de packages plus récentes dès qu’elles sont disponibles. Pour SQL Server 2017 Machine Learning Services (de-de base de données) et SQL Server 2016 R Services (de-de base de données), vous pouvez obtenir des versions plus récentes de R et Python en modifiant le *liaison* à partir de la maintenance de SQL Server [Microsoft Machine Learning Server](https://docs.microsoft.com/en-us/machine-learning-server/index) et [politique du cycle de vie moderne](https://support.microsoft.com/help/30881/modern-lifecycle-policy).

Liaison ne modifie pas les notions de base de votre installation : intégration R et Python fait toujours partie d’une instance du moteur de base de données, Gestionnaire de licences n’est pas modifiée (sans coût supplémentaire associé aux liaisons), et contiennent les stratégies de prise en charge de SQL Server pour la base de données moteur. Mais la reliaison modifie comment sont traitées les packages R et Python. Le reste de cet article décrit le mécanisme de liaison et son fonctionnement pour chaque version de SQL Server.

> [!NOTE]
> Liaison s’applique uniquement les instances (de-de base de données). Liaison s’applique pas à une installation (autonome).

**Considérations relatives à la liaison SQL Server 2017**

Pour SQL Server 2017 Machine Learning Services, vous pouvez envisager de liaison uniquement quand Microsoft Machine Learning Server commence à offrir des packages supplémentaires ou des versions plus récentes sur ce que vous ont déjà.

**Considérations relatives à la liaison SQL Server 2016**

Pour SQL Server 2016 R Services fournit des clients, la liaison de mise à jour des packages R, les nouveaux packages ne fait pas partie de l’installation d’origine et de modèles préformés, qui peut plus être actualisé à chaque nouvelle version majeure et mineure du serveur de Microsoft Machine Learning. Liaison ne donne pas une prise en charge de Python, qui est une fonctionnalité de SQL Server 2017. 

## <a name="version-map"></a>Mappage de version

Le tableau suivant est un mappage de version, affichant des versions de package sur véhicules de version afin que vous pouvez déterminer les chemins d’accès de la mise à niveau potentional lorsque vous liez à Microsoft Machine Learning Server (anciennement appelé R Server avant l’ajout de la prise en charge de Python à compter de MLS 9.2.1). 

Notez que liaison ne garantit pas la version récente de R ou Anaconda. Lorsque vous liez à Microsoft Machine Learning serveur (MLS), vous obtenez la version de R ou Python installée via le programme d’installation, qui ne peut pas être la dernière version disponible sur le web.

[**SQL Server 2016 R Services**](../install/sql-r-services-windows-install.md)

Composant |Version initiale | [R Server 9.0.1](https://docs.microsoft.com/machine-learning-server/install/r-server-install-windows) | [R Server 9.1](https://docs.microsoft.com/machine-learning-server/install/r-server-install-windows) | [MLS 9.2.1](https://docs.microsoft.com/machine-learning-server/install/machine-learning-server-windows-install) | [MLS 9.3](https://docs.microsoft.com/machine-learning-server/install/machine-learning-server-windows-install) |
----------|----------------|----------------|--------------|---------|-------|
Microsoft R Open (MRO) sur R | R 3.2.2     | R 3.3.2   |R 3.3.3   | R 3.4.1  | R 3.4.3 |
[RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler) | 8.0.3  | 9.0.1 |  9.1 |  9.2.1 |  9.3 |
[MicrosoftML](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/microsoftml-package)| néant | 9.0.1 |  9.1 |  9.2.1 |  9.3 |
[modèles préformés](https://docs.microsoft.com/machine-learning-server/install/microsoftml-install-pretrained-models)| néant | 9.0.1 |  9.1 |  9.2.1 |  9.3 |
[sqlrutils](https://docs.microsoft.com/machine-learning-server/r-reference/sqlrutils/sqlrutils)| néant | 1.0 |  1.0 |  1.0 |  1.0 |
[olapR](https://docs.microsoft.com/machine-learning-server/r-reference/olapr/olapr) | néant | 1.0 |  1.0 |  1.0 |  1.0 |


[**SQL Server 2017 apprentissage Services**](../install/sql-machine-learning-services-windows-install.md)

Composant |Version initiale | MLS 9.3 | | | |
----------|----------------|---------|-|-|-|-|
Microsoft R Open (MRO) sur R | R 3.3.3 | R 3.4.3 | | | |
[RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler) |   9.2 |  9.3 | | | |
[MicrosoftML](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/microsoftml-package) | 9.2  | 9.3| | | |
[sqlrutils](https://docs.microsoft.com/machine-learning-server/r-reference/sqlrutils/sqlrutils)| 1.0 |  1.0 | | | |
[olapR](https://docs.microsoft.com/machine-learning-server/r-reference/olapr/olapr) | 1.0 |  1.0 | | | |
Anaconda 4.2 sur Python 3.5  | 4.2/3.5.2 | 4.2/3.5.2 | | | |
[revoscalepy](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/revoscalepy-package) | 9.2  | 9.3| | | |
 [microsoftml](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/microsoftml-package) | 9.2  | 9.3| | | |
[modèles préformés](https://docs.microsoft.com/machine-learning-server/install/microsoftml-install-pretrained-models) | 9.2 | 9.3| | | |

## <a name="how-component-upgrade-works"></a>Fonctionne de la mise à niveau du composant

Mise à niveau du composant s’effectue via *liaison* une instance de SQL Server 2016 R Services (ou une instance de SQL Server 2017 Machine Learning Services) à [Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/index). 

Microsoft Machine Learning Server est un produit de serveur local séparer à partir de SQL Server, mais avec le même interpréteurs et packages. Liaison d’échanges, le mécanisme de mise à jour du service SQL Server afin que vous puissiez utiliser les packages R et Python livrée avec Microsoft Machine Learning Server, qui sont souvent plus récents que ceux installés par SQL Server. Les stratégies de commutation est une option avantageuse pour les équipes de science des données qui ont besoin de nouvelle génération R et des modules Python pour leurs solutions. 

Liaison est exécutée par le [programme d’installation MLS](https://docs.microsoft.com/machine-learning-server/install/machine-learning-server-windows-install). Le programme d’installation met à jour des packages R et Python spécifiques, mais ne remplace pas votre instance de la base de données SQL Server avec une autonome, l’installation du serveur déconnecté.

+ Sans liaison, les packages R et Python sont corrigés pour corriger les bogues lorsque vous installez un service pack SQL Server ou la mise à jour cumulative (CU). 
+ La liaison de versions plus récentes de package peuvent être appliquées à votre instance, indépendamment de la planification du déclenchement CU, sous la [politique moderne](https://support.microsoft.com/help/30881/modern-lifecycle-policy) et les versions de Microsoft Machine Learning Server. La stratégie de prise en charge du cycle de vie moderne offre des mises à jour plus fréquentes sur une durée de vie plus courte d’un an. Après la liaison, vous continue à utiliser le programme d’installation MLS pour les futures mises à jour de R et Python dès qu’elles deviennent disponibles dans Microsoft Machine Learning Server.

Liaison s’applique uniquement les fonctions R et Python. À savoir, packages open source pour les fonctionnalités de R et Python (Microsoft R Open, Anaconda) et le propriétaires packages RevoScaleR, revoscalepy et ainsi de suite. Liaison ne modifie pas le modèle de prise en charge pour l’instance du moteur de base de données et ne change pas la version de SQL Server.

La liaison est réversible. Vous pouvez revenir à la maintenance de SQL Server par [la séparation de l’instance](#bkmk_Unbind) et réparer votre instance de moteur de base de données SQL Server.

Étapes pour la liaison totalise, sont les suivantes :

+ Démarrez avec une installation existante, configuration de SQL Server 2016 R Services (ou SQL Server 2017 Machine Learning Services).
+ Déterminer la version du serveur de Microsoft Machine Learning a les composants mis à niveau que vous souhaitez utiliser.
+ Téléchargez et exécutez le programme d’installation pour cette version. Le programme d’installation détecte l’instance existante, ajoute une option de liaison et retourne une liste d’instances compatibles.
+ Choisissez l’instance que vous souhaitez lier, puis sur Terminer le programme d’installation pour exécuter la liaison.

En termes d’expérience utilisateur, la technologie et la façon dont vous travaillez est inchangée. La seule différence est la présence de packages avec version plus récente et éventuellement d’autres packages non disponibles à l’origine via SQL Server (par exemple, MicrosoftML pour les clients de SQL Server 2016 R Services).

## <a name="bkmk_BindWizard"></a>Lier à MLS à l’aide du programme d’installation

Le programme d’installation de Microsoft Machine Learning détecte les fonctionnalités existantes et la version de SQL Server et appelle un utilitaire appelé SqlBindR.exe pour modifier la liaison. En interne, SqlBindR est chaîné au programme d’installation et utilisée indirectement. Une version ultérieure, vous pouvez appeler SqlBindR directement à partir de la ligne de commande d’exercer des options spécifiques.

1. Dans SSMS, exécutez `SELECT @@version` pour vérifier le serveur répond aux exigences de version minimale. 

   Pour SQL Server 2016 R Services, la valeur minimale est [Service Pack 1](https://www.microsoft.com/download/details.aspx?id=54276) et [CU3](https://support.microsoft.com/help/4019916/cumulative-update-3-for-sql-server-2016-sp1).

1. Vérifiez la version de base de R et de packages de RevoScaleR pour vérifier que les versions existantes sont inférieures à ce que vous envisagez de les remplacer par. Pour SQL Server 2016 R Services, package de Base de R est 3.2.2 et RevoScaleR est 8.0.3.

    ```SQL
    EXECUTE sp_execute_external_script
    @language=N'R'
    ,@script = N'str(OutputDataSet);
    packagematrix <- installed.packages();
    Name <- packagematrix[,1];
    Version <- packagematrix[,3];
    OutputDataSet <- data.frame(Name, Version);'
    , @input_data_1 = N''
    WITH RESULT SETS ((PackageName nvarchar(250), PackageVersion nvarchar(max) ))
    ```

1. Télécharger Microsoft Machine Learning Server sur l’ordinateur qui dispose de l’instance que vous souhaitez mettre à niveau. Nous vous recommandons du [version la plus récente](https://docs.microsoft.com/machine-learning-server/install/machine-learning-server-windows-install#download-machine-learning-server-installer).

1. Décompressez le dossier et démarrer ServerSetup.exe, situé sous MLSWIN93.

   ![Assistant Installation du serveur de Microsoft Machine Learning](media/mls-921-installer-start.PNG)

1. Sur **configurer l’installation**, vérifiez les composants à mettre à niveau et passez en revue la liste des instances compatibles. 

   Sur la gauche, choisissez toutes les fonctionnalités que vous souhaitez conserver ou de mettre à niveau. Vous ne peut pas mettre à niveau certaines fonctionnalités et pas d’autres. Une case à cocher vide supprime cette fonctionnalité, en supposant qu’il est actuellement installée. Dans la capture d’écran, une instance donnée de SQL Server 2016 R Services (MSSQL13), R et la version de R des modèles dont l’apprentissage sont sélectionnés. Cette configuration est valide, car SQL Server 2016 prend en charge R, mais pas les Python.

   Sur la droite, sélectionnez la case à cocher en regard du nom d’instance. Si aucune instance n’est répertorié, vous avez une combinaison incompatible. Si vous ne sélectionnez pas une instance, une nouvelle installation autonome du serveur de Machine Learning est créée, et les bibliothèques de SQL Server sont identiques. Si vous ne pouvez pas sélectionner une instance, il ne peut pas être à [SP1 CU3](https://support.microsoft.com/help/4019916/cumulative-update-3-for-sql-server-2016-sp1). 

    ![Assistant Installation du serveur de Microsoft Machine Learning](media/mls-931-installer-mssql13.png)

1. Sur le **contrat de licence** page, sélectionnez **J’accepte les termes** pour accepter les termes du contrat de licence pour l’apprentissage d’ordinateur serveur. 

1. Sur des pages successives, fournir son consentement pour les conditions de licences supplémentaires pour tous les composants open source que vous avez sélectionné, telles que Microsoft R Open ou la distribution de Python Anaconda.

1. Sur le **presque** page, notez le dossier d’installation. Le dossier par défaut est \Program Files\Microsoft\ML Server.

    Si vous souhaitez modifier le dossier d’installation, cliquez sur **avancé** pour revenir à la première page de l’Assistant. Toutefois, vous devez répéter toutes les sélections précédentes.

Pendant le processus d’installation, toutes les bibliothèques R ou Python utilisés par SQL Server sont remplacés et Launchpad est mis à jour pour utiliser les composants les plus récents. Par conséquent, si l’instance utilisé précédemment des bibliothèques dans le dossier R_SERVICES par défaut, après mise à niveau ces bibliothèques sont supprimés et les propriétés pour le service Launchpad sont modifiées pour utiliser les bibliothèques dans le nouvel emplacement.

Liaison affecte le contenu de ces dossiers : C:\Program Files\Microsoft SQL Server\MSSQL13. MSSQLSERVER\R_SERVICES\library est remplacé par le contenu de C:\Program Files\Microsoft\ML Server\R_SERVER. Le deuxième dossier et son contenu est créé par le programme d’installation du serveur Microsoft Machine Learning. 

Si la mise à niveau échoue, vérifiez [codes d’erreur SqlBindR](#sqlbindr-error-codes) pour plus d’informations.

## <a name="confirm-binding"></a>Confirmer la liaison

Vérifiez la version de R et RevoScaleR pour confirmer que les versions plus récentes. Utilisez la console R distribuée avec les packages R dans votre instance du moteur de base de données pour obtenir des informations de package :

```SQL
EXECUTE sp_execute_external_script
@language=N'R'
,@script = N'str(OutputDataSet);
packagematrix <- installed.packages();
Name <- packagematrix[,1];
Version <- packagematrix[,3];
OutputDataSet <- data.frame(Name, Version);'
, @input_data_1 = N''
WITH RESULT SETS ((PackageName nvarchar(250), PackageVersion nvarchar(max) ))
```

Pour SQL Server 2016 R Services liés à la Machine Learning serveur 9.3, package de Base de R doit être 3.4.1, RevoScaleR doit être 9.3, et vous devez également avoir MicrosoftML 9.3. 

Si vous avez ajouté les modèles dont l’apprentissage, les modèles sont incorporés dans la bibliothèque MicrosoftML et vous pouvez les appeler via les fonctions MicrosoftML. Pour plus d’informations, consultez [exemples R pour MicrosoftML](https://docs.microsoft.com/machine-learning-server/r/sample-microsoftml).

## <a name="offline-binding-no-internet-access"></a>Liaison en mode hors connexion (aucun accès internet)

Pour les systèmes sans connexion internet, vous pouvez télécharger les fichiers de programme d’installation et .cab sur un ordinateur connecté à internet et puis transférer les fichiers vers le serveur isolé. 

Le programme d’installation (ServerSetup.exe) inclut les packages de Microsoft (RevoScaleR, MicrosoftML, olapR, sqlRUtils). Les fichiers .cab fournissent d’autres composants. Par exemple, le fichier cab « SRO » fournit R Open, distribution de Microsoft open source r

Les instructions suivantes expliquent comment placer les fichiers pour une installation en mode hors connexion.

1. Téléchargez le programme d’installation MLS. Il télécharge sous la forme d’un seul fichier zip. Nous vous recommandons du [version la plus récente](https://docs.microsoft.com/machine-learning-server/install/machine-learning-server-windows-install#download-machine-learning-server-installer), mais vous pouvez également installer [les versions antérieures](https://docs.microsoft.com/machine-learning-server/install/r-server-install-windows-offline#download-required-components).

1. Télécharger les fichiers .cab. Les liens suivants concernent la version 9.3. Si vous avez besoin des versions antérieures, les liens supplémentaires sont accessibles dans [R Server 9.1](https://docs.microsoft.com/machine-learning-server/install/r-server-install-windows-offline#download-required-components). Rappelez-vous que Python/Anaconda peut uniquement être ajouté à une instance de SQL Server 2017 Machine Learning Services. Il existe des modèles dont l’apprentissage de R et Python ; le fichier .cab fournit des modèles dans les langues que vous utilisez.

    | Fonctionnalité | Télécharger |
    |---------|----------|
    | R       | [SRO_3.4.3.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=867186&clcid=1033) |
    | Python  | [SPO_9.3.0.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=859054) | 
    | Modèles dont l’apprentissage | [MLM_9.3.0.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=859053) |

1. Transférer les fichiers .zip et .cab sur le serveur cible.

1. Sur le serveur, tapez `%temp%` dans la commande Exécuter pour obtenir l’emplacement physique du répertoire temporaire. Le chemin d’accès physique varie selon l’ordinateur, mais il est généralement `C:\Users\<your-user-name>\AppData\Local\Temp`.

1. Placez les fichiers .cab dans le dossier % Temp%.

1. Décompressez le programme d’installation.

1. Exécutez ServerSetup.exe et suivez les instructions à l’écran pour terminer l’installation.

## <a name="bkmk_BindCmd"></a>Opérations de ligne de commande

Une fois que vous exécutez Microsoft Machine Learning serveur, un utilitaire de ligne de commande appelé SqlBindR.exe devient disponible que vous pouvez utiliser pour la liaison des opérations. Par exemple, si vous décidez d’inverser une liaison, vous pourriez exécuter à nouveau le programme d’installation ou utiliser l’utilitaire de ligne de commande. En outre, vous pouvez utiliser cet outil pour vérifier pour l’instance de compatibilité et la disponibilité.

> [!TIP]
> Impossible de trouver SqlBindR ? Le programme d’installation n’ont probablement pas exécutées. SqlBindR est disponible uniquement après l’exécution de programme d’installation du serveur Machine Learning.

1. Ouvrez une invite de commandes en tant qu’administrateur et accédez au dossier contenant sqlbindr.exe. L’emplacement par défaut est C:\Program Files\Microsoft\MLServer\Setup

2. Tapez la commande suivante pour afficher la liste des instances disponibles : `SqlBindR.exe /list`
  
   Notez le nom complet de l’instance tel qu’il est répertorié. Par exemple, le nom d’instance peut être MSSQL14. MSSQLSERVER pour une instance par défaut, ou quelque chose comme nom du serveur. MYNAMEDINSTANCE.

3. Exécutez le **SqlBindR.exe** avec la */lier* argument et spécifiez le nom de l’instance à mettre à niveau, à l’aide du nom de l’instance qui a été retourné à l’étape précédente.

   Par exemple, pour mettre à niveau l’instance par défaut, tapez :  `SqlBindR.exe /bind MSSQL14.MSSQLSERVER`

4. Une fois la mise à niveau terminée, redémarrez le service de Launchpad associé à n’importe quelle instance qui a été modifié.

## <a name="bkmk_Unbind"></a>Rétablir ou dissocier une instance

Vous pouvez restaurer une instance liée à une installation initiale des composants R et Python, établie par le programme d’installation de SQL Server. Il existe trois parties pour le retour à la maintenance de SQL Server.

+ [Étape 1 : Supprimer la liaison à partir du serveur d’apprentissage Microsoft](#step-1-unbind)
+ [Étape 2 : Restaurer l’instance à l’état d’origine](#step-2-restore)
+ [Étape 3 : Réinstallez tous les packages que vous avez ajouté à l’installation](#step-3-reinstall-packages)

<a name="step-1-unbind"></a> 

### <a name="step-1-unbind"></a>Étape 1 : supprimer la liaison

Vous avez deux options de l’annulation de la liaison : ré-exécutez à nouveau le programme d’installation ou d’utiliser l’utilitaire de ligne de commande SqlBindR.

#### <a name="bkmk_wizunbind"></a> Supprimer la liaison à l’aide du programme d’installation

1. Recherchez le programme d’installation pour l’apprentissage d’ordinateur serveur. Si vous avez supprimé le programme d’installation, vous devrez peut-être télécharger à nouveau, ou copiez-la depuis un autre ordinateur.
2. Veillez à exécuter le programme d’installation sur l’ordinateur qui a l’instance que vous souhaitez supprimer la liaison.
2. Le programme d’installation identifie les instances locales qui sont des candidats pour l’annulation de la liaison.
3. Désactivez la case à cocher en regard de l’instance que vous souhaitez revenir à la configuration d’origine.
4. Acceptez le contrat de licence. Vous devez indiquer votre acceptation des termes du contrat de licence même lors de l’installation.
5. Cliquez sur **Terminer**. Le processus prend un certain temps.

#### <a name="bkmk_cmdunbind"></a> Supprimer la liaison à l’aide de la ligne de commande

1. Ouvrez une invite de commandes et accédez au dossier qui contient **sqlbindr.exe**, comme décrit dans la section précédente.

2. Exécutez la commande **SqlBindR.exe** avec l’argument */unbind*, puis spécifiez l’instance.

   Par exemple, la commande suivante reprend l’instance par défaut :
   
    `SqlBindR.exe /unbind MSSQL14.MSSQLSERVER`

<a name="step-2-restore"></a> 

###  <a name="step-2-repair-the-sql-server-instance"></a>Étape 2 : Réparez l’instance de SQL Server

Exécutez le programme d’installation de SQL Server pour réparer l’instance du moteur de base de données a les caractéristiques de R et Python. Mises à jour existantes sont conservées, mais si vous avez manqué les mises à jour des packages R et Python de maintenance de SQL Server, cette étape s’applique les correctifs.

Il s’agit plus de travail, mais vous pouvez également désinstaller complètement et réinstaller l’instance du moteur de base de données et ensuite appliquer toutes les mises à jour de service.

<a name="step-3-reinstall-packages"></a> 

### <a name="step-3-add-any-third-party-packages"></a>Étape 3 : Ajouter des packages tiers

Vous avez ajouté des autres packages tiers ou open source à votre bibliothèque de package. Étant donné que l’emplacement de la bibliothèque de package par défaut en inversant la liaison des commutateurs, vous devez réinstaller les packages à la bibliothèque R et Python est maintenant en utilisant. Pour plus d’informations, consultez [par défaut des packages](installing-and-managing-r-packages.md), [installer de nouveaux packages R](install-additional-r-packages-on-sql-server.md), et [installer de nouveaux packages Python](../python/install-additional-python-packages-on-sql-server.md).

## <a name="sqlbindrexe-command-syntax"></a>Syntaxe de commande SqlBindR.exe

### <a name="usage"></a>Utilisation

`sqlbindr [/list] [/bind <SQL_instance_ID>] [/unbind <SQL_instance_ID>]`

### <a name="parameters"></a>Paramètres

|Nom   |Description|
|------|------|
|*list*| Affiche une liste de tous les ID d’instances de bases de données SQL sur l’ordinateur actuel|
|*bind*| Met à niveau l’instance de base de données SQL spécifiée vers la version la plus récente de R Server, et garantit que l’instance obtient automatiquement les mises à niveau ultérieures de R Server|
|*unbind*|Désinstalle la version la plus récente de R Server de l’instance de base de données SQL spécifiée, et empêche les mises à niveau ultérieures de R Server d’affecter l’instance|

<a name="sqlbinder-error-codes"><a/>

## <a name="binding-errors"></a>Erreurs de liaison

Programme d’installation MLS et SqlBindR retournent les codes d’erreur suivants et les messages.

|Code d'erreur  | Message           | Détails               |
|------------|-------------------|-----------------------|
|Erreur 0 de liaison | OK (réussite) | Liaison transmis sans erreur. |
|Erreur 1 de liaison | Arguments non valides | Erreur de syntaxe. |
|Erreur 2 de liaison | Action non valide | Erreur de syntaxe. |
|Erreur 3 de liaison | Instance non valide | Une instance existe, mais n’est pas valide pour la liaison. |
|Erreur 4 de liaison | Ne peut pas être liée | |
|Liaison d’erreur 5 | Déjà liée | Vous avez exécuté la commande *bind* , mais l’instance spécifiée est déjà liée. |
|Erreur 6 de liaison | Échec de la liaison | Une erreur s’est produite lors de l’annulation de l’instance de la liaison. Cette erreur peut se produire si vous exécutez le programme d’installation MLS sans sélectionner toutes les fonctionnalités. Liaison exige que vous sélectionnez une instance MSSQL et R et Python, en supposant que l’instance SQL Server 2017.|
|Liaison d’erreur 7 | Non lié | L’instance du moteur de base de données a R Services ou SQL Server Machine Learning Services. L’instance n’est pas lié à Microsoft Machine Learning Server. |
|Erreur 8 de liaison | Annuler la liaison a échoué | Une erreur s’est produite lors de l’annulation de l’instance de la liaison. |
|Erreur 9 de liaison | Aucune instance trouvée | Aucune instance de moteur de base de données a été trouvée sur cet ordinateur. |

## <a name="known-issues"></a>Problèmes connus

Cette section répertorie spécifique de problèmes connus liés à l’aide de l’utilitaire SqlBindR.exe ou aux mises à niveau du serveur Machine Learning susceptibles d’affecter les instances de SQL Server.

### <a name="restoring-packages-that-were-previously-installed"></a>Restauration des packages qui ont été précédemment installées

Si vous mettez à niveau Microsoft R Server 9.0.1, la version de SqlBindR.exe pour cette version Impossible de restaurer les packages d’origine ou les composants R complètement, demandant à l’utilisateur d’exécuter la réparation de SQL Server sur l’instance, s’appliquent à toutes les versions de service, puis redémarrez l’instance.

Une version ultérieure de SqlBindR automatiquement restaurer les fonctions R d’origine, en éliminant la nécessité pour la réinstallation des composants de R ou ré-appliquer le correctif. Toutefois, vous devez installer les mises à jour du package de R qui ont peut-être été ajoutées après l’installation initiale.

Si vous avez utilisé les rôles de gestion de package à installer et partager le package, cette tâche est beaucoup plus facile : vous pouvez utiliser des commandes R pour synchroniser les packages installés dans le système de fichiers à l’aide d’enregistrements dans la base de données et vice versa. Pour plus d’informations, consultez [gestion des packages R pour SQL Server](install-additional-r-packages-on-sql-server.md).

### <a name="problems-with-multiple-upgrades-from-sql-server"></a>Problèmes avec plusieurs mises à niveau à partir de SQL Server

Si vous avez précédemment mis à niveau une instance de SQL Server 2016 R Services à 9.0.1, lorsque vous exécutez le programme d’installation de nouveau pour Microsoft R Server 9.1.0, il affiche une liste de toutes les instances valides, puis sélectionne précédemment liée d’instances par défaut. Si vous continuez, les instances précédemment liés sont indépendants. Par conséquent, la 9.0.1 antérieures installation est supprimée, y compris tous les packages, mais la nouvelle version de Microsoft R Server (9.1.0) n’est pas installée.

Pour résoudre ce problème, vous pouvez modifier l’installation de R Server existante comme suit :
1. Dans le panneau de configuration, ouvrez **Ajout / Suppression de programmes**.
2. Recherchez Microsoft R Server, puis cliquez sur **modification/modifier**.
3. Lorsque le programme d’installation démarre, sélectionnez les instances que vous souhaitez lier à 9.1.0.

Microsoft Machine Learning Server 9.2.1 et 9.3 n’ont pas ce problème.

### <a name="binding-or-unbinding-leaves-multiple-temporary-folders"></a>La liaison ou la séparation laisse plusieurs dossiers temporaires

La liaison et annulation de la liaison des opérations échouent parfois à nettoyer les dossiers temporaires.
Si vous trouvez des dossiers avec un nom tel que cela, vous pouvez le supprimer une fois l’installation terminée : R_SERVICES_<guid>

> [!NOTE]
> Veillez à attendre que l’installation est terminée. Il peut prendre beaucoup de temps à supprimer les bibliothèques R associé liés une version, puis ajoutez les nouvelles bibliothèques R. Lorsque l’opération est terminée, les dossiers temporaires sont supprimés.

## <a name="see-also"></a>Voir aussi

+ [Installer Machine Learning pour Windows Server (connectés à Internet)](https://docs.microsoft.com/machine-learning-server/install/machine-learning-server-windows-install)
+ [Installer Machine Learning pour Windows Server (hors connexion)](https://docs.microsoft.com/machine-learning-server/install/machine-learning-server-windows-offline)
+ [Problèmes connus dans Machine Learning Server](https://docs.microsoft.com/machine-learning-server/resources-known-issues)
+ [Annonces de fonctionnalités à partir de la version précédente de R Server](https://docs.microsoft.com/r-server/whats-new-in-r-server)
+ [Fonctionnalités déconseillées, supprimées ou modifiées](https://docs.microsoft.com/machine-learning-server/resources-deprecated-features)
