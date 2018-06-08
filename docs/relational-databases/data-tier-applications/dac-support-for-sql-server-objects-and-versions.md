---
title: Prise en charge DAC pour les objets et versions SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: data-tier-applications
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-data-tier-apps
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- data-tier application [SQL Server], supported objects
- objects [SQL Server], data-tier applications
ms.assetid: b1b78ded-16c0-4d69-8657-ec57925e68fd
caps.latest.revision: 19
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 52d1896a61c0238337d8730c4038861841fd6124
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="dac-support-for-sql-server-objects-and-versions"></a>Prise en charge DAC pour les objets et versions SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Une application de la couche Données (DAC) prend en charge les objets du [!INCLUDE[ssDE](../../includes/ssde-md.md)] les plus couramment utilisés.  
  
 **Dans cette rubrique**  
  
-   [Objets SQL Server pris en charge](#SupportedObjects)  
  
-   [Prise en charge de l'application de la couche Données par les versions de SQL Server](#SupportByVersion)  
  
-   [Limitations lors du déploiement de données](#DeploymentLimitations)  
  
-   [Considérations supplémentaires concernant les actions de déploiement](#Considerations)  
  
##  <a name="SupportedObjects"></a> Objets SQL Server pris en charge  
 Seuls les objets pris en charge peuvent être spécifiés dans une application de la couche Données lors de sa création ou de sa modification. Vous n'êtes pas autorisé à extraire, inscrire ou importer une DAC à partir d'une base de données existante qui contient des objets qui ne sont pas pris en charge dans une DAC. [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] prend en charge les objets suivants dans une DAC.  
  
|||  
|-|-|  
|DATABASE ROLE|FUNCTION : table incluse|  
|FUNCTION : table à instructions multiples|FUNCTION : scalaire|  
|INDEX : cluster|INDEX : non-cluster|  
|INDEX : spatial|INDEX : unique|  
|LOGIN|Autorisations|  
|Appartenances aux rôles|SCHEMA|  
|Statistiques|PROCÉDURE STOCKÉE : Transact-SQL|  
|Synonymes|TABLE : contrainte de validation|  
|TABLE : classement|TABLE : colonne, notamment les colonnes calculées|  
|TABLE : contrainte, par défaut|TABLE : contrainte, clé étrangère|  
|TABLE : contrainte, index|TABLE : contrainte, clé primaire|  
|TABLE : contrainte, unique|TRIGGER : DML|  
|TYPE : HIERARCHYID, GEOMETRY, GEOGRAPHY|TYPE : type de données défini par l'utilisateur|  
|TYPE : type de table défini par l'utilisateur|USER|  
|VIEW||  
  
##  <a name="SupportByVersion"></a> Prise en charge de l'application de la couche Données par les versions de SQL Server  
 Les versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ont des niveaux de prise en charge différents selon les opérations DAC. Toutes les opérations DAC prises en charge par une version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont prises en charge par toutes les éditions de cette version.  
  
 Les instances du [!INCLUDE[ssDE](../../includes/ssde-md.md)] prennent en charge les opérations DAC suivantes :  
  
-   L'exportation et l'extraction sont prises en charge dans toutes les versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]prises en charge.  
  
-   Toutes les opérations sont prises en charge dans [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)] et dans toutes les versions de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], et [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].  
  
-   Toutes les opérations sont prises en charge dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] Service Pack 2 (SP2) ou version ultérieure et dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] SP4 ou version ultérieure.  
  
 Le DAC Framework comporte les outils côté client pour générer et traiter les packages et les fichiers d'exportation DAC. Le DAC Framework est inclus dans les produits suivants.  
  
-   [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] incluent le DAC Framework 3.0, qui prend en charge toutes les opérations DAC.  
  
-   [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP1 et Visual Studio 2010 SP1 incluent le DAC Framework 1.1, qui prend en charge toutes les opérations DAC à l'exception des opérations d'exportation et d'importation.  
  
-   [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] et Visual Studio 2010 incluent le DAC Framework 1.0, qui prend en charge toutes les opérations DAC à l'exception des opérations d'exportation, d'importation et de mise à niveau sur place.  
  
-   Les outils clients des versions antérieures de SQL Server ou Visual Studio ne prennent pas en charge les opérations DAC.  
  
 Un package DAC ou un fichier d'exportation créé avec une version de DAC Framework ne peut pas être traité par une version antérieure du DAC Framework. Par exemple, un package DAC extrait à l'aide des outils clients de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] ne peut pas être déployé à l'aide des outils clients de [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] .  
  
 Un package DAC ou un fichier d'exportation créé avec une version de DAC Framework peut être traité par toute version ultérieure du DAC Framework. Par exemple, un package DAC extrait à l'aide des outils clients de [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] peut être déployé à l'aide de [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP1 ou des outils clients d'une version supérieure.  
  
##  <a name="DeploymentLimitations"></a> Limitations lors du déploiement de données  
 Notez les limitations de fidélité dans le moteur de déploiement de données du DAC Framework dans SQL Server 2012 SP1. Les limitations s'appliquent aux actions suivantes du DAC Framework : déployer ou publier un fichier .dacpac, et importer un fichier .bacpac.  
  
1.  La perte des métadonnées de certaines conditions et types de base dans les colonnes sql_variant. Dans les cas affectés, un avertissement avec le message suivant s’affiche :  **Certaines propriétés sur certains types de données utilisés dans une colonne sql_variant ne sont pas conservées lorsqu’elles sont déployées par le DAC Framework.**  
  
    -   Types de base MONEY, SMALLMONEY, NUMERIC, DECIMAL : la précision n'est pas conservée.  
  
        -   Types de base de DECIMAL/NUMERIC avec une précision de 38 : les métadonnées sql_variant « TotalBytes » sont toujours définies sur 21.  
  
    -   Tous les types de base de texte : le classement par défaut de la base de données est appliqué à l'ensemble du texte.  
  
    -   Types de base BINARY : la propriété de longueur maximale n'est pas conservée.  
  
    -   Types de base TIME, DATETIMEOFFSET : la précision est toujours définie à 7.  
  
2.  Perte de données dans les colonnes sql_variant. Dans le cas affecté, un avertissement avec le message suivant s'affiche : **Des données seront perdues si une valeur dans une colonne sql_variant DATETIME2 avec une échelle supérieure à 3 est déployée par le DAC Framework. La valeur DATETIME2 est limitée à une échelle égale à 3 pendant le déploiement.**  
  
    -   Type de base DATETIME2 avec une échelle supérieure à 3 : l'échelle est limitée pour être égale à 3.  
  
3.  L'opération de déploiement échoue dans les cas suivants dans les colonnes sql_variant. Dans les cas affectés, une boîte de dialogue contenant le message suivant s'affiche :  **L'opération a échoué en raison des limitations de données dans le DAC Framework.**  
  
    -   Types de base DATETIME2, SMALLDATETIME et DATE : si la valeur se trouve hors de la plage DATETIME (par exemple, l'année est antérieure à 1753).  
  
    -   Type de base DECIMAL, NUMERIC : lorsque la précision de la valeur est supérieure à 28.  
  
##  <a name="Considerations"></a> Considérations supplémentaires concernant les actions de déploiement  
 Notez les points suivants pour les actions de déploiement de données DAC Framework :  
  
-   **Extraction/exportation** : pour les actions qui utilisent le DAC Framework dans le but de créer un package à partir d’une base de données (par exemple, extraire un fichier .dacpac, exporter un fichier .bacpac), ces restrictions ne s’appliquent pas. Les données du package sont une représentation haute fidélité des données dans la base de données source. Si l'une de ces conditions est présente dans le package, le journal d'extraction/exportation contient un résumé des problèmes au moyen des messages indiqués ci-dessus. Permet d'avertir l'utilisateur des problèmes potentiels de déploiement de données avec le package qu'ils ont créé. L'utilisateur voit également le message de synthèse suivant dans le journal : **Ces limitations n'affectent pas la fiabilité des types de données et des valeurs stockées dans le package DAC créé par le DAC Framework ; elles s'appliquent uniquement aux types de données et valeurs résultant du déploiement d'un package DAC dans une base de données. Pour plus d’informations sur les données affectées et la façon de contourner cette limitation, consultez** [cette rubrique](http://go.microsoft.com/fwlink/?LinkId=267086).  
  
-   **Déploiement/publication/importation** : pour les actions qui utilisent le DAC Framework dans le but de déployer un package dans une base de données, par exemple pour déployer ou publier un fichier .dacpac et importer un fichier .bacpac, ces limitations s’appliquent. Les données qui donnent la base de données cible ne doivent pas contenir une représentation haute fidélité des données du package. Le journal de déploiement/importation contient un message, indiqué ci-dessus, pour chaque instance dans laquelle le problème est rencontré. L'opération est bloquée par les erreurs (consultez la catégorie 3 ci-dessus), mais se poursuit avec les autres avertissements.  
  
     Pour plus d’informations sur les données affectées dans ce scénario et la façon de contourner cette limitation pour les actions de déploiement/publication/importation, consultez [cette rubrique](http://go.microsoft.com/fwlink/?LinkId=267087).  
  
-   **Solutions de contournement** : les opérations d’extraction et d’exportation écrivent les fichiers de données BCP haute fidélité dans les fichiers .dacpac ou .bacpac. Pour éviter les limitations, utilisez l'utilitaire en ligne de commande SQL Server BCP.exe pour déployer des données haute fidélité dans une base de données cible d'un package DAC.  
  
## <a name="see-also"></a> Voir aussi  
 [Applications de la couche Données](../../relational-databases/data-tier-applications/data-tier-applications.md)  
  
  
