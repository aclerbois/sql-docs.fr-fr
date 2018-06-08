---
title: Interface SQLXML | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 7c67be98-efb5-446c-a0e3-ee67c43cb170
caps.latest.revision: 19
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 2cb8975c4eda311b93c7a26c1d83eecbbfa581f4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlxml-interface"></a>Interface SQLXML
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Le pilote JDBC prend en charge l'API JDBC 4.0, ce qui permet l'introduction de l'interface java.sql.SQLXML. L’interface SQLXML définit des méthodes pour interagir et manipuler des données XML. Le **SQLXML** type de données correspond à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] **xml** type de données.  
  
 L’interface SQLXML fournit des méthodes pour accéder à la valeur XML comme un **chaîne**, un **lecteur** ou **Writer**, ou en tant qu’un **flux**. La valeur XML est également accessible via un **Source** ou défini comme un **résultat**, qui sont utilisés avec l’API d’analyseurs XML tels que le modèle DOM (Document Object), l’API Simple pour XML (SAX) et l’API de diffusion en continu pour XML StAX (), en tant que ainsi qu’avec XSLT transforme et XPath.  
  
## <a name="remarks"></a>Notes  
 Le tableau suivant décrit les méthodes définies dans l'interface SQLXML :  
  
|Syntaxe de la méthode|Description de la méthode|  
|-------------------|------------------------|  
|[free() void](http://go.microsoft.com/fwlink/?LinkId=131685)|Cette méthode libère l'objet SQLXML ainsi que les ressources qu'il détient.|  
|[InputStream getBinaryStream()](http://go.microsoft.com/fwlink/?LinkId=131754)|Retourne un flux d'entrée pour la lecture des données de l'objet SQLXML.|  
|[Lecteur getCharacterStream()](http://go.microsoft.com/fwlink/?LinkId=131755)|Retourne le **XML** données en tant qu’objet java.io.Reader ou en tant que flux de caractères.|  
|[T étend Source T getSource (classe\<T > sourceClass)](http://go.microsoft.com/fwlink/?LinkId=131756)|Retourne un **Source** pour la lecture du **XML** valeur spécifiée par ce **SQLXML** objet.<br /><br /> **Remarque :** la méthode getSource prend en charge les sources suivantes : javax.xml.transform.dom.DOMSource, javax.xml.transform.sax.SAXSource, javax.xml.transform.stax.StAXSource et java.io.InputStream.|  
|[Chaîne getString()](http://go.microsoft.com/fwlink/?LinkId=131757)|Retourne une représentation de chaîne de la **XML** valeur désignée par cet objet SQLXML.|  
|[OutputStream setBinaryStream()](http://go.microsoft.com/fwlink/?LinkId=131758)|Récupère un flux qui peut être utilisé pour écrire la **XML** valeur représentée par cet objet SQLXML.|  
|[Enregistreur setCharacterStream()](http://go.microsoft.com/fwlink/?LinkId=131759)|Retourne un flux à utiliser pour écrire le **XML** valeur représentée par cet objet SQLXML.|  
|[T étend résultat T setResult (classe\<T > resultClass)](http://go.microsoft.com/fwlink/?LinkId=131760)|Retourne un **résultat** pour le paramètre de la **XML** valeur spécifiée par ce **SQLXML** objet.<br /><br /> **Remarque :** la méthode setResult prend en charge les sources suivantes : javax.xml.transform.dom.DOMResult, javax.xml.transform.sax.SAXResult, javax.xml.transform.stax.StaxResult et java.io.OutputStream.|  
|[void setString(String value)](http://go.microsoft.com/fwlink/?LinkId=131762)|Définit la valeur XML désignée par cet objet SQLXML spécifié **chaîne** représentation.|  
  
 Les applications ne peuvent lire et écrire des valeurs XML dans un objet SQLXML qu'une seule fois.  
  
 Lorsque la méthode free() est appelée, un objet SQLXML devient non valide et n’est ni explicite ni accessible en écriture. Si l’application tente d’appeler une méthode sur cet objet SQLXML autre que la méthode free(), une exception est levée.  
  
 L’objet SQLXML devient accessible en lecture ni en écriture lorsque l’application appelle les méthodes getter suivantes : getSource, getCharacterStream, getBinaryStream et getString.  
  
 L’objet SQLXML devient accessible en lecture ni en écriture lorsque l’application appelle les méthodes setter suivantes : setResult, setCharacterStream, setBinaryStream et setString.  
  
## <a name="see-also"></a>Voir aussi  
 [Prise en charge des données XML](../../connect/jdbc/supporting-xml-data.md)  
  
  
