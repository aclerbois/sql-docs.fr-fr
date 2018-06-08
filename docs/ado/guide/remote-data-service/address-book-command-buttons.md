---
title: Boutons de commande de carnet d’adresses | Documents Microsoft
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
- address book application scenario [ADO], command buttons
- RDS scenarios [ADO], command buttons
ms.assetid: 80676831-6488-4dad-a558-c47c52256a22
caps.latest.revision: 16
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 5db9a77104bab65e12b54d860ddcb49ced321f95
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="address-book-command-buttons"></a>Boutons de commande de carnet d’adresses
L’application de carnet d’adresses comprend les boutons de commande suivants :  
  
-   A **trouver** pour envoyer une requête à la base de données.  
  
-   A **effacer** bouton pour effacer les zones de texte avant de démarrer une nouvelle recherche.  
  
-   Un **mise à jour de profil** bouton pour enregistrer les modifications apportées à un enregistrement d’employé.  
  
-   A **annuler les modifications** bouton pour ignorer les modifications.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
## <a name="find-button"></a>Bouton Rechercher  
 En cliquant sur le **trouver** bouton Active la procédure Sub « Find_OnClick » VBScript qui génère et envoie la requête SQL. Ce bouton remplit la grille de données.  
  
## <a name="building-the-sql-query"></a>Création de la requête SQL  
 La première partie de la procédure Sub « Find_OnClick » génère la requête SQL, une phrase à la fois, en ajoutant des chaînes de texte à une instruction SQL SELECT globale. Il commence en définissant la variable `myQuery` à une instruction SQL SELECT qui demande de toutes les lignes de données à partir de la table de source de données. Ensuite, la procédure Sub analyse chacune des quatre zones d’entrée sur la page.  
  
 Étant donné que le programme utilise le mot `like` dans la création des instructions SQL, les requêtes sont des recherches de sous-chaînes plutôt que des correspondances exactes.  
  
 Par exemple, si le **nom** zone contenait l’entrée « Berge » et le **titre** zone contenait l’entrée « Programme Manager », l’instruction SQL (valeur de `myQuery`) serait :  
  
```  
Select FirstName, LastName, Title, Email, Building, Room, Phone from Employee where lastname like 'Berge%' and title like 'Program Manager%'  
```  
  
 Si la requête a réussi, toutes les personnes avec un nom contenant le texte « Berge » (par exemple Berge ou Berger) et avec un titre qui contient les mots « Program Manager » (par exemple, responsable de programme, les Technologies avancées) sont affichent dans la grille de données HTML.  
  
## <a name="preparing-and-sending-the-query"></a>Préparation et envoi de la requête  
 La dernière partie de la procédure Sub « Find_OnClick » se compose de deux instructions. La première instruction affecte le [SQL](../../../ado/reference/rds-api/sql-property.md) propriété de la [RDS. DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md) à l’objet. La deuxième instruction provoque le **RDS. DataControl** objet (`DC1`) pour interroger la base de données et afficher les nouveaux résultats de la requête dans la grille.  
  
```  
Sub Find_OnClick  
   '...  
   DC1.SQL = myQuery  
   DC1.Refresh  
End Sub  
```  
  
## <a name="update-profile-button"></a>Bouton de mise à jour le profil  
 En cliquant sur le **mise à jour de profil** bouton Active la procédure Sub Update_OnClick de VBScript qui exécute le [RDS. DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md) l’objet (`DC1`) [SubmitChanges](../../../ado/reference/rds-api/submitchanges-method-rds.md) et [Actualiser](../../../ado/reference/rds-api/refresh-method-rds.md) méthodes.  
  
```  
Sub Update_OnClick  
   DC1.SubmitChanges  
   DC1.Refresh  
End Sub  
```  
  
 Lorsque `DC1.SubmitChanges` s’exécute, le Service de données distant toutes les informations de mise à jour des packages et l’envoie au serveur via HTTP. La mise à jour est tout ou rien ; Si une partie de la mise à jour échoue, aucune des modifications est effectuée et un message d’état est retourné. `DC1.Refresh` n’est pas nécessaire après **SubmitChanges** avec le Service de données distant, mais il garantit des données actualisées.  
  
## <a name="cancel-changes-button"></a>Modifications bouton Annuler  
 En cliquant sur **annuler les modifications** Active la procédure Sub Cancel_OnClick de VBScript qui exécute le [RDS. DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md) l’objet (`DC1)` [CancelUpdate](../../../ado/reference/rds-api/cancelupdate-method-rds.md) (méthode).  
  
```  
Sub Cancel_OnClick  
   DC1.CancelUpdate  
End Sub  
```  
  
 Lorsque `DC1.CancelUpdate` s’exécute, il ignore toutes les modifications par un utilisateur à un enregistrement d’employé dans la grille de données depuis la dernière requête ou mise à jour. Il restaure les valeurs d’origine.  
  
## <a name="see-also"></a>Voir aussi  
 [Boutons de Navigation de carnet d’adresses](../../../ado/guide/remote-data-service/address-book-navigation-buttons.md)   
 [DataControl, objet (RDS)](../../../ado/reference/rds-api/datacontrol-object-rds.md)


