---
title: Configuration de DataFactory sécurisé ou non restreint | Documents Microsoft
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
- DataFactory configuration in RDS [ADO]
ms.assetid: 8ff24805-dc7a-42ae-b600-5bad0e3f51b8
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: e3af0bf538b8d2fb774b06644e8089cf201b5fc6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configuring-datafactory-for-safe-or-unrestricted-modes"></a>Configuration de DataFactory sécurisé ou non restreint
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
 ADO est installé par défaut, avec un « sécurisé » [RDSServer.DataFactory](../../../ado/reference/rds-api/datafactory-object-rdsserver.md) configuration. Le mode sans échec des composants serveur des services Bureau à distance signifie que les conditions suivantes sont remplies :  
  
1.  Gestionnaire est requis avec l’objet RDSServer.DataFactory (imposé par un paramètre de clé de Registre).  
  
2.  Le gestionnaire par défaut, msdfmap.handler, est enregistré, présent dans la liste de gestionnaires sécurisés et marquée en tant que gestionnaire par défaut.  
  
3.  Fichier Msdfmap.ini est installé dans le répertoire Windows. Vous devez configurer ce fichier selon vos besoins, avant d’utiliser des services Bureau à distance en mode trois niveaux.  
  
 Si vous le souhaitez, vous pouvez configurer un illimitée **DataFactory** installation. **DataFactory** peut être utilisé directement sans le gestionnaire personnalisé. Les utilisateurs peuvent toujours utiliser un gestionnaire personnalisé en modifiant les chaînes de connexion, mais il n’est pas requis. Pour plus d’informations sur les conséquences de l’utilisation de la **RDSServer.DataFactory** d’objets, consultez [sécurisation des Applications RDS](../../../ado/guide/remote-data-service/securing-rds-applications.md).  
  
 Le handsafe.reg de fichier de Registre a été fourni pour configurer les entrées de Registre de gestionnaire pour une configuration sécurisée. Pour exécuter en mode sans échec, exécutez handsafe.reg.  
  
 Après l’exécution de handsafe.reg, vous devez arrêter et redémarrer le Service de publication World Wide Web sur le serveur Web en tapant les commandes suivantes dans une fenêtre d’invite de commandes : « NET STOP W3SVC » et « NET démarrer W3SVC ».  
  
## <a name="see-also"></a>Voir aussi  
 [DataFactory, personnalisation](../../../ado/guide/remote-data-service/datafactory-customization.md)   
 [Principes de base de RDS](../../../ado/guide/remote-data-service/rds-fundamentals.md)



