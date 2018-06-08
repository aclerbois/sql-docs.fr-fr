---
title: Réparer une installation défectueuse de SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 09/08/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: install
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 90c11b28-892b-44d6-978e-0eee48c75b7d
caps.latest.revision: 18
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 4944e5949dc1febab41f36e5749c595a2f4dc01b
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34771635"
---
# <a name="repair-a-failed-sql-server-installation"></a>Réparer une installation défectueuse de SQL Server

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

L'opération de réparation peut être utilisée dans les scénarios suivants :  
  
- Réparer une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui est endommagée après une installation réussie. 
  
- Réparer une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si l'opération de mise à niveau est annulée ou échoue une fois que le nom de l'instance est mappé à l'instance nouvellement mise à niveau. 
  
    - Si le message suivant s'affiche dans le journal résumé, vous pouvez réparer l'instance de mise à niveau qui a échoué :  
  
         La mise à niveau de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a échoué. Pour continuer, déterminez la cause de l'échec, résolvez le problème, puis réparez votre installation. »  
  
    - Si le message suivant s'affiche dans le journal résumé, vous devez désinstaller, puis réinstaller [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Vous ne pouvez pas réparer l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . 
  
         La mise à niveau de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a échoué. Pour continuer, déterminez la cause de l'échec, résolvez le problème. »  
  
 Lorsque vous réparez une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
- tous les fichiers manquants ou endommagés sont remplacés ; 
  
- toutes les clés de Registre manquantes ou endommagées sont remplacées ; 
  
- les valeurs par défaut sont définies pour toutes les valeurs de configuration manquantes ou non valides. 
  
 Avant de continuer, pour les clusters de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , passez en revue les informations importantes suivantes :  
  
- La réparation doit être effectuée sur des nœuds de cluster individuels. 
  
- Pour réparer un nœud de cluster de basculement après une préparation ayant échoué, utilisez **Supprimer un nœud** et effectuez à nouveau la préparation. Pour plus d’informations, consultez [Ajouter ou supprimer des nœuds dans un cluster de basculement SQL Server &#40;programme d’installation&#41;](../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md). 
  
## <a name="repair-a-failed-installation-of-includessnoversionincludesssnoversion-mdmd-from-the-installation-center"></a>Réparer une installation défectueuse de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir du Centre d’installation 
  
1. Lancez le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (setup.exe) à partir du support d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . 
  
2. Après avoir vérifié les composants requis et le système, le programme d'installation affiche la page Centre d'installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . 
  
3. Cliquez sur **Maintenance** dans la zone de navigation gauche, puis sur **Réparer** pour démarrer la réparation. 
  
   >[!TIP]  
   > Si le Centre d'Installation a été lancé à l'aide du menu Démarrer, vous devez fournir l'emplacement actuel du support d'installation. 
  
4. La règle de support d'installation et les routines de fichiers sont exécutées pour garantir que les composants requis sont installés sur votre système et que les règles de validation du programme d'installation ont été correctement exécutées sur l'ordinateur. Cliquez sur **OK** ou sur **Installer** pour continuer. 
  
5. Dans la page Sélectionner une instance, sélectionnez l’instance à réparer, puis cliquez sur **Suivant** pour continuer. 
  
6. Les règles de réparation sont exécutées pour valider l'opération. Pour continuer, cliquez sur **Suivant**. 
  
7. La page Prêt à réparer indique que l'opération peut se poursuivre. Pour continuer, cliquez sur **Réparer**. 
  
8. La page Progression de la réparation indique l'état de l'opération de réparation. La page Terminé indique que l'opération est terminée. 
  
### <a name="to-repair-a-failed-installation-of-includessnoversionincludesssnoversion-mdmd-using-command-prompt"></a>Pour réparer une installation défectueuse de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'aide de l'invite de commandes  
  
1. À une invite de commandes, exécutez la commande suivante :  
  
    ```  
    Setup.exe /q /ACTION=Repair /INSTANCENAME=instancename  
    ```  
  
## <a name="see-also"></a> Voir aussi  
 [Afficher et lire les fichiers journaux d’installation de SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)   
 [Articles de procédures relatives à l’installation](http://msdn.microsoft.com/library/59de41e7-557f-462a-8914-53ec35496baa)  
  
  
