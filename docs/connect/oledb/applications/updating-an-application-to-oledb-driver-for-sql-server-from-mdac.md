---
title: Mise à jour d’une Application, le pilote OLE DB pour SQL Server à partir de MDAC | Documents Microsoft
description: Mise à jour d’une application de pilote OLE DB pour SQL Server à partir de MDAC
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: oledb|applications
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- MDAC [SQL Server]
- MSOLEDBSQL, vs. MDAC
- OLE DB Driver for SQL Server, vs. MDAC
- data access [OLE DB Driver for SQL Server], vs. MDAC
- OLE DB Driver for SQL Server, updating applications
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: 907e1c08f422809a04d3e3c8846d91f7982bb7ea
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="updating-an-application-to-ole-db-driver-for-sql-server-from-mdac"></a>Mise à jour d’une Application, le pilote OLE DB pour SQL Server à partir de MDAC
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Il existe plusieurs différences entre le pilote OLE DB pour SQL Server et Microsoft Data Access Components (MDAC) ; à compter de Windows Vista, les composants d’accès aux données sont maintenant appelés Windows Data Access Components (ou Windows DAC). Bien que les deux fournissent l’accès aux données native [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] des bases de données, pilote OLE DB pour SQL Server a été spécifiquement conçu pour exposer les nouvelles fonctionnalités de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], tout en conservant une compatibilité descendante avec les versions antérieures.   

 En outre, bien que MDAC contienne des composants à l’utilisation d’OLE DB, ODBC et ActiveX Data Objects (ADO), pilote OLE DB pour SQL Server implémente uniquement OLE DB (bien que ADO peut accéder aux fonctionnalités du pilote OLE DB pour SQL Server).  

 Pilote OLE DB pour SQL Server et MDAC diffèrent dans les domaines suivants :  

-   Les utilisateurs qui utilisent ADO pour accéder à un pilote OLE DB pour SQL Server peuvent trouver moins de fonctionnalités de filtrage qu’en un fournisseur SQL OLE DB.  

-   Si une application ADO utilise le pilote OLE DB pour SQL Server et tente de mettre à jour une colonne calculée, une erreur sera signalée. Avec MDAC, la mise à jour était acceptée mais ignorée.  

-   Pilote OLE DB pour SQL Server est un fichier de bibliothèque (DLL) de liaison dynamique autonome unique. Les interfaces exposées publiquement ont été limitées en nombre pour faciliter la distribution et limiter l'exposition de sécurité.  

-   Seules les interfaces OLE DB sont prises en charge.  

-   Le pilote OLE DB pour les noms de SQL Server sont différents de ceux utilisés avec MDAC.  

-   Des fonctionnalités accessibles par l’utilisateur fournie par les composants MDAC sont disponibles lors de l’utilisation du pilote OLE DB pour SQL Server. Cela comprend, entre autres, le regroupement de connexions, la prise en charge des objets ADO et la prise en charge du curseur client. Lorsque aucune de ces fonctionnalités sont utilisées, pilote OLE DB pour SQL Server fournit la connectivité de base de données uniquement. MDAC fournit des fonctionnalités telles que le suivi, des contrôles de gestion et des compteurs de performance.  

-   Applications peuvent utiliser les services principaux OLE DB avec le pilote OLE DB pour SQL Server, mais si vous utilisez le moteur de curseur OLE DB, ils doivent utiliser l’option de compatibilité de type de données pour éviter les problèmes potentiels qui peuvent se produire, car le moteur de curseur n’a aucune connaissance de la nouvelle [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] des types de données.  

-   Pilote OLE DB pour SQL Server prend en charge l’accès à la précédente [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] bases de données.  

-   Pilote OLE DB pour SQL Server ne contient pas d’intégration de XML. Pilote OLE DB pour SQL Server prend en charge SELECT... POUR les requêtes de XML, mais ne prend pas en charge d’autres fonctionnalités XML. Toutefois, le pilote OLE DB pour SQL Server prend en charge la **xml** type de données introduits dans [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)].  

-   Pilote OLE DB pour SQL Server prend en charge la configuration des bibliothèques réseau côté client à l’aide des attributs de chaîne de connexion uniquement. Pour configurer une bibliothèque réseau de manière plus complète, vous devez utiliser le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  

-   Chaînes de connexion MDAC autorisent une valeur booléenne (**true**) pour le **Trusted_Connection** (mot clé). Un pilote OLE DB pour la chaîne de connexion SQL Server doit utiliser **Oui** ou **aucun**.  

-   Des changements mineurs affectent les avertissements et les erreurs. Avertissements et erreurs retournées par le serveur maintenant conservent la même gravité lorsqu’ils sont passées au pilote OLE DB pour SQL Server. Vous devez vous assurer d'avoir rigoureusement testé votre application si vous comptez sur l'interception d'avertissements et d'erreurs particuliers.  

-   Pilote OLE DB pour SQL Server a la vérification que MDAC, ce qui signifie que certaines applications non conformes strictement aux spécifications OLE DB peuvent se comporter différemment des erreurs plus stricte. Par exemple, le fournisseur SQLOLEDB n’applique pas la règle de noms de paramètre doivent commencer par ' @' pour résultat paramètres, mais le pilote OLE DB pour SQL Server effectue.  

-   Pilote OLE DB pour SQL Server se comporte différemment de MDAC en ce qui concerne les connexions ayant échouées. Par exemple, MDAC retourne des valeurs de propriété mises en cache pour une connexion qui a échoué, alors que le pilote OLE DB pour SQL Server signale une erreur à l’application appelante.  

-   Pilote OLE DB pour SQL Server ne génère pas d’événements Visual Studio Analyzer, mais génère à la place des événements de suivi Windows.  

-   Pilote OLE DB pour SQL Server ne peut pas être utilisé avec perfmon. Perfmon est un outil Windows qui peut être uniquement utilisé avec des noms de source de données (DSN) qui utilisent le pilote MDAC SQLODBC inclus avec Windows.  

-   Lorsque le pilote OLE DB pour SQL Server est connecté à [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] et versions ultérieures, erreur de serveur 16947 est retournée en tant que SQL_ERROR. Cette erreur se produit lorsqu'une mise à jour ou une suppression positionnée ne parvient pas à mettre à jour ou à supprimer une ligne. Avec MDAC lors de la connexion à n'importe quelle version de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], l'erreur de serveur 16947 est retournée en tant qu'avertissement (SQL_SUCCESS_WITH_INFO).  

-   Pilote OLE DB pour SQL Server implémente la **IDBDataSourceAdmin** d’interface, qui est une interface OLE DB facultative qui n’était pas précédemment implémentée, mais seule la **CreateDataSource** méthode de ce interface facultative est implémentée. [!INCLUDE[ssNoteDepFutureAvoid](../../../includes/ssnotedepfutureavoid-md.md)]  

-   Le pilote OLE DB pour SQL Server retourne des synonymes dans les TABLES et TABLE_INFO les ensembles de lignes de schéma, avec attribuée à TABLE_TYPE.  

-   Retourner des valeurs de type de données **varchar (max)**, **nvarchar (max)**, **varbinary (max)**, **xml**, **udt**, ou d’autres types d’objet volumineux ne peuvent pas être retournés pour les versions client antérieures à [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]. Si vous souhaitez utiliser ces types en tant que valeurs de retour, vous devez utiliser le pilote OLE DB pour SQL Server.  

-   MDAC autorise les instructions suivantes à exécuter au début de transactions manuelles et implicites, mais n’est pas le cas du pilote OLE DB pour SQL Server. Elles doivent être exécutées en mode de validation automatique.  

    -   Tous les opérations de texte intégral (DDL d'index et de catalogue)  

    -   Toutes les opérations de base de données (create database, alter database, drop database)  

    -   Reconfiguration  

    -   Arrêter  

    -   Kill  

    -   Backup  

-   Lorsque des applications MDAC se connectent à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], les types de données introduits dans [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] apparaissent en tant que types de données compatibles avec [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)], comme indiqué dans le tableau suivant.  

    |Type SQL Server 2005|Type SQL Server 2000|  
    |--------------------------|--------------------------|  
    |**varchar(max)**|**text**|  
    |**nvarchar(max)**|**ntext**|  
    |**varbinary(max)**|**image**|  
    |**udt**|**varbinary**|  
    |**xml**|**ntext**|  

     Ce mappage de type affecte les valeurs retournées pour les métadonnées de colonne. Par exemple, un **texte** colonne a une taille maximale de 2 147 483 647, mais le pilote OLE DB pour SQL Server indique la taille maximale de **varchar (max)** colonnes en tant que 2 147 483 647 ou -1, en fonction de la plateforme.  

-   Pilote OLE DB pour SQL Server autorise l’ambiguïté dans les chaînes de connexion (par exemple, certains mots clés peuvent être spécifiés plusieurs fois et mots clés en conflit peuvent être autorisés avec la résolution en fonction de la position ou la précédence) pour des raisons de compatibilité descendante. Les versions futures de pilote OLE DB pour SQL Server ne permettent pas d’ambiguïté dans les chaînes de connexion. Il est recommandé lors de la modification des applications pour utiliser le pilote OLE DB pour SQL Server afin d’éliminer toute dépendance sur l’ambiguïté de chaîne de connexion.  

-   Si vous utilisez un appel OLE DB pour démarrer des transactions, il est d’une différence de comportement entre le pilote OLE DB pour SQL Server et MDAC : les transactions commencent immédiatement avec le pilote OLE DB pour SQL Server, mais les transactions commencent après la première base de données d’accès à l’aide de MDAC. Cela peut affecter le comportement des procédures stockées et lots car [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] requiert@TRANCOUNT soient identiques après un lot ou une procédure stockée termine son exécution telle qu’elle était au démarrage de la procédure stockée ou le lot.  

-   Avec le pilote OLE DB pour SQL Server, ITransactionLocal::BeginTransaction entraîne une transaction doit être démarré immédiatement. Avec MDAC le démarrage de transaction est différé jusqu'à ce que l'application exécute une instruction qui requiert une transaction en mode de transaction implicite. Pour plus d’informations, consultez [SET IMPLICIT_TRANSACTIONS &#40;Transact-SQL&#41;](../../../t-sql/statements/set-implicit-transactions-transact-sql.md).  


 Les deux pilote OLE DB pour SQL Server et MDAC lecture d’isolation de transaction validée à l’aide de la version de la ligne, mais uniquement pilote OLE DB pour SQL Server prend en charge la capture instantanée d’isolation des transactions. (En termes de programmation, l'isolation de la transaction de lecture validée à l'aide du contrôle de version de ligne est la même chose que la transaction de lecture validée.)  

## <a name="see-also"></a>Voir aussi  
 [Génération d’applications avec OLE DB Driver pour SQL Server](../../oledb/applications/building-applications-with-oledb-driver-for-sql-server.md)  
