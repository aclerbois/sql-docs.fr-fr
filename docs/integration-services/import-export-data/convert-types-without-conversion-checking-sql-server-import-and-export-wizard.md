---
title: Convertir les types et ne pas vérifier la conversion (Assistant Importation-Exportation SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 01/11/2017
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
- sql13.dts.impexpwizard.nomappingfile.f1
ms.assetid: 87d9d3e5-477f-4117-a37f-bff53ea3e14d
caps.latest.revision: 25
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 9c493d453d11648f75eddd4a426ec3d9bdd26a45
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="convert-types-without-conversion-checking-sql-server-import-and-export-wizard"></a>Convertir les types sans vérification de la conversion (Assistant Importation et Exportation SQL Server)
  Une fois que vous avez sélectionné les tables et vues existantes pour copier ou vérifier la requête que vous avez fournie, l’Assistant Importation et Exportation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut afficher **Convertir les types sans vérification de la conversion**. L’Assistant affiche cette page quand il ne peut pas trouver un ou plusieurs fichiers de conversion et de mappage de types de données dont il a besoin pour mapper les types de données entre la source et la destination. La page inclut des informations qui vous aident à comprendre ce qui est manquant.
  
 Pour continuer sans connaître l’issue des conversions des types de données, cliquez sur **Suivant** . Sinon, cliquez sur **Précédent** pour modifier vos sélections, ou cliquez sur **Annuler** pour quitter l’Assistant.

## <a name="screen-shot-of-the-convert-types-page"></a>Capture d’écran de la page Convertir des types  
  
La capture d’écran suivante montre un exemple de la page **Convertir les types sans vérification de la conversion** de l’Assistant.

![Convertir les types](../../integration-services/import-export-data/media/convert-types.png)

Le problème ici est que l’Assistant ne peut pas trouver de fichier de mappage qui mappe les types de données pour la destination sélectionnée.

Les informations de cette page n’incluent pas le nom du fichier de mappage manquant. Étant donné que l’Assistant ne sait pas si le fichier existe pour le fournisseur de données spécifié, il ne peut pas fournir de nom pour le fichier manquant.

## <a name="whats-next"></a>Quelle est l’étape suivante ?  
 Après avoir cliqué sur **Suivant** pour poursuivre sans savoir si les conversions des types de données vont réussir, vous tombez sur la page **Enregistrer et exécuter le package**. Dans cette page, vous spécifiez si vous souhaitez exécuter l’opération de copie immédiatement. Selon votre configuration, vous pouvez également être en mesure d’enregistrer le package [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] créé par l’Assistant pour le personnaliser et le réutiliser ultérieurement. Pour plus d’informations, consultez [Enregistrer et exécuter le package](../../integration-services/import-export-data/save-and-run-package-sql-server-import-and-export-wizard.md).  

## <a name="see-also"></a>Voir aussi
[Mappage de type de données dans l’Assistant Importation et Exportation SQL Server](../../integration-services/import-export-data/data-type-mapping-in-the-sql-server-import-and-export-wizard.md)
