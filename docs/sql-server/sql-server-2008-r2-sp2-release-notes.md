---
title: Notes de publication de SQL Server 2008 R2 SP2 | Microsoft Docs
ms.prod: sql
ms.prod_service: sql
ms.technology: supportability
ms.custom: ''
ms.date: 01/31/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- SQL Server 2008 R2 SP2
- Release Notes, SQL Server 2008 R2 SP2
ms.assetid: e2bd3de7-674c-4ea7-8d53-bb40bba86fae
author: craigg-msft
ms.author: craigg
manager: jhubbard
monikerRange: = sql-server-2014 || = sqlallproducts-allversions
ms.openlocfilehash: 214be1c0aac58e06c931876d49f7066cad86953b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-2008-r2-sp2-release-notes"></a>Notes de publication de SQL Server 2008 R2 SP2
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
Ce document Notes de publication décrit les problèmes connus que vous devez examiner avant d'installer ou de dépanner Microsoft SQL Server 2008 R2 Service Pack 2. Le présent document Notes de publication s'applique à toutes les éditions de SQL Server 2008 R2 SP2 et est disponible en ligne uniquement. Il est mis à jour régulièrement.  
  
## <a name="10-whats-new-in-service-pack-2"></a>1.0 Nouveautés du Service Pack 2  
Ajout de la vue de gestion dynamique (DMV) **sys.dm_db_stats_properties**. Vous pouvez utiliser cette DMV pour retourner les propriétés de statistiques d'une table ou d'une vue indexée spécifique de la base de données active. Par exemple, cette DMV retourne le nombre de lignes échantillonnées et le nombre d'étapes dans l'histogramme.  
  
## <a name="20-before-you-install"></a>2.0 Avant d’installer  
Pour plus d'informations sur l'installation des mises à jour de [!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)] , consultez la [documentation relative à la maintenance de SQL Server 2008 R2](http://msdn.microsoft.com/library/dd638062(SQL.105).aspx).  
  
Pour plus d'informations d'ordre général sur le démarrage et l'installation de SQL Server 2008 R2, consultez le fichier Lisez-moi de SQL Server 2008 R2. Le document Lisez-moi est disponible sur le support d'installation. Vous pouvez également trouver des informations supplémentaires dans la [documentation en ligne de SQL Server](http://msdn.microsoft.com/library/ms130214.aspx) et sur les [forums SQL Server](http://social.msdn.microsoft.com/Forums/category/sqlserver/).  
  
### <a name="21-choose-the-correct-file-to-download-and-install"></a>2.1 Choisir le fichier approprié à télécharger et à installer  
Utilisez le tableau suivant pour déterminer le fichier à télécharger et installer. Vérifiez que vous disposez de la configuration requise avant d'installer le Service Pack. La configuration requise est disponible sur les pages de téléchargement liées dans le tableau.  
  
|Si la version actuellement installée est...|Vous souhaitez…|Téléchargez et installez...|  
|-------------------------------------------|----------------------|---------------------------|  
|Une version 32 bits de n’importe quelle édition de SQL Server 2008 R2 ou de SQL Server 2008 R2 SP1|Effectuer la mise à niveau vers la version 32 bits de SQL Server 2008 R2 SP2|SQLServer2008R2SP2-KB2630458-x86-ENU à partir d' [ici](http://go.microsoft.com/fwlink/p/?LinkId=251790)|  
|Une version 32 bits de SQL Server 2008 R2 RTM Express ou de SQL Server 2008 R2 SP1 Express|Effectuer la mise à niveau vers la version 32 bits de SQL Server 2008 R2 SP2|SQLServer2008R2SP2-KB2630458-x86-ENU.exe à partir d' [ici](http://go.microsoft.com/fwlink/p/?LinkId=251790)|  
|Une version 32 bits de seulement le client et les outils de gestion de SQL Server 2008 R2 ou de SQL Server 2008 R2 SP1 (y compris SQL Server 2008 R2 Management Studio)|Effectuer la mise à niveau du client et des outils de gestion vers la version 32 bits de SQL Server 2008 R2 SP2|SQLServer2008R2SP2-KB2630458-x86-ENU.exe à partir d' [ici](http://go.microsoft.com/fwlink/p/?LinkId=251790)|  
|Une version 32 bits de SQL Server 2008 R2 Management Studio Express ou de SQL Server 2008 R2 SP1 Management Studio Express|Effectuer la mise à niveau vers la version 32 bits de SQL Server 2008 R2 SP2 Management Studio Express|SQLManagementStudio_x86_ENU.exe à partir d' [ici](http://go.microsoft.com/fwlink/p/?LinkId=251791)|  
|Une version 32 bits d'une édition quelconque de SQL Server 2008 R2 ou SQL Server 2008 R2 SP1 **et** une version 32 bits du client et des outils de gestion (y compris SQL Server 2008 R2 RTM Management Studio)|Effectuer la mise à niveau de tous les produits vers la version 32 bits de SQL Server 2008 R2 SP2|SQLServer2008R2SP2-KB2630458-x86-ENU.exe à partir d' [ici](http://go.microsoft.com/fwlink/p/?LinkId=251790)|  
|Une version 32 bits d'un ou de plusieurs outils de [Microsoft SQL Server 2008 R2 RTM Feature Pack](http://www.microsoft.com/download/en/details.aspx?id=16978)|Effectuer la mise à niveau des outils vers la version 32 bits du Feature Pack Microsoft SQL Server 2008 R2 SP2|Un ou plusieurs fichiers de [Microsoft SQL Server 2008 R2 SP2 Feature Pack](http://go.microsoft.com/fwlink/?LinkId=251792)|  
|Pas d’installation 32 bits de SQL Server 2008 R2|Installez SQL Server 2008 R2, y compris le SP2|Accédez à [SQL Server 2008 R2 SP2 – Express Edition](http://go.microsoft.com/fwlink/?LinkId=251791) et suivez les instructions.|  
|Pas d’installation 32 bits de SQL Server 2008 R2 Management Studio|Installez SQL Server 2008 R2 Management Studio, y compris le SP2|SQLManagementStudio_x86_ENU.exe à partir d' [ici](http://go.microsoft.com/fwlink/p/?LinkId=251791) pour installer la version gratuite de SQL Server 2008 R2 SP2 Management Studio Express Edition.|  
|Une version 64 bits d'une édition quelconque de SQL Server 2008 R2 ou SQL Server 2008 R2 SP1|Effectuez la mise à niveau vers la version 64 bits de SQL Server 2008 R2 SP2|SQLServer2008R2SP2-KB2630458-x64-ENU ou SQLServer2008R2SP2-KB2630455-IA64-ENU.exe à partir d’ [ici](http://go.microsoft.com/fwlink/p/?LinkId=251790)|  
|Une version 64 bits de SQL Server 2008 R2 RTM Express ou SQL Server 2008 R2 SP1 Express|Effectuez la mise à niveau vers la version 64 bits de SQL Server 2008 R2 SP2|SQLServer2008R2SP2-KB2630458-x64-ENU.exe ou SQLServer2008R2SP2-KB2630455-IA64-ENU.exe à partir d' [ici](http://go.microsoft.com/fwlink/p/?LinkId=251790)|  
|Une version 64 bits uniquement du client et des outils de gestion de SQL Server 2008 R2 ou SQL Server 2008 R2 SP1 (y compris SQL Server 2008 R2 Management Studio)|Mettez à niveau le client et les outils de gestion vers la version 64 bits de SQL Server 2008 R2 SP2|SQLServer2008R2SP2-KB2630458-x64-ENU.exe ou SQLServer2008R2SP2-KB2630455-IA64-ENU.exe à partir d' [ici](http://go.microsoft.com/fwlink/p/?LinkId=251790)|  
|Une version 64 bits de SQL Server 2008 R2 Management Studio Express ou SQL Server 2008 R2 SP1 Management Studio Express|Effectuez la mise à niveau vers la version 64 bits de SQL Server 2008 R2 SP2 Management Studio Express|SQLManagementStudio_x64_ENU.exe à partir d' [ici](http://go.microsoft.com/fwlink/p/?LinkId=251791)|  
|Une version 64 bits d'une édition quelconque de SQL Server 2008 R2 ou SQL Server 2008 R2 SP1 **et** une version 64 bits du client et des outils de gestion (y compris SQL Server 2008 R2 RTM Management Studio)|Effectuez la mise à niveau de tous les produits vers la version 64 bits de SQL Server 2008 R2 SP2|SQLServer2008R2SP2-KB2630458-x64-ENU.exe à partir d' [ici](http://go.microsoft.com/fwlink/p/?LinkId=251790)|  
|Une version 64 bits d'un ou de plusieurs outils de [Microsoft SQL Server 2008 R2 RTM Feature Pack](http://www.microsoft.com/download/en/details.aspx?id=16978)|Mettez à niveau les outils vers la version 64 bits de Microsoft SQL Server 2008 R2 SP2 Feature Pack|Un ou plusieurs fichiers de [Microsoft SQL Server 2008 R2 SP2 Feature Pack](http://go.microsoft.com/fwlink/?LinkId=251792)|  
|Aucune installation version 64 bits de SQL Server 2008 R2|Installez SQL Server 2008 R2, y compris le SP2|Accédez à [SQL Server 2008 R2 SP2 – Express Edition](http://go.microsoft.com/fwlink/?LinkId=251791) et suivez les instructions.|  
|Aucune installation version 64 bits de SQL Server 2008 R2 Management Studio|Installez SQL Server 2008 R2 Management Studio, y compris le SP2|SQLManagementStudio_x64_ENU.exe à partir d' [ici](http://go.microsoft.com/fwlink/p/?LinkId=251791) pour installer la version gratuite de SQL Server 2008 R2 SP2 Management Studio Express Edition.|  
  
### <a name="22-setup-might-fail-if-sqagtresdll-is-locked-by-another-process"></a>2.2 Le programme d'installation peut échouer si SQAGTRES.dll est verrouillé par un autre processus  
**Problème**: une opération d’installation de SQL peut échouer avec cette erreur : `Upgrading of cluster resource C:\Program Files\Microsoft SQL Server\MSSQL10_50.<Instance name>\MSSQL\Binn\SQAGTRES.DLL on machine <Computer name> failed with Win32Exception. Please look at inner exception for details.` L’origine du problème est que C:\Windows\system32\SQAGTRES.DLL étant verrouillé par un autre processus, le programme d’installation n’a pas pu le mettre à jour.  
  
**Solution de contournement**: renommez C:\Windows\system32\SQAGTRES.DLL en un nom temporaire, tel que C:\Windows\system32\SQAGTRES_old.DLL, puis sélectionnez l'option de nouvelle tentative dans le message d'erreur d'installation. Ainsi, le programme d'installation continue. Après un redémarrage, supprimez le fichier temporaire C:\Windows\system32\SQAGTRES_old.DLL.  
  
## <a name="30-known-issues-fixed-in-this-service-pack"></a>3.0 Problèmes connus résolus dans ce Service Pack  
Pour obtenir la liste des bogues et problèmes connus corrigés dans ce Service Pack, consultez l' [article principal de la Base de connaissances](http://support.microsoft.com/kb/2630455).  
  
## <a name="see-also"></a> Voir aussi  
[Comment déterminer la version et l'édition de SQL Server](http://support.microsoft.com/kb/321185)  
  
