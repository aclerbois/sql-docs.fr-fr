---
title: ISQLServerErrorInfo::GetErrorInfo (OLE DB) | Documents Microsoft
description: ISQLServerErrorInfo::GetErrorInfo (OLE DB)
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: ole-db-interfaces
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: reference
apiname:
- ISQLServerErrorInfo::GetErrorInfo (OLE DB)
apitype: COM
helpviewer_keywords:
- GetErrorInfo method
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: 057e677242ad12cb9df8a669c129b75c3cb67f2c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="isqlservererrorinfogeterrorinfo-ole-db"></a>ISQLServerErrorInfo::GetErrorInfo (OLE DB)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Retourne un pointeur vers un pilote OLE DB pour SQL Server SSERRORINFO structure contenant les [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] détails de l’erreur.  
  
 Le pilote OLE DB pour SQL Server définit le **ISQLServerErrorInfo** interface d’erreur. Cette interface retourne les détails d’un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] erreur, y compris sa gravité et son état.  

  
## <a name="syntax"></a>Syntaxe  
  
```  
  
HRESULT GetErrorInfo(  
   SSERRORINFO**ppSSErrorInfo,  
   OLECHAR**ppErrorStrings);  
```  
  
## <a name="arguments"></a>Arguments  
 *ppSSErrorInfo*[out]  
 Pointeur vers une structure SSERRORINFO. Si la méthode échoue ou si aucun [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] les informations relatives à l’erreur, le fournisseur n’alloue pas de mémoire et garantit que le *ppSSErrorInfo* argument est un pointeur null en sortie.  
  
 *ppErrorStrings*[out]  
 Pointeur vers un pointeur de chaîne de caractère Unicode. Si la méthode échoue ou si aucun [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] les informations associées à une erreur, le fournisseur n’alloue pas de mémoire et garantit que le *ppErrorStrings* argument est un pointeur null en sortie. La libération de la *ppErrorStrings* argument avec le **IMalloc::Free** méthode libère les trois membres de chaîne individuels de la structure SSERRORINFO retournée, comme la mémoire est allouée dans un bloc.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Cette méthode signale les erreurs en attribuant à la propriété Nombre de l'objet Err global l'une des valeurs du tableau suivant.  
 S_OK  
  
 E_INVALIDARG  
 Soit le *ppSSErrorInfo* ou *ppErrorStrings* argument était NULL.  
  
 E_OUTOFMEMORY  
 Le pilote OLE DB pour SQL Server n’a pas pu allouer suffisamment de mémoire pour compléter la demande.  
  
## <a name="remarks"></a>Notes  
 Le pilote OLE DB pour SQL Server alloue la mémoire pour les chaînes SSERRORINFO et OLECHAR retournées à travers les pointeurs passés par le consommateur. Le consommateur doit libérer cette mémoire en utilisant la **IMalloc::Free** méthode lorsqu’il ne nécessite plus l’accès aux données d’erreur.  
  
 La structure SSERRORINFO est définie comme suit :  
  
```  
typedef struct tagSSErrorInfo  
   {  
   LPOLESTR pwszMessage;  
   LPOLESTR pwszServer;  
   LPOLESTR pwszProcedure;  
   LONG lNative;  
   BYTE bState;  
   BYTE bClass;  
   WORD wLineNumber;  
   }  
SSERRORINFO;  
```  
  
|Membre| Description|  
|------------|-----------------|  
|*pwszMessage*|Message d'erreur de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Le message est retourné par le biais du **IErrorInfo::GetDescription** (méthode).|  
|*pwszServer*|Nom de l'instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur laquelle l'erreur s'est produite.|  
|*pwszProcedure*|Le nom de la procédure stockée qui génère l'erreur si l'erreur s'est produite dans une procédure stockée ; sinon, une chaîne vide.|  
|*lNative*|Numéro d'erreur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Le numéro d’erreur est identique à celui retourné dans le *plNativeError* paramètre de la **ISQLErrorInfo::GetSQLInfo** (méthode).|  
|*bState*|État de l'erreur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|*bClass*|Gravité de l'erreur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|*wLineNumber*|Le cas échéant, la ligne d'une procédure stockée [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui a généré le message d'erreur. Si aucune procédure n'est impliquée, la valeur par défaut est 1.|  
  
 Pointeurs dans la structure de référencent les adresses figurant dans la chaîne retournée dans le *ppErrorStrings* argument.  
  
## <a name="see-also"></a>Voir aussi  
 [ISQLServerErrorInfo &#40;OLE DB&#41;](http://msdn.microsoft.com/library/a8323b5c-686a-4235-a8d2-bda43617b3a1)   
 [RAISERROR &#40;Transact-SQL&#41;](../../../t-sql/language-elements/raiserror-transact-sql.md)  
  
  
