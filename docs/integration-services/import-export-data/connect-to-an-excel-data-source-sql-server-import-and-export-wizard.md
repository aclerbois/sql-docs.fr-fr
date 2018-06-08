---
title: Se connecter à une source de données Excel (Assistant Importation et Exportation SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 04/02/2018
ms.prod: sql
ms.prod_service: integration-services
ms.component: import-export-data
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 43fbaca0-36d8-4583-9056-af7010209b87
caps.latest.revision: 12
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 4dc09d4196975ebbbad7abe937b2d4eb14a89ff8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connect-to-an-excel-data-source-sql-server-import-and-export-wizard"></a>Se connecter à une source de données Excel (Assistant Importation et Exportation SQL Server)
Cet article vous montre comment vous connecter à une source de données **Microsoft Excel** à partir de la page **Choisir une source de données** ou **Choisir une destination** de l’Assistant Importation et Exportation SQL Server.

La capture d’écran suivante montre un exemple de connexion à un classeur Microsoft Excel.

![Connexion à Excel](../../integration-services/import-export-data/media/excel-connection.png) 

Vous pouvez être amené à télécharger et installer des fichiers supplémentaires pour vous connecter aux fichiers Excel. Pour plus d’informations, consultez [Se procurer les fichiers nécessaires pour se connecter à Excel](../load-data-to-from-excel-with-ssis.md#files-you-need).

> [!IMPORTANT]
> Pour obtenir des informations détaillées sur la connexion à des fichiers Excel, et sur les limitations et les problèmes connus liés au chargement de données depuis ou vers des fichiers Excel, consultez [Charger des données depuis ou vers Excel avec SQL Server Integration Services (SSIS)](../load-data-to-from-excel-with-ssis.md).

## <a name="options-to-specify"></a>Options à spécifier

> [!NOTE]
> Les options de connexion de ce fournisseur de données sont les mêmes, qu’Excel soit la source ou la destination. Autrement dit, les options que vous voyez sont identiques dans les pages **Choisir une source de données** et **Choisir une destination** de l’Assistant.

**Chemin de fichier Excel**  
 Spécifiez le chemin et le nom du fichier Excel. Exemple :
-   Pour un fichier sur un ordinateur local, **C:\\MyData.xlsx**.
-   Pour un fichier sur un partage réseau, **\\\\Ventes\\Base de données\\Northwind.xlsx**.

Ou cliquez sur **Parcourir**.  
  
 **Parcourir**  
 Permet de rechercher la feuille de calcul à l’aide de la boîte de dialogue **Ouvrir**.  

> [!NOTE]
> L’Assistant ne peut pas ouvrir un fichier Excel protégé par mot de passe.

 **Version Excel**  
Permet de sélectionner la version d’Excel utilisée par la source ou le classeur de destination.

**La première ligne possède des noms de colonnes**  
Indiquez si la première ligne de données contient des noms de colonne.
-   Si les données ne contiennent aucun nom de colonne et que vous activez cette option, l’Assistant traite la première ligne de données comme des noms de colonne.
-   Si les données contiennent des noms de colonne, mais que vous désactivez cette option, l’Assistant traite la ligne de noms de colonne comme étant la première ligne de données.

Si vous précisez que les données sources n’ont pas de noms de colonne, l’Assistant utilise F1, F2, et ainsi de suite comme titres de colonne.

## <a name="i-dont-see-excel-in-the-list-of-data-sources"></a>Je ne vois pas Excel dans la liste des sources de données
Si vous ne voyez pas Excel dans la liste des sources de données, utilisez-vous l’Assistant 64 bits ? Les fournisseurs pour Excel et Access sont généralement en 32 bits et ils ne sont donc pas visibles dans l’Assistant 64 bits. Exécutez l’Assistant 32 bits à la place.

> [!NOTE]
> Pour utiliser la version 64 bits de l’Assistant Importation et Exportation SQL Server, vous devez installer SQL Server. SQL Server Data Tools (SSDT) et SQL Server Management Studio (SSMS) sont des applications 32 bits qui installent uniquement des fichiers 32 bits, y compris la version 32 bits de l’Assistant.

## <a name="see-also"></a>Voir aussi
[Charger des données depuis ou vers Excel avec SQL Server Integration Services (SSIS)](../load-data-to-from-excel-with-ssis.md)  
[Choisir une source de données](../../integration-services/import-export-data/choose-a-data-source-sql-server-import-and-export-wizard.md)  
[Choisir une destination](../../integration-services/import-export-data/choose-a-destination-sql-server-import-and-export-wizard.md)

