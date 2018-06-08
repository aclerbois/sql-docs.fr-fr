---
title: Execute (méthode) (RDS) | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
apitype: COM
helpviewer_keywords:
- Execute method [ADO]
ms.assetid: 2d9c30e9-ab5b-4920-91b8-48454c2fb5d8
caps.latest.revision: 20
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: c050ef685d6e3ef566d78307db2c10c2c01c23e7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="execute-method-rds"></a>Execute (méthode) (RDS)
Exécute la requête et crée un jeu d’enregistrements ADO pour une utilisation dans ADO 2.5 et versions ultérieures.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
object.Execute(ConnectionString As String, HandlerString As String, QueryString As String, lFetchOptions As Long, Properties, TableId, lExecuteOptions As Long, pParameters, [lcid As Long], [pInformation])  
```  
  
#### <a name="parameters"></a>Paramètres  
 *connectionString*  
 Chaîne utilisée pour se connecter au fournisseur OLE DB où la demande va être envoyée pour l’exécution. Si un gestionnaire est spécifié à l’aide de *HandlerString* il peut modifier ou remplacer la chaîne de connexion.  
  
 *HandlerString*  
 Une chaîne en deux parties qui identifie le gestionnaire à utiliser avec cette exécution. La chaîne contient deux parties. La première partie contient le nom (ProgID) du gestionnaire à utiliser. La deuxième partie contient les arguments à passer au gestionnaire. Les détails de l’interprétation de la chaîne d’arguments sont spécifiques à chaque gestionnaire. Les deux parties sont séparées par la première instance d’une virgule dans la chaîne. La chaîne d’arguments peut contenir des virgules supplémentaires. Les arguments sont facultatifs.  
  
 *Chaîne de requête*  
 Une commande dans le langage de commande pris en charge par le fournisseur OLE DB identifié dans la chaîne de connexion. Pour les fournisseurs SQL, *QueryString* peut contenir une instruction de commande Transact-SQL, mais les fournisseurs non-SQL (par exemple, MSDataShape) cela peut ne pas être un [!INCLUDE[tsql](../../../includes/tsql_md.md)] instruction de requête.  
  
 Si un gestionnaire est utilisé, le gestionnaire peut modifier ou remplacer la valeur spécifiée ici. Par exemple, le gestionnaire remplace généralement *QueryString* avec une chaîne de requête à partir de son fichier .ini. Par défaut, le fichier Msdfmap.ini est utilisé.  
  
 *lFetchOptions*  
 Indique le type de l’extraction asynchrone.  
  
 Pour plus d’informations, consultez [propriété FetchOptions (RDS)](../../../ado/reference/rds-api/fetchoptions-property-rds.md).  
  
 *TableID*  
 A **Variant** de type VT_EMPTY ou VT_BSTR. Si cette valeur est de type VT_EMPTY, elle est ignorée. S’il est de type VT_BSTR, le jeu d’enregistrements est créé à l’aide de **adCmdTableDirect** et la valeur spécifiée ici et *QueryString* paramètre est ignoré.  
  
 *lExecuteOptions*  
 Un masque de bits des options d’exécution :  
  
 1 =*ReadOnly* s’ouvre à l’aide de l’objet recordset **adLockReadOnly**.  
  
 2 =*NoBatch* s’ouvre à l’aide de l’objet recordset **adLockOptimistic**.  
  
 4 =*AllParamInfoSupplied* l’appelant garantit que les informations de paramètre pour tous les paramètres sont fournies dans *pParameters*.  
  
 8 =*GetInfo* informations de paramètre pour la requête seront obtenues à partir du fournisseur OLE DB et retournées dans la *pParameters* paramètre. La requête n’est pas exécutée et aucun jeu d’enregistrements n’est retournée.  
  
 16 =*GetHiddenColumns* s’ouvre à l’aide de l’objet recordset **adLockBatchOptimistic** et les colonnes masquées seront inclus dans le jeu d’enregistrements.  
  
 *En lecture seule*, *NoBatch* et *GetHiddenColumns* sont des options mutuellement exclusives ; Toutefois, il ne génère pas d’une erreur pour définir plusieurs d'entre eux. Si plusieurs options sont définies, *GetHiddenColumns* est prioritaire sur toutes les autres, suivi par *ReadOnly*. Si aucune option est spécifiée, par défaut, le jeu d’enregistrements est ouvert à l’aide de **adLockBatchOptimistic** et les colonnes masquées ne sont pas inclus dans le jeu d’enregistrements.  
  
 *pParameters*  
 A **Variant** qui contient un tableau sécurisé de définitions de paramètres. Si le *GetInfo* option a été spécifiée dans *lExecuteOptions*, ce paramètre est utilisé pour retourner les définitions de paramètres obtenues à partir du fournisseur OLE DB. Dans le cas contraire, ce paramètre peut être vide.  
  
 *lcid*  
 Le LCID est utilisé pour générer des erreurs qui sont retournées dans *pInformation*.  
  
 *pInformation*  
 Pointeur vers les informations d’erreur par Execute. Si NULL, aucune information d’erreur n’est retournée.  
  
## <a name="remarks"></a>Notes  
 Le *HandlerString* paramètre peut être null. Que se passe-t-il dans ce cas dépend de la façon dont le serveur de services Bureau à distance est configuré. Une chaîne de gestionnaire de « MSDFMAP.handler » indique que le gestionnaire fourni par Microsoft (Msdfmap.dll) doit être utilisé. Une chaîne de gestionnaire de « MASDFMAP.handler,sample.ini » indique que le Gestionnaire de Msdfmap.dll doit être utilisé et que l’argument « sample.ini » doit être passé au gestionnaire. MSDFMAP.dll interprète l’argument comme étant une direction à utiliser le sample.ini pour vérifier les chaînes de connexion et la requête.  
  
## <a name="applies-to"></a>S'applique à  
 [DataFactory, objet (RDSServer)](../../../ado/reference/rds-api/datafactory-object-rdsserver.md)


