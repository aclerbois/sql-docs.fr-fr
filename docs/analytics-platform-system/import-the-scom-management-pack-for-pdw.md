---
title: Importer le Pack d’administration SCOM - système de plateforme Analytique | Documents Microsoft
description: Suivez ces étapes pour importer les packs d’administration de System Center Operations Manager (SCOM) pour le système de plateforme Analytique (APS). Les packs d’administration sont requises pour analyser les Parallel Data Warehouse de SCOM.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: e60d87ae58b0804a0a7296f8b489df7441683c5b
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="import-the-scom-management-pack---analytics-platform-system"></a>Importer le Pack d’administration SCOM - système de plateforme Analytique
Suivez ces étapes pour importer les packs d’administration de System Center Operations Manager (SCOM) pour le système de plateforme Analytique (APS). Les packs d’administration sont requises pour analyser les Parallel Data Warehouse de SCOM. 
  
## <a name="BeforeBegin"></a>Avant de commencer  
**Configuration requise**  
  
System Center Operations Manager 2007 R2 doit être installé et en cours d’exécution.  
  
Les packs d’administration doivent être installés. Consultez [installer les Packs d’administration SCOM &#40;système de plateforme Analytique&#41;](install-the-scom-management-packs.md).  
  
## <a name="Step1"></a>Étape 1 : Importer le Pack d’administration de Base SQL Server Appliance  
  
1.  Ouvrez une session l’ordinateur avec un compte qui est membre du rôle Administrateurs Operations Manager pour le groupe d’administration Operations Manager 2007.  
  
2.  Dans la console Opérateur, cliquez sur **Administration**.  
  
3.  Cliquez sur le **packs d’administration** nœud, puis cliquez sur **importer les packs d’administration**.  
  
    ![Cliquez sur Importer les packs d’administration](./media/import-the-scom-management-pack-for-pdw/SCOM_IMP.png "SCOM")  
  
4.  Dans la liste des packs d’administration, sélectionnez le Pack d’administration que vous souhaitez importer, cliquez sur **sélectionnez**, puis cliquez sur **ajouter**.  
  
    ![Liste des packs d’administration](./media/import-the-scom-management-pack-for-pdw/SCOM_IMP2.png "SCOM_IMP2")  
  
5.  Recherchez **Appliance** , puis accédez à la Base d’Appliance SQL Server, puis cliquez **ajouter** les deux options.  
  
    ![Base d’appliance SQL Server](./media/import-the-scom-management-pack-for-pdw/SCOM_IMP3.png "SCOM_IMP3")  
  
6.  Une fois que les deux Packs d’administration ont été dans le volet sélectionné en bas, puis cliquez sur **OK**.  
  
    ![Sélectionnez les packs d’administration](./media/import-the-scom-management-pack-for-pdw/SCOM_IMP4.png "SCOM_IMP4")  
  
7.  Cliquez sur **Installer**.  
  
    ![Cliquez sur Installer](./media/import-the-scom-management-pack-for-pdw/SCOM_IMP5.png "SCOM_IMP5")  
  
8.  Une fois terminé, cliquez sur **fermer**.  
  
    ![Une fois terminé, cliquez sur Fermer](./media/import-the-scom-management-pack-for-pdw/SCOM_IMP6.png "SCOM_IMP6")  
  
## <a name="Step2"></a>Importer le Pack de surveillance pour le matériel de l’entrepôt de données parallèles de Microsoft SQL Server 2008 R2  
  
1.  Cliquez sur le **packs d’administration** nœud, puis cliquez sur **importer les packs d’administration**.  
  
2.  Choisissez **à partir du disque**...  
  
    ![Avec le bouton droit de packs d’administration](./media/import-the-scom-management-pack-for-pdw/SCOM_PDW.png "SCOM_PDW")  
  
3.  Accédez à l’emplacement où vous avez extrait les Packs d’administration SQL Server PDW et choisissez les trois packs d’administration qui se trouvent dans la section « Sélectionnés des packs d’administration pour importer ». Pour cela, en sélectionnant le, en cliquant sur la touche MAJ enfoncée et en sélectionnant le dernier signet. Une fois qu’ils sont tous sélectionnés, cliquez sur **ouvrir**.  
  
    ![Sélectionnez les packs d’administration](./media/import-the-scom-management-pack-for-pdw/SCOM_PDW2.png "SCOM_PDW2")  
  
4.  Cliquez sur **Installer**.  
  
    ![Cliquez sur Installer](./media/import-the-scom-management-pack-for-pdw/SCOM_PDW3.png "SCOM_PDW3")  
  
5.  Cliquez sur **Fermer**.  
  
    ![Cliquez sur Fermer](./media/import-the-scom-management-pack-for-pdw/SCOM_PDW4.png "SCOM_PDW4")  
  
## <a name="next-step"></a>Étape suivante  
Maintenant que vous avez importé les packs d’administration, passez à l’étape suivante : [configurer SCOM pour système de plateforme d’analyse Analytique &#40;système de plateforme Analytique&#41;](configure-scom-to-monitor-analytics-platform-system.md).  
  
<!-- MISSING LINKS ## See Also  
[Common Metadata Query Examples &#40;SQL Server PDW&#41;](../sqlpdw/common-metadata-query-examples-sql-server-pdw.md)  -->  
  
