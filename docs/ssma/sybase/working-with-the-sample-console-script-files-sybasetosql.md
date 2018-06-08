---
title: Travailler avec les fichiers de Script de la Console exemple (SybaseToSQL) | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-sybase
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
helpviewer_keywords:
- Sybase Console,Sample Console Script Files
ms.assetid: ef221118-b442-4ca6-9409-6ee1d9f8d948
caps.latest.revision: 9
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 04f24032be49973a03ffd8d79b9c642b244c0ef2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="working-with-the-sample-console-script-files-sybasetosql"></a>Travailler avec les fichiers de Script de la Console exemple (SybaseToSQL)
Quelques exemples de fichiers ont été fournis avec le produit pour la référence de l’utilisateur et son utilisation. Cette section décrit la façon de personnaliser ces scripts en fonction des besoins des utilisateurs finaux.  
  
## <a name="sample-console-script-files"></a>Exemples de fichiers de Script Console  
Les fichiers de script de console exemple suivants couvrant les différents scénarios ont été fournies pour la référence de l’utilisateur :  
  
-   ServersConnectionFileSample.xml  
  
-   VariableValueFileSample.xml  
  
-   AssessmentReportGenerationSample.xml  
  
-   SqlStatementConversionSample.xml  
  
-   ConversionAndDataMigrationSample.xml  
  
-   **ServersConnectionFileSample.xml :**  
  
    -   Cet exemple donne les différents modes de connexion n’est disponible pour la base de données source et cible et l’utilisateur peut sélectionner n’importe quel mode conformément à la spécification. Cet exemple contient les définitions de serveur.  
  
    -   L’utilisateur peut se connecter à la base de données requis en modifiant simplement les valeurs à la source requis et les définitions de serveur cible. Dans l’exemple fourni toutes les valeurs ont été fournis comme variable les valeurs qui sont disponibles dans le **VariableValueFileSample.xml**.  Tous les autres paramètres de connexion peuvent être supprimées du fichier de connexion de serveur de travail de l’utilisateur.  
  
    -   Pour plus d’informations sur la connexion au serveur source et cible, consultez [création des fichiers de connexion serveur &#40;SybaseToSQL&#41;](../../ssma/sybase/creating-the-server-connection-files-sybasetosql.md).  
  
-   **VariableValueFileSample.xml :**  
    Fichiers de script de toutes les variables qui ont été utilisés dans l’exemple de console et `ServersConnectionFileSample.xml` ont été assemblés dans ce fichier. Pour exécuter les exemples de scripts de console que l’utilisateur a simplement à remplacer la variable de l’exemple avec l’utilisateur, les valeurs celles définies et passer ce fichier comme un argument de ligne de commande supplémentaires, ainsi que le fichier de script.  
  
    Pour plus d’informations sur le fichier de valeurs Variable, consultez [création de fichiers de valeur Variable &#40;SybaseToSQL&#41;](../../ssma/sybase/creating-variable-value-files-sybasetosql.md).  
  
-   **AssessmentReportGenerationSample.xml :**  
    Cet exemple permet à l’utilisateur Générer un rapport d’évaluation xml qui peut être utilisé par l’utilisateur pour l’analyse avant de commencer à convertir et migrer des données.  
  
    Dans le `generate-assessment-report` l’utilisateur doit obligatoirement modifier la valeur de la variable de commande (consultez **VariableValueFileSample.xml**) dans le `object-name` attribut le nom de base de données est en cours d’utilisation par l’utilisateur. Selon le type d’objet spécifié, le `object-type` valeur devra également être modifié.  
  
    Si l’utilisateur doit évaluer plusieurs objets / bases de données qu’il peut spécifier plusieurs `metabase-object` nœuds, comme illustré dans le `generate-assessment-report` 4 d’exemple de la commande de l’exemple de fichier de script de console.  
  
    Pour plus d’informations sur la génération de rapports, consultez [génération de rapports &#40;SybaseToSQL&#41;](../../ssma/sybase/generating-reports-sybasetosql.md).  
  
    > [!NOTE]  
    > -   Assurez-vous que l’argument de ligne de commande de fichier de valeur de la variable est passée à l’application de console et VariableValueFileSample.xml est mis à jour avec l’utilisateur spécifié les valeurs.  
    > -   Assurez-vous qu’argument de ligne de commande de fichier de connexion serveur est passé à l’application de console et le ServersConnectionFileSample.xml est mis à jour avec les valeurs de paramètre de serveur correct.  
  
-   **SqlStatementConversionSample.xml :**  
    Cet exemple permet à l’utilisateur générer le correspondant `t-sql` script pour la base de données source `sql` fournie comme entrée de commande.  
  
    Dans le `convert-sql-statement` l’utilisateur doit obligatoirement modifier la valeur de la variable de commande (consultez **VariableValueFileSample.xml**) dans le `context` nom à l’attribut de base de données qui est en cours d’utilisation par l’utilisateur. L’utilisateur devra également modifier le `sql` valeur d’attribut à la base de données source `sql` commande il nécessitant à convertir.  
  
    L’utilisateur peut également fournir les fichiers sql à convertir. Cela a été illustré dans le `convert-sql-statement` 4 d’exemple de la commande de l’exemple de fichier de script de console.  
  
    > [!NOTE]  
    > Assurez-vous que l’argument de ligne de commande de fichier de valeur de la variable est passée à l’application de console et VariableValueFileSample.xml est mis à jour avec l’utilisateur spécifié les valeurs.  
  
-   **ConversionAndDataMigrationSample.xml :**  
     Cet exemple permet à l’utilisateur effectuer une migration de bout en bout à partir de la conversion de la migration des données. Vous trouverez ci-dessous la liste des valeurs d’attribut obligatoires qui ils devront modifier :  
  
    **Nom de la commande**  
  
    `map-schema`  
  
    Mappage de schéma de base de données source vers le schéma cible.  
  
    **Attribut**  
  
    -   `source-schema:` Spécifie la base de données source qui nécessite d’être converti.  
  
    -   `sql-server-schema`: Spécifie la base de données cible qui doit être migré vers  
  
    **Nom de la commande**  
  
    `convert-schema`  
  
    -   Effectue une conversion de schéma à partir de la source vers le schéma cible.  
  
    -   Si l’utilisateur doit évaluer plusieurs objets / bases de données qu’il peut spécifier plusieurs `metabase-object` nœuds, comme illustré dans le `convert-schema` 4 d’exemple de la commande de l’exemple de fichier de script de console.  
  
    **Attribut**  
  
    `object-name`: Permet de spécifier la base de données source / nom qui nécessite d’être converti de l’objet. Vérifiez que le correspondant `object-type` est modifié en fonction du type d’objet qui est spécifié dans le `object-name`  
  
    **Nom de la commande**  
  
    `synchronize-target`  
  
    -   Synchronise les objets cibles avec la base de données cible.  
  
    -   Si l’utilisateur doit évaluer plusieurs objets / bases de données qu’il peut spécifier plusieurs `metabase-object` nœuds, comme illustré dans le `synchronize-target` 3 d’exemple de la commande de l’exemple de fichier de script de console.  
  
    **Attribut**  
  
    `object-name:` Spécifiez la base de données sql server / nom qui nécessite la création de l’objet. Vérifiez que le correspondant `object-type` est modifié en fonction du type d’objet qui est spécifié dans le `object-name`  
  
    **Nom de la commande**  
  
    `migrate-data`  
  
    -   Migre les données source à la cible.  
  
    -   Si l’utilisateur doit évaluer plusieurs objets / bases de données qu’il peut spécifier plusieurs `metabase-object` nœuds, comme illustré dans le `migrate-data` exemple 2 de la commande de l’exemple de fichier de script de console.  
  
    **Attribut**  
  
    `object-name:` Spécifie la source de données / tables nom qui nécessite d’être migrée. Vérifiez que le correspondant `object-type` est modifié en fonction du type d’objet qui est spécifié dans le `object-name`  
  
## <a name="see-also"></a>Voir aussi  
[Création de fichiers de la valeur de la Variable &#40;SybaseToSQL&#41;](../../ssma/sybase/creating-variable-value-files-sybasetosql.md)  
[Création des fichiers de connexion serveur &#40;SybaseToSQL&#41;](../../ssma/sybase/creating-the-server-connection-files-sybasetosql.md)  
[Génération de rapports &#40;SybaseToSQL&#41;](../../ssma/sybase/generating-reports-sybasetosql.md)  
  
