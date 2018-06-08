---
title: Supprimer et recréer des clés de chiffrement (Gestionnaire de configuration de SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 05/31/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: install-windows
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- re-creating encryption keys
- encryption keys [Reporting Services]
- deleting encryption keys
- symmetric keys [Reporting Services]
- removing encryption keys
- resetting encryption keys
ms.assetid: 201afe5f-acc9-4a37-b5ec-121dc7df2a61
caps.latest.revision: 9
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 8f179b9474d715ad0e94cb0261af35c4829f947c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ssrs-encryption-keys---delete-and-re-create-encryption-keys"></a>Clés de chiffrement SSRS - Supprimer et recréer des clés de chiffrement
  La suppression et la recréation de clés de chiffrement sont des activités qui dépassent le cadre d'une simple opération de maintenance des clés de chiffrement. Vous effectuez ces tâches en réponse à une menace spécifique pesant sur votre serveur de rapports ou comme ultime recours si vous ne pouvez plus accéder à la base de données du serveur de rapports.  
  
-   Recréez la clé symétrique lorsque vous estimez que la sécurité de la clé existante est compromise. Vous pouvez effectuer cette opération régulièrement en tant que mesure de sécurité.  
  
-   Supprimez les clés de chiffrement existantes et le contenu chiffré inutilisable lorsqu'il vous est impossible de restaurer la clé symétrique.  
  
## <a name="re-creating-encryption-keys"></a>recréation des clés de chiffrement  
 Si vous avez la preuve que la clé symétrique est connue d'utilisateurs non autorisés ou si votre serveur de rapports a subi une attaque et que vous voulez, par mesure de précaution, réinitialiser la clé symétrique, procédez à une nouvelle création de clé. Cette opération implique un nouveau chiffrement de toutes les valeurs chiffrées à l'aide de la nouvelle valeur. Si vous exécutez plusieurs serveurs de rapports dans un déploiement évolutif, tous les exemplaires de la clé symétrique seront mis à jour avec cette nouvelle valeur. Le serveur de rapports utilise les clés publiques disponibles pour cette clé symétrique qu'il met à jour pour chaque serveur dans le déploiement.  
  
 La création d'une nouvelle clé symétrique n'est possible que si le serveur de rapports est en cours de fonctionnement. Cette opération, ainsi que le nouveau chiffrement de contenu, peuvent perturber les activités du serveur. Vous devez mettre le serveur en mode hors connexion tandis que vous procédez au nouveau chiffrement. Aucune demande ne doit être adressée au serveur de rapports durant cette opération.  
  
 Vous pouvez utiliser l’outil de configuration de Reporting Services ou l’utilitaire **rskeymgmt** pour réinitialiser la clé symétrique et les données chiffrées. Pour plus d’informations sur la création de la clé symétrique, consultez [Initialiser un serveur de rapports &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-initialize-a-report-server.md).  
  
### <a name="how-to-re-create-encryption-keys-reporting-services-configuration-tool"></a>Procédure de recréation des clés de chiffrement (outil de configuration de Reporting Services)  
  
1.  Désactivez le service web Report Server et l’accès HTTP en modifiant la propriété **IsWebServiceEnabled** dans le fichier rsreportserver.config. Cette étape arrête temporairement l'envoi des demandes d'authentification au serveur de rapports sans arrêter complètement le serveur. Vous devez avoir le service minimal afin de pouvoir recréer les clés.  
  
     Si vous recréez des clés de chiffrement pour un déploiement évolutif de serveurs de rapports, désactivez cette propriété sur toutes les instances du déploiement.  
  
    1.  Ouvrez l’Explorateur Windows et naviguez jusqu’à *lecteur*:\Program Files\Microsoft SQL Server\\*instance_serveur_rapports*\Reporting Services. Remplacez *lecteur* par votre lettre de lecteur et *instance_serveur_rapports* par le nom du dossier qui correspond à l’instance de serveur de rapports pour laquelle vous souhaitez désactiver le service web et l’accès HTTP. Par exemple, C:\Program Files\Microsoft SQL Server\MSRS10_50.MSSQLSERVER\Reporting Services.  
  
    2.  Ouvrez le fichier rsreportserver.config.  
  
    3.  Pour la propriété **IsWebServiceEnabled** , spécifiez **False**, puis enregistrez vos modifications.  
  
2.  Démarrez l'outil de configuration de Reporting Services et connectez-vous à l'instance de serveur de rapports à configurer.  
  
3.  Dans la page Clés de chiffrement, cliquez sur **Modifier**. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
4.  Redémarrez le service Windows Report Server. Si vous recréez des clés de chiffrement pour un déploiement évolutif, redémarrez le service sur toutes les instances.  
  
5.  Réactivez le service web et l’accès HTTP en modifiant la propriété **IsWebServiceEnabled** dans le fichier rsreportserver.config. Effectuez cette action pour toutes les instances si vous utilisez un déploiement évolutif.  
  
### <a name="how-to-re-create-encryption-keys-rskeymgmt"></a>Procédure de recréation des clés de chiffrement (rskeymgmt)  
  
1.  Désactivez le service Web Report Server et l'accès HTTP. Suivez les instructions de la procédure précédente pour interrompre les opérations du service Web.  
  
2.  Exécutez le fichier **rskeymgmt.exe** localement sur l'ordinateur qui héberge le serveur de rapports. Utilisez l’argument **-s** pour réinitialiser la clé symétrique. Aucun autre argument n'est obligatoire :  
  
    ```  
    rskeymgmt -s  
    ```  
  
3.  Redémarrez le service Windows Reporting Services.  
  
## <a name="deleting-unusable-encrypted-content"></a>Suppression du contenu chiffré inutilisable  
 Si, pour une raison ou pour une autre, vous ne pouvez pas restaurer la clé de chiffrement, le serveur de rapports sera dans l'incapacité totale de déchiffrer les données chiffrées avec cette clé pour les utiliser. Pour que le serveur de rapports puisse fonctionner de nouveau, vous devez supprimer les valeurs chiffrées qui sont actuellement stockées dans la base de données du serveur de rapports et redéfinir manuellement les valeurs dont vous avez besoin.  
  
 La suppression des clés de chiffrement efface toutes les informations de clé symétrique de la base de données du serveur de rapports et supprime tout contenu chiffré. Les données non chiffrées sont intactes, seul le contenu chiffré est supprimé. Lorsque vous détruisez les clés de chiffrement, le serveur de rapports se réinitialise automatiquement en ajoutant une nouvelle clé symétrique. Les événements suivants se produisent lorsque vous supprimez des données chiffrées :  
  
-   Les chaînes de connexion des sources de données partagées sont supprimées. Les utilisateurs qui exécutent des rapports reçoivent le message d'erreur suivant : « La propriété ConnectionString n'a pas été initialisée ».  
  
-   Les informations d'identification sont supprimées. Les rapports et les sources de données partagées sont reconfigurées pour utiliser les informations d'identification qui sont demandées.  
  
-   Les rapports basés sur des modèles (et qui font appel à des sources de données partagées configurées à l'aide d'informations d'identification stockées ou non) ne s'exécutent plus.  
  
-   Les abonnements sont désactivés.  
  
 Le contenu chiffré qui est supprimé est irrémédiablement perdu. Vous devez spécifier à nouveau des chaînes de connexion et des informations d'identification stockées, puis activer les abonnements.  
  
 Vous pouvez utiliser l’outil de configuration de Reporting Services ou l’utilitaire **rskeymgmt** pour supprimer ces valeurs.  
  
### <a name="how-to-delete-encryption-keys-reporting-services-configuration-tool"></a>Procédure de suppression des clés de chiffrement (outil de configuration de Reporting Services)  
  
1.  Démarrez l'outil de configuration de Reporting Services et connectez-vous à l'instance de serveur de rapports à configurer.  
  
2.  Cliquez sur **Clés de chiffrement**, puis sur **Supprimer**. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
3.  Redémarrez le service Windows Report Server. Pour un déploiement évolutif, effectuez cette opération sur toutes les instances de serveurs de rapports.  
  
### <a name="how-to-delete-encryption-keys-rskeymmgt"></a>Procédure de suppression des clés de chiffrement (rskeymmgt)  
  
1.  Exécutez le fichier **rskeymgmt.exe** localement sur l'ordinateur qui héberge le serveur de rapports. Vous devez utiliser l’argument d’application **-d** . L'exemple suivant illustre l'argument que vous devez spécifier :  
  
    ```  
    rskeymgmt -d  
    ```  
  
2.  Redémarrez le service Windows Report Server. Pour un déploiement évolutif, effectuez cette opération sur toutes les instances de serveurs de rapports.  
  
### <a name="how-to-re-specify-encrypted-values"></a>Procédure de redéfinition des valeurs chiffrées  
  
1.  Indiquez de nouveau la chaîne de connexion de chaque source de données partagée.  
  
2.  Pour chaque rapport et chaque source de données partagée utilisant des informations d'identification stockées, vous devez retaper le nom d'utilisateur et le mot de passe, puis les enregistrer. Pour plus d’informations, consultez [Spécifier des informations d’identification et de connexion pour les sources de données de rapport](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md) dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
3.  Pour les abonnements pilotés par les données, ouvrez chaque abonnement et retapez les informations d'identification dans la base de données d'abonnement.  
  
4.  Pour les abonnements utilisant des données chiffrées (ce qui comprend l'extension de remise dans le partage de fichiers et toute extension de remise créées par d'autres développeurs qui utilisent le chiffrement), ouvrez chaque abonnement et retapez les informations d'identification. Les abonnements utilisant la remise par messagerie électronique du serveur de rapports n'ont pas recours aux données chiffrées, ils ne sont donc pas concernés par les modifications apportées à la clé.  
  
## <a name="see-also"></a> Voir aussi  
 [Configurer et gérer des clés de chiffrement &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-manage-encryption-keys.md)   
 [Stocker des données chiffrées du serveur de rapports &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-store-encrypted-report-server-data.md)  
  
  
