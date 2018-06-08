---
title: Configuration d’une DLL pour être exécuté sur DCOM | Documents Microsoft
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
- DLL on DCOM in RDS [ADO]
- DCOM in RDS [ADO]
- business objects in RDS [ADO]
ms.assetid: 5f1c2205-191c-4fb4-9bd9-84c878ea46ed
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 16c2fdf5ab5d06b032b87115a38c115e86dcf830
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="enabling-a-dll-to-run-on-dcom"></a>Configuration d’une DLL pour s’exécuter sur DCOM
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
 Les étapes suivantes décrivent comment activer un .dll d’objet métier à utiliser DCOM et Microsoft Internet Information Services (protocole HTTP) via les Services de composants.  
  
1.  Créer un nouveau package vide dans le composant logiciel enfichable MMC Services de composants.  
  
     Vous allez utiliser le composant logiciel enfichable MMC Services de composants pour créer un package et ajouter la DLL dans ce package. Cela rend le fichier .dll accessible via DCOM, mais il supprime l’accessibilité via IIS. (Si vous recherchez dans le Registre pour le fichier .dll, le **Inproc** clé est maintenant vide ; définissant l’attribut d’Activation, expliqué plus loin dans cette rubrique, ajoute une valeur dans la **Inproc** clé.)  
  
2.  Installez un objet métier dans le package.  
  
     -ou-  
  
     Importer le [RDSServer.DataFactory](../../../ado/reference/rds-api/datafactory-object-rdsserver.md) objet dans le package.  
  
3.  Définissez l’attribut d’Activation pour le package à **dans le processus du créateur** (application bibliothèque).  
  
     Pour rendre le fichier .dll accessible via DCOM et IIS sur le même ordinateur, vous devez définir l’attribut d’Activation du composant dans le composant logiciel enfichable MMC Services de composants. Une fois que vous affectez à l’attribut **dans le processus du créateur**, vous remarquerez qu’une **Inproc** clé dans le Registre du serveur a été ajouté pointant vers un composant des Services de substitution .dll.  
  
 Pour plus d’informations sur les Services de composants (ou Service Microsoft Transaction, si vous utilisez Windows NT) et comment effectuer ces étapes, visitez le site Web de Microsoft Transaction Server.


