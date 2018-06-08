---
title: Objets métier reconnus sûrs pour l’écriture de scripts | Documents Microsoft
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
- business objects in RDS [ADO]
ms.assetid: 0be98d1a-ab3d-4dce-a166-dacda10d154a
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 821f52e440e30f076a25104da8d5c928ff744efb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="marking-business-objects-as-safe-for-scripting"></a>Objets métier reconnus sûrs pour l’écriture de scripts
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
 Pour aider à sécuriser un environnement Internet, vous devez marquer les objets métier instanciés avec la [RDS. DataSpace](../../../ado/reference/rds-api/dataspace-object-rds.md) l’objet [CreateObject](../../../ado/reference/rds-api/createobject-method-rds.md) méthode comme étant « sécurisé pour le script ». Vous devez vous assurer qu’ils sont marqués comme tels dans la zone License du Registre système avant de pouvoir être utilisés dans DCOM.  
  
> [!NOTE]
>  La mention « sécurisé pour le script » ou l’initialisation des objets métier peuvent instanciés et initialisés par toute personne sur le réseau. Marque un objet métier « sécurisé pour le script » ne rend pas celui-ci sans échec. Il est essentiel de s’assurer que les objets métier sont codées avec la sécurité la plus élevée pour vous assurer que ces objets ne représentent pas un point d’accès non protégé pour les données sensibles.  
  
 Pour configurer manuellement votre objet métier comme sécurisés pour les scripts, créez un fichier texte avec une extension .reg qui contient le texte suivant. Dans cet exemple, \< *MyActiveXGUID*> est le numéro GUID hexadécimal de votre objet métier. Les deux numéros suivants activer la fonctionnalité sécurisé pour le script :  
  
```  
[HKEY_CLASSES_ROOT\CLSID\<MyActiveXGUID>\Implemented   
Categories\{7DD95801-9882-11CF-9FA9-00AA006C42C4}]  
[HKEY_CLASSES_ROOT\CLSID\<MyActiveXGUID>\Implemented   
Categories\{7DD95802-9882-11CF-9FA9-00AA006C42C4}]  
```  
  
 Enregistrez le fichier et fusionner dans le Registre à l’aide de l’Éditeur du Registre ou en double-cliquant sur le fichier .reg dans l’Explorateur Windows.  
  
 Les objets métier créés dans Microsoft Visual Basic peuvent être automatiquement marqués comme « sécurisé pour le script » avec l’Assistant de déploiement et le Package. Lorsque l’Assistant vous invite à spécifier les paramètres de sécurité, sélectionnez **pour l’initialisation** et **sécurisé pour le script**.  
  
 Sur la dernière étape, l’Assistant crée un .htm et un fichier .cab. Vous pouvez ensuite copier ces deux fichiers à l’ordinateur cible et double-cliquez sur le fichier .htm pour charger la page et enregistrer correctement le serveur.  
  
 Par défaut, l’objet métier sera installé dans le répertoire Windows\System32\Occache déplacez-le vers le répertoire Windows\System32 et modifier le **HKEY_CLASSES_ROOT\CLSID\\**  \< *MyActiveXGUID*>\\**InprocServer32** clé de Registre pour faire correspondre le chemin d’accès correct.


