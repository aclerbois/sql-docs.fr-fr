---
title: Configuration des services Bureau à distance sur Windows 2000 | Documents Microsoft
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
- RDS configuration [ADO], Windows 2000
ms.assetid: ef37e858-c05f-4f52-a65f-3ce6037e0d03
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 1efc5f21b829e85e063dbbd3dbb3d72131d43149
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configuring-rds-on-windows-2000"></a>Configuration des services Bureau à distance sur Windows 2000
Si vous rencontrez des difficultés dans la mise en route des services Bureau à distance pour fonctionner correctement une fois que vous mettez à niveau vers Windows 2000, procédez comme suit pour résoudre le problème :  
  
1.  Assurez-vous que le premier que le Service de publication World Wide Web est en cours d’exécution en accédant à http:// serveur à l’aide d’Internet Explorer. Si vous ne pouvez pas accéder au serveur de Web de cette manière, ouvrez une invite de commandes et entrez la commande suivante, « NET démarrer W3SVC ».  
  
2.  Dans le menu Démarrer, sélectionnez Exécuter. Tapez msdfmap.ini et puis cliquez sur OK pour ouvrir le fichier msdfmap.ini dans le bloc-notes. Consultez la section [CONNECT DEFAULT] et si le paramètre d’accès a la valeur NOACCESS, modifiez-la en lecture seule.  
  
3.  À l’aide de l’utilitaire RegEdit, accédez à « HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\DataFactory\HandlerInfo » et assurez-vous que **HandlerRequired** est définie sur 0 et **DefaultHandler** est « » (chaîne vide).  
  
     **Remarque** si vous apportez des modifications à cette section du Registre, vous devez arrêter et redémarrer le Service de publication World Wide Web en entrant les commandes suivantes à une invite de commandes : « NET STOP W3SVC » et « NET démarrer W3SVC ».  
  
4.  À l’aide de l’utilitaire RegEdit, accédez à « HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC\Parameters\ADCLaunch » dans le Registre et vérifiez qu’il existe une clé nommée **RDSServer.Datafactory**. Si ce n’est pas le cas, créez-le.  
  
5.  À l’aide du Gestionnaire des Services Internet, localisez le site Web par défaut et afficher les propriétés de la racine virtuelle MSADC. Vérifiez que le répertoire sécurité / l’adresse IP et les Restrictions de nom de domaine. Si le « accès refusé » est cochée, puis sélectionnez « Granted ».  
  
 Veillez à essayer de redémarrer le serveur si les modifications n’apparaissent ne pas à résoudre le problème dans un premier temps.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565). À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows. Migrer des applications qui utilisent des services Bureau à distance à [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
## <a name="see-also"></a>Voir aussi  
 [Principes de base de RDS](../../../ado/guide/remote-data-service/rds-fundamentals.md)


