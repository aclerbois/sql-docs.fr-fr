---
title: Execute21, méthode (RDS) | Documents Microsoft
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
- Execute21 method [RDS]
ms.assetid: 9f131c8d-1497-416d-8209-abb481c38f7b
caps.latest.revision: 17
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 1136e547d464f511aff92f4a6805da0262c7ed35
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="execute21-method-rds"></a>Execute21, méthode (RDS)
Exécute la requête et crée un jeu d’enregistrements ADO pour une utilisation dans ADO 2.1.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
object.Execute21(ConnectionString As String, HandlerString As String, QueryString As String, lMarshalOptions As Long, Properties, TableId, lExecuteOptions As Long, pParameters)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *connectionString*  
 Chaîne utilisée pour se connecter au fournisseur OLE DB où la demande va être envoyée pour l’exécution. Si un gestionnaire est spécifié à l’aide de *HandlerString*, il peut modifier ou remplacer la chaîne de connexion.  
  
 *HandlerString*  
 La chaîne identifie le gestionnaire à utiliser avec cette exécution. La chaîne contient deux parties. La première partie contient le nom (ProgID) du gestionnaire à utiliser. La deuxième partie de la chaîne contient des arguments à passer au gestionnaire. Interprétation de la chaîne d’arguments est gestionnaire spécifique. Les deux parties sont séparées par la première instance d’une virgule dans la chaîne (bien que la chaîne d’arguments peut contenir des virgules supplémentaires). Les arguments sont facultatifs.  
  
 *Chaîne de requête*  
 Une commande dans le langage de commande pris en charge par le fournisseur OLE DB identifié dans la chaîne de connexion. Pour les fournisseurs SQL, il peut contenir un [!INCLUDE[tsql](../../../includes/tsql_md.md)] instruction de commande, mais les fournisseurs non-SQL (par exemple, MSDataShape) cela peut ne pas être un [!INCLUDE[tsql](../../../includes/tsql_md.md)] instruction de requête.  
  
 En outre, si un gestionnaire est utilisé (et il est fortement recommandé qu’un gestionnaire d’être utilisé), le gestionnaire peut modifier ou remplacer la valeur spécifiée ici. Par exemple, le gestionnaire remplace généralement *QueryString* avec une chaîne de requête à partir de son fichier .ini. Par défaut, le fichier Msdfmap.ini est utilisé.  
  
 *lMarshalOptions*  
 Utilisé pour définir les options de marshaling sur le recordset/ensemble de lignes retourné.  
  
 *TableID*  
 Une variante de type VT_EMPTY ou VT_BSTR. Si cette valeur est de type VT_EMPTY, elle est ignorée. S’il est de type VT_BSTR, le jeu d’enregistrements est créé à l’aide de **adCmdTableDirect** à l’aide de la valeur spécifiée ici et *QueryString* paramètre est ignoré.  
  
 *lExecuteOptions*  
 Masque de bits des options d’exécution :  
  
 1 =*ReadOnly* s’ouvre à l’aide de l’objet recordset **adLockReadOnly**.  
  
 2 =*NoBatch* s’ouvre à l’aide de l’objet recordset **adLockOptimistic**.  
  
 4 =*AllParamInfoSupplied* l’appelant garantit que les informations de paramètre pour tous les paramètres sont fournies dans *pParameters*.  
  
 8 =*GetInfo* informations de paramètre pour la requête seront obtenues à partir du fournisseur OLE DB et retournées dans la *pParameters* paramètre. La requête n’est pas exécutée et aucun jeu d’enregistrements n’est retournée.  
  
 16 = GetHiddenColumns s’ouvre à l’aide de l’objet recordset **adLockBatchOptimistic** et les colonnes masquées seront inclus dans le jeu d’enregistrements.  
  
 Bien que *ReadOnly*, *NoBatch* et *GetHiddenColumns* sont des options mutuellement exclusives, il n’est pas une erreur pour définir plusieurs d'entre eux. Si plusieurs options sont définies, *GetHiddenColumns* est prioritaire sur toutes les autres options, suivies par *ReadOnly*. Si aucune option est spécifiée, par défaut, le jeu d’enregistrements est ouvert à l’aide de **adLockBatchOptimistic** , mais les colonnes masquées ne sont pas inclus dans le jeu d’enregistrements.  
  
 *pParameters*  
 Argument de type variant qui contient un tableau sécurisé de définitions de paramètres. Si le *GetInfo* option a été spécifiée dans *lExecuteOptions*, ce paramètre est utilisé pour retourner les définitions de paramètres obtenues à partir du fournisseur OLE DB. Dans le cas contraire, ce paramètre peut être vide.  
  
## <a name="remarks"></a>Notes  
 Le *HandlerString* paramètre peut être null. Ce qui se produit dans ce cas dépend de la façon dont le serveur de services Bureau à distance est configuré. Une chaîne de gestionnaire de « MSDFMAP.handler » indique que le gestionnaire fourni par Microsoft (Msdfmap.dll) doit être utilisé. Une chaîne de gestionnaire de « MASDFMAP.handler,sample.ini » indique que le Gestionnaire de Msdfmap.dll doit être utilisé et que l’argument « sample.ini » doit être passé au gestionnaire. MSDFMAP.dll interprète l’argument comme étant une direction à utiliser le sample.ini pour vérifier les chaînes de connexion et la requête.  
  
> [!NOTE]
>  Le **Execute21** (méthode) est une version de la [Execute (méthode) (RDS)](../../../ado/reference/rds-api/execute-method-rds.md). Où vous devez utiliser le **Execute** méthode pour communiquer avec ADO 2.1, le **Execute21** méthode peut être appelée à la place. Les fonctionnalités de la **Execute** méthode dans ADO 2.5 et versions ultérieur sont un sur-ensemble des fonctionnalités de la même méthode dans ADO 2.1.  
  
## <a name="applies-to"></a>S'applique à  
 [DataFactory, objet (RDSServer)](../../../ado/reference/rds-api/datafactory-object-rdsserver.md)


