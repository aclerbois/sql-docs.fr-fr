---
title: Publier une base de données (SQL Server Management Studio) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: databases
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 98b2914e-7147-40af-ba7d-87253bbe8bf9
caps.latest.revision: 10
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 09255d7a04063de609a7f54c395f50f83e765643
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="publish-a-database-sql-server-management-studio"></a>Publier une base de données (SQL Server Management Studio)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Vous pouvez utiliser l' **Assistant Générer et publier des scripts** pour publier une base de données entière ou des objets de base de données individuels sur un fournisseur d'hébergement Web.  
  
> [!NOTE]  
>  Les fonctionnalités décrites dans cette rubrique étaient fournies par l'Assistant Publication de base de données. La fonctionnalité de publication a été ajoutée à l'Assistant Générer et publier des scripts, et l'Assistant Publication de base de données a été supprimé.  
  
## <a name="generate-and-publish-scripts-wizard"></a>Assistant Générer et publier des scripts  
 L'Assistant Générer et publier des scripts peut être utilisé pour publier une base de données ou des objets de base de données sélectionnés sur un fournisseur d'hébergement Web. Un fournisseur d'hébergement Web SQL Server est une interface de connectivité vers un service Web. Le service Web est créé à l'aide du projet Database Publishing Services depuis le SQL Server Hosting Toolkit sur CodePlex. Le service Web permet aux clients de l'hébergeur Web de publier plus facilement leurs bases de données sur le service à l'aide de l'Assistant Générer et publier des scripts. Pour plus d'informations sur le téléchargement du SQL Server Hosting Toolkit, consultez [SQL Server Database Publishing Services](http://go.microsoft.com/fwlink/?LinkId=142025)(en anglais).  
  
 L'Assistant Générer et publier des scripts peut également être utilisé pour créer un script pour le transfert d'une base de données.  
  
#### <a name="to-publish-a-database-to-a-web-service"></a>Pour publier une base de données sur un service Web  
  
1.  Dans l’Explorateur d’objets, développez **Bases de données**, cliquez avec le bouton droit sur une base de données, pointez sur **Tâches**et cliquez sur **Générer et publier des scripts**. Exécutez les étapes de l'Assistant pour générer un script pour la publication des objets de la base de données.  
  
2.  Dans la page **Sélectionner les objets** , sélectionnez les objets à publier sur le service d'hébergement Web.  
  
3.  Dans la page **Définir les options de script** , sélectionnez **Publier sur le service Web**.  
  
    1.  Dans la zone **Fournisseur** , spécifiez le fournisseur de votre service Web. Si vous n'avez pas configuré de fournisseur d'hébergement Web, sélectionnez **Gérer les fournisseurs** et utilisez la boîte de dialogue **Gérer les fournisseurs** pour configurer un fournisseur pour votre service Web.  
  
    2.  Pour spécifier des options d'édition avancées, sélectionnez le bouton **Avancé** dans la section **Publier sur le service Web** .  
  
4.  Dans la page **Résumé** , vérifiez vos sélections. Cliquez sur **Précédent** pour modifier vos sélections. Cliquez sur **Suivant** pour publier les objets que vous avez sélectionnés.  
  
5.  Dans la page **Enregistrer ou publier des scripts** , surveillez la progression de la publication.  
  
## <a name="see-also"></a> Voir aussi  
 [Générer des scripts &#40;SQL Server Management Studio&#41;](../../relational-databases/scripting/generate-scripts-sql-server-management-studio.md)   
 [Copier des bases de données sur d'autres serveurs](../../relational-databases/databases/copy-databases-to-other-servers.md)  
  
  
