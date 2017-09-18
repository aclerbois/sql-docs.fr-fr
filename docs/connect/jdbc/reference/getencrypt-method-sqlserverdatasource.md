---
title: "Méthode getEncrypt (SQLServerDataSource) | Documents Microsoft"
ms.custom: 
ms.date: 01/19/2017
ms.prod: sql-non-specified
ms.reviewer: 
ms.suite: 
ms.technology:
- drivers
ms.tgt_pltfrm: 
ms.topic: article
apiname:
- getEncrypt Method (SQLServerDataSource)
apilocation:
- getEncrypt Method (SQLServerDataSource)
apitype: Assembly
ms.assetid: 1cdb12dd-6e6f-4bbd-8f5f-9e630f3ee2c9
caps.latest.revision: 19
author: MightyPen
ms.author: genemi
manager: jhubbard
ms.translationtype: MT
ms.sourcegitcommit: f7e6274d77a9cdd4de6cbcaef559ca99f77b3608
ms.openlocfilehash: c8edaf27e2c08b1738d8dedc3e059c0fecb5043b
ms.contentlocale: fr-fr
ms.lasthandoff: 09/09/2017

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
 [Membres de SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource, classe](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  