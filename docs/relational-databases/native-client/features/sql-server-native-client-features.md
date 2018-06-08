---
title: Fonctionnalités SQL Server Native Client | Documents Microsoft
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client|features
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- MDAC [SQL Server]
- SQL Server Native Client ODBC driver, about SQL Server Native Client ODBC driver
- SQLNCLI, about SQL Server Native Client
- data access [SQL Server Native Client], features
ms.assetid: 7bb32865-5afb-41ab-98b4-3fa545ee8953
caps.latest.revision: 59
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: aa19218cfcb20bf4b696edfcdbffd4925d46aff8
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34708397"
---
# <a name="sql-server-native-client-features"></a>Fonctionnalités de SQL Server Native Client
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

  En plus de l'exposition des fonctionnalités Windows (anciennement Microsoft) Data Access Components (WDAC), [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client implémente plusieurs autres fonctionnalités pour tirer parti des fonctionnalités [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
## <a name="in-this-section"></a>Dans cette section  
 [Changement de comportement du pilote ODBC lors de la gestion des conversions de caractères](../../../relational-databases/native-client/features/odbc-driver-behavior-change-when-handling-character-conversions.md)  
 Décrit un changement de comportement à compter de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 2012 Native Client.  
  
 [Utilisation de la mise en miroir de bases de données](../../../relational-databases/native-client/features/using-database-mirroring.md)  
 Explique comment [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client prend en charge l’utilisation de bases de données miroir, qui est la capacité à conserver une copie, ou miroir, d’un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] base de données sur un serveur de secours.  
  
 [Exécution d’opérations asynchrones](../../../relational-databases/native-client/features/performing-asynchronous-operations.md)  
 Explique comment [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client prend en charge les opérations asynchrones, à savoir la capacité d'un retour immédiat sans blocage sur le thread appelant.  
  
 [Utilisation de MARS &#40;Multiple Active Result Sets&#41;](../../../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md)  
 Explique comment [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client prend en charge MARS (Multiple Active Result Set). MARS permet d'exécuter et de recevoir plusieurs jeux de résultats à l'aide d'une seule connexion de base de données.  
  
 [Utilisation de types de données XML](../../../relational-databases/native-client/features/using-xml-data-types.md)  
 Explique comment [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client prend en charge le type de données XML, qui peut être utilisé comme type de colonne, type de variable, type de paramètre ou type de retour de fonction.  
  
 [Utilisation de types définis par l’utilisateur](../../../relational-databases/native-client/features/using-user-defined-types.md)  
 Explique comment [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client prend en charge User-Defined Types (UDT), qui étend le système de type SQL en vous permettant de stocker des objets et des structures de données personnalisées dans un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] base de données.  
  
 [Utilisation de types de valeur élevée](../../../relational-databases/native-client/features/using-large-value-types.md)  
 Explique comment [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client prend en charge les types de données de valeur élevée, à savoir les types de données LOB.  
  
 [Changement des mots de passe par programmation](../../../relational-databases/native-client/features/changing-passwords-programmatically.md)  
 Explique comment [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client prend en charge la gestion des mots de passe périmés afin que les mots de passe puissent désormais être modifiés sur le client sans intervention de l'administrateur.  
  
 [Utilisation du niveau d’isolement de capture instantanée](../../../relational-databases/native-client/features/working-with-snapshot-isolation.md)  
 Explique comment [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client prend en charge l'amélioration apportée au contrôle de version de ligne qui optimise les performances de la base de données en évitant les scénarios de blocage du lecteur/enregistreur.  
  
 [Utilisation de notifications de requêtes](../../../relational-databases/native-client/features/working-with-query-notifications.md)  
 Explique comment [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client prend en charge la notification des consommateurs en cas de modification de l'ensemble de lignes.  
  
 [Exécution d'opérations de copie en bloc](../../../relational-databases/native-client/features/performing-bulk-copy-operations.md)  
 Explique comment [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client prend en charge les opérations de copie en bloc qui autorisent le transfert de grandes quantités de données dans ou hors d’un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] table ou vue.  
  
 [Utilisation du chiffrement sans validation](../../../relational-databases/native-client/features/using-encryption-without-validation.md)  
 Explique comment utiliser [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client pour chiffrer les données envoyées au serveur sans validation du certificat.  
  
 [Paramètres table &#40;SQL Server Native Client&#41;](../../../relational-databases/native-client/features/table-valued-parameters-sql-server-native-client.md)  
 Explique comment [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client prend en charge les paramètres table.  
  
 [Types CLR volumineux définis par l’utilisateur](../../../relational-databases/native-client/features/large-clr-user-defined-types.md)  
 Explique la prise en charge des types UDT volumineux du CLR.  
  
 [Prise en charge de FILESTREAM](../../../relational-databases/native-client/features/filestream-support.md)  
 Traite [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client prend en charge la fonctionnalité FILESTREAM améliorée.  
  
 [Nom de Principal du service &#40;SPN&#41; prise en charge dans les connexions clientes](../../../relational-databases/native-client/features/service-principal-name-spn-support-in-client-connections.md)  
 Explique comment la prise en charge des noms de principaux du service a été étendue pour permettre l'authentification mutuelle à travers l'ensemble des protocoles.  
  
 [Prise en charge des colonnes éparses dans SQL Server Native Client](../../../relational-databases/native-client/features/sparse-columns-support-in-sql-server-native-client.md)  
 Décrit la prise en charge des colonnes éparses par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client.  
  
 [Améliorations des types de données date et heure](../../../relational-databases/native-client/features/date-and-time-improvements.md)  
 Décrit la prise en charge par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client des nouveaux types de données date et time.  
  
 [Détection des métadonnées](../../../relational-databases/native-client/features/metadata-discovery.md)  
 Décrit les améliorations apportées à la découverte des métadonnées dans [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)].  
  
 [Prise en charge de UTF-16 dans SQL Server Native Client 11.0](../../../relational-databases/native-client/features/utf-16-support-in-sql-server-native-client-11-0.md)  
 Décrit un changement de comportement introduit dans [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]. Si vous fournissez une mémoire tampon de longueur fixe lors de la liaison d’un paramètre de résultat ou de la sortie de colonne et si les **wchar** caractères écrits dans la mémoire tampon avant le caractère de fin est un point de code de substitut étendu d’une paire de substitution et si la prochaine **wchar** caractère est un point de code de substitution faible [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client n’ajoutera pas le point de code de substitut pour la mémoire tampon.  
  
 [Prise en charge des fonctionnalités de récupération d'urgence, haute disponibilité par SQL Server Native Client](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)  
 Explique comment votre application peut être configurée pour tirer parti de la récupération d’urgence haute disponibilité, des fonctionnalités ajoutées dans [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)].  
  
 [Accès aux informations de diagnostic dans le journal des événements étendus](../../../relational-databases/native-client/features/accessing-diagnostic-information-in-the-extended-events-log.md)  
 Présente les améliorations apportées à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client et au suivi de données qui vous donne accès aux informations de diagnostic dans la mémoire tampon en anneau et le journal XEvents.  
  
 [Prise en charge de SQL Server Native Client pour la base de données locale](../../../relational-databases/native-client/features/sql-server-native-client-support-for-localdb.md)  
 Décrit la prise en charge de la fonctionnalité LocalDB par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client.  
  
## <a name="see-also"></a>Voir aussi  
 [Programmation de SQL Server Native Client](../../../relational-databases/native-client/sql-server-native-client-programming.md)   
 [Rubriques de procédures ODBC](../../../relational-databases/native-client-odbc-how-to/odbc-how-to-topics.md)   
 [Rubriques de procédures OLE DB](../../../relational-databases/native-client-ole-db-how-to/ole-db-how-to-topics.md)   
 [Installation de SQL Server Native Client](../../../relational-databases/native-client/applications/installing-sql-server-native-client.md)  
  
  
