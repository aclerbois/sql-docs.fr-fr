---
title: MSSQLSERVER_824 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: language-reference
helpviewer_keywords:
- 824 (Database Engine error)
ms.assetid: 2aa22246-2712-4fdb-9744-36e7e6f3175e
caps.latest.revision: 17
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 1e60f3587806dfb9bc6f8a86c0f870e898fac95d
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="mssqlserver824"></a>MSSQLSERVER_824
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  
## <a name="details"></a>Détails  
  
|||  
|-|-|  
|Nom du produit|SQL Server|  
|ID d'événement|824|  
|Source de l'événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|B_HARDSSERR|  
|Texte du message|SQL Server a détecté une erreur d'E/S logique et relative à la cohérence. L'erreur %ls s'est produite pendant une opération de %S_MSG de la page %S_PGID dans la base de données avec l'ID %d au niveau du décalage %#016I64x dans le fichier '%ls'.  Vous trouverez peut-être plus de détails dans les messages supplémentaires qui figurent dans le journal des erreurs et le journal des événements système de SQL Server.|  
  
## <a name="explanation"></a>Explication  
Cette erreur indique que Windows considère que la page est correctement lue à partir du disque alors que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] signale qu'il y a un problème avec la page. Cette erreur est similaire à l'erreur 823, à la différence qu'elle n'a pas été détectée par Windows. Cela indique généralement un problème dans le sous-système d'E/S, comme un lecteur de disque défaillant, des problèmes avec le microprogramme d'un disque, un pilote de périphérique défectueux, etc. Pour plus d’informations sur les erreurs d’E/S, consultez [Concepts de base des E/S Microsoft SQL Server, chapitre 2](http://go.microsoft.com/fwlink/?LinkId=69370).  
  
## <a name="user-action"></a>Action de l'utilisateur  
  
### <a name="look-for-hardware-failure"></a>Rechercher une défaillance matérielle  
Exécutez les diagnostics matériels et corrigez les éventuels problèmes rencontrés. Examinez également les journaux système et d'applications de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows ainsi que le journal des erreurs de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour vérifier si l'erreur est due à une défaillance matérielle. Corrigez les éventuels problèmes matériels contenus dans les journaux.  
  
Si vous avez des problèmes persistants de données endommagées, tentez d'échanger votre ordinateur, vos contrôleurs et vos lecteurs de disque contre d'autres composants. Assurez-vous que votre système n'a pas une mémoire cache d'écriture active sur le contrôleur de disque. Si vous soupçonnez qu’il s’agit là de la source du problème, contactez votre fournisseur de matériel.  
  
Enfin, il peut s'avérer bénéfique d'utiliser un matériel totalement nouveau, avec reformatage des lecteurs de disque et réinstallation du système d'exploitation.  
  
### <a name="restore-from-backup"></a>Restaurer à partir de la sauvegarde  
S'il ne s'agit pas d'un problème de matériel et qu'une sauvegarde saine est disponible, restaurez la base de données à partir de cette sauvegarde.  
  
Envisagez de modifier les bases de données afin qu'elles utilisent l'option PAGE_VERIFY CHECKSUM. Pour plus d’informations sur PAGE_VERIFY, consultez [ALTER DATABASE &#40;Transact-SQL&#41;](~/t-sql/statements/alter-database-transact-sql-set-options.md).  
  
## <a name="see-also"></a> Voir aussi  
[Gérer la table suspect_pages &#40;SQL Server&#41;](~/relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)  
  
