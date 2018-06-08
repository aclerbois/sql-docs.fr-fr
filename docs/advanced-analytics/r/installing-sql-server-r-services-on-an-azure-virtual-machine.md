---
title: Installer les fonctionnalités de formation l’ordinateur SQL Server sur une machine virtuelle Azure | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: a0780d4f974761af563ff2ed6e20e444b2d85ef9
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34563677"
---
# <a name="install-sql-server-machine-learning-features-on-an-azure-virtual-machine"></a>Installer les fonctionnalités sur une machine virtuelle Azure d’apprentissage automatique SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]
 
Nous vous recommandons d’utiliser le [machine virtuelle de science des données](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/provision-vm), mais si vous souhaitez un ordinateur virtuel avec SQL Server 2017 Machine Learning Services ou SQL Server 2016 R Services, cet article vous guide à travers les étapes.

## <a name="create-a-virtual-machine-on-azure"></a>Créer un ordinateur virtuel sur Azure

1. Dans le portail Azure, dans la liste de gauche, cliquez sur **virtuels** puis cliquez sur **ajouter**.
2. Recherche de SQL Server 2017 Enterprise Edition ou SQL Server 2016 Enterprise Edition.
3. Définissez le nom du serveur et les autorisations de compte, puis sélectionnez un plan tarifaire.
4. Dans **paramètres SQL Server** (étape 4 de l’Assistant Installation de machine virtuelle), recherchez **Machine Learning Services (Analytique avancée)** (ou **R Services** pour SQL Server 2016) et cliquez sur  **Activer**.
5. Après avoir passé en revue le récapitulatif présenté pour la validation, cliquez sur **OK**.
6. Une fois que la machine virtuelle est prête, connectez-vous à cette machine, puis ouvrez SQL Server Management Studio, qui est préinstallé. Machine learning n’est prêt à s’exécuter.
7. Pour le vérifier, ouvrez une nouvelle fenêtre de requête et exécutez une instruction simple, comme celle ci-dessous, qui utilise R pour générer une séquence de nombres de 1 à 10.

    ```
    EXEC sp_execute_external_script
    @language = N'R'
    , @script = N' OutputDataSet <- as.data.frame(seq(1, 10, ));'
    , @input_data_1 = N'   ;'
    WITH RESULT SETS (([Sequence] int NOT NULL));
    ```

6. Si vous envisagez de vous connecter à l’instance à partir d’un client de science des données à distance, effectuez [des étapes supplémentaires](#additional-steps) si nécessaire.

### <a name="disable-machine-learning-features-on-a-sql-server-vm"></a>Désactiver les fonctionnalités d’apprentissage machine sur une machine virtuelle SQL Server

Vous pouvez également activer ou désactiver la fonctionnalité sur un ordinateur virtuel de SQL Server existant à tout moment.

1. Ouvrez le panneau de l’ordinateur virtuel.
2. Cliquez sur **Paramètres**, puis sélectionnez **Configuration de SQL Server**.
3. Apporter des modifications aux fonctionnalités et s’appliquent.

### <a name="existing"></a>Ajouter R Services à un SQL Server 2016 Enterprise machine virtuelle existante

Si vous avez créé une machine virtuelle Azure incluant SQL Server sans l’apprentissage automatique, vous pouvez ajouter la fonctionnalité en suivant ces étapes :

1. Ré-exécutez le programme d’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et ajoutez la fonctionnalité à partir de la page **Configuration du serveur** de l’Assistant.
2. Activez l’exécution des scripts externes, puis redémarrez l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [installer SQL Server 2016 R Services](../install/sql-r-services-windows-install.md).
3. (Facultatif) Configurez l’accès aux bases de données pour les comptes de travail R, si cela est nécessaire pour l’exécution des scripts à distance.
4. (Facultatif) Modifiez une règle de pare-feu sur la machine virtuelle Azure, si vous souhaitez autoriser l’exécution de script R à partir des clients de science des données distants. Pour plus d’informations, consultez [Désactiver une règle du pare-feu](#firewall).
5. Installez ou activez les bibliothèques réseau nécessaires. Pour plus d’informations, consultez [Ajouter des protocoles réseau](#network).

## <a name="additional-steps"></a>Étapes supplémentaires

Quelques étapes supplémentaires sont nécessaires si vous pensez que les clients distants d’accéder au serveur en tant qu’un contexte de calcul de SQL Server à distance.

### <a name="firewall"></a>Désactiver une règle du pare-feu

Par défaut, le pare-feu sur l’ordinateur virtuel Azure comprend une règle qui bloque accès pour les comptes d’utilisateurs locaux l'réseau.

Vous devez désactiver cette règle pour vous assurer que vous pouvez accéder à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance à partir d’un client de science des données distantes.  Sinon, votre code d’apprentissage ne peut pas s’exécute dans des contextes de calcul qui utilisent l’espace de travail de l’ordinateur virtuel.

Pour activer l’accès à partir de clients de science des données à distance :

1. Sur la machine virtuelle, ouvrez le Pare-feu Windows avec fonctions avancées de sécurité.
2. Sélectionnez **Règles sortantes**.
3. Désactivez la règle suivante :
  
     `Block network access for R local user accounts in SQL Server instance MSSQLSERVER`
  
### <a name="enable-odbc-callbacks-for-remote-clients"></a>Autoriser les rappels ODBC pour les clients distants

Si vous prévoyez que les clients de l’appel du serveur doivent émettre des requêtes ODBC dans le cadre de leur solutions d’apprentissage, vous devez vous assurer que le Launchpad peut effectuer des appels ODBC part du client distant. Pour cela, vous devez autoriser les comptes de travail SQL utilisés par Launchpad à se connecter à l’instance.
Pour plus d’informations, consultez [installer SQL Server 2016 R Services](../install/sql-r-services-windows-install.md).

### <a name="network"></a>Ajouter des protocoles réseau

+ Activer les canaux nommés
  
  [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] utilise le protocole des canaux nommés pour les connexions entre les ordinateurs client et serveur, ainsi que pour certaines connexions internes. Si ce protocole n’est pas activé, vous devez l’installer et l’activer sur la machine virtuelle Azure et sur tous les clients de science des données susceptibles de se connecter au serveur.
  
+ Activer TCP/IP

  TCP/IP est requis pour les connexions de bouclage. Si vous obtenez l’erreur suivante, activez TCP/IP sur l’ordinateur virtuel qui prend en charge de l’instance :

  « DBNETLIB ; SQL Server n’existe pas ou l’accès refusé »
