---
title: Didacticiel RDS | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 02/15/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS tutorial [ADO]
ms.assetid: 6e3305a0-7bc7-40d1-9122-235c15d23ab2
caps.latest.revision: 16
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 40ab51d55e33549c2ecc5a615f00f396dd9bb6c9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="rds-tutorial"></a>Didacticiel RDS
Ce didacticiel illustre l’utilisation du modèle de programmation des services Bureau à distance pour interroger et mettre à jour une source de données. Tout d’abord, il décrit les étapes nécessaires pour accomplir cette tâche. Ensuite, le didacticiel est répété dans Microsoft® Visual Basic Scripting Edition (avec ADO pour Windows Foundation Classes (ADO/WFC)).  
  
 Ce didacticiel est codé dans différents langages pour deux raisons :  
  
-   La documentation pour les services Bureau à distance suppose que le lecteur code en Visual Basic. Cela rend la documentation pratique pour les programmeurs Visual Basic, mais il est moins utile pour les programmeurs qui utilisent d’autres langages.  
  
-   Si vous avez des doutes sur une fonctionnalité particulière de services Bureau à distance et que vous connaissez un peu d’une autre langue, vous pourrez peut-être résoudre votre problème en recherchant cette même fonction exprimée dans une autre langue.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
## <a name="how-the-tutorial-is-presented"></a>Présentation du didacticiel  
 Ce didacticiel est basé sur le modèle de programmation des services Bureau à distance. Il traite chaque étape du modèle de programmation individuellement. En outre, il décrit chaque étape avec un fragment de code Visual Basic.  
  
 L’exemple de code est répété dans d’autres langages avec un minimum de détails. Chaque étape dans un didacticiel de langage de programmation donné est marquée avec l’étape correspondante dans le modèle de programmation et le didacticiel descriptif. Utilisez le numéro de l’étape pour faire référence à la discussion dans le didacticiel descriptif.  
  
 Le modèle de programmation des services Bureau à distance est indiqué dans la section suivante. Utilisez-le comme une feuille de route avant de continuer le didacticiel.  
  
## <a name="rds-programming-model-with-objects"></a>Modèle de programmation des services Bureau à distance avec des objets  
  
-   Spécifiez le programme à appeler sur le serveur et obtenir un moyen (proxy) pour faire référence à celui-ci à partir du client.  
  
-   Appeler le programme serveur. Passer des paramètres pour le programme de serveur qui identifie la source de données et la commande à émettre.  
  
-   Le programme serveur obtient un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet à partir de la source de données, généralement à l’aide d’ADO. Si vous le souhaitez, le **Recordset** objet est traité sur le serveur.  
  
-   Le programme serveur retourne le dernier **Recordset** objet à l’application cliente.  
  
-   Sur le client, le **Recordset** objet est éventuellement placé dans un formulaire qui peut être facilement utilisé par des contrôles visuels.  
  
-   Modifications apportées à la **Recordset** objet sont envoyés sur le serveur et permet de mettre à jour la source de données.  
  
 Ce didacticiel contient les rubriques suivantes.  
  
-   [Étape 1 : Spécifier un programme serveur (didacticiel RDS)](../../../ado/guide/remote-data-service/step-1-specify-a-server-program-rds-tutorial.md)  
  
-   [Étape 2 : Appeler le programme serveur (didacticiel RDS)](../../../ado/guide/remote-data-service/step-2-invoke-the-server-program-rds-tutorial.md)  
  
-   [Étape 3 : Le serveur obtient un recordset (didacticiel RDS)](../../../ado/guide/remote-data-service/step-3-server-obtains-a-recordset-rds-tutorial.md)  
  
-   [Étape 4 : Le serveur retourne un recordset (didacticiel RDS)](../../../ado/guide/remote-data-service/step-4-server-returns-the-recordset-rds-tutorial.md)  
  
-   [Étape 5 : DataControl devient utilisable (didacticiel RDS)](../../../ado/guide/remote-data-service/step-5-datacontrol-is-made-usable-rds-tutorial.md)  
  
-   [Étape 6 : Les changements sont envoyés au serveur (didacticiel RDS)](../../../ado/guide/remote-data-service/step-6-changes-are-sent-to-the-server-rds-tutorial.md)  
  
-   [Didacticiel RDS (VBScript)](../../../ado/guide/remote-data-service/rds-tutorial-vbscript.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Étape 1 : Spécifier un programme serveur (didacticiel RDS)](../../../ado/guide/remote-data-service/step-1-specify-a-server-program-rds-tutorial.md)   
 [Didacticiel RDS (VBScript)](../../../ado/guide/remote-data-service/rds-tutorial-vbscript.md)   
