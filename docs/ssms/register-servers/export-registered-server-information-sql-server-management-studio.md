---
title: Exporter les informations des serveurs inscrits (SQL Server Management Studio) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
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
- sql13.swb.exportregisteredservers.f1
helpviewer_keywords:
- Registered Servers [SQL Server], exporting
- exporting registered server information
- transferring registered server information
ms.assetid: b65e168f-b6bf-489c-b8ad-3b8644acf0b6
caps.latest.revision: 27
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 8296dcfc65dcc4c9156aec7b7fd09b63decacea6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="export-registered-server-information-sql-server-management-studio"></a>Exporter les informations des serveurs inscrits (SQL Server Management Studio)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  Cette rubrique explique comment enregistrer et exporter les informations des serveurs inscrits dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]et les distribuer à d'autres employés ou serveurs. Vous pouvez utiliser cette fonction d'exportation pour présenter une interface utilisateur cohérente sur plusieurs ordinateurs.  
  
 Si vous exportez puis importez les fichiers de serveurs inscrits, vous pouvez configurer facilement plusieurs ordinateurs en leur affectant les mêmes serveurs dans Serveurs inscrits. Ceci peut être utile si vous gérez un grand nombre de serveurs à partir d'ordinateurs situés à différents endroits ou si vous voulez configurer des paramètres de connexion de base pour un utilisateur moins expérimenté.  
  
> [!NOTE]  
>  Les inscriptions de serveurs qui utilisent l’authentification SQL Server stockent les mots de passe par utilisateur. Après avoir exporté et importé les inscriptions de serveurs, les utilisateurs doivent entrer le mot de passe pour chaque serveur la première fois qu'ils se connectent et stocker leurs mots de passe dans les listes de serveurs inscrits. Par contre, ce n'est pas nécessaire pour les serveurs inscrits à l'aide de l'authentification Windows.  
  
##  <a name="SSMSProcedure"></a>  
  
#### <a name="to-export-registered-server-information"></a>Pour exporter les informations des serveurs inscrits  
  
1.  Dans Serveurs inscrits, cliquez avec le bouton droit de la souris sur un groupe de serveurs, puis cliquez sur **Exporter**.  
  
    > [!NOTE]  
    >  Vous pouvez exporter un serveur individuel, toute l'arborescence des serveurs inscrits ou un sous-ensemble de l'arborescence des serveurs inscrits.  
  
2.  Dans la boîte de dialogue **Exporter les serveurs inscrits** , effectuez les sélections suivantes.  
  
     **Groupe de serveurs**  
     Spécifiez le groupe de serveurs à exporter. Vous pouvez exporter dans le fichier d'exportation tous les serveurs inscrits, les serveurs inscrits d'un groupe de serveurs donné ou un seul serveur inscrit. La fonctionnalité d'exportation est récursive : par exemple, si le groupe de serveurs A contient le groupe de serveurs B et que le groupe de serveurs B contient les groupes de serveurs C et D, en exportant le groupe de serveurs A vous exportez tous les enregistrements des groupes de serveurs A, B, C et D.  
  
     Le groupe de serveurs affiche uniquement les groupes de serveurs de l'arborescence actuelle de serveurs inscrits.  
  
     **Fichier d'exportation**  
     Tapez le nom du fichier d’exportation dans la zone de texte ou utilisez le bouton Parcourir (**...**) pour rechercher un fichier d’exportation sur l’ordinateur client. Si vous sélectionnez un fichier existant, les informations de serveur inscrit sont ajoutées au fichier. Utilisez l'extension .regsrvr. Pour rendre les informations de serveurs inscrits accessibles à d'autres utilisateurs ou un autre ordinateur, vous pouvez enregistrer le fichier sur le réseau. Les autres utilisateurs peuvent accéder au fichier et importer l'ensemble ou une partie des informations de serveurs inscrits. Si vous sélectionnez un fichier d'exportation existant, le contenu du fichier est remplacé par les informations d'inscription des serveurs.  
  
     **Ne pas inclure les noms d'utilisateur ni les mots de passe dans le fichier d'exportation**  
     Exclut les noms d'utilisateur lors de l'exportation du fichier.  
  
    > [!IMPORTANT]  
    >  Bien que les fichiers d'exportation soient chiffrés, si les noms d'utilisateur et les mots de passe d'authentification SQL Server sont inclus dans le fichier, l'accès à ce fichier doit être soigneusement contrôlé. Les noms d'utilisateur et les mots de passe sont par conséquent exclus par défaut du fichier d'exportation.  
  
## <a name="see-also"></a> Voir aussi  
 [Importer les informations des serveurs inscrits &#40;SQL Server Management Studio&#41;](../../tools/sql-server-management-studio/import-registered-server-information-sql-server-management-studio.md)   
 [Créer un nouveau serveur inscrit &#40;SQL Server Management Studio&#41;](../../tools/sql-server-management-studio/create-a-new-registered-server-sql-server-management-studio.md)  
  
  
