---
title: Fonctionnalités de SQL Server OLE DB pilote | Documents Microsoft
description: Pilote OLE DB pour les fonctionnalités de SQL Server
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: oledb|features
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- MDAC [SQL Server]
- MSOLEDBSQL, about OLE DB Driver for SQL Server
- data access [OLE DB Driver for SQL Server], features
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: 3f1cc26981dae02bd76133c204c5eff142db76c5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ole-db-driver-for-sql-server-features"></a>Pilote de base de données OLE pour les fonctionnalités SQL Server
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Outre l’exposition des fonctionnalités des composants Windows (anciennement Microsoft) données Access (WDAC), pilote OLE DB pour SQL Server implémente également plusieurs autres fonctionnalités pour exposer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] fonctionnalité.  
  
## <a name="in-this-section"></a>Dans cette section    
 [Utilisation de la mise en miroir de bases de données](../../oledb/features/using-database-mirroring.md)  
 Explique comment pilote OLE DB pour SQL Server prend en charge l’utilisation de bases de données miroir, qui est la capacité à conserver une copie, ou miroir, d’un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] base de données sur un serveur de secours.  
  
 [Exécution d’opérations asynchrones](../../oledb/features/performing-asynchronous-operations.md)  
 Explique comment pilote OLE DB pour SQL Server prend en charge les opérations asynchrones, qui est la possibilité de retourner immédiatement sans blocage sur le thread appelant.  
  
 [Utilisation de MARS &#40;Multiple Active Result Sets&#41;](../../oledb/features/using-multiple-active-result-sets-mars.md)  
 Explique comment pilote OLE DB pour SQL Server prend en charge les jeux de résultats actifs multiples (MARS). MARS permet d'exécuter et de recevoir plusieurs jeux de résultats à l'aide d'une seule connexion de base de données.  
  
 [Utilisation de types de données XML](../../oledb/features/using-xml-data-types.md)  
 Explique comment pilote OLE DB pour SQL Server prend en charge le type de données XML, qui est un type de données XML qui peut être utilisé comme un type de colonne, le type de variable, le type de paramètre ou le type de retour de fonction.  
  
 [À l’aide des Types définis par l’utilisateur](../../oledb/features/using-user-defined-types.md)  
 Explique comment pilote OLE DB pour SQL Server prend en charge User-Defined Types (UDT), qui étend le système de type SQL en vous permettant de stocker des objets et des structures de données personnalisées dans un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] base de données.  
  
 [À l’aide des Types de valeur élevée](../../oledb/features/using-large-value-types.md)  
 Explique comment pilote OLE DB pour SQL Server prend en charge les types de données de valeur élevée, qui sont des types de données objet volumineux (LOB).  
  
 [Changement des mots de passe par programmation](../../oledb/features/changing-passwords-programmatically.md)  
 Explique comment pilote OLE DB pour SQL Server prend en charge la gestion des mots de passe périmés afin que les mots de passe peuvent désormais être modifiés sur le client sans intervention de l’administrateur.  
  
 [Utilisation de l’isolement d’instantané](../../oledb/features/working-with-snapshot-isolation.md)  
 Explique comment pilote OLE DB pour SQL Server prend en charge l’extension pour le contrôle de version de ligne qui améliore les performances de la base de données en évitant les scénarios de blocage de lecteur-writer.  
  
 [Utilisation de notifications de requêtes](../../oledb/features/working-with-query-notifications.md)  
 Explique comment pilote OLE DB pour SQL Server prend en charge la notification des consommateurs sur la modification de l’ensemble de lignes.  
  
 [Exécution d'opérations de copie en bloc](../../oledb/features/performing-bulk-copy-operations.md)  
 Explique comment pilote OLE DB pour SQL Server prend en charge les opérations de copie en bloc qui autorisent le transfert de grandes quantités de données dans ou hors d’un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] table ou vue.  
  
 [Utilisation du chiffrement sans validation](../../oledb/features/using-encryption-without-validation.md)  
 Explique comment utiliser le pilote OLE DB pour SQL Server pour chiffrer les données envoyées au serveur sans validation du certificat.  
  
 [Paramètres table &#40;pilote OLE DB SQL Server&#41;](../../oledb/features/table-valued-parameters-oledb-driver-for-sql-server.md)  
 Traite le pilote OLE DB pour la prise en charge de SQL Server pour les paramètres table.  
  
 [Types CLR volumineux définis par l’utilisateur](../../oledb/features/large-clr-user-defined-types.md)  
 Explique la prise en charge des types UDT volumineux du CLR.  
  
 [Prise en charge de FILESTREAM](../../oledb/features/filestream-support.md)  
 Décrit le pilote OLE DB pour la prise en charge de SQL Server pour la fonctionnalité FILESTREAM améliorée.  
  
 [Nom de Principal du service &#40;SPN&#41; prise en charge dans les connexions clientes](../../oledb/features/service-principal-name-spn-support-in-client-connections.md)  
 Explique comment la prise en charge des noms de principaux du service a été étendue pour permettre l'authentification mutuelle à travers l'ensemble des protocoles.  
  
 [Prise en charge des colonnes éparses dans OLE DB Driver pour SQL Server](../../oledb/features/sparse-columns-support-in-oledb-driver-for-sql-server.md)  
 Décrit le pilote OLE DB pour la prise en charge de SQL Server pour les colonnes éparses.  
  
 [Améliorations des types de données date et heure](../../oledb/features/date-and-time-improvements.md)  
 Décrit la prise en charge par pilote OLE DB pour SQL Server pour les types de données de date et d’heure.  
  
 [Détection des métadonnées](../../oledb/features/metadata-discovery.md)  
 Décrit les améliorations apportées à la découverte des métadonnées dans [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)].  
  
 [Prise en charge d’UTF-16 dans OLE DB Driver pour SQL Server](../../oledb/features/utf-16-support-in-oledb-driver-for-sql-server.md)  
 Décrit un changement de comportement introduit dans [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]. Si vous fournissez une mémoire tampon de longueur fixe lors de la liaison d’un paramètre de résultat ou de la sortie de colonne et si les **wchar** caractères écrits dans la mémoire tampon avant le caractère de fin est un point de code de substitut étendu d’une paire de substitution et si la prochaine **wchar** caractère est un point de code de substitution faible, le pilote OLE DB pour SQL Server n’ajoutera pas le point de code de substitut pour la mémoire tampon.  
  
 [Prise en charge de la récupération d’urgence et de la haute disponibilité par OLE DB Driver pour SQL Server](../../oledb/features/oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md)  
 Explique comment votre application peut être configurée pour tirer parti de la récupération d’urgence haute disponibilité, des fonctionnalités ajoutées dans [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)].  
  
 [Accès aux informations de diagnostic dans le journal des événements étendus](../../oledb/features/accessing-diagnostic-information-in-the-extended-events-log.md)  
 Décrit les améliorations apportées au pilote OLE DB pour SQL Server et le suivi des données qui vous permet d’accéder aux informations de diagnostic dans la mémoire tampon en anneau et le journal des événements étendus.  
  
 [Prise en charge de la base de données locale par OLE DB Driver pour SQL Server](../../oledb/features/oledb-driver-for-sql-server-support-for-localdb.md)  
 Décrit le pilote OLE DB pour la prise en charge de SQL Server pour la fonctionnalité de base de données locale.  
  
## <a name="see-also"></a>Voir aussi  
 [Pilote d’OLE DB pour SQL Server](../../oledb/oledb-driver-for-sql-server.md)      
 [Rubriques de procédures OLE DB](../../oledb/ole-db-how-to/ole-db-how-to-topics.md)   
 [Installation d’OLE DB Driver pour SQL Server](../../oledb/applications/installing-oledb-driver-for-sql-server.md)  
  
  
