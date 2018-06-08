---
title: Section UserList du fichier de personnalisation | Documents Microsoft
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
- UserList section in rds [ADO]
- customization file in RDS [ADO]
ms.assetid: 42e8ec20-eaac-4a95-8cb8-4bba93a75bcb
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 2f27f83f6b3753ad5298a869e5d8891af089e4ea
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="customization-file-userlist-section"></a>Section du fichier de personnalisation UserList
Le **userlist** section relative à la **connecter** section avec la même *identificateur* paramètre.  
  
 Cette section peut contenir un *entrée d’accès utilisateur*, qui spécifie l’accès de droits pour l’utilisateur spécifié et remplace le *par défaut* *d’accéder à entrée* dans la mise en correspondance **connecter** section.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
## <a name="syntax"></a>Syntaxe  
 Une entrée d’accès utilisateur est sous la forme :  
  
 *Nom d’utilisateur* **=**   
 ***accessRights***  
  
|Élément| Description|  
|----------|-----------------|  
|*userName*|Le *nom d’utilisateur* de la personne utilisant cette connexion. Noms d’utilisateur valides sont établies avec IIS **Service Manager** boîte de dialogue.|  
|***accessRights***|L’un des droits d’accès suivants :<br /><br /> -   **NoAccess** : utilisateur ne peut pas accéder à la source de données.<br />-   **En lecture seule** : l’utilisateur peut lire la source de données.<br />-   **ReadWrite** : utilisateur peut lire ou écrire dans la source de données.|  
  
## <a name="see-also"></a>Voir aussi  
 [Section Connect du fichier de personnalisation](../../../ado/guide/remote-data-service/customization-file-connect-section.md)   
 [Section de personnalisation de fichiers journaux](../../../ado/guide/remote-data-service/customization-file-logs-section.md)   
 [Section de personnalisation de fichier SQL](../../../ado/guide/remote-data-service/customization-file-sql-section.md)   
 [DataFactory, personnalisation](../../../ado/guide/remote-data-service/datafactory-customization.md)   
 [Paramètres Client requis](../../../ado/guide/remote-data-service/required-client-settings.md)   
 [Présentation du fichier de personnalisation](../../../ado/guide/remote-data-service/understanding-the-customization-file.md)   
 [Écriture d’un gestionnaire personnalisé](../../../ado/guide/remote-data-service/writing-your-own-customized-handler.md)


