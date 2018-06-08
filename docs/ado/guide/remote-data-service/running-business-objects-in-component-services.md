---
title: Exécution d’objets métier dans les Services de composants | Documents Microsoft
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
- component services in RDS [ADO]
ms.assetid: 3077d0b6-42d6-4f10-8e5d-42e6204f1109
caps.latest.revision: 17
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 491f6554be66926469f940cd83c12a5469ae3073
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="running-business-objects-in-component-services"></a>Exécution d’objets métier dans les Services de composants
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
 Objets métier peuvent être des fichiers exécutables (.exe) ou des bibliothèques de liens dynamiques (.dll). La configuration que vous utilisez pour exécuter l’objet métier varie selon que l’objet est un fichier .dll ou .exe :  
  
-   Les objets métier créés en tant que fichiers .exe peuvent être appelés via DCOM. Si ces objets métier sont utilisés par le biais d’Internet Information Services (IIS), ils sont soumis à plus de marshaling de données, ce qui ralentissent les performances du client.  
  
-   Les objets métier créés comme fichiers .dll peuvent être utilisées via IIS et donc également par HTTP. Ils peuvent également être utilisés via DCOM uniquement via les Services de composants, ou par Microsoft Transaction Server, si vous utilisez Windows NT. DLL de l’objet métier seront doivent être enregistrés sur l’ordinateur du serveur IIS pour y accéder via IIS. Pour plus d’informations sur la configuration d’une DLL pour s’exécuter sur DCOM, consultez la section [l’activation d’une DLL pour être exécuté sur DCOM](../../../ado/guide/remote-data-service/enabling-a-dll-to-run-on-dcom.md).  
  
> [!NOTE]
>  Lorsque des objets métier sur la couche intermédiaire sont implémentés en tant que composants des Services de composants à l’aide de **GetObjectContext**, **SetComplete**, et **SetAbort**, l’entreprise les objets peuvent utiliser les Services de composants (ou MTS, si vous utilisez Windows NT) des objets de contexte pour conserver leur état entre plusieurs appels clients. Ce scénario est possible avec DCOM, qui est généralement implémentée entre les clients approuvés et les serveurs intranet. Dans ce cas, le [RDS. DataSpace](../../../ado/reference/rds-api/dataspace-object-rds.md) objet et [CreateObject](../../../ado/reference/rds-api/createobject-method-rds.md) méthode côté client sont remplacés par l’objet de contexte de transaction et **CreateInstance** (méthode), qui sont fournies par le **ITransactionContext** interface et implémentée par les Services de composants.  
  
## <a name="see-also"></a>Voir aussi  
 [Principes de base de RDS](../../../ado/guide/remote-data-service/rds-fundamentals.md)


