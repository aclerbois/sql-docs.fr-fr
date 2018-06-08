---
title: Installer des modèles d’apprentissage automatique dont l’apprentissage sur SQL Server | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/31/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: e3abc1b1581216bb0207fbba2d857993b947afae
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34707577"
---
# <a name="install-pre-trained-machine-learning-models-on-sql-server"></a>Installer dont l’apprentissage d’apprentissage des modèles sur SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article décrit comment ajouter dont l’apprentissage d’apprentissage des modèles pour la personnalisation de la sentiment analysis et l’image à une instance (de-de base de données) de SQL Server qui a déjà R Services ou SQL Server Machine Learning Services installé. 

Il existe des modèles dont l’apprentissage pour aider les clients qui doivent effectuer l’analyse des sentiments ou la personnalisation de l’image, mais n’ont pas les ressources pour obtenir des jeux de données volumineux ou de former un modèle complex. L’équipe Machine Learning Server créé et formé de ces modèles pour vous aider à démarrer sur le texte et image traiter efficacement. Pour plus d’informations, consultez la [ressources](#bkmk_resources) section de cet article.

Pour obtenir un exemple montrant comment utiliser les modèles dont l’apprentissage avec des données SQL Server, consultez le blog par l’équipe SQL Server Machine Learning : [analyse des sentiments avec Python dans Machine Learning Services SQL Server](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2017/11/01/sentiment-analysis-with-python-in-sql-server-machine-learning-services/)

## <a name="pre-trained-model-availability"></a>Disponibilité du modèle préentraîné

Dont l’apprentissage des modèles sont installés à l’aide du support d’installation de Microsoft Machine Learning Server (ou Microsoft R Server, si vous ajoutez des modèles à une installation de SQL Server 2016). Vous pouvez utiliser l’édition développeur libre pour installer les modèles. Il n’existe pas d’autres coûts associés à l’installation du modèle. 

Dont l’apprentissage des modèles fonctionnent avec les produits et les langues suivantes. Le programme d’installation détecte la langue intégration, les MicrosoftML ou microsoftml la bibliothèque et puis insère les modèles dont l’apprentissage dans la bibliothèque respectif. Lors de l’installation du modèle est terminée, les modèles d’accéder à via les fonctions de bibliothèque.

+ SQL Server 2016 R Services (de-de base de données) - R uniquement, avec la [MicrosoftML bibliothèque](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/microsoftml-package)
+ SQL Server 2016 R Server (autonome) - R uniquement, avec la [MicrosoftML bibliothèque](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/microsoftml-package)
+ SQL Server 2017 Machine Learning Services (de-de base de données) - R avec les [MicrosoftML bibliothèque](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/microsoftml-package), Python avec le [microsoftml bibliothèque](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/microsoftml-package)
+ 2017 Machine Learning serveur de SQL Server (autonome) - R avec les [MicrosoftML bibliothèque](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/microsoftml-package), Python avec le [microsoftml bibliothèque](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/microsoftml-package)

Le processus d’installation diffère légèrement selon votre version de SQL Server. Consultez les sections suivantes pour obtenir des instructions pour chaque version.

> [!NOTE]
> Il n’est pas possible de lire ou modifier les modèles dont l’apprentissage. Elles sont compressées à l’aide d’un format natif pour améliorer les performances.

## <a name="obtain-files-for-an-offline-installation"></a>Obtenir les fichiers pour une installation en mode hors connexion

Pour installer les modèles dont l’apprentissage sur un serveur qui n’a pas accès à internet, vous devez télécharger les programmes d’installation appropriées à l’avance et copiez le programme d’installation vers un dossier local sur le serveur. 

Consultez cette page pour les liens de téléchargement pour tous les programmes d’installation de R Server et serveur de Machine Learning : [installation hors connexion](https://docs.microsoft.com/machine-learning-server/install/machine-learning-server-windows-offline)

## <a name="install-pre-trained-models-on-sql-server-2016-r-services"></a>Installer les modèles dont l’apprentissage sur SQL Server 2016 R Services

Avec SQL Server 2016, vous pouvez installer et utiliser les modèles R préentraînés uniquement si vous mettez à niveau les composants, dans un processus appelé d’apprentissage automatique tout d’abord **liaison**. 

Pour ce faire, en cours d’exécution du programme d’installation Windows distinct pour Microsoft R Server ou Machine Learning Server sur un ordinateur ayant une installation de SQL Server 2016 R Services et en sélectionnant une instance ou des instances de **lier**. Un moyen d’instance associé à la stratégie de prise en charge de l’instance de liaison est modifiée pour autoriser les mises à jour plus fréquentes sur R. 

1. Lancer l’installation basée sur Windows distincte pour une [R Server](https://docs.microsoft.com/machine-learning-server/rebranding-microsoft-r-server) ou [Machine Learning Server](https://docs.microsoft.com/machine-learning-server/install/machine-learning-server-windows-install).

2. Sélectionnez l’instance à mettre à niveau, puis sélectionnez l’option pour obtenir les modèles dont l’apprentissage.

    Pour plus d’informations, consultez [mise à niveau des composants d’apprentissage machine utilisés par SQL Server](use-sqlbindr-exe-to-upgrade-an-instance-of-sql-server.md).

    Si vous avez déjà effectué la liaison à mettre à jour les composants R et que vous souhaitez simplement ajouter les modèles dont l’apprentissage, laissez toutes les sélections précédentes **étant**, puis sélectionnez l’option dont l’apprentissage des modèles. 
    
    Ne désélectionnez pas les options précédemment sélectionnées ; Si vous procédez ainsi, le programme d’installation supprime les composants.

3. Nous vous recommandons d’accepter les paramètres par défaut pour les emplacements du modèle.

4. Acceptez toutes les invites, y compris les contrats de licence.

Une fois que la liaison est terminée, la version de R et bibliothèques associés à l’instance sont remplacés avec les versions plus récentes fournies dans le serveur de R ou Machine Learning. 

Avec SQL Server 2016, vous devez effectuer quelques étapes supplémentaires pour inscrire les modèles avec la bibliothèque d’instance de SQL Server 2016. Répétez ces étapes pour chaque instance où vous avez installé les modèles dont l’apprentissage.

1. Ouvrez une invite de commandes Windows **en tant qu’administrateur**.

2. Accédez au dossier d’installation d’amorçage pour SQL Server, lequel contient également le programme d’installation de Microsoft R. Dans une instance par défaut de SQL Server 2016, le dossier serait :
    
    `C:\Program Files\Microsoft SQL Server\130\Setup Bootstrap\SQL2017\x64\`

3. Exécutez `RSetup.exe` et indiquer le composant à installer, la version et le dossier contenant les fichiers de source de modèle, à l’aide de cette syntaxe :

    `RSetup.exe /install /component MLM /version <version> /language 1033 /destdir "<SQL_DB_instance_folder>\R_SERVICES\library\MicrosoftML\mxLibs\x64"`. 

    Les valeurs suivantes sont prises en charge pour le paramètre de version :

    + Version release candidate 0 : **9.1.0.0**
    + Version release candidate 1 : **9.2.0.22**
    + RTM : **9.2.0.100**
    + Mise à jour cumulative 1 : **9.2.0.24**
    + Mise à jour cumulative 4 : **9.3.0**

    > [!NOTE]
    > Les versions de SQL Server correspondantes sont répertoriées pour vous donner une idée de l’heure à laquelle la mise à jour a été publiée. Si vous n’êtes pas sûr de la version installée avec SQL Server, ouvrez le dossier R_SERVICES pour l’instance et ouvrez RGui (`~\R_SERVICES\bin\x64`). L’écran de démarrage répertorie les versions de la version Microsoft R Open et le serveur de Machine Learning. 

    Par exemple, d’utiliser R dont l’apprentissage des modèles avec une instance par défaut de **R_SERVICES**, vous exécutez cette commande :

    `RSetup.exe /install /component MLM /version 9.2.0.24 /language 1033 /destdir "C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\library\MicrosoftML\mxLibs\x64"`

    Sur une instance nommée, la commande serait quelque chose comme ceci :

    `RSetup.exe /install /component MLM /version 9.2.0.24 /language 1033 /destdir "C:\Program Files\Microsoft SQL Server\MSSQL13.MyInstanceName\R_SERVICES\library\MicrosoftML\mxLibs\x64"`

4. Si l’installation est réussie, les modèles suivants doivent être ajoutés à votre `R_SERVICES` dossier :

    + AlexNet\_Updated.model
    + ImageNet1K\_mean.xml
    + pretrained.Model
    + ResNet\_101\_Updated.model
    + ResNet\_18\_Updated.model
    + ResNet\_50\_Updated.model

## <a name="install-pre-trained-models-on-sql-server-2017-machine-learning-services-in-database"></a>Installer les modèles dont l’apprentissage sur SQL Server 2017 Machine Learning Services (de-de base de données)

Si vous avez déjà installé SQL Server 2017, vous pouvez obtenir les modèles dont l’apprentissage de deux manières :

+ Installer uniquement les modèles dont l’apprentissage
+ Mettre à niveau les composants de Python et R en utilisant une liaison et installez les modèles dont l’apprentissage en même temps

### <a name="add-pre-trained-models-only"></a>Ajouter uniquement les modèles dont l’apprentissage

Pour ajouter les modèles dont l’apprentissage, vous pouvez exécuter RSetup.exe à partir de la ligne de commande.

Pour la version de R, des modèles, installez le composant MLM à R_SERVICES :

```
RSetup.exe /install /component MLM /version 9.2.0.24 /language 1033 /destdir "C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES"
```

Pour la version de Python, des modèles, installez le composant MLM à PYTHON_SERVICES :

```
RSetup.exe /install /component MLM /version 9.2.0.24 /language 1033 /destdir "C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES"
```

### <a name="bind-and-install-pre-trained-models"></a>Lier et installer dont l’apprentissage des modèles

Les instructions suivantes décrivent le processus de mise à niveau les composants d’apprentissage machine et l’obtention des modèles dont l’apprentissage en même temps.

1. Exécutez le programme d’installation pour l’apprentissage d’ordinateur serveur basé sur Windows. Vous pouvez télécharger le programme d’installation à partir des liens de cette page : [installer machine Learning Server pour Windows](https://docs.microsoft.com/machine-learning-server/install/machine-learning-server-windows-install).

2. Si vous installez les modèles vers un serveur qui n’a pas accès à internet, veillez à également télécharger le programme d’installation pour les modèles dont l’apprentissage à partir de cette page : [installation hors connexion pour le serveur de Machine Learning](https://docs.microsoft.com/machine-learning-server/install/machine-learning-server-windows-offline).

3. Exécutez le programme d’installation.

4. Pour mettre à niveau les composants de R ou Python en même temps, sélectionnez la langue (R, Python ou les deux) que vous souhaitez mettre à jour.

    Si vous avez précédemment installé le serveur Machine Learning et mis à jour des composants de R ou Python à l’aide de l’option de liaison, laissez toutes les sélections précédentes **étant**, puis sélectionnez les options dont l’apprentissage des modèles. Ne désélectionnez pas les options précédemment sélectionnées ; Si vous procédez ainsi, le programme d’installation supprime les composants.

5. Acceptez toutes les invites, y compris les contrats de licence.

Avec SQL Server 2017, aucune configuration supplémentaire n’est requise.

> [!NOTE]
> 
> Pour les modèles de Python, vous pouvez obtenir une erreur lorsque vous appelez le fichier de modèle à partir de code Python. Il s’agit d’une limitation de l’implémentation actuelle de Python, ce qui limite la longueur du chemin d’accès au fichier de modèle. Ce problème a été résolu et sera disponible dans une version de service à venir.

## <a name="install-pre-trained-models-with-sql-server-standalone-r-server"></a>Installer les modèles dont l’apprentissage avec serveur R autonome de SQL Server

Si vous avez installé une version de R Server (autonome) à l’aide du programme d’installation de SQL Server 2016, vous pouvez ajouter la possibilité d’utiliser les modèles dont l’apprentissage en mettant à niveau le serveur R à l’aide du programme d’installation Windows plus récente. 

Les modèles dont l’apprentissage introduite en tant qu’option avec [Microsoft R Server 9.1](https://blogs.technet.microsoft.com/dataplatforminsider/2017/04/19/introducing-microsoft-r-server-9-1-release/), mais les mises à niveau ont été ajoutés à chaque version. Nous recommandons d’obtenir la version la plus récente possible, mais les versions précédentes sont répertoriées ici [R Server libère](https://docs.microsoft.com/machine-learning-server/install/r-server-install)

Les instructions suivantes décrivent comment mettre à niveau les composants R et en même temps ajouter les modèles dont l’apprentissage.

1. Lancer l’installation basée sur Windows distincte pour une [R Server](https://docs.microsoft.com/machine-learning-server/rebranding-microsoft-r-server) ou [Machine Learning Server](https://docs.microsoft.com/machine-learning-server/install/machine-learning-server-windows-install).

2. Sélectionnez les langues que vous souhaitez mettre à jour, puis sélectionnez le **Pre-trained modèles** option.

    > [!TIP]
    > Si vous avez précédemment exécuté le programme d’installation pour mettre à jour de R Server (autonome) et que vous souhaitez simplement ajouter les modèles dont l’apprentissage, laissez toutes les sélections précédentes **étant**, puis sélectionnez simplement la version antérieure de **-apprentissage des modèles** option . **Ne le faites pas** désélectionner toutes les options précédemment sélectionnées ; si vous procédez ainsi, le programme d’installation supprime les composants.

    Nous vous recommandons d’accepter les paramètres par défaut pour les emplacements du modèle.

3. Cliquez sur **Continuer**. 

4. Acceptez toutes les invites, y compris les contrats de licence.

Une fois l’installation terminée, vous devez effectuer quelques étapes supplémentaires pour inscrire les modèles dont l’apprentissage.

1. Ouvrez une invite de commandes Windows **en tant qu’administrateur**.

2. Accédez au dossier d’installation d’amorçage pour R Server (autonome), qui contient également le programme d’installation de Microsoft R. 

3. Exécutez `RSetup.exe` et indiquer le composant à installer, la version et le dossier contenant les fichiers de source de modèle, à l’aide de cette syntaxe :

    `RSetup.exe /install /component MLM /version <version> /language 1033 /destdir "~\R_SERVER\library\MicrosoftML\mxLibs\x64"`

    Les valeurs suivantes sont prises en charge pour le paramètre de version :

    + Version release candidate 0 : **9.1.0.0**
    + Version release candidate 1 : **9.2.0.22**
    + RTM : **9.2.0.100**
    + Mise à jour cumulative 1 : **9.2.0.24**
    + Mise à jour cumulative 4 : **9.3.0**

    Par exemple, pour activer l’utilisation de la version la plus récente des modèles dont l’apprentissage de R, dans une installation par défaut de R Server (autonome), vous exécuteriez cette instruction :

    `RSetup.exe /install /component MLM /version 9.3.0 /language 1033 /destdir "C:\Program Files\Microsoft SQL Server\130\R_SERVER\library\MicrosoftML\mxLibs\"`

## <a name="install-pre-trained-models-with-sql-server-2017-standalone-server"></a>Installer des modèles dont l’apprentissage avec un serveur autonome SQL Server 2017

Si vous avez installé le serveur d’apprentissage Machine à l’aide du programme d’installation de SQL Server 2017, vous ajoutez les modèles dont l’apprentissage en exécutant le programme d’installation basé sur Windows. Vous pouvez sélectionner l’option pour mettre à niveau les composants de R ou Python et en même temps ajouter les modèles dont l’apprentissage. 

Aucune configuration supplémentaire n’est requise après que l’installation est terminée.

## <a name="bkmk_resources"></a> Recherche et ressources

Actuellement, les modèles qui sont disponibles sont des modèles de réseau neuronaux en profondeur (profonds DNN) pour la classification du sentiment analysis et image. Tous les modèles dont l’apprentissage a été formés à l’aide de Microsoft [boîte à outils de calcul réseau](https://cntk.ai/Features/Index.html), ou **CNTK**.

La configuration de chaque réseau était basée sur les implémentations de référence suivantes :

+ ResNet-18
+ ResNet à 50.
+ ResNet-101
+ AlexNet

Pour plus d’informations sur les algorithmes utilisés dans ces modèles d’apprentissage approfondie et la façon dont elles sont implémentées et formé à l’aide de CNTK, consultez les articles suivants :

+ [Algorithme d’aux chercheurs Microsoft définit ImageNet défi jalon](https://www.microsoft.com/research/blog/microsoft-researchers-algorithm-sets-imagenet-challenge-milestone/)

+ [Microsoft le Kit de ressources de calcul réseau offre la plus efficace deep distribuée apprentissage des performances de calcul](https://www.microsoft.com/research/blog/microsoft-computational-network-toolkit-offers-most-efficient-distributed-deep-learning-computational-performance/)

## <a name="how-to-use-pre-trained-models-for-text-analysis"></a>Comment utiliser des modèles dont l’apprentissage pour l’analyse de texte

Consultez l’exemple suivant pour une démonstration de l’utilisation du modèle de la personnalisation de texte dont l’apprentissage de classification de texte :

[Analyse des sentiments à l’aide du Générateur de fonctionnalités de texte](https://github.com/Microsoft/microsoft-r/tree/master/microsoft-ml/Samples/101/BinaryClassification/SimpleSentimentAnalysis)

## <a name="how-to-use-pre-trained-models-for-image-detection"></a>Comment utiliser des modèles dont l’apprentissage de la détection d’images

Le modèle dont l’apprentissage pour les images prend en charge la personnalisation des images que vous fournissez. Pour utiliser le modèle, vous appelez le **featurizeImage** transformer. L’image est chargée, redimensionnée et les fonctionnalités par le modèle formé. La sortie du Générateur de fonctionnalités profonds DNN est ensuite utilisée pour former un modèle linéaire pour la classification d’image.

Pour utiliser ce modèle, toutes les images doivent être redimensionnés pour satisfaire les exigences du modèle formé. Par exemple, si vous utilisez un modèle AlexNet, l’image doit être redimensionnée à 227 x 227 px.

Pour plus d’informations, consultez [dont l’apprentissage des modèles pour la détection de sentiment analysis et image](https://docs.microsoft.com/machine-learning-server/install/microsoftml-install-pretrained-models)
