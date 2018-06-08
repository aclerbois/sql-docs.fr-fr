---
title: Microsoft ActiveX Data Objects (ADO) | Documents Microsoft
ms.custom: ''
ms.date: 07/25/2017
ms.reviewer: ''
ms.suite: sql
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- ADO, about
ms.assetid: 2fa6237b-44b8-4b6c-9952-5acd80a54e20
caps.latest.revision: 20
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 40e0ba7ff0b05a8f1faa4d0b62893f51145c81c3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="microsoft-activex-data-objects-ado"></a>Microsoft ActiveX Data Objects (ADO)

ADO est utilisé dans les programmes C++ pour vous connecter à SQL Server. Bien entendu, il fonctionne également pour vous connecter à la base de données SQL Azure dans le cloud.

Chaque section de cet article décrit un composant de l’objet ADO.

> [!NOTE]
> ADO.NET est différente de celle ADO. ADO.NET et de nombreux autres pilotes de connexion SQL et leurs langues, sont présentées en commençant à [pilotes SQL Server](../connect/sql-connection-libraries.md).

  
## <a name="ado"></a>ADO  
 Microsoft ActiveX Data Objects (ADO) permettent à vos applications clientes d’accéder et de manipuler des données à partir de diverses sources via un fournisseur OLE DB. Ses avantages principaux sont la simplicité d’utilisation, haute vitesse, une charge de mémoire faible et une encombrement disque limité. ADO prend en charge les fonctionnalités clés pour la création d’applications Web et client/serveur.  
  
## <a name="ado-md"></a>ADO MD  
 Microsoft ActiveX Data Objects (multidimensionnel) (ADO MD) fournit un accès facile aux données multidimensionnelles à partir de langages tels que Microsoft Visual Basic et Microsoft Visual C++. ADO MD étend Microsoft ActiveX Data Objects (ADO) pour inclure des objets spécifiques aux données multidimensionnelles, tels que les objets CubeDef et Cellset. Avec ADO MD, vous pouvez parcourir un schéma multidimensionnel, un cube de requête et récupérer les résultats.  
  
 Comme ADO, ADO MD utilise un fournisseur OLE DB sous-jacent pour accéder aux données. Pour utiliser ADO MD, le fournisseur doit être un fournisseur de données multidimensionnelles (MDP) tel que défini par la spécification OLE DB pour OLAP. Fournisseurs MDP présente des données dans des vues multidimensionnelles par opposition aux fournisseurs de données tabulaires (TDP) qui présentent les données sous forme de tableaux. Consultez la documentation de votre fournisseur OLE DB pour OLAP pour des informations plus détaillées sur la syntaxe spécifique et les comportements pris en charge par votre fournisseur.  
  
## <a name="rds"></a>SERVICES BUREAU À DISTANCE  
 Service de données à distance (RDS) est une fonctionnalité de l’objet ADO, avec lequel vous pouvez déplacer des données à partir d’un serveur vers une application cliente ou d’une page Web, manipuler les données sur le client et renvoyer les mises à jour sur le serveur dans un seul aller-retour.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
## <a name="adox"></a>ADOX  
 Microsoft ActiveX Data Objects Extensions pour le langage de définition de données et de sécurité (ADOX) est une extension pour les objets ADO et le modèle de programmation. ADOX inclut des objets pour la création de schémas et de modification, ainsi que de sécurité. S’agissant d’une approche basée sur l’objet de manipulation de schéma, vous pouvez écrire du code qui fonctionne sur des données de différentes sources, quelle que soit les différences dans les syntaxes natives.  
  
 ADOX est une bibliothèque d’accompagnement pour les objets ADO de base. Il expose des objets supplémentaires pour la création, modification et suppression d’objets de schéma, tels que des tables et des procédures. Il inclut également des objets de sécurité pour gérer les utilisateurs et groupes et pour accorder et révoquer des autorisations sur les objets.  
  
## <a name="documentation"></a>Documentation  
 [Problèmes de conception de la sécurité d’ADO](../ado/guide/ado-security-design-issues.md)  
  
 [Guide du programmeur ADO](../ado/guide/ado-programmer-s-guide.md)  
  
 Introduction à l’aide d’ADO, RDS, ADO MD et ADOX.  
  
 [Guide de référence du programmeur ADO](../ado/reference/ado-programmer-s-reference.md)  
  
 Cette section de la documentation ADO contient des rubriques pour chaque ADO, RDS, ADO MD, ADOX objet, collection, propriété, propriété dynamique, méthode, événement et énumération.  
  
 [Glossaire ADO](../ado/ado-glossary.md)  
  
## <a name="support"></a>Support technique  
 Gratuitement aide avec ADO problèmes, essayez de validation dans le groupe de discussion public ADO. Ce groupe de discussion est analysée par les techniciens du support technique Microsoft Product Support Services (PSS) qui couvrent ADO et par d’autres développeurs expérimentés de ADO.  
  
 Vous trouverez plus d’informations sur les options de prise en charge sur le site Web de prendre en charge et de Microsoft Help.


