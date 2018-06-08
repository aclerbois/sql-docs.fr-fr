---
title: Présentation du fichier de personnalisation | Documents Microsoft
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
- customization file in RDS [ADO]
ms.assetid: 136f74bf-8d86-4a41-be66-c86cbcf81548
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 99a565fe6ee25f1fb8d0911b80c0b629c02b3cdf
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="understanding-the-customization-file"></a>Présentation du fichier de personnalisation
Chaque en-tête de section dans le fichier de personnalisation est constitué de crochets (**[]**) contenant un type et un paramètre. Les quatre types de section sont indiquées par les chaînes littérales **connecter**, **sql**, **userlist**, ou **journaux**. Le paramètre est la chaîne littérale, la valeur par défaut, un identificateur spécifié par l’utilisateur ou rien du tout.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
 Par conséquent, chaque section est marquée avec l’un des en-têtes de section suivants :  
  
```  
  
[ connect default ] [ connect    
identifier   
] [ sql  default ] [ sql    
identifier   
] [ userlist    
identifier   
] [ logs ]  
  
```  
  
 Les en-têtes de la section des éléments suivants.  
  
|Élément| Description|  
|----------|-----------------|  
|**Se connecter**|Une chaîne littérale qui modifie une chaîne de connexion.|  
|**sql**|Une chaîne littérale qui modifie une chaîne de commande.|  
|**userlist**|Une chaîne littérale qui modifie les droits d’accès d’un utilisateur spécifique.|  
|**Journaux**|Une chaîne littérale qui spécifie un fichier journal de l’enregistrement des erreurs opérationnelles.|  
|**default**|Une chaîne littérale est utilisée si aucun identificateur n’est spécifié ou trouvé.|  
|*Identificateur*|Chaîne qui correspond à une chaîne dans le **connecter** ou **commande** chaîne.<br /><br /> -Utilisez cette section si l’en-tête de section contient **connecter** et la chaîne d’identificateur est trouvée dans la chaîne de connexion.<br />-Utilisez cette section si l’en-tête de section contient **sql** et la chaîne d’identificateur est trouvée dans la chaîne de commande.<br />-Utilisez cette section si l’en-tête de section contient **userlist** et la chaîne de l’identificateur correspond à un **connecter** identificateur de section.|  
  
 Le **DataFactory** appelle le gestionnaire, en passant les paramètres client. Le Gestionnaire de recherche des chaînes entières dans les paramètres client qui correspondent aux identificateurs dans les en-têtes de la section appropriée. Si une correspondance est trouvée, le contenu de cette section est appliqué au paramètre du client.  
  
 Une section spécifique est utilisée dans les circonstances suivantes :  
  
-   A **connecter** section est utilisée si la valeur du client de connexion mot clé de chaîne, « **Source de données = *** valeur*», correspond à un **connecter** identificateur de la section *.*  
  
-   Un **sql** section est utilisée si la chaîne de commande client contient une chaîne qui correspond à un **sql** identificateur de section.  
  
-   A **connecter** ou **sql** section avec un paramètre par défaut est utilisée si aucun identificateur correspondant.  
  
-   A **userlist** section est utilisée si le **userlist** section identificateur correspondances un **connecter** identificateur de section. S’il existe une correspondance, le contenu de la **userlist** section sont appliqués à la connexion régie par le **connecter** section.  
  
-   Si la chaîne dans une chaîne de connexion ou de commande ne correspond pas à l’identificateur dans les **connecter** ou **sql** section d’en-tête et il est aucun **connecter** ou **sql**  section d’en-tête avec un paramètre par défaut, la chaîne du client est utilisé sans modification.  
  
-   Le **journaux** section est utilisée chaque fois que le **DataFactory** est dans l’opération.  
  
## <a name="see-also"></a>Voir aussi  
 [Section Connect du fichier de personnalisation](../../../ado/guide/remote-data-service/customization-file-connect-section.md)   
 [Section de personnalisation de fichiers journaux](../../../ado/guide/remote-data-service/customization-file-logs-section.md)   
 [Section de personnalisation de fichier SQL](../../../ado/guide/remote-data-service/customization-file-sql-section.md)   
 [Section du fichier de personnalisation UserList](../../../ado/guide/remote-data-service/customization-file-userlist-section.md)   
 [DataFactory, personnalisation](../../../ado/guide/remote-data-service/datafactory-customization.md)   
 [Paramètres Client requis](../../../ado/guide/remote-data-service/required-client-settings.md)   
 [Écriture d’un gestionnaire personnalisé](../../../ado/guide/remote-data-service/writing-your-own-customized-handler.md)




















