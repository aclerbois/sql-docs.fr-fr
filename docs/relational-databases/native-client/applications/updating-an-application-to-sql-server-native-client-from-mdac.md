---
title: Mise à jour d’une Application vers SQL Server Native Client à partir de MDAC | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client|applications
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- MDAC [SQL Server]
- SQLNCLI, vs. MDAC
- SQL Server Native Client, vs. MDAC
- data access [SQL Server Native Client], vs. MDAC
- SQL Server Native Client, updating applications
ms.assetid: 2860efdd-c59a-4deb-8a0e-5124a8f4e6dd
caps.latest.revision: 81
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 26efb6569ba997ed8eb53036987081b2cf8f5f2a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="updating-an-application-to-sql-server-native-client-from-mdac"></a>Mise à jour d'une application vers SQL Server Native Client à partir de MDAC
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

  Il existe plusieurs différences entre [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client et MDAC (Microsoft Data Access Components). Notez qu'à compter de Windows Vista, MDAC prend le nom de « Windows DAC » (Windows Data Access Components). Même si ces deux solutions fournissent aux données natives un accès aux bases de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client a été spécialement conçu pour exposer les nouvelles fonctionnalités de [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], tout en restant compatible avec les versions antérieures.  
  
 Les informations contenues dans cette rubrique permettent de mettre à jour votre application MDAC (ou Windows DAC) pour la rendre conforme à la version de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client fournie avec [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]. Pour vous aider à rendre cette application en cours avec la version de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client fournie avec [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)], consultez [mise à jour d’une Application à partir de SQL Server 2005 Native Client](../../../relational-databases/native-client/applications/updating-an-application-from-sql-server-2005-native-client.md).  
  
 Par ailleurs, bien que MDAC contienne des composants permettant d'utiliser OLE DB, ODBC et des objets ADO (ActiveX Data Object), [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client implémente uniquement OLE DB et ODBC (bien que les objets ADO puissent accéder aux fonctionnalités de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client).  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client et MDAC diffèrent également dans les domaines suivants :  
  
-   Les utilisateurs qui utilisent des objets ADO pour accéder à un fournisseur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client peuvent trouver moins de fonctionnalités de filtrage qu'en accédant à un fournisseur OLE DB SQL.  
  
-   Si une application ADO utilise [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client et qu'elle tente de mettre à jour une colonne calculée, une erreur est signalée. Avec MDAC, la mise à jour était acceptée mais ignorée.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client est un fichier bibliothèque de liens dynamiques (DLL) autonome unique. Les interfaces exposées publiquement ont été limitées en nombre pour faciliter la distribution et limiter l'exposition de sécurité.  
  
-   Seules les interfaces OLE DB et ODBC sont prises en charge.  
  
-   Le fournisseur OLE DB Native Client [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et les noms de pilote ODBC sont différents de ceux utilisés avec MDAC.  
  
-   Les fonctionnalités accessibles à l'utilisateur fournies par les composants MDAC sont disponibles lorsque vous utilisez [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client. Cela comprend, entre autres, le regroupement de connexions, la prise en charge des objets ADO et la prise en charge du curseur client. Lorsque l'une de ces fonctionnalités est utilisée, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client fournit uniquement la connectivité de base de données. MDAC fournit des fonctionnalités telles que le suivi, des contrôles de gestion et des compteurs de performance.  
  
-   Les applications peuvent utiliser les services principaux OLE DB avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client, mais si elles utilisent le moteur de curseur OLE DB, elles doivent utiliser l'option de compatibilité du type de données pour éviter tout problème pouvant résulter du fait que le moteur de curseur n'a pas connaissance des nouveaux types de données [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)].  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client prend en charge l'accès aux bases de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] précédentes.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client n'offre pas l'intégration XML. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client prend en charge SELECT... POUR les requêtes de XML, mais ne prend pas en charge d’autres fonctionnalités XML. Toutefois, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ne prend pas en charge la **xml** type de données introduits dans [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)].  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client prend uniquement en charge la configuration de bibliothèques réseau côté client à l'aide d'attributs de chaîne de connexion. Pour configurer une bibliothèque réseau de manière plus complète, vous devez utiliser le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client n'est pas compatible avec odbcbcp.dll. Les applications qui utilisent les deux ODBC et **bcp** API doit être reconstruites pour établir un lien vers sqlncli11.lib afin d’utiliser [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client n'est pas pris en charge à partir du fournisseur Microsoft OLE DB pour ODBC (MSDASQL). Si vous utilisez le pilote MDAC SQLODBC avec MSDASQL ou le pilote MDAC SQLODBC avec ADO, utilisez OLE DB dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client.  
  
-   Chaînes de connexion MDAC autorisent une valeur booléenne (**true**) pour le **Trusted_Connection** (mot clé). A [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] chaîne de connexion Native Client doit utiliser **Oui** ou **aucun**.  
  
-   Des changements mineurs affectent les avertissements et les erreurs. Les avertissements et les erreurs retournées par le serveur conservent désormais la même gravité lorsqu'ils sont passés à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client. Vous devez vous assurer d'avoir rigoureusement testé votre application si vous comptez sur l'interception d'avertissements et d'erreurs particuliers.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client effectue une vérification des erreurs plus stricte que MDAC, ce qui signifie que certaines applications qui ne sont pas strictement conformes aux spécifications ODBC et OLE DB peuvent se comporter différemment. Par exemple, contrairement au fournisseur OLE DB [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client, le fournisseur SQLOLEDB n'applique pas la règle selon laquelle les noms de paramètre doivent commencer par « @ »pour les paramètres de résultat.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client se comporte différemment de MDAC en ce qui concerne les connexions échouées. Par exemple, MDAC retourne des valeurs de propriété mises en cache pour une connexion qui a échoué, alors que [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client signale une erreur à l'application appelante.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ne génère pas d'événements Visual Studio Analyzer, mais il génère à la place des événements de suivi Windows.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ne peut pas être utilisé avec perfmon. Perfmon est un outil Windows qui peut être uniquement utilisé avec des noms de source de données (DSN) qui utilisent le pilote MDAC SQLODBC inclus avec Windows.  
  
-   Lorsque [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client est connecté à [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] et versions ultérieures, l'erreur de serveur 16947 est retournée en tant qu'erreur (SQL_ERROR). Cette erreur se produit lorsqu'une mise à jour ou une suppression positionnée ne parvient pas à mettre à jour ou à supprimer une ligne. Avec MDAC lors de la connexion à n'importe quelle version de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], l'erreur de serveur 16947 est retournée en tant qu'avertissement (SQL_SUCCESS_WITH_INFO).  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client implémente le **IDBDataSourceAdmin** d’interface, qui est une interface OLE DB facultative qui n’était pas précédemment implémentée, mais seule la **CreateDataSource** ce Optional (méthode) interface est implémentée. [!INCLUDE[ssNoteDepFutureAvoid](../../../includes/ssnotedepfutureavoid-md.md)]  
  
-   Le fournisseur OLE DB [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client retourne des synonymes dans les ensembles de lignes de schéma TABLES et TABLE_INFO, avec la valeur SYNONYM attribuée à TABLE_TYPE.  
  
-   Retourner des valeurs de type de données **varchar (max)**, **nvarchar (max)**, **varbinary (max)**, **xml**, **udt**, ou d’autres types d’objet volumineux ne peuvent pas être retournés pour les versions client antérieures à [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]. Si vous souhaitez utiliser ces types comme valeurs de retour, vous devez utiliser [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client.  
  
-   Contrairement à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client, MDAC permet l'exécution des instructions suivantes au démarrage de transactions manuelles et implicites. Elles doivent être exécutées en mode de validation automatique.  
  
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
  
     Ce mappage de type affecte les valeurs retournées pour les métadonnées de colonne. Par exemple, un **texte** colonne a une taille maximale de 2 147 483 647, mais [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ODBC Native Client signale la taille maximale de **varchar (max)** colonnes en tant que SQL_SS_LENGTH_UNLIMITED, et [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] OLE DB Native Client signale la taille maximale de **varchar (max)** colonnes en tant que 2 147 483 647 ou -1, en fonction de la plateforme.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client autorise l'ambiguïté dans les chaînes de connexion (par exemple, quelques mots clés peuvent être spécifiés plusieurs fois et des mots clés en conflit peuvent être autorisés avec la résolution basée sur la position ou la précédence) pour des raisons de compatibilité descendante. Les versions ultérieures de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client n'autoriseront peut-être pas l'ambiguïté dans les chaînes de connexion. Lors de la modification d'applications, il est conseillé d'utiliser [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client pour éliminer toute dépendance vis-à-vis de l'ambiguïté de chaîne de connexion.  
  
-   Si vous utilisez un appel ODBC ou OLE DB pour démarrer des transactions, une différence de comportement existe entre [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client et MDAC : les transactions commencent immédiatement avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client, tandis qu'elles commencent après le premier accès à la base de données avec MDAC. Cela peut affecter le comportement des procédures stockées et lots car [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] requiert@TRANCOUNT soient identiques après un lot ou une procédure stockée termine son exécution telle qu’elle était au démarrage de la procédure stockée ou le lot.  
  
-   Avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client, ITransactionLocal::BeginTransaction entraîne une transaction doit être démarré immédiatement. Avec MDAC le démarrage de transaction est différé jusqu'à ce que l'application exécute une instruction qui requiert une transaction en mode de transaction implicite. Pour plus d’informations, consultez [SET IMPLICIT_TRANSACTIONS &#40;Transact-SQL&#41;](../../../t-sql/statements/set-implicit-transactions-transact-sql.md).  
  
-   Vous pouvez rencontrer des erreurs lors de l’utilisation [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pilote du Client natif avec System.Data.Odbc pour accéder à un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ordinateur du serveur qui expose de nouveaux [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-types de données spécifiques ou des fonctionnalités. System.Data.Odbc fournit une implémentation ODBC générique et par la suite ne pas exposer des fonctionnalités spécifiques du fournisseur ou extensions. (Le pilote de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client est mis à jour pour prendre en charge les fonctionnalités les plus récentes de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en mode natif.) Pour contourner ce problème, vous pouvez rétablir MDAC ou migrer vers System.Data.SqlClient.  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client et MDAC prennent en charge l'isolation de la transaction de lecture validée à l'aide du contrôle de version de ligne, mais seul [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client prend en charge l'isolation de la transaction d'instantané. (En termes de programmation, l'isolation de la transaction de lecture validée à l'aide du contrôle de version de ligne est la même chose que la transaction de lecture validée.)  
  
## <a name="see-also"></a>Voir aussi  
 [Génération d’Applications avec SQL Server Native Client](../../../relational-databases/native-client/applications/building-applications-with-sql-server-native-client.md)  
  
  
