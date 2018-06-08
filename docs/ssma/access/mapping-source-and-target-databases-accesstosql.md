---
title: Mappage de Source et bases de données cibles (AccessToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-access
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
helpviewer_keywords:
- database schemas
- mapping, databases
- schemas, mapping to
- schemas, SQL Azure
- schemas, SQL Server
- source database
- target database
ms.assetid: 69bee937-7b2c-49ee-8866-7518c683fad4
caps.latest.revision: 17
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: ade8c301a8cb519c22413a4db9a236f58f5dd870
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="mapping-source-and-target-databases-accesstosql"></a>Source de mappage et de bases de données cibles (AccessToSQL)
Lorsque vous vous connectez à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure, vous devez spécifier une base de données cible pour la migration. Si vous avez plusieurs bases de données Access vous pouvez les mapper à plusieurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] bases de données (ou les schémas) ou à plusieurs schémas sous la base de données SQL Azure connectée.  
  
## <a name="sql-server-or-sql-azure-database-schemas"></a>SQL Server ou les schémas de base de données Azure SQL  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] bases de données utilisent le concept de schémas pour séparer des objets au sein d’une base de données en groupes logiques. Par exemple, une base de données de bibliothèque peut utiliser trois schémas nommés **la documentation**, **audio**, et **vidéo** pour séparer les objets de données audio et vidéo book, entre eux. Par défaut, la base de données access est mappé à **master** base de données et **dbo** schéma dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] et à la base de données connectée et **dbo** schéma dans SQL Azure.  
  
Sauf si vous personnalisez le mappage entre chaque base de données Access et la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données et le schéma, SSMA allez migrer tous les schémas et les données associées à la base de données de l’accès à la base de données par défaut mappé.  
  
## <a name="modifying-the-target-database-and-schema"></a>Modification de la base de données cible et le schéma  
SSMA vous permet de mapper chaque base de données Access [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou de base de données SQL Azure et de schéma. La procédure suivante décrit comment personnaliser le mappage par base de données.  
  
**Pour modifier la base de données cible et le schéma**  
  
1.  Dans le volet Explorateur de métadonnées d’accès, sélectionnez **-accès aux métadonnées**.  
  
    Mappage de schéma est également disponible lorsque vous sélectionnez le **bases de données** nœud ou n’importe quel nœud de la base de données. La liste de mappage de schéma est personnalisée pour l’objet sélectionné.  
  
2.  Dans le volet droit, cliquez sur le **schéma de mappage** onglet.  
  
    Vous verrez une table contenant les accès des noms de base de données et sa ssNoVersion correspondante ou son schéma de Sql Azure. Le schéma cible est représentée dans une notation de deux parties (database.schema).  
  
3.  Sélectionnez la ligne qui contient le mappage que vous souhaitez personnaliser, puis cliquez sur **modifier**.  
  
4.  Dans le **choisir un schéma cible** boîte de dialogue, vous pourrez consulter pour la base de données cible disponible et de schéma ou de type de la base de données et le schéma de nom dans la zone de texte dans une notation de deux parties (database.schema), puis **OK**.  
  
**Modes de mappage**  
  
-   Mappage à SQL Server  
  
Vous pouvez mapper la base de données source vers une base de données cible. Par défaut la base de données source est mappé à la cible [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données avec laquelle vous vous êtes connecté à l’aide de SSMA. Si la base de données cible en cours de mappage n’existe pas sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], puis vous serez invité avec un message **« la base de données et/ou un schéma n’existe pas dans la cible [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] métadonnées. Il est créé lors de la synchronisation. Voulez-vous continuer ? »** Cliquez sur Oui. De même, vous pouvez mapper le schéma non existants de schéma sous la cible [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données qui sera créé pendant la synchronisation.  
  
-   Mappage à SQL Azure  
  
Vous pouvez mapper la base de données source à la cible connectée [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données ou à n’importe quel schéma dans la cible connectée [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données. Si vous mappez la source de schéma à n’importe quel schéma inexistante sous base de données cibles connectés, vous serez invité avec un message **« schéma n’existe pas dans les métadonnées de la cible. Il est créé lors de la synchronisation. Voulez-vous continuer ? »** Cliquez sur Oui.  
  
## <a name="reverting-to-your-initial-database-and-schema"></a>Retour à votre base de données initiale et le schéma  
Si vous personnalisez le mappage entre une base de données Access et un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou base de données SQL Azure et le schéma, vous pouvez annuler le mappage à la base de données que vous avez spécifié lorsque vous connecté à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure.  
  
**Pour réinitialiser le schéma et de la base de données de la valeur par défaut**  
  
1.  Sous l’onglet mappage de schéma, sélectionnez n’importe quelle ligne, puis cliquez sur **rétablir par défaut** pour rétablir la base de données par défaut et le schéma.  
  
## <a name="next-step"></a>Étape suivante  
L’étape suivante du processus de migration est [convertir des objets de base de données](http://msdn.microsoft.com/en-us/e0ef67bf-80a6-4e6c-a82d-5d46e0623c6c)  
  
## <a name="see-also"></a>Voir aussi  
[Migration des bases de données de l’accès à SQL Server](http://msdn.microsoft.com/en-us/76a3abcf-2998-4712-9490-fe8d872c89ca)  
  
