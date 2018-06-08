---
title: Section de personnalisation de fichier SQL | Documents Microsoft
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
helpviewer_keywords:
- SQL section in RDS [ADO]
- customization file in RDS [ADO]
ms.assetid: e65c2871-9986-44ff-b8b7-7f5eda91b3fa
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 792168588c13af5007ff35b7af9004f3cc78d385
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="customization-file-sql-section"></a>Section de personnalisation de fichier SQL
Le **sql** section peut contenir une nouvelle chaîne SQL qui remplace la chaîne de commande client. S’il n’existe aucune chaîne SQL dans la section, la section sera ignorée.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
 La nouvelle chaîne SQL peut être *paramétrable*. Autrement dit, les paramètres dans le **sql** section chaîne SQL (désignée par la ' ?' caractère) peuvent être remplacés par les arguments correspondants dans un *identificateur* dans la chaîne de commande client (désigné par un liste délimitée entre parenthèses). L’identificateur et la liste d’arguments se comportent comme un appel de fonction.  
  
 Par exemple, supposons que la chaîne de commande client est `"CustomerByID(4)"`, l’en-tête de la section SQL `[SQL CustomerByID]`, et la nouvelle chaîne de la section SQL est `"SELECT * FROM Customers WHERE CustomerID = ?".` génère le gestionnaire `"SELECT * FROM Customers WHERE CustomerID = 4"` et utiliser cette chaîne pour interroger la source de données.  
  
 Si la nouvelle instruction SQL est une chaîne vide (« »), puis la section est ignorée.  
  
 Si la nouvelle chaîne d’instruction SQL n’est pas valide, l’exécution de l’instruction échoue. Le paramètre client est ignoré. Vous pouvez effectuer cela intentionnellement pour « désactiver » toutes les commandes SQL de client en spécifiant :  
  
```  
[SQL default]   
SQL = " "  
```  
  
## <a name="syntax"></a>Syntaxe  
 Un chaîne d’entrée SQL de remplacement est au format :  
  
 **SQL=**   
 ***sqlString***  
  
|Élément| Description|  
|----------|-----------------|  
|**SQL**|Une chaîne littérale qui indique qu’il est une entrée de section SQL.|  
|***sqlString***|Une chaîne SQL qui remplace la chaîne du client.|  
  
## <a name="see-also"></a>Voir aussi  
 [Section Connect du fichier de personnalisation](../../../ado/guide/remote-data-service/customization-file-connect-section.md)   
 [Section de personnalisation de fichiers journaux](../../../ado/guide/remote-data-service/customization-file-logs-section.md)   
 [Section du fichier de personnalisation UserList](../../../ado/guide/remote-data-service/customization-file-userlist-section.md)   
 [DataFactory, personnalisation](../../../ado/guide/remote-data-service/datafactory-customization.md)   
 [Paramètres Client requis](../../../ado/guide/remote-data-service/required-client-settings.md)   
 [Présentation du fichier de personnalisation](../../../ado/guide/remote-data-service/understanding-the-customization-file.md)   
 [Écriture d’un gestionnaire personnalisé](../../../ado/guide/remote-data-service/writing-your-own-customized-handler.md)


