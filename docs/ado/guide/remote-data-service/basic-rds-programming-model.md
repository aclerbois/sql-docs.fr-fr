---
title: Modèle de programmation de base RDS | Documents Microsoft
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
- RDS programming model [ADO]
ms.assetid: 0bdd236b-edff-4aac-94c3-93e1465ca6c5
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 911413ee9bc02c2edbdff0c2e1445dfa94bb5c9c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="basic-rds-programming-model"></a>Modèle de programmation RDS de base
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
 Services Bureau à distance est destiné aux applications qui existent dans l’environnement suivant : une application cliente spécifie un programme qui s’exécute sur un serveur et les paramètres requis pour retourner les informations souhaitées. Le programme appelé sur le serveur accède à la source de données spécifié, récupère les informations, traite éventuellement les données, puis retourne les informations obtenues à votre application cliente dans un formulaire qu’il peut utiliser facilement. Services Bureau à distance fournit les moyens d’effectuer la séquence d’actions suivante :  
  
-   Spécifiez le programme à appeler sur le serveur et obtenir un moyen pour faire référence à celui-ci à partir du client. (Cette référence est parfois appelé un *proxy*. Il représente le programme de serveur distant. L’application cliente « appelle » le proxy comme s’il s’agissait d’un programme local, mais il appelle le programme de serveur distant.)  
  
-   Appeler le programme serveur. Passer des paramètres pour le programme de serveur qui identifient la source de données et la commande à émettre. (Le programme de serveur utilise en fait ADO pour accéder à la source de données. ADO établit une connexion avec l’un des paramètres fournis et envoie ensuite la commande spécifiée dans l’autre paramètre.)  
  
-   Le programme serveur obtient un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet à partir de la source de données. Si vous le souhaitez, le **Recordset** objet est traité sur le serveur.  
  
-   Le programme serveur retourne le dernier **Recordset** objet à l’application cliente.  
  
-   Sur le client, le **Recordset** objet est placé dans une forme facilement utilisable par des contrôles visuels.  
  
-   Les modifications apportées à la **Recordset** objet sont envoyés au programme du serveur, ce qui les utilise pour mettre à jour la source de données.  
  
 Ce modèle de programmation contient certaines fonctionnalités de commodité. Si vous n’avez pas besoin d’un programme de serveur complexe pour accéder à la source de données, et si vous fournissez la connexion requises et les paramètres de commande, les services Bureau à distance sera automatiquement récupérer les données spécifiées avec un programme de serveur simple, par défaut.  
  
 Si vous avez besoin d’un traitement plus complexe, vous pouvez spécifier votre propre programme serveur personnalisé. Par exemple, un programme de serveur personnalisé ayant la puissance de l’objet ADO à sa disposition, peut se connecter à plusieurs sources de données, combiner leurs données dans des opérations complexes et puis de retourner un résultat simple et traité à l’application cliente.  
  
 Enfin, si vos besoins sont quelque part entre les deux, ADO prend désormais en charge personnaliser le comportement du programme de serveur par défaut.  
  
## <a name="see-also"></a>Voir aussi  
 [Modèle de programmation des services Bureau à distance en détail](../../../ado/guide/remote-data-service/rds-programming-model-in-detail.md)   
 [Scénario des services Bureau à distance](../../../ado/guide/remote-data-service/rds-scenario.md)   
 [Didacticiel RDS](../../../ado/guide/remote-data-service/rds-tutorial.md)   
 [Objet Recordset (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)   
 [Utilisation et sécurité de RDS](../../../ado/guide/remote-data-service/rds-usage-and-security.md)


