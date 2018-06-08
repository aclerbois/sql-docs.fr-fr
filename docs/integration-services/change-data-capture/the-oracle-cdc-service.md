---
title: Oracle CDC Service| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: change-data-capture
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 47759ddc-358d-405b-acb9-189ada76ea6d
caps.latest.revision: 8
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 8df4a1f29d0b5d0f864362cf0658f23eeb26b0e8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="the-oracle-cdc-service"></a>Service de capture de données modifiées Oracle
  Le service de capture de données modifiées Oracle est un service Windows qui exécute le programme xdbcdcsvc.exe. Ce service peut être configuré pour exécuter plusieurs services Windows sur le même ordinateur, chacun avec un nom différent de service Windows. La création de plusieurs services Windows de capture de données modifiées Oracle sur un seul ordinateur est souvent réalisée pour obtenir une meilleure séparation entre eux, ou lorsque chacun d'eux doit fonctionner avec une autre instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Un service de capture de données modifiées Oracle est créé à l'aide de la console de configuration du service de capture de données modifiées Oracle ou est défini par l'interface de ligne de commande intégrée au programme xdbcdcsvc.exe. Dans les deux cas, chaque service de capture de données modifiées Oracle créé est associé à une seule instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (qui peut être regroupée ou mise en miroir avec l’installation **AlwaysOn** ) et les informations de connexion (chaîne de connexion et informations d’identification d’accès) font partie de la configuration du service.  
  
 Lorsqu'un service de capture de données modifiées Oracle est démarré, il tente de se connecter à l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à laquelle il est associé, d'obtenir la liste des instances Oracle CDC à gérer et effectue une première validation de l'environnement. Les erreurs qui se produisent lors du démarrage du service et toutes les informations de démarrage et d'arrêt sont toujours écrites dans le journal des événements des applications Windows. Quand une connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est établie, les erreurs et messages d’information sont écrits dans la table **dbo.xdbcdc_trace** de la base de données MSXDBCDC de l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Un des contrôles effectués au démarrage est la vérification qu'aucun autre service de capture de données modifiées Oracle portant le même nom ne fonctionne actuellement. Si un service du même nom est actuellement connecté à partir d'un autre ordinateur, le service de capture de données modifiées Oracle entre dans une boucle d'attente et attend que l'autre service se déconnecte avant de continuer à gérer la capture de données modifiées Oracle.  
  
 Quand le service de capture de données modifiées Oracle passe toutes les vérifications de démarrage, il vérifie la table **dbo.xdbcdc_databases** dans la base de données MSXDBCDC pour toutes les instances Oracle CDC actives. Pour chaque instance Oracle CDC active, le service démarre un sous-processus afin de gérer cette instance Oracle CDC.  
  
 Au démarrage d'une instance Oracle CDC, il accède à la base de données SQL Server CDC du même nom que l'instance de capture de données modifiées et extrait son état à partir de l'exécution précédente. Il vérifie également que tout s'exécute correctement. Il reprend ensuite le traitement des modifications ; lit les journaux des transactions Oracle et écrit les modifications apportées à la base de données CDC.  
  
 Le service de capture de données modifiées Oracle analyse périodiquement la table **dbo.xdbcdc_tables** dans la base de données MSXDBCDC pour déterminer s’il y a des modifications de configuration apportées à la configuration de l’une des instances Oracle CDC. Si une modification est trouvée, le service de capture de données modifiées Oracle avertit l'instance Oracle CDC qu'elle doit vérifier sa configuration. La plupart des modifications de configuration, telles que l'ajout et la suppression d'instances de capture, peuvent être appliquées lorsque l'instance Oracle CDC est active, d'autres nécessitent un redémarrage de cette instance.  
  
 Lorsque vous utilisez la console du concepteur de capture de données modifiées Oracle, les modifications sont automatiquement détectées. Lors de la mise à jour de la configuration de capture de données modifiées Oracle directement à l'aide de SQL, la procédure suivante doit être appelée pour que le service de capture de données modifiées Oracle remarque la modification de configuration :  
  
```sql
DECLARE @dbname nvarchar(128) = 'HRcdc'  
EXECUTE [MSXDBCDC].[dbo].[xdbcdc_update_config_version] @dbname  
GO  
  
```  
  
 Le processus d’instance Oracle CDC met à jour son état dans la table système **cdc.xdbcdc_state** et écrit les informations d’erreur dans la table **cdc.xdbcdc_trace** . La table **xdbcdc_state** est utile pour surveiller l’état de l’instance Oracle CDC. Elle fournit l’état à jour, différents compteurs (tels que le nombre de modifications lues à partir d’Oracle, le nombre de modifications écrites dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le numéro de la transaction validée écrite et le nombre actuel de transactions en cours) et l’indication de latence.  
  
 La configuration de l’instance Oracle CDC est enregistrée dans la table **cdc.xdbcdc_config** , qui est la table avec laquelle la console du concepteur de capture de données modifiées Oracle s’exécute. Étant donné que la configuration entière d'une instance Oracle CDC se trouve dans l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cible et les bases de données CDC, il est possible de créer des scripts de déploiement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour une instance Oracle CDC. Cette opération s'effectue à l'aide des consoles de configuration du service de capture de données modifiées Oracle et du concepteur de capture de données modifiées Oracle.  
  
## <a name="security-considerations"></a>Considérations relatives à la sécurité  
 Le section suivante décrit les exigences de sécurité nécessaires pour utiliser le service de capture de données modifiées pour Oracle.  
  
### <a name="protection-of-source-oracle-data"></a>Protection des données Oracle sources  
 Le service de capture de données modifiées Oracle n'a pas besoin d'accéder aux données sources Oracle et est protégé en veillant à ce que les informations d'identification pour l'exploration des journaux n'accordent pas l'autorisation SELECT sur les tables Oracle clientes.  
  
### <a name="protection-of-source-oracle-change-data"></a>Protection des données modifiées Oracle sources  
 Le service de capture de données modifiées Oracle est fourni avec des informations d'identification pour l'exploration des données de journaux qui permettent au service de capturer les modifications apportées à une table de la base de données Oracle. Les données modifiées ne disposent pas des autorisations d'accès granulaires dont disposent les tables régulières ; par conséquent, l'accès aux données modifiées passe outre les contrôles d'accès aux données Oracle intégrés.  
  
 Les tables Oracle sources capturées peuvent contenir des tables miroir vides avec les mêmes schéma et nom de table dans la base de données CDC. Les données capturées sont stockées dans les instances de capture [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et offrent la même protection que celle est fournie pour les modifications capturées dans la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour accéder aux données modifiées associées à une instance de capture, l'utilisateur doit pouvoir accéder à toutes les colonnes capturées de la table miroir associée. De plus, si un rôle de régulation est spécifié lors de la création de l'instance de capture, l'appelant doit également être membre du rôle de régulation spécifié. Les autres fonctions de capture de données modifiées générales pour accéder aux métadonnées sont accessibles à tous les utilisateurs de base de données par le biais du rôle public, bien que l'accès aux métadonnées retournées soit en général également régulé par le biais de l'accès choisi aux tables sources sous-jacentes et par l'appartenance aux rôles de régulation définis.  
  
 Cela signifie que les utilisateurs avec le rôle serveur fixe **sysadmin** ou le rôle de base de données fixe **db_owner** ont (par défaut) l’accès complet aux données capturées, et plus d’accès peut être accordé par le biais des rôles de régulation ou en accordant l’accès choisi aux colonnes capturées.  
  
### <a name="protection-of-source-oracle-log-mining-credentials"></a>Protection des informations d'identification pour l'exploration des données de journaux Oracle sources  
 La configuration du service de capture de données modifiées Oracle, stockée dans la base de données CDC (dans la table cdc.xdbcdc_config) inclut le nom d'utilisateur d'exploration de données de journaux et son mot de passe associé.  
  
 Le mot de passe d'exploration de données de journaux est stocké chiffré à l'aide d'une clé asymétrique avec le nom fixe `xdbcdc_asym_key` qui est automatiquement créé avec la commande suivante :  
  
```sql
USE [<cdc-database-name>]  
CREATE ASYMMETRIC KEY xdbcdc_asym_key  
    WITH ALGORITHM = RSA_1024  
    ENCRYPTION BY PASSWORD = '<cdc-database-name><asym-key-password>'  
  
```  
  
 Si un algorithme différent est utilisé, cette clé peut être supprimée et une nouvelle portant le même nom et chiffrée par le même mot de passe peut être créée.  
  
 Le mot de passe de la clé asymétrique est le principal mot de passe qui est enregistré dans le Registre sous le chemin **HKLM\Software\Microsoft\XDBCDCSVC\\<nom_service>**. Cette clé est accessible uniquement aux administrateurs locaux et au compte de service Windows de capture de données modifiées Oracle. La clé contient une valeur binaire chiffrée **AsymmetricKeyPassword** qui a stocké le mot de passe de la clé asymétrique. L'accès à cette clé de Registre est requis pour pouvoir accéder aux informations d'identification d'exploration de données de journaux Oracle.  
  
 Pour utiliser la clause ENCRYPTION BY PASSWORD, le mot de passe doit satisfaire aux critères de la stratégie de mot de passe Windows de l'ordinateur qui exécute l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cette opération s'effectue en sélectionnant le mot de passe de la clé asymétrique d'après cette stratégie.  
  
 Si le mot de passe de la clé asymétrique est perdu, les informations d'identification d'exploration de données de journaux pour chaque instance Oracle CDC doivent être spécifiées de nouveau dans le concepteur de service de capture de données modifiées Oracle.  
  
 La clé asymétrique est automatiquement créée dans la base de données CDC lorsque le service de capture de données modifiées détecte une base de données CDC d'instance Oracle qui n'a pas cette clé asymétrique ou lorsque la clé existe mais que le mot de passe ne correspond pas.  
  
### <a name="oracle-cdc-service-windows-service-account"></a>Compte de service Windows pour le service de capture de données modifiées Oracle  
 Le compte de service utilisé avec le service Windows de capture de données modifiées Oracle ne requiert aucune autorisation supplémentaire. Ce compte doit pouvoir utiliser l'API Oracle Native Client et l'API ODBC SQL Server Native Client. Il doit également avoir accès à la clé de configuration du service dans le Registre (cette console de configuration du service de capture de données modifiées configure une liste de contrôle d'accès à cet effet).  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Prise en charge de la haute disponibilité](../../integration-services/change-data-capture/high-availability-support.md)  
  
-   [Autorisations de connexion SQL Server nécessaires pour le service CDC](../../integration-services/change-data-capture/sql-server-connection-required-permissions-for-the-cdc-service.md)  
  
-   [Rôles d’utilisateur](../../integration-services/change-data-capture/user-roles.md)  
  
-   [Utilisation du service de capture de données modifiées Oracle](../../integration-services/change-data-capture/working-with-the-oracle-cdc-service.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Procédure : gérer un service de capture de données modifiées local](../../integration-services/change-data-capture/how-to-manage-a-local-cdc-service.md)   
 [Gérer un service CDC Oracle](../../integration-services/change-data-capture/manage-an-oracle-cdc-service.md)  
  
  
