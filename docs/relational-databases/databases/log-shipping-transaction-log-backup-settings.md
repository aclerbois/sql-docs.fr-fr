---
title: Paramètres de sauvegarde des journaux de transaction | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: log-shipping
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.databaseproperties.logshipping.settings.tlogback.f1
ms.assetid: 9a6e6c16-7f71-412b-bba6-7bffac001277
caps.latest.revision: 27
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 574677284e2614f665fb1cf663d78b898dcdafe2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="log-shipping-transaction-log-backup-settings"></a>Paramètres de sauvegarde des journaux de transactions
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Utilisez cette boîte de dialogue pour configurer et modifier les paramètres de sauvegarde des journaux de transactions pour une configuration spécifique de la copie des journaux de transactions.  
  
 Pour obtenir des explications sur les concepts de copie des journaux de transaction, consultez [À propos de la copie des journaux de transaction &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md).  
  
## <a name="options"></a>Options  
 **Chemin d'accès réseau au dossier de sauvegarde**  
 Tapez dans cette zone le partage réseau de votre dossier de sauvegarde. Le dossier local où sont enregistrées vos sauvegardes de journaux de transactions doit être partagé afin que les travaux de copie des journaux de transactions puissent copier ces fichiers sur le serveur secondaire. Vous devez accorder des autorisations de lecture sur ce partage réseau au compte proxy à partir duquel le travail de copie sera exécuté sur l'instance du serveur secondaire. Par défaut, il s'agit du compte du service SQLServerAgent de l'instance du serveur secondaire, mais un administrateur peut choisir un autre compte proxy pour exécuter le travail.  
  
 **Si le dossier de sauvegarde se trouve sur le serveur principal, tapez un chemin d'accès local au dossier**  
 Tapez la lettre du lecteur local et le chemin d'accès du dossier de sauvegarde si le dossier de sauvegarde se trouve sur le serveur principal. Si le dossier de sauvegarde ne se trouve pas sur le serveur principal, vous pouvez laisser ce champ vide.  
  
 Si vous spécifiez ici un chemin local, la commande BACKUP utilisera ce chemin pour créer les sauvegardes des journaux de transactions. Par contre, si vous ne spécifiez aucun chemin local, la commande BACKUP utilisera le chemin réseau spécifié dans la zone **Chemin d’accès réseau au dossier de sauvegarde** .  
  
> [!NOTE]  
>  Si le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute sous le compte système local sur le serveur principal, vous devez créer le dossier de sauvegarde sur le serveur principal et spécifier ici le chemin d'accès local de ce dossier. Le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de l'instance du serveur principal doit disposer d'autorisations de lecture et d'écriture sur ce dossier.  
  
 **Supprimer les fichiers antérieurs à**  
 Spécifiez la durée pendant laquelle vous souhaitez que les sauvegardes des journaux de transactions restent dans votre répertoire de sauvegarde avant d'être supprimées.  
  
 **Envoyer une alerte si aucune sauvegarde ne se produit en l'espace de**  
 Spécifiez le délai après lequel vous voulez que la copie des journaux de transactions déclenche une alerte pour vous informer qu'aucune sauvegarde de journaux de transactions n'a été effectuée.  
  
 **Nom du travail**  
 Affiche le nom du travail de l'Agent SQL Server servant à créer les sauvegardes des journaux de transactions pour la copie des journaux de transactions. Lors de la création du travail, vous pouvez modifier son nom en tapant un nouveau nom dans cette zone.  
  
 **Planification**  
 Affiche la planification actuelle pour la sauvegarde des journaux de transactions de la base de données principale. Avant la création du travail de sauvegarde, vous pouvez modifier cette planification en cliquant sur **Planification...**. Une fois que le travail a été créé, vous pouvez modifier cette planification en cliquant sur **Modifier le travail...**.  
  
### <a name="backup-job"></a>Travail de sauvegarde  
 **Planification...**  
 Permet de modifier la planification qui est créée au moment de la création du travail de l'Agent SQL Server.  
  
 **Modifier le travail...**  
 Permet de modifier les paramètres du travail de l'Agent SQL Server pour le travail qui effectue les sauvegardes des journaux de transactions de la base de données principale.  
  
 **Désactiver ce travail**  
 Désactive le travail de l'Agent SQL Server et la création de sauvegardes de journaux de transactions.  
  
### <a name="compression"></a>Compression  
 [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] (ou une version ultérieure) prend en charge la [compression de sauvegarde](../../relational-databases/backup-restore/backup-compression-sql-server.md).  
  
 **Définir la compression de la sauvegarde**  
 Dans [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] (ou une version ultérieure), sélectionnez l'une des valeurs de compression de sauvegarde suivantes pour les sauvegardes de journaux de cette configuration de la copie des journaux de transaction :  
  
|||  
|-|-|  
|**Utiliser le paramètre du serveur par défaut**|Cliquez sur cette option pour utiliser la valeur par défaut au niveau du serveur.<br /><br /> Cette valeur par défaut est définie par l’option de configuration de serveur **Compression par défaut des sauvegardes** . Pour plus d’informations sur l’affichage du paramétrage actuel de cette option, consultez [Afficher ou configurer l’option de configuration du serveur valeur par défaut de compression de la sauvegarde](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md).|  
|**Compresser la sauvegarde**|Cliquez sur cette option pour compresser la sauvegarde, indépendamment de la valeur par défaut au niveau du serveur.<br /><br /> **\*\* Important \*\*** Par défaut, la compression augmente considérablement l’utilisation de l’UC et l’UC supplémentaire consommée par le processus de compression peut avoir un impact néfaste sur les opérations simultanées. Par conséquent, il peut être préférable, dans une session où l’utilisation de l’UC est limitée, de créer une sauvegarde compressée de priorité basse à l’aide de [Resource Governor](../../relational-databases/resource-governor/resource-governor.md). Pour plus d'informations, consultez [Utiliser Resource Governor pour limiter l’utilisation de l’UC par compression de la sauvegarde &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/use-resource-governor-to-limit-cpu-usage-by-backup-compression-transact-sql.md).|  
|**Ne pas compresser la sauvegarde**|Cliquez sur cette option pour créer une sauvegarde non compressée, indépendamment de la valeur par défaut au niveau du serveur.|  
  
## <a name="see-also"></a> Voir aussi  
 [Configurer un utilisateur de manière à créer et à gérer des travaux de l'Agent SQL Server](http://msdn.microsoft.com/library/67897e3e-b7d0-43dd-a2e2-2840ec4dd1ef)   
 [À propos de la copie des journaux de transaction &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)  
  
  
