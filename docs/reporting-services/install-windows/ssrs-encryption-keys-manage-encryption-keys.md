---
title: Configurer et gérer des clés de chiffrement (Gestionnaire de configuration de SSRS) | Microsoft Docs
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
- encryption keys [Reporting Services]
- private keys [Reporting Services]
- cryptography [Reporting Services]
- symmetric keys [Reporting Services]
- encryption [Reporting Services]
- public keys [Reporting Services]
ms.assetid: 58e61636-88a2-4338-ae5f-3dd210aee887
caps.latest.revision: 8
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 581abfca863baaa024d0c0b74eea1b6521357824
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ssrs-encryption-keys---manage-encryption-keys"></a>Clés de chiffrement SSRS - Gérer les clés de chiffrement
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] utilise des clés de chiffrement pour protéger les informations de connexion et d’identification stockées dans la base de données du serveur de rapports. Dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], le chiffrement est pris en charge par la combinaison d'une clé publique, d'une clé privée et d'une clé symétrique, dont le but est de protéger les données sensibles. La clé symétrique est créée au cours de l'initialisation du serveur de rapports lorsque vous installez ou configurez le serveur de rapports. Elle est utilisée par le serveur de rapports pour chiffrer les données sensibles stockées sur le serveur de rapports. Les clés publiques et privées sont créées par le système d'exploitation et servent à protéger la clé symétrique. Une paire de clés privée et publique est créée pour chaque instance du serveur de rapports qui stocke des données sensibles dans une base de données de serveur de rapports.  
  
 La gestion des clés de chiffrement consiste à créer une copie de sauvegarde de la clé symétrique et à savoir quand et comment restaurer, supprimer ou modifier les clés. Si vous migrez une installation de serveur de rapports ou que vous configurez un déploiement avec montée en puissance parallèle, vous devez disposer d'une copie de sauvegarde de la clé symétrique pour pouvoir l'appliquer à la nouvelle installation.  
  
> [!IMPORTANT]  
>  Par mesure de sécurité, il est recommandé de modifier régulièrement la clé de chiffrement Reporting Services. Il est recommandé de modifier la clé juste après la mise à niveau d'une version principale de Reporting Services. La modification de la clé après une mise à niveau réduit l'interruption de service provoquée par la modification de la clé de chiffrement Reporting Services hors du cycle de mise à niveau.  
  
 Pour gérer les clés symétriques, utilisez l’outil de configuration de Reporting Services ou l’utilitaire **rskeymgmt** . Les outils fournis dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] servent à la gestion de la clé symétrique uniquement (les clés publique et privée sont gérées par le système d'exploitation). L’outil de configuration de Reporting Services et l’utilitaire **rskeymgmt** prennent en charge les tâches suivantes :  
  
-   Sauvegarde d'un exemplaire de la clé symétrique pour permettre la récupération de l'installation d'un serveur de rapports ou pour servir dans le processus d'une migration planifiée.  
  
-   Restauration dans une base de données de serveur de rapports d'une clé symétrique préalablement enregistrée afin de permettre à une nouvelle instance de serveur de rapports d'accéder à des données existantes qu'elle n'a pas chiffrées au départ.  
  
-   Suppression des données chiffrées d'une base de données de serveur de rapports au cas improbable où l'accès à ces données chiffrées se révélerait impossible.  
  
-   Recréation de clés symétriques et nouveau chiffrement des données au cas improbable où la sécurité de la clé symétrique serait compromise. La création périodique d'une nouvelle clé symétrique (tous les deux ou trois mois par exemple) est préconisée en tant que mesure de sécurité pour protéger la base de données d'un serveur de rapports contre les cyber-attaques qui tentent de déchiffrer la clé.  
  
-   Ajout ou suppression d'une instance de serveur de rapports dans un déploiement avec montée en puissance parallèle où plusieurs serveurs de rapports partagent une base de données du serveur de rapports unique ainsi que la clé symétrique assurant le chiffrement réversible de cette dernière.  
  
## <a name="in-this-section"></a>Dans cette section  
 [Initialiser un serveur de rapports &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-initialize-a-report-server.md)  
 Explique la procédure de création des clés de chiffrement.  
  
 [Sauvegarder et restaurer les clés de chiffrement Reporting Services](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md)  
 Explique la procédure de sauvegarde des clés de chiffrement et la façon de restaurer ces clés afin de récupérer l'installation d'un serveur de rapports ou de procéder à sa migration.  
  
 [Stocker des données chiffrées du serveur de rapports &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-store-encrypted-report-server-data.md)  
 Décrit le chiffrement sur un serveur de rapports.  
  
 [Supprimer et recréer des clés de chiffrement &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-delete-and-re-create-encryption-keys.md)  
 Explique comment remplacer une clé symétrique par une nouvelle version de cette dernière et comment recommencer l'opération de chiffrement depuis le début, si la validation des clés symétriques est impossible.  
  
 [Ajouter et supprimer des clés de chiffrement pour un déploiement évolutif &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/add-and-remove-encryption-keys-for-scale-out-deployment.md)  
 Explique la procédure de création et d'ajout des clés de chiffrement pour contrôler l'intégration des serveurs de rapports à un déploiement évolutif.  
  
## <a name="see-also"></a> Voir aussi  
[Gestionnaire de configuration de Reporting Services (mode natif)](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)
