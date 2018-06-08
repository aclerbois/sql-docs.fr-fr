---
title: Créer la base de données (Assistant Importation et Exportation SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: import-export-data
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.impexpwizard.createdatabase.f1
ms.assetid: 56a8a79f-086c-4bdc-8888-0045bb4b0cbf
caps.latest.revision: 54
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 4dfc038b8fa769ce5a3d5a463fe24ce6b4abae25
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-database-sql-server-import-and-export-wizard"></a>Créer la base de données (Assistant Importation et Exportation SQL Server)
Si vous sélectionnez **Nouveau** dans la page **Choisir une destination** pour créer une base de données de destination SQL Server, l’Assistant Importation et Exportation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] présente la boîte de dialogue **Créer une base de données** . Dans cette page, spécifiez un nom pour la nouvelle base de données. Vous pouvez éventuellement modifier les paramètres de la taille initiale et la croissance automatique de la nouvelle base de données et de son fichier journal. 

La boîte de dialogue **Créer une base de données** de l’Assistant offre uniquement les options de base disponibles pour la création d’une base de données SQL Server. Pour afficher et configurer toutes les options d’une nouvelle base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], utilisez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour créer la base de données, ou la configurer après sa création par l’Assistant. 

> [!NOTE]
> Si vous recherchez des informations sur l’instruction CREATE DATABASE [!INCLUDE[tsql](../../includes/tsql-md.md)] et non sur la boîte de dialogue **Créer une base de données** de l’Assistant Importation et Exportation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-sql-server-transact-sql.md).  

## <a name="screen-shot-of-the-create-database-page"></a>Capture d’écran de la page Créer une base de données  
La capture d’écran suivante montre la boîte de dialogue **Créer une base de données** de l’Assistant.  

![Page Créer une base de données de l’Assistant Importation et Exportation](../../integration-services/import-export-data/media/create-database.png "Page Créer une base de données de l’Assistant Importation et Exportation")  

## <a name="provide-a-name-for-the-new-database"></a>Fournir un nom pour la nouvelle base de données  
**Nom**  
 Fournissez un nom pour la base de données SQL Server de destination.
 
### <a name="naming-requirements"></a>Exigences concernant l’affectation des noms
Veillez à respecter les conventions de nommage SQL Server quand vous nommez la base de données.  
  
-   Le nom de la base de données doit être unique au sein d’une instance de SQL Server.  
  
-   Le nom de la base de données peut avoir un maximum de 123 caractères. (5 caractères sont autorisés pour les suffixes que SQL Server ajoute au fichier de données et au fichier journal, ce qui ne dépasse pas du nombre maximum de 128 caractères.)  
  
-   Le nom de la base de données doit suivre les règles applicables aux identificateurs dans SQL Server. Voici les critères les plus importants.  
  
    -   Le premier caractère doit être une lettre, un trait de soulignement (_), une arobase (@) ou un signe dièse (#).  
  
    -   Les caractères qui suivent peuvent être des lettres, des chiffres, une arobase, un symbole dollar ($), un signe dièse ou un trait de soulignement.  
  
    -   Vous ne pouvez pas utiliser d’espaces ni d’autres caractères spéciaux.  
  
Pour obtenir des informations détaillées sur ces critères, consultez [Identificateurs de base de données](../../relational-databases/databases/database-identifiers.md).  

## <a name="optionally-specify-data-file-and-log-file-options"></a>Spécifiez éventuellement les options de fichier journal et de fichier de données

> [!TIP]
> Vous devez fournir un nom pour la nouvelle base de données dans le champ **Nom** , mais vous pouvez généralement laisser les autres paramètres de taille et de croissance de fichier avec leurs valeurs par défaut.

### <a name="data-file-options"></a>Options pour les fichiers de données  
 **Taille initiale**  
 Définissez le nombre de mégaoctets de la taille initiale du fichier de données.  
  
 **Aucune croissance autorisée**  
 Indiquez si la taille du fichier de données peut augmenter au-delà de la taille initiale spécifiée.  
  
 **Augmenter de (pourcentage)**  
 Définissez le pourcentage d'augmentation de la taille du fichier de données.  
  
 **Augmenter de (taille)**  
 Définissez le nombre d'octets d'augmentation de la taille du fichier de données.  
  
### <a name="log-file-options"></a>Options pour les fichiers journaux  
 **Taille initiale**  
 Définissez le nombre de mégaoctets de la taille initiale du fichier journal.  
  
 **Aucune croissance autorisée**  
 Indiquez si la taille du fichier journal peut augmenter au-delà de la taille initiale spécifiée.  
  
 **Augmenter de (pourcentage)**  
 Définissez le pourcentage d'augmentation du fichier journal.  
  
 **Augmenter de (taille)**  
 Définissez le nombre d'octets d'augmentation de la taille du fichier journal.  

### <a name="more-info"></a>En savoir plus
Pour plus d’informations sur les options de taille de fichier que vous voyez sur cette page, consultez [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-sql-server-transact-sql.md). 

## <a name="whats-next"></a>Quelle est l’étape suivante ?  
 Une fois que vous avez fourni un nom pour la nouvelle base de données que l’Assistant va créer et cliqué sur **OK**, la boîte de dialogue **Créer une base de données** vous renvoie à la page **Choisir une Destination** . Pour plus d’informations, consultez [Choisir une destination](../../integration-services/import-export-data/choose-a-destination-sql-server-import-and-export-wizard.md).  

