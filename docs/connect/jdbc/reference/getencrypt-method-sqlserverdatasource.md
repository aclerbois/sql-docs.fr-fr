---
title: Méthode getEncrypt (SQLServerDataSource) | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
apiname:
- getEncrypt Method (SQLServerDataSource)
apilocation:
- getEncrypt Method (SQLServerDataSource)
apitype: Assembly
ms.assetid: 1cdb12dd-6e6f-4bbd-8f5f-9e630f3ee2c9
caps.latest.revision: 19
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 9262c2fb18160f5072d21a71c082bd00d17fc302
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="getencrypt-method-sqlserverdatasource"></a>Méthode getEncrypt (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Retourne un **booléenne** valeur qui indique si la propriété encrypt est activée.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public boolean getEncypt()  
```  
  
## <a name="return-value"></a>Valeur retournée  
 **true** si chiffrer est activé. Dans le cas contraire, la valeur est **false**.  
  
## <a name="remarks"></a>Notes  
 Si la propriété encrypt a la valeur **true**, le [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] garantit que [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] utilise le chiffrement SSL pour toutes les données envoyées entre le client et le serveur si le serveur possède un certificat installé.  
  
 Si la propriété de chiffrement est n’est pas spécifiée ou la valeur **false**, le pilote ne force pas le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] pour prendre en charge le chiffrement SSL. Si le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] instance n’est pas configurée pour forcer le chiffrement SSL, une connexion est établie sans codage. Si le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] instance est configurée pour forcer le chiffrement SSL, le [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] automatiquement activer le chiffrement SSL lors de fonctionne correctement configuration de Machine virtuelle Java (JVM), sinon la connexion est interrompue et le pilote génère une erreur. Si la propriété de chiffrement n’est pas définie, la [getEncrypt](../../../connect/jdbc/reference/getencrypt-method-sqlserverdatasource.md) méthode retourne la valeur par défaut **false**.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerDataSource, membres](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource, classe](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
