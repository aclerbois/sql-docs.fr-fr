---
title: Transactions (Master Data Services) | Microsoft Docs
ms.custom: ''
ms.date: 01/10/2017
ms.prod: sql
ms.prod_service: mds
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology:
- master-data-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- transactions [Master Data Services], about transactions
- transactions [Master Data Services]
ms.assetid: 4cd2fa6f-9c76-4b7a-ae18-d4e5fd2f03f5
caps.latest.revision: 15
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: 2ecedca3bec602e5f278fb689080ca1b58b0224e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="transactions-master-data-services"></a>Transactions (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]



--------------------------------------------------
  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], une transaction est enregistrée à chaque fois qu'une action est effectuée sur un membre. Les transactions peuvent être affichées par tous les utilisateurs et être inversées par les administrateurs. Les transactions affichent la date, l'heure et l'utilisateur qui a effectué l'action, ainsi que d'autres détails. Les utilisateurs peuvent ajouter une annotation à une transaction, pour indiquer pourquoi une transaction a eu lieu.  
  
## <a name="when-transaction-are-recorded"></a>Critères d'enregistrement des transactions  
 Des transactions sont enregistrées lorsque des membres :  
  
-   sont créés, supprimés ou réactivés ;  
  
-   ont des valeurs d'attribut modifiées ;  
  
-   sont inclus dans une hiérarchie.  
  
 Les transactions ne sont pas enregistrées lorsque les règles d'entreprise modifient des valeurs d'attribut.  
  
## <a name="view-and-manage-transactions"></a>Afficher et gérer des transactions  
 Dans la zone fonctionnelle **Explorateur** , vous pouvez afficher et annoter les transactions (leur ajouter des commentaires) que vous avez effectuées. 
  
 Dans la zone fonctionnelle **Gestion des versions** , les administrateurs peuvent consulter toutes les transactions de tous les utilisateurs pour les modèles auxquels ils ont accès, et inverser l'une quelconque de ces transactions.
 
> [!NOTE]  
>  Les administrateurs peuvent afficher toutes les transactions pour tous les utilisateurs tant qu’ils n’ont pas le niveau d’autorisation de lecture seule appliqué dans la zone fonctionnelle **Gestion des versions**. Par exemple, si le niveau d’autorisation de lecture seule et de mise à jour est défini pour l’administrateur, celui-ci ne peut pas voir les autres transactions utilisateur, car l’autorisation de lecture seule est prioritaire sur l’autorisation de mise à jour.
  
 Vous pouvez configurer la durée de conservation des données de journal des transactions en définissant la propriété **Nombre de jours de rétention du journal** dans les paramètres système de la base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] et en configurant le paramètre **Nombre de jours de rétention du journal** lorsque vous créez ou modifiez un modèle. Pour plus d’informations, consultez [Paramètres système &#40;Master Data Services&#41;](../master-data-services/system-settings-master-data-services.md) et [Créer un modèle &#40;Master Data Services&#41;](../master-data-services/create-a-model-master-data-services.md).  
  
 Le travail SQL Server Agent, MDS_MDM_Sample_Log_Maintenace, déclenche le nettoyage des journaux des transactions et s’exécute toutes les nuits. Vous pouvez utiliser SQL Server Agent pour modifier la planification de ce travail.  
  
 Vous pouvez également appeler les procédures stockées ci-après pour nettoyer les journaux des transactions.  
  
|Procédure stockée|Description|  
|----------------------|-----------------|  
|mdm.udpTransactionsCleanup|Nettoie l’historique des transactions|  
|mdm.udpValidationsCleanup|Nettoie l’historique de validation|  
|mdm.udpEntityStagingBatchTableCleanup|Nettoie la table de mise en lots|  
  
 **Exemple**  
  
```  
DECLARE @CleanupOlderThanDate date = '2014-11-11',  
@ModelID INT = 7  
--Clean up Transaction Logs  
EXEC mdm.udpTransactionsCleanup @ModelID, @CleanupOlderThanDate;  
  
--Clean up Validation History  
EXEC mdm.udpValidationsCleanup @ModelID, @CleanupOlderThanDate;  
  
--Clean up EBS tables  
EXEC mdm.udpEntityStagingBatchTableCleanup @ModelID, @CleanupOlderThanDate;  
  
```  
  
## <a name="system-settings"></a>Paramètres système  
 Il existe un paramètre dans le [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] qui détermine si les transactions sont enregistrées ou non lors de l'organisation des enregistrements. Vous pouvez ajuster ce paramètre dans le [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] ou directement dans la table Paramètres système de la base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] . Pour plus d’informations, consultez [Paramètres système &#40;Master Data Services&#41;](../master-data-services/system-settings-master-data-services.md).  
  
 Lors de l'importation de données dans cette version de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], vous pouvez spécifier s'il faut enregistrer les transactions lors de l'initialisation de la procédure stockée. Pour plus d’informations, consultez [Procédure stockée de mise en lots &#40;Master Data Services&#41;](../master-data-services/staging-stored-procedure-master-data-services.md).  
  
## <a name="concurrency"></a>Accès concurrentiel  
 Si une valeur d'entité spécifique est affichée simultanément dans plusieurs sessions de l'Explorateur, les modifications simultanées d'une même valeur sont possibles. Les modifications simultanées ne sont pas détectées automatiquement par MDS. Cela peut se produire lorsque plusieurs utilisateurs utilisent plusieurs sessions de l'explorateur MDS dans le navigateur Web, par exemple sur plusieurs ordinateurs, sous plusieurs onglets de navigateur ou dans plusieurs fenêtres, ou avec plusieurs comptes utilisateur.  
  
 Plusieurs utilisateurs peuvent mettre à jour les mêmes valeurs d'entité sans erreur malgré les transactions activées. En général, la dernière modification de la valeur dans un même intervalle de temps prévaut. Les conflits de modifications en double peuvent être observés dans l'historique des transactions et peuvent être corrigés manuellement par l'administrateur. L'historique des transactions affiche les transactions individuelles pour **Valeur précédente** et **Nouvelle valeur** pour l'attribut en question dans chaque session, mais ne corrige pas automatiquement le conflit lorsque plusieurs **Nouvelles valeurs** existent pour la même valeur de départ.  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Description de la tâche|Rubrique|  
|----------------------|-----------|  
|Annuler une action en inversant une transaction (administrateurs uniquement)|[Inverser une transaction &#40;Master Data Services&#41;](../master-data-services/reverse-a-transaction-master-data-services.md)|  
  
## <a name="external-resources"></a>Ressources externes  
 Billet de blog [Nettoyage des journaux des transactions, de l’historique des problèmes de validation et des tables de mise en lots](http://go.microsoft.com/fwlink/p/?LinkId=615374)(en anglais) sur msdn.com.  
  
## <a name="related-content"></a>Contenu associé  
  
-   [Administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)  
  
-   [Annotations &#40;Master Data Services&#41;](../master-data-services/annotations-master-data-services.md)  
  
  
