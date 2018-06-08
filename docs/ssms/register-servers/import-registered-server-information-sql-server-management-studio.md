---
title: Importer des informations de serveur inscrit (SQL Server Management Studio) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-registration
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.importregisteredservers.f1
helpviewer_keywords:
- transferring registered server information
- Registered Servers [SQL Server], importing
- importing registered server information
ms.assetid: cc497a14-1360-4887-b70c-002f042823b6
caps.latest.revision: 28
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 986d96e0aade6a69e2cc39abab767a9b44d3708e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="import-registered-server-information-sql-server-management-studio"></a>Importer des informations de serveur inscrit (SQL Server Management Studio)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  Cette rubrique explique comment importer les informations du serveur inscrit enregistrées dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. L'exportation puis l'importation de fichiers de serveurs inscrits vous permet de configurer aisément plusieurs ordinateurs avec les mêmes serveurs dans Serveurs inscrits. Cela est utile lors de la gestion d'un grand nombre de serveurs à partir d'ordinateurs situés à des emplacements différents ou lorsque vous voulez configurer des paramètres de connexion de base pour un utilisateur peu expérimenté.  
  
> [!NOTE]  
>  Vous ne pouvez pas importer d'informations sur les serveurs inscrits dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à partir des versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
##  <a name="SSMSProcedure"></a>  
  
#### <a name="to-import-registered-server-information"></a>Pour importer des informations de serveur inscrit  
  
1.  Dans Serveurs inscrits, cliquez sur le type de serveur dans la barre d'outils Serveurs inscrits. Le type de serveur doit être identique au type de fichier d'exportation du serveur inscrit. Si, par exemple, vous avez exporté des informations de serveur inscrit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous devez cliquer sur **SQL Server** dans la barre d'outils Serveurs inscrits.  
  
2.  Cliquez avec le bouton droit sur un groupe de serveurs, puis sélectionnez **Importer**.  
  
3.  Dans la boîte de dialogue **Importer des serveurs inscrits** , sélectionnez le fichier de serveurs inscrits à importer, puis cliquez sur **OK**.  
  
     **Fichier d'importation**  
     Tapez le nom du fichier d’importation dans la zone de texte ou cliquez sur le bouton Parcourir (**...**) pour rechercher le fichier d’importation sur l’ordinateur client. Si vous sélectionnez un fichier existant, les informations de serveur inscrit sont ajoutées au fichier. Le fichier d'importation peut être uniquement un fichier de serveur inscrit précédemment exporté. Les fichiers de serveur inscrit ont l'extension .regsrvr.  
  
     **Sélectionnez le groupe de serveurs vers lequel importer**  
     Sélectionnez le nœud racine ou un groupe de serveurs particulier vers lequel seront importées les entrées de serveur inscrit dans le fichier. Vous pouvez importer dans le fichier d'exportation tous les serveurs inscrits, les serveurs inscrits d'un groupe de serveurs particulier ou un serveur inscrit individuel. La fonctionnalité d'importation est récursive ; par exemple, si le groupe de serveurs A contient le groupe de serveurs B, et que le groupe de serveurs B contient les groupes de serveurs C et D, l'importation du groupe de serveur A entraîne l'exportation de toutes les entrées de A, B, C et D.  
  
     Le groupe de serveurs affiche uniquement les groupes de serveurs de l'arborescence actuelle de serveurs inscrits.  
  
     Si vous choisissez d'importer un serveur ou un groupe de serveurs existant, un message confirme que vous voulez remplacer le serveur ou le groupe de serveurs existant.  
  
 Les inscriptions de serveurs qui utilisent l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stockent les mots de passe par utilisateur. Après l'importation des inscriptions de serveurs, les utilisateurs doivent entrer le mot de passe pour chaque serveur la première fois qu'ils se connectent et stocker les mots de passe dans leurs listes de serveurs inscrits. Cela n'est pas nécessaire pour les serveurs inscrits via l'authentification Windows.  
  
## <a name="see-also"></a> Voir aussi  
 [Modifier l’inscription d’un serveur &#40;SQL Server Management Studio&#41;](../../tools/sql-server-management-studio/change-a-server-s-registration-sql-server-management-studio.md)   
 [Exporter les informations des serveurs inscrits &#40;SQL Server Management Studio&#41;](../../tools/sql-server-management-studio/export-registered-server-information-sql-server-management-studio.md)   
 [Créer un nouveau serveur inscrit &#40;SQL Server Management Studio&#41;](../../tools/sql-server-management-studio/create-a-new-registered-server-sql-server-management-studio.md)  
  
  
