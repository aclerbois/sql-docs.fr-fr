---
title: Se connecter à une source de données Access (Assistant Importation et Exportation SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 06/20/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: import-export-data
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: b44c159a-c33d-4f3c-bdb8-9832f35317c8
caps.latest.revision: 11
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 87efb901b3bdf01140db90b3aa2ee0d72d2ae67a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connect-to-an-access-data-source-sql-server-import-and-export-wizard"></a>Se connecter à une source de données Access (Assistant Importation et Exportation SQL Server)
Cette rubrique décrit comment se connecter à une source de données **Microsoft Access** à partir de la page **Choisir une source de données** ou **Choisir une destination** de l’Assistant Importation et Exportation SQL Server.

La capture d’écran suivante montre un exemple de connexion à une base de données Microsoft Access. Dans cet exemple, vous n’avez pas besoin d’entrer un nom d’utilisateur et un mot de passe car la base de données cible n’utilise pas de fichier de groupe de travail.

![Connexion à Access](../../integration-services/import-export-data/media/connect-to-access.jpg)

## <a name="options-to-specify"></a>Options à spécifier

> [!NOTE]
> Les options de connexion de ce fournisseur de données sont les mêmes qu’Access soit la source ou la destination. Autrement dit, les options que vous voyez sont identiques dans les pages **Choisir une source de données** et **Choisir une destination** de l’Assistant.

**Source de données**  
La liste de fournisseurs de données peut contenir plusieurs entrées pour Microsoft Access. Sélectionnez la dernière version installée, ou la version correspondant à la version d’Access ayant créé le fichier de base de données.

|Source de données|Version d’Office|
|-------|-------|
|Microsoft Access (Microsoft.ACE.OLEDB.16.0)|Office 2016|
|Microsoft Access (Microsoft.ACE.OLEDB.15.0)|Office 2013|
|Microsoft Access (Moteur de base de données Microsoft Access)|Office 2010 et Office 2007|
|Microsoft Access (Moteur de base de données Microsoft Jet)|Versions d’Office antérieures à Office 2007|

> [!IMPORTANT]
> Vous devrez peut-être télécharger et installer des fichiers supplémentaires pour vous connecter aux bases de données Access. Pour plus d’informations, consultez [Se procurer les fichiers nécessaires pour se connecter à Access](#officeDownloads) dans cette page.

 **Nom de fichier**  
Spécifiez le chemin et le nom de fichier du fichier Access. Par exemple, **C:\\MyData.mdb** pour un fichier sur l’ordinateur local ou **\\\\Sales\\Database\\Northwind.mdb** pour un fichier sur un partage réseau. Ou cliquez sur **Parcourir**. 

 >   [!NOTE] 
 > Si vous cliquez sur **Parcourir** pour localiser le fichier Access, la boîte de dialogue **Ouvrir** filtre les fichiers pour n’afficher par défaut que les fichiers dans l’ancien format et portant l’ancienne extension de fichier .MDB. Toutefois, le fournisseur de données peut également ouvrir des fichiers dans le nouveau format et portant la nouvelle extension de fichier .ACCDB.
  
 **Parcourir**  
 Permet de rechercher le fichier de la base de données à l’aide de la boîte de dialogue **Ouvrir**.  
  
 **User name**  
Si un fichier de groupe de travail est associé à la base de données, fournissez un nom d’utilisateur valide.  
  
 **Mot de passe**  
Si un fichier de groupe de travail est associé à la base de données, fournissez le mot de passe de l’utilisateur ici.
 
Si la base de données est protégée par un mot de passe unique pour tous les utilisateurs, consultez [Le fichier de base de données est-il protégé par un mot de passe ?](#database_password).
  
 **Avancé**  
Spécifiez les options avancées, comme le mot de passe de la base de données ou un autre fichier de groupe de travail que celui défini par défaut, dans la boîte de dialogue **Propriétés des liaisons de données**.  

## <a name="i-dont-see-access-in-the-list-of-data-sources"></a>Je ne vois pas Access dans la liste des sources de données
Si vous ne voyez pas Access dans la liste des sources de données, utilisez-vous l’Assistant 64 bits ? Les fournisseurs pour Excel et Access sont généralement en 32 bits et ils ne sont donc pas visibles dans l’Assistant 64 bits. Exécutez l’Assistant 32 bits à la place.

> [!NOTE]
> Pour utiliser la version 64 bits de l’Assistant Importation et Exportation SQL Server, vous devez installer SQL Server. SQL Server Data Tools (SSDT) et SQL Server Management Studio (SSMS) sont des applications 32 bits qui installent uniquement des fichiers 32 bits, notamment la version 32 bits de l’Assistant.

## <a name="officeDownloads"></a>Se procurer les fichiers nécessaires pour se connecter à Access  
Vous devrez peut-être télécharger les composants de connectivité pour les sources de données Microsoft Office, notamment Access et Excel, s’ils ne sont pas déjà installés. Téléchargez la dernière version des composants de connectivité pour les fichiers Excel et Access ici : [Microsoft Access Database Engine 2016 Redistributable](https://www.microsoft.com/download/details.aspx?id=54920).
  
La dernière version des composants peut ouvrir des fichiers créés dans des versions antérieures d’Access.

Si la version 32 bits de Microsoft Office est installée sur l’ordinateur, vous devez installer la version 32 bits des composants et vérifier que vous exécutez le package en mode 32 bits.

Si vous avez un abonnement Office 365, veillez à télécharger Access Database Engine 2016 Redistributable et non Microsoft Access 2016 Runtime. Lorsque vous exécutez le programme d’installation, il est possible qu’un message d’erreur s’affiche indiquant que vous ne pouvez pas installer le téléchargement côte à côte avec les composants Office « Démarrer en un clic ». Pour contourner ce message d’erreur, exécutez l’installation en mode silencieux en ouvrant une fenêtre d’invite de commandes et en exécutant le fichier .EXE que vous avez téléchargé avec l’option `/quiet`. Exemple :

`C:\Users\<user name>\Downloads\AccessDatabaseEngine.exe /quiet`

## <a name="database_password"></a> Le fichier de base de données est-il protégé par un mot de passe ?
Dans certains cas, une base de données Access est protégée par un mot de passe mais n’utilise pas de fichier de groupe de travail. Tous les utilisateurs doivent fournir le même mot de passe mais n’ont pas besoin d’entrer un nom d’utilisateur. Pour fournir un mot de passe de base de données, effectuez les étapes suivantes :

1.  Dans la page **Choisir une source de données** ou **Choisir une destination**, cliquez sur le bouton **Avancé** pour ouvrir la boîte de dialogue **Propriétés des liaisons de données**.  
2.  Dans la boîte de dialogue **Propriétés des liaisons de données**, sélectionnez l’onglet **Tout**.  
3.  Dans la liste des propriétés et des valeurs, sélectionnez **Jet OLEDB:Database Password**.   
    
    ![Spécifier le mot de passe Access, écran 1](../../integration-services/import-export-data/media/specify-access-password-screen-1.jpg) 
4.  Cliquez sur **Modifier la valeur** pour ouvrir la boîte de dialogue **Modifier la valeur d’une propriété**.  
    
    ![Spécifier le mot de passe Access, écran 2](../../integration-services/import-export-data/media/specify-access-password-screen-2.jpg)
5.  La boîte de dialogue **Modifier la valeur d’une propriété**, entrez le mot de passe de la base de données.
6.  Cliquez sur **OK** dans chaque boîte de dialogue pour revenir dans la page **Choisir une source de données** ou **Choisir une destination** de l’Assistant et continuer.

## <a name="keep-your-autonumber-values-when-you-export-from-access"></a>Conserver les valeurs de numérotation automatique ou d’identité quand vous exportez depuis Access
Pour que les valeurs d’identité existantes dans les données sources puissent être insérées dans une colonne d’identité de la table de destination, choisissez l’option **Activer l’insertion d’identité** dans la boîte de dialogue **Mappage de colonnes**. Par défaut, la colonne d’identité de destination ne permet pas d’insérer des valeurs existantes. Pour afficher la boîte de dialogue **Mappage de colonnes**, sélectionnez **Modifier les mappages** dans la page **Sélectionner les tables et les vues sources de l’Assistant**. Ces pages sont décrites dans [Sélectionner les tables et les vues sources](../../integration-services/import-export-data/select-source-tables-and-views-sql-server-import-and-export-wizard.md) et [Mappage de colonnes](../../integration-services/import-export-data/column-mappings-sql-server-import-and-export-wizard.md).

En général, si vos clés primaires existantes sont dans une colonne d’identité, une colonne NuméroAuto ou son équivalent, vous devez sélectionner cette option pour conserver les valeurs de vos clés primaires existantes. Sinon, la colonne d’identité de destination assigne de nouvelles valeurs.

## <a name="see-also"></a>Voir aussi
[Choisir une source de données](../../integration-services/import-export-data/choose-a-data-source-sql-server-import-and-export-wizard.md)  
[Choisir une destination](../../integration-services/import-export-data/choose-a-destination-sql-server-import-and-export-wizard.md)

