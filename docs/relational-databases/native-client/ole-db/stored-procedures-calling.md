---
title: Appel d’une procédure stockée (OLE DB) | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-ole-db
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- calling stored procedures
- RPC escape sequence
- OLE DB, stored procedures
- parameters [SQL Server Native Client], OLE DB
- ODBC CALL escape sequence
- stored procedures [OLE DB], calling
- SQL Server Native Client OLE DB provider, stored procedures
ms.assetid: 8e5738e5-4bbe-4f34-bd69-0c0633290bdd
caps.latest.revision: 39
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: e853d37f3ada75ef5259fbc8fedabfb3e48d8c9d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="stored-procedures---calling"></a>Procédures stockées - appel
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

  Une procédure stockée peut avoir entre zéro et plusieurs paramètres. Elle peut également retourner une valeur. Lorsque vous utilisez le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Client fournisseur OLE DB natif, les paramètres à une procédure stockée peuvent être passés par :  
  
-   via un codage effectué de manière irréversible de la valeur de données ;  
  
-   via un marqueur de paramètre (?) pour spécifier les paramètres, lier une variable de programme au marqueur de paramètre, puis placer la valeur de données dans la variable de programme.  
  
> [!NOTE]  
>  Lors de l'appel des procédures stockées [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à l'aide de paramètres nommés avec OLE DB, les noms de paramètres doivent commencer par le caractère « @ ». Il s'agit d'une restriction spécifique à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Le fournisseur OLE DB de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client applique cette restriction plus strictement que MDAC.  
  
 Pour prendre en charge les paramètres, le **ICommandWithParameters** interface est exposée sur l’objet de commande. Pour utiliser des paramètres, le consommateur décrit tout d’abord les paramètres au fournisseur en appelant le **ICommandWithParameters::SetParameterInfo** (méthode) (ou prépare éventuellement une instruction qui appelle la **GetParameterInfo** méthode). Le consommateur crée ensuite un accesseur qui spécifie la structure d'une mémoire tampon dans laquelle il place des valeurs de paramètre. Enfin, il passe le handle de l’accesseur et un pointeur vers la mémoire tampon à **Execute**. Sur les appels ultérieurs à **Execute**, le consommateur place les nouvelles valeurs de paramètre dans la mémoire tampon et les appels **Execute** avec le pointeur de handle et de la mémoire tampon d’accesseur.  
  
 Une commande qui appelle une procédure stockée temporaire à l’aide de paramètres doive commencer par appeler **ICommandWithParameters::SetParameterInfo** pour définir les informations de paramètre, avant que la commande peut être préparée avec succès. En effet, le nom interne d'une procédure stockée temporaire diffère du nom externe utilisé par un client ; par ailleurs, SQLOLEDB ne peut pas interroger les tables système afin de déterminer les informations de paramètre pour une procédure stockée temporaire.  
  
 Voici les étapes du processus de liaison des paramètres :  
  
1.  Indiquez les informations de paramètre dans un tableau de structures DBPARAMBINDINFO, notamment le nom du paramètre, le nom spécifique au fournisseur pour le type de données du paramètre ou un nom de type de données standard, etc. Chaque structure du tableau décrit un paramètre. Ce tableau est ensuite transmis à la **SetParameterInfo** (méthode).  
  
2.  Appelez le **ICommandWithParameters::SetParameterInfo** méthode pour décrire les paramètres au fournisseur. **SetParameterInfo** Spécifie le type de données natif de chaque paramètre. **SetParameterInfo** arguments sont :  
  
    -   le nombre de paramètres pour lesquels définir des informations de type ;  
  
    -   un tableau d'ordinaux de paramètres pour lesquels définir des informations de type ;  
  
    -   un tableau de structures DBPARAMBINDINFO.  
  
3.  Créer un accesseur de paramètre à l’aide de la **IAccessor::CreateAccessor** commande. L'accesseur spécifie la structure d'une mémoire tampon dans laquelle il place les valeurs de paramètre. Le **CreateAccessor** commande crée un accesseur à partir d’un jeu de liaisons. Ces liaisons sont décrites par le consommateur via un tableau de structures DBBINDING. Chaque liaison associe un paramètre unique à la mémoire tampon du consommateur et contient les informations suivantes :  
  
    -   ordinal du paramètre auquel la liaison s'applique ;  
  
    -   élément lié (valeur de données, longueur et état) ;  
  
    -   offset en mémoire tampon pour chacun de ces composants ;  
  
    -   longueur et type de la valeur de données dans la mémoire tampon du consommateur.  
  
     Un accesseur est identifié par son handle, qui est de type HACCESSOR. Ce handle est retourné par la **CreateAccessor** (méthode). Chaque fois que le consommateur termine à l’aide d’un accesseur, le consommateur doit appeler le **ReleaseAccessor** méthode pour libérer la mémoire qu’il contient.  
  
     Lorsque le consommateur appelle une méthode, telles que **ICommand::Execute**, il passe le handle à un accesseur et un pointeur vers une mémoire tampon. Le fournisseur utilise cet accesseur pour déterminer comment transférer les données contenues en mémoire tampon.  
  
4.  Remplissez la structure DBPARAMS. Les variables de consommateur à partir de quel paramètre d’entrée, les valeurs sont extraites et pour le paramètre de sortie, les valeurs sont écrites sont passées au moment de l’exécution à **ICommand::Execute** dans la structure DBPARAMS. La structure DBPARAMS inclut trois éléments :  
  
    -   un pointeur vers la mémoire tampon à partir de laquelle le fournisseur récupère les données de paramètre d'entrée et dans laquelle il retourne les données de paramètre de sortie, en fonction des liaisons spécifiées par le handle d'accesseur ;  
  
    -   le nombre de jeux de paramètres en mémoire tampon ;  
  
    -   le handle d'accesseur créé à l'étape 3.  
  
5.  Exécutez la commande à l’aide de **ICommand::Execute**.  
  
## <a name="methods-of-calling-a-stored-procedure"></a>Méthodes d'appel d'une procédure stockée  
 Lors de l’exécution d’une procédure stockée dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] prend en charge du fournisseur OLE DB Native Client le :  
  
-   séquence d'échappement ODBC CALL ;  
  
-   séquence d'échappement d'appel de procédure distante (RPC, Remote Procedure Call) ;  
  
-   instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)] EXECUTE.  
  
### <a name="odbc-call-escape-sequence"></a>Séquence d'échappement ODBC CALL  
 Si vous connaissez les informations de paramètre, appelez **ICommandWithParameters::SetParameterInfo** méthode pour décrire les paramètres au fournisseur. Sinon, lorsque la syntaxe ODBC CALL est utilisée pour appeler une procédure stockée, le fournisseur appelle une fonction d'assistance afin de rechercher les informations de paramètre de la procédure stockée.  
  
 Si vous n'êtes pas sûr des informations de paramètre (métadonnées de paramètre), la syntaxe ODBC CALL est recommandée.  
  
 La syntaxe générale pour l'appel d'une procédure à l'aide de la séquence d'échappement ODBC CALL est :  
  
 {[**? =**]**appel ***nom_procédure*[**(**[*paramètre*] [**,**[*paramètre*]]...** )**]}  
  
 Par exemple :  
  
```  
{call SalesByCategory('Produce', '1995')}  
```  
  
### <a name="rpc-escape-sequence"></a>Séquence d'échappement RPC  
 La séquence d'échappement d'appel de procédure distante (RPC, Remote Procedure Call) est semblable à la syntaxe ODBC CALL de l'appel d'une procédure stockée. Si vous devez appeler la procédure plusieurs fois, la séquence d'échappement RPC offre les meilleures performances parmi les trois méthodes d'appel d'une procédure stockée.  
  
 Lorsque la séquence d'échappement RPC est utilisée pour exécuter une procédure stockée, le fournisseur n'appelle pas de fonction d'assistance pour déterminer les informations de paramètre (comme il le fait dans le cas de la syntaxe ODBC CALL). La syntaxe RPC est plus simple que la syntaxe ODBC CALL ; par conséquent, la commande est analysée plus rapidement, ce qui améliore les performances. Dans ce cas, vous devez fournir les informations de paramètre en exécutant **ICommandWithParameters::SetParameterInfo**.  
  
 La séquence d'échappement RPC nécessite que vous disposiez d'une valeur de retour. Si la procédure stockée ne retourne pas de valeur, le serveur retourne 0 par défaut. De plus, vous ne pouvez pas ouvrir de curseur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur la procédure stockée. La procédure stockée est préparée implicitement et l’appel à **ICommandPrepare::Prepare** échoue. En raison de l’incapacité de préparer un appel RPC, vous ne pouvez pas interroger les métadonnées de colonne ; IColumnsInfo::GetColumnInfo et IColumnsRowset::GetColumnsRowset retourneront DB_E_NOTPREPARED.  
  
 Si vous connaissez toutes les métadonnées de paramètre, la séquence d'échappement RPC est la méthode recommandée pour exécuter les procédures stockées.  
  
 Voici un exemple de séquence d'échappement RPC pour l'appel d'une procédure stockée :  
  
```  
{rpc SalesByCategory}  
```  
  
 Pour un exemple d’application qui illustre une séquence d’échappement RPC, consultez [exécuter une procédure stockée & #40 ; À l’aide de la syntaxe RPC & #41 ; et traiter le retour Codes et les paramètres de sortie & #40 ; OLE DB & #41 ; ](../../../relational-databases/native-client-ole-db-how-to/results/execute-stored-procedure-with-rpc-and-process-output.md).  
  
### <a name="transact-sql-execute-statement"></a>Instruction Transact-SQL EXECUTE  
 La séquence d’échappement ODBC CALL et la séquence d’échappement RPC sont les méthodes recommandées pour l’appel d’une procédure stockée plutôt que la [EXECUTE](../../../t-sql/language-elements/execute-transact-sql.md) instruction. Le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client utilise le mécanisme RPC de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour optimiser le traitement des commandes. Ce protocole RPC augmente les performances en supprimant une bonne partie du traitement des paramètres et de l'analyse des instructions sur le serveur.  
  
 Il s’agit d’un exemple de la [!INCLUDE[tsql](../../../includes/tsql-md.md)] **EXECUTE** instruction :  
  
```  
EXECUTE SalesByCategory 'Produce', '1995'  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées](../../../relational-databases/native-client/ole-db/stored-procedures.md)  
  
  
