---
title: Considérations sur la sécurité pour l’apprentissage dans SQL Server | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: c8aa787088873adff5256208636063ba6f59b8b6
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="security-considerations-for-machine-learning-in-sql-server"></a>Considérations sur la sécurité pour l’apprentissage dans SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article répertorie les considérations de sécurité que l’administrateur ou un architecte doit-elle garder à l’esprit lors de l’utilisation des services de machine learning.

**S’applique à :** SQL Server 2016 R Services, SQL Server 2017 d’apprentissage automatique Services

## <a name="use-a-firewall-to-restrict-network-access"></a>Utilisez un pare-feu pour restreindre l’accès réseau

Dans une installation par défaut, une règle de pare-feu Windows est utilisée pour bloquer tous les accès réseau sortant des processus d’exécution externe. Les règles de pare-feu doivent être créées pour empêcher les processus du runtime externe de télécharger des packages ou à partir d’autres appels de réseau qui peuvent être potentiellement malveillant.

Si vous utilisez un autre programme de pare-feu, vous pouvez également créer des règles pour bloquer les connexions réseau sortantes pour runtimes externes, en définissant des règles pour les comptes d’utilisateur local ou pour le groupe représenté par le pool de comptes d’utilisateur.

Nous recommandons fortement d’activer le pare-feu Windows (ou un autre pare-feu de votre choix) pour empêcher tout accès réseau non restreint par les exécutions de R ou Python.

## <a name="authentication-methods-supported-for-remote-compute-contexts"></a>Méthodes d’authentification pris en charge pour les contextes de calcul à distance

[!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] prend en charge les connexions à la fois l’authentification intégrée Windows et SQL lors de la création des connexions entre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et un client de science des données distantes.

Par exemple, que vous développez une solution R sur votre ordinateur portable et que vous souhaitez effectuer des calculs sur l’ordinateur SQL Server. Vous devez créer une source de données SQL Server dans R, à l’aide de la **rx** fonctions et la définition d’une chaîne de connexion en fonction de vos informations d’identification Windows.

Lorsque vous modifiez le _contexte de calcul_ à partir de votre ordinateur portable à l’ordinateur SQL Server, tout le code R est exécuté sur l’ordinateur SQL Server, si votre compte Windows dispose des autorisations nécessaires. En outre, toutes les requêtes SQL exécutées en tant que partie du code R sont exécutées sous également vos informations d’identification.

L’utilisation de connexions SQL est également pris en charge dans ce scénario. Toutefois, cela requiert que l’instance SQL Server est configuré pour autoriser l’authentification en mode mixte.

### <a name="implied-authentication"></a>Authentification implicite

 En règle générale, le [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)] démarre l’exécution du script externe et exécute des scripts sous son propre compte. Toutefois, si le runtime externe effectue un appel ODBC, le [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)] emprunte l’identité de l’identification de l’utilisateur qui a envoyé la commande pour vous assurer que l’appel ODBC n’échoue pas. C’est ce que l’on appelle l’*authentification implicite*.
 
 > [!IMPORTANT]
 > Pour l’authentification implicite réussisse, le groupe d’utilisateurs Windows qui contient les comptes de travail (par défaut, **SQLRUserGroup**) doit avoir un compte dans la base de données master pour l’instance et ce compte doivent être autorisé à Connectez-vous à l’instance.
 > 
 > Le groupe **SQLRUserGroup** est également utilisé lors de l’exécution de scripts Python. 

En général, nous recommandons que vous déplacez des jeux de données volumineux dans SQL Server au préalable, au lieu de tentez de lire les données à l’aide de RODBC ou une autre bibliothèque. En outre, utilisez une requête SQL Server ou une vue en tant que votre source de données principale, pour de meilleures performances. 

Par exemple, vous pouvez obtenir vos données d’apprentissage (en général, les données plus volumineux) à partir de SQL Server et obtenir la liste des facteurs à partir d’une source externe. Vous pouvez définir un serveur lié pour obtenir des données à partir de la plupart des sources de données ODBC. Pour plus d’informations, consultez [créer un serveur lié](https://docs.microsoft.com/sql/relational-databases/linked-servers/create-linked-servers-sql-server-database-engine).

Pour réduire la dépendance sur les appels ODBC à des sources de données externes, vous pourrez également effectuer l’ingénierie de données comme un processus séparé et enregistrer les résultats dans une table ou utiliser T-SQL. Consultez le didacticiel pour un bon exemple d’ingénierie de données dans Visual Studio SQL. R : [créent des fonctionnalités de données à l’aide de T-SQL](../tutorials/sqldev-create-data-features-using-t-sql.md).

## <a name="no-support-for-encryption-at-rest"></a>Aucune prise en charge pour le chiffrement au repos

[Chiffrement transparent des données (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption) n’est pas prise en charge pour les données envoyées ou reçues à partir de l’exécution du script externe. La raison est que R (ou Python) s’exécute en dehors du processus SQL Server ; Par conséquent, les données utilisées par le runtime externe ne sont pas protégées par les fonctionnalités de chiffrement du moteur de base de données.  Ce comportement n’est pas différent de n’importe quel autre client en cours d’exécution sur l’ordinateur SQL Server qui lit les données à partir de la base de données et effectue une copie.

Par conséquent, le chiffrement transparent des données **n’est pas** appliquée à toutes les données que vous utilisez dans les scripts R ou Python, ou à toutes les données enregistrées sur disque, ou aucun résultat intermédiaire persistant. Toutefois, autres types de chiffrement, tel que le chiffrement BitLocker de Windows ou tiers chiffrement appliqué au niveau du fichier ou dossier, toujours s’appliquent.

Dans le cas de [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/overview-of-key-management-for-always-encrypted), runtimes externes n’ont pas accès aux clés de chiffrement ; par conséquent, les données ne peuvent pas être envoyées aux scripts.

## <a name="resources"></a>Ressources

Pour plus d’informations sur la gestion du service et comment configurer les comptes d’utilisateur utilisés pour exécuter des scripts de machine learning, consultez [configurer et gérer les Extensions d’Analytique avancée](../../advanced-analytics/r/configure-and-manage-advanced-analytics-extensions.md).

Pour obtenir une explication de l’architecture de sécurité générales, consultez :

+ [Vue d’ensemble de la sécurité pour R](security-overview-sql-server-r.md)
+ [Vue d’ensemble de la sécurité pour Python](../python/security-overview-sql-server-python-services.md)
