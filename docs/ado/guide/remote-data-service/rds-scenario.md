---
title: Scénario des services Bureau à distance | Documents Microsoft
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
- address book application scenario [ADO]
- RDS scenarios [ADO]
ms.assetid: a7dcad87-aaf0-4b02-9660-472f8469761c
caps.latest.revision: 16
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: bf193da58e2f849981a9d1f0c11885dfc40e7be2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="rds-scenario"></a>Scénario des services Bureau à distance
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
 L’application de carnet d’adresses est un scénario qui vous montre comment utiliser le Service de données à distance (RDS) pour créer une application Web simple, bases de données : un carnet d’adresses d’entreprise en ligne. Ce scénario est utile pour Microsoft Visual Basic Scripting Edition (VBScript), les programmeurs COM qui souhaitent apprendre à utiliser les contrôles ActiveX prenant en charge les données avec les services Bureau à distance et des logiciels plus expérimenté aux développeurs qui souhaitent créer des applications Web centrées sur les données.  
  
 Ce scénario suppose que vous savez comment utiliser les balises de mise en page HTML, les techniques de liaison de données utilisez DHTML et programme base avec des contrôles ActiveX.  
  
 Si vous avez installé le Kit de développement, vous pouvez trouver le code source complet pour l’exemple d’application de carnet d’adresses dans le répertoire SDK samples\dataaccess\rds\AddressBook\AddressBook.asp. Pour afficher le scénario de carnet d’adresses, dans Internet Explorer 4.0 ou version ultérieure, tapez **http://*webserver*/RDS/AddressBook/AddressBook.asp** où *webserver* est le nom donné sur votre ordinateur de serveur Windows NT 4.0 ou Windows 2000 Web qui exécute Internet Information Services (IIS) et ASP.  
  
## <a name="introduction-to-address-book"></a>Introduction au carnet d’adresses  
 L’exemple d’application de carnet d’adresses fournit un carnet d’adresses en ligne simple que vous pouvez utiliser pour publier un répertoire consultable via un intranet. Le carnet d’adresses est conçu afin qu’un utilisateur puisse entrer une chaîne de recherche dans un ou plusieurs champs pour demander des informations sur les employés. Pour vous montrer les fonctionnalités de base du Service de données distant, l’application d’exemple est intentionnellement réduite, avec un nombre minimal d’objets et des champs de recherche.  
  
 L’interface de l’application se compose des éléments suivants :  
  
-   Non visuels **RDS. DataControl** objet de liaison de données qui est utilisé par le client pour se connecter à la base de données.  
  
-   Critères de recherche de zones de texte HTML qui agissent comme des champs d’entrée pour l’attribut de l’employé.  
  
-   Boutons de commande HTML pour générer des requêtes, effacer les champs de recherche, mettre à jour de la base de données avec les informations sur les employés, annuler les modifications en attente et parcourir les lignes de données affichées dans la grille.  
  
-   Liaison de données DHTML pour afficher des données retournées par les requêtes par rapport à une base de données principale (via la **RDS. DataControl** objet de liaison de données) dans une table.  
  
-   Routines de VBScript qui se connecter à chacun des éléments qui ont été précédemment mentionnés et leur permettent d’interagir. Le code VBScript est également utilisé pour initialiser le **RDS. DataControl** de l’objet et de créer dynamiquement les en-têtes de colonne dans la table HTML dans les noms de la **RDS. DataControl** champs du recordset.  
  
 Suivez les liens à partir de l’étape à l’étape pour configurer et exécuter le scénario et pour en savoir plus sur le fonctionne du scénario.  
  
 Ce scénario comprend les rubriques suivantes.  
  
-   [Configuration système exigée pour l’application Carnet d’adresses](../../../ado/guide/remote-data-service/system-requirements-for-the-address-book-application.md)  
  
-   [Exécution du script SQL Carnet d’adresses](../../../ado/guide/remote-data-service/running-the-address-book-sql-script.md)  
  
-   [Exécution de l’exemple d’application Carnet d’adresses](../../../ado/guide/remote-data-service/running-the-address-book-sample-application.md)  
  
-   [Objet de liaison de données de l’application Carnet d’adresses](../../../ado/guide/remote-data-service/address-book-data-binding-object.md)  
  
-   [Boutons de commande de l’application Carnet d’adresses](../../../ado/guide/remote-data-service/address-book-command-buttons.md)  
  
-   [Boutons de navigation de l’application Carnet d’adresses](../../../ado/guide/remote-data-service/address-book-navigation-buttons.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Configuration système requise pour l’Application de carnet d’adresses](../../../ado/guide/remote-data-service/system-requirements-for-the-address-book-application.md)   
 [Microsoft ActiveX Data Objects (ADO)](../../../ado/microsoft-activex-data-objects-ado.md)   
 [Principes de base des services Bureau à distance](../../../ado/guide/remote-data-service/rds-fundamentals.md)   
 [Didacticiel RDS](../../../ado/guide/remote-data-service/rds-tutorial.md)


