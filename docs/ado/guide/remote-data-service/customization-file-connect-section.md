---
title: Section Connect du fichier de personnalisation | Documents Microsoft
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
- connect section in RDS [ADO]
- customization file in RDS [ADO]
ms.assetid: d50eb3cc-a822-486f-b80b-65bb50547ecd
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 12d1560220a9c281425a1d75c43f0ef95845d611
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="customization-file-connect-section"></a>Section Connect du fichier de personnalisation
Le comportement par défaut du gestionnaire est à refuser toutes les connexions. Le **connecter** section indique les exceptions à ce comportement. Par exemple, si tous les **connecter** sections sont absents ou vides, puis par défaut aucune connexion n’a pu être établie.  
  
 Le **connecter** section peut contenir :  
  
-   Une entrée d’accès par défaut qui spécifie la valeur par défaut lire et écrire des opérations autorisées sur cette connexion. S’il n’existe aucune entrée d’accès par défaut dans la section, la section sera ignorée.  
  
-   Nouvelle chaîne de connexion qui remplace la chaîne de connexion du client.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
## <a name="syntax"></a>Syntaxe  
 Une entrée d’accès par défaut est de la forme :  
  
```  
  
Access=  
accessRight  
  
```  
  
 Une entrée de chaîne de connexion de remplacement a la forme suivante :  
  
```  
  
Connect=  
connectionString  
  
```  
  
## <a name="remarks"></a>Notes  
  
|Élément| Description|  
|----------|-----------------|  
|**Se connecter**|Une chaîne littérale qui indique qu’il est une entrée de chaîne de connexion.|  
|***connectionString***|Chaîne qui remplace la chaîne de connexion client global.|  
|**Accès**|Une chaîne littérale qui indique qu’il est une entrée d’accès.|  
|***accessRight***|L’un des droits d’accès suivants :<br /><br /> -   **NoAccess** : utilisateur ne peut pas accéder à la source de données.<br />-   **En lecture seule** : l’utilisateur peut lire la source de données.<br />-   **ReadWrite** : utilisateur peut lire ou écrire dans la source de données.|  
  
 Si vous souhaitez autoriser toute connexion (en désactivant, le comportement par défaut du gestionnaire), définissez l’entrée d’accès le **connecter par défaut** section `Access=ReadWrite`et supprimez ou commentez à n’importe quel autre **connecter** *identificateur* section.  
  
## <a name="see-also"></a>Voir aussi  
 [Section de personnalisation de fichiers journaux](../../../ado/guide/remote-data-service/customization-file-logs-section.md)   
 [Section de personnalisation de fichier SQL](../../../ado/guide/remote-data-service/customization-file-sql-section.md)   
 [Section du fichier de personnalisation UserList](../../../ado/guide/remote-data-service/customization-file-userlist-section.md)   
 [DataFactory, personnalisation](../../../ado/guide/remote-data-service/datafactory-customization.md)   
 [Paramètres Client requis](../../../ado/guide/remote-data-service/required-client-settings.md)   
 [Présentation du fichier de personnalisation](../../../ado/guide/remote-data-service/understanding-the-customization-file.md)   
 [Écriture d’un gestionnaire personnalisé](../../../ado/guide/remote-data-service/writing-your-own-customized-handler.md)



