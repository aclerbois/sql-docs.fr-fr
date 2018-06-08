---
title: Instructions et Limitations des codes XML (SQLXML 4.0) | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: sqlxml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- updategrams [SQLXML], about updategrams
ms.assetid: b5231859-14e2-4276-bc17-db2817b6f235
caps.latest.revision: 31
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 9dda62acddf1dbda2fe37c4ed458c37f5725ffdd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="guidelines-and-limitations-of-xml-updategrams-sqlxml-40"></a>Règles et limitations des codes de mise à jour XML (SQLXML 4.0)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Tenez compte des éléments suivants lorsque vous utilisez des codes de mise à jour (updategrams) XML :  
  
-   Si vous utilisez une mise à jour pour une opération d’insertion avec uniquement une seule paire de  **\<avant >** et  **\<après >** blocs, le  **\<avant >** bloc peut être omis. À l’inverse, en cas d’une opération de suppression, le  **\<après >** bloc peut être omis.  
  
-   Si vous utilisez une mise à jour avec plusieurs  **\<avant >** et  **\<après >** blocs dans le  **\<synchronisation >** de balise, les deux  **\<avant >** blocs et  **\<après >** doivent être spécifiés pour le formulaire  **\<avant >** et  **\<après >** paires.  
  
-   Les mises à jour dans un code de mise à jour sont appliquées à la vue XML fournie avec le schéma XML. Par conséquent, pour que le mappage par défaut soit un succès, vous devez spécifier le nom du fichier de schéma dans le code de mise à jour ou, si le nom de fichier n'est pas fourni, les noms d'élément et d'attribut doivent correspondre aux noms de table et de colonne dans la base de données.  
  
-   SQLXML 4.0 nécessite que toutes les valeurs de colonne dans un code de mise à jour soient explicitement mappées dans le schéma (XDR ou XSD) fourni pour composer la vue XML de ses éléments enfants. Ce comportement diffère des versions antérieures de SQLXML, qui a une valeur autorisée pour une colonne non mappée dans le schéma, si elle a été impliquée dans le cadre de la clé étrangère dans une **SQL : Relationship** annotation. Notez que ce changement n'affecte pas la propagation des valeurs de clé primaire dans les éléments enfants qui a toujours lieu dans SQLXML 4.0 si aucune valeur n'est explicitement spécifiée pour l'élément enfant.  
  
-   Si vous utilisez une mise à jour pour modifier les données dans une colonne binaire (telles que la [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] **image** type de données), vous devez fournir un schéma de mappage dans lequel le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] type de données (par exemple, **SQL : DataType = « image »**) et le type de données XML (par exemple, **dt : type = « binhex »** ou **dt : type = « binbase64**) doit être spécifié. Les données pour la colonne binaire doivent être spécifiées dans la mise à jour ; le **SQL : url-encode** annotation est spécifiée dans le schéma de mappage est ignorée par la mise à jour.  
  
-   Lorsque vous écrivez un schéma XSD, si la valeur que vous spécifiez pour le **SQL : relation** ou **SQL : Field** annotation inclut un caractère spécial, tel qu’un espace (par exemple, dans le nom de la table « Order Details »), cette valeur doit être entre crochets (par exemple, « [Order Details] »).  
  
-   Lors de l'utilisation de codes de mise à jour, les relations de chaîne ne sont pas prises en charge. Par exemple, si les tables A et C sont liées par une relation de chaîne qui utilise la table B, l'erreur suivante survient lorsque vous tentez d'exécuter le code de mise à jour :  
  
    ```  
    There is an inconsistency in the schema provided.  
    ```  
  
     Même si le schéma et le code de mise à jour à la fois apparaissent corrects et formés comme il se doit, cette erreur se produira s'il existe une relation de chaîne.  
  
-   Codes n’autorisent pas le passage de **image** type de données en tant que paramètres au cours des mises à jour.  
  
-   Types d’objet binaire volumineux (BLOB) tels que **text, ntext** et les images ne doivent pas être utilisés dans les  **\<avant >** bloquer lorsque vous travaillez avec des codes, car cela les inclura pour une utilisation dans le contrôle d’accès concurrentiel. Cela peut entraîner des problèmes avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en raison des limitations sur la comparaison pour les types BLOB. Par exemple, le mot clé LIKE est utilisé dans la clause WHERE pour effectuer une comparaison entre les colonnes de la **texte** de type de données ; Toutefois, les comparaisons échouent dans le cas des types d’objets BLOB où la taille des données est supérieure à 8 Ko.  
  
-   Caractères spéciaux dans **ntext** de données peuvent entraîner des problèmes avec SQLXML 4.0 en raison des limitations sur la comparaison pour les types d’objets BLOB. Par exemple, l’utilisation de « [Serializable] » dans le  **\<avant >** bloc d’un codes lorsqu’il est utilisé dans la vérification d’une colonne de l’accès concurrentiel **ntext** type échoue avec la description d’erreur SQLOLEDB suivante :  
  
    ```  
    Empty update, no updatable rows found   Transaction aborted  
    ```  
  
## <a name="see-also"></a>Voir aussi  
 [Considérations de sécurité de mise à jour &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/updategram-security-considerations-sqlxml-4-0.md)  
  
  
