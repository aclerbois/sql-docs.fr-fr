---
title: Méthode setEncrypt (SQLServerDataSource) | Documents Microsoft
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
- setEncrypt Method (SQLServerDataSource)
apilocation:
- setEncrypt Method (SQLServerDataSource)
apitype: Assembly
ms.assetid: 0c85a9c1-f27c-457e-8461-403cc03e2d17
caps.latest.revision: 22
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 723c5c5402fb32f0ad74bf303dd7c682b88d1c84
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="setencrypt-method-sqlserverdatasource"></a>Méthode setEncrypt (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Définit un **booléenne** valeur qui indique si la propriété encrypt est activée.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void setEncypt(boolean encrypt)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Chiffrer*  
  
 **true** si le chiffrement Secure Sockets Layer (SSL) est activé entre le client et le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)]. Dans le cas contraire, la valeur est **false**.  
  
## <a name="remarks"></a>Notes  
 Si la propriété encrypt a la valeur **true**, le [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] garantit que [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] utilise le chiffrement SSL pour toutes les données envoyées entre le client et le serveur si le serveur possède un certificat installé. La valeur par défaut est **false**.  
  
 Le pilote JDBC détecte la machine virtuelle Java (JVM) sur laquelle il est exécuté lors de la tentative d'établissement d'une négociation SSL.  
  
 Si la propriété encrypt a la valeur **true**, le [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] utilise le fournisseur de sécurité JSSE par défaut de la machine virtuelle Java pour négocier le chiffrement SSL avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)]. Le fournisseur de sécurité par défaut peut ne pas prendre en charge toutes les fonctionnalités requises pour négocier le chiffrement SSL avec succès. Par exemple, le fournisseur de sécurité par défaut peut prend pas en charge la taille de la clé publique RSA utilisée dans le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] certificat SSL. Dans ce cas, le fournisseur de sécurité par défaut peut générer une erreur qui forcera le pilote JDBC à mettre fin à la connexion. Pour résoudre ce problème, effectuez l'une des opérations suivantes :  
  
-   Configurer le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] avec un certificat de serveur qui a une clé publique RSA inférieure  
  
-   Configurer la machine virtuelle Java pour utiliser un autre fournisseur de sécurité JSSE dans le «\<java-home > / lib/security/java.security « fichier de propriétés de sécurité  
  
-   Utiliser une autre machine virtuelle Java  
  
 Si la propriété de chiffrement est n’est pas spécifiée ou la valeur **false**, le pilote ne force pas le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] pour prendre en charge le chiffrement SSL. Si le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] instance n’est pas configurée pour forcer le chiffrement SSL, une connexion est établie sans codage. Si le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] instance est configurée pour forcer le chiffrement SSL, le [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] automatiquement activer le chiffrement SSL lors de fonctionne correctement configuré JVM, sinon la connexion est interrompue et le pilote génère une erreur.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerDataSource, membres](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource, classe](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
