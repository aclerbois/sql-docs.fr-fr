---
title: L’exécution de la Console SSMA (SybaseToSQL) | Documents Microsoft
ms.custom: ''
ms.date: 09/27/2017
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
- Sybase Console,Database Connection Commands
- Sybase Console,Manageability Commands
- Sybase Console,Migration Commands
- Sybase Console,Migration Preparation Command
- Sybase Console,Project Commands
- Sybase Console,Report Commands
- Sybase Console,Script File Commands
- Sybase Console,Script Generation Commands
ms.assetid: ea8950b7-fabc-4aa4-89f8-9573a2617d70
caps.latest.revision: 22
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: bbad5207581a713327112b9d319c69f706ad2f61
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="executing-the-ssma-console-sybasetosql"></a>L’exécution de la Console SSMA (SybaseToSQL)
Microsoft vous fournit un ensemble robuste de script de commandes du fichier à exécuter et contrôler les activités SSMA. Les sections qui en découlent décrit en détail le même.  
  
## <a name="script-file-commands"></a>Commandes de fichier de script  
L’application console utilise certaines commandes de fichier de script standard comme énuméré dans cette section.  
  
## <a name="project-commands"></a>Commandes du projet  
Les commandes de projet gèrent la création de projets, ouvrir, enregistrer et les projets en cours de fermeture.  
  
### <a name="create-new-project"></a>créer à nouveau projet  
Cette commande crée un nouveau projet SSMA.  
  
-   `project-folder` Indique le dossier du projet créé.  
  
-   `project-name` Indique le nom du projet. {string}  
  
-   `overwrite-if-exists`Attribut facultatif indique si un projet existant doit être remplacé. {valeur} booléenne  
  
-   `project-type:`Attribut facultatif. Indique le type de projet, qui est le projet « sql server 2005 » ou « sql server 2008 » projet ou projet de « sql server 2012 » ou « sql server 2014 » projet ou projet « sql azure ». Valeur par défaut est « sql-server-2008 ».  
  
**Exemple de syntaxe :**  
  
```xml  
<create-new-project  
  
  project-folder="<project-folder>"  
  
  project-name="<project-name>"  
  
  overwrite-if-exists="<true/false>" (optional)  
  
   project-type=”<sql-server-2008/sql-server-2005/sql-server-2012/sql-server-2014/sql-azure>”  
/>  
```  
Est de l’attribut 'Remplacer-if-exists' **false** par défaut.  
  
Est de l’attribut 'type de projet' **sql-server-2008** par défaut.  
  
### <a name="open-project"></a>projet ouvert  
Cette commande ouvre le projet.

-   `project-folder` Indique le dossier du projet créé. La commande échoue si le dossier spécifié n’existe pas.  {string}  
  
-   `project-name` Indique le nom du projet. La commande échoue si le projet spécifié n’existe pas.  {string}  
  
**Exemple de syntaxe :**  
  
```xml  
<open-project  
  
  project-folder="<project-folder>"  
  
  project-name="<project-name>"  
  
/>  
```  
> [!NOTE]  
> SSMA pour l’Application de Console SAP ASE prend en charge la compatibilité descendante. Vous pouvez l’utiliser pour ouvrir des projets créés par une version précédente de SSMA.  
  
### <a name="save-project"></a>enregistrer le projet  
Cette commande enregistre le projet de migration.  
  
**Exemple de syntaxe :**  
  
```xml  
<save-project/>  
```  
  
### <a name="close-project"></a>projet de fermer  
Cette commande ferme le projet de migration.  
  
**Exemple de syntaxe :**  
  
```xml  
<close-project   
  if-modified="<save/error/ignore>"   (optional)  
/>  
```  
Attribut 'if-modifié' est facultatif, **ignorer** par défaut.  
  
## <a name="database-connection-commands"></a>Commandes de connexion de base de données  
Les commandes de connexion de base de données aident à vous connecter à la base de données.  
  
> [!NOTE]  
> - Le **Parcourir** fonctionnalité de l’interface utilisateur n’est pas prise en charge dans la console.  
> - Pour plus d’informations sur « Créer des fichiers de Script », consultez [création de fichiers de Script &#40;SybaseToSQL&#41;](../../ssma/sybase/creating-script-files-sybasetosql.md).  
  
### <a name="connect-source-database"></a>se connecter--base de données source  
Cette commande effectue la connexion à la base de données source et charge les métadonnées de haut niveau de la base de données source, mais pas toutes les métadonnées.
  
Si la connexion à la source ne peut pas être établie, une erreur est générée et l’application console arrête davantage l’exécution.
  
La définition de serveur est récupérée à partir de l’attribut de nom défini pour chaque connexion dans la section serveur de fichier de connexion de serveur ou le fichier de script.  
  
**Exemple de syntaxe :**  
  
```xml  
<connect-source-database  server="<server-unique-name>"/>  
```  
  
### <a name="force-load-sourcetarget-database"></a>force-charge-source/cible-base de données  
Cette commande charge les métadonnées de la source, et il est utile pour travailler sur le projet de migration hors connexion.  
  
Si la connexion à la source/cible ne peut pas être établie, une erreur est générée et l’application console arrête davantage l’exécution.  
  
Cette commande nécessite un ou plusieurs nœuds de la métabase en tant que paramètre de ligne de commande.  
  
**Exemple de syntaxe :**  
  
```xml  
<force-load metabase=”<source/target>” >  
  
  <metabase-object object-name="<object-name>"/>  
  
</force-load>  
```  
  
### <a name="reconnect-source-database"></a>reconnecter--base de données source  
Cette commande se reconnecte à la base de données source mais ne charge pas toutes les métadonnées, contrairement à la commande connect--base de données source.  
  
Si la (connexion avec la source re) ne peut pas être établie, une erreur est générée et l’application console arrête davantage l’exécution.  
  
**Exemple de syntaxe :**  
  
```xml  
<reconnect-source-database  server="<server-unique-name>"/>  
```  
  
### <a name="connect-target-database"></a>se connecter--base de données cible  
Cette commande se connecte à la base de données SQL Server et charge les métadonnées de haut niveau de la base de données cible mais pas les métadonnées entièrement.  
  
Si la connexion à la cible ne peut pas être établie, une erreur est générée et l’application console arrête davantage l’exécution.  
  
La définition de serveur est récupérée à partir de l’attribut de nom défini pour chaque connexion dans la section serveur de fichier de connexion de serveur ou le fichier de script.  
  
**Exemple de syntaxe :**  
  
```xml  
<connect-target-database  server="<server-unique-name>"/>  
```  
  
### <a name="reconnect-target-database"></a>reconnecter--base de données cible  
  
Cette commande se reconnecte à la base de données cible, mais ne charge pas toutes les métadonnées, contrairement à la commande connect--base de données cible.  
  
Si la (connexion à la cible re) ne peut pas être établie, une erreur est générée et l’application console arrête davantage l’exécution.  
  
**Exemple de syntaxe :**  
  
```xml  
<reconnect-target-database  server="<server-unique-name>"/>  
```  
  
## <a name="report-commands"></a>Commandes de rapport  
Les commandes de rapports génèrent des rapports sur les performances de diverses activités de la Console de SSMA.  
  
### <a name="generate-assessment-report"></a>Générer--rapport d’évaluation  
  
Cette commande génère des rapports d’évaluation de la base de données source.  
  
Si la connexion de base de données source n’est pas effectuée avant d’exécuter cette commande, une erreur est générée et l’application console se ferme.  
  
Impossible de se connecter au serveur de base de données source lors de l’exécution de la commande, entraîne également à la fin de l’application console.  
  
-   `conversion-report-folder:` Spécifie le dossier dans lequel le rapport d’évaluation peut être stocké. (attribut facultatif)  
  
-   `object-name:` Spécifie l’ou les objets pris en compte pour la génération de rapports d’évaluation (prend en charge les noms de l’objet ou un nom d’objet de groupe).  
  
-   `object-type:` Spécifie le type de l’objet appelé dans l’attribut de nom de l’objet (si la catégorie d’objet est spécifié, le type d’objet sera « catégorie »).  
  
-   `conversion-report-overwrite:` Spécifie s’il faut remplacer le dossier de rapport d’évaluation s’il existe déjà.  
  
    **Valeur par défaut :** false. (attribut facultatif)  
  
-   `write-summary-report-to:` Spécifie le chemin d’accès à laquelle le rapport sera généré.  
  
    Si seul le chemin d’accès du dossier est indiqué, puis de fichiers par nom **AssessmentReport&lt;n&gt;. XML** est créé. (attribut facultatif)  
  
    La création de rapports a deux sous-catégories supplémentaires :  
  
    -   `report-errors` (= « true/false », valeur par défaut en tant que « false » (attributs facultatifs))  
  
    -   `verbose` (= « true/false », valeur par défaut en tant que « false » (attributs facultatifs))  
  
**Exemple de syntaxe :**  
  
```xml  
<generate-assessment-report  
  
  object-name="<object-name>"  
  
  object-type="<object-category>"  
  
  write-summary-report-to="<file-name/folder-name>”             (optional)  
  
  verbose="<true/false>"                       (optional)  
  
  report-errors="<true/false>"                 (optional)  
  
  assessment-report-folder="<folder-name>"          (optional)  
  
  conversion-report-overwrite="<true/false>"   (optional)  
  
/>  
```  
ou  
  
```xml  
<generate-assessment-report  
  
  assessment-report-folder="<folder-name>"            (optional)  
  
  conversion-report-overwrite="<true/false>"     (optional)  
  
>  
<metabase-object object-name="<object-name>"  
  
        object-type="<object-category>"/>  
  
</generate-assessment-report>  
```  
  
## <a name="migration-commands"></a>Commandes de migration  
Les commandes de Migration de convertir le schéma de base de données cible au schéma source et migrent des données vers le serveur cible.  
  
### <a name="convert-schema"></a>convert-schema  
Cette commande effectue la conversion de schéma à partir de la source vers le schéma cible.  
  
Si la connexion de base de données source ou cible n’est pas effectuée avant d’exécuter cette commande, ou la connexion au serveur de base de données source ou cible échoue pendant l’exécution de la commande, une erreur est générée et l’application console se ferme.  
  
-   `conversion-report-folder:` Spécifie le dossier dans lequel le rapport d’évaluation peut être stocké. (attribut facultatif)  
  
-   `object-name:` Spécifie l’ou les objets source pris en compte pour la conversion de schéma (prend en charge les noms de l’objet ou un nom d’objet de groupe).  
  
-   `object-type:` Spécifie le type de l’objet appelé dans l’attribut de nom de l’objet (si la catégorie d’objet est spécifié, le type d’objet sera « catégorie »).  
  
-   `conversion-report-overwrite:` Spécifie s’il faut remplacer le dossier de rapport d’évaluation s’il existe déjà.  
  
    **Valeur par défaut :** false. (attribut facultatif)  
  
-   `write-summary-report-to:` Spécifie le chemin d’accès auquel le rapport de résumé sera généré.  
  
    Si seul le chemin d’accès du dossier est indiqué, puis de fichiers par nom **SchemaConversionReport&lt;n&gt;. XML** est créé. (attribut facultatif)  
  
    La création de rapports a deux sous-catégories supplémentaires :  
  
    -   `report-errors` (= « true/false », valeur par défaut en tant que « false » (attributs facultatifs))  
  
    -   `verbose` (= « true/false », valeur par défaut en tant que « false » (attributs facultatifs))  
  
**Exemple de syntaxe :**  
  
```xml  
<convert-schema  
  
  object-name="<object-name>"  
  
  object-type="<object-category>"  
  write-summary-report-to="<file-name/folder-name>"     (optional)  
  
  verbose="<true/false>"                          (optional)  
  
  report-errors="<true/false>"                    (optional)  
  
  conversion-report-folder="<folder-name>"             (optional)  
  
  conversion-report-overwrite="<true/false>"      (optional)  
  
/>  
```  
ou  
  
```xml  
<convert-schema  
  
  conversion-report-folder="<folder-name>"         (optional)  
  
  conversion-report-overwrite="<true/false>"> (optional)  
  
  <metabase-object object-name="<object-name>"  
  
    object-type="<object-category>"/>  
  
</convert-schema>  
```  
  
### <a name="migrate-data"></a>migrer des données  
Cette commande migre les données source à la cible.  
  
-   `object-name:` Spécifie l’ou les objets source pris en compte pour la migration de données (prend en charge les noms de l’objet ou un nom d’objet de groupe).  
  
-   `object-type:` Spécifie le type de l’objet appelé dans l’attribut de nom de l’objet (si la catégorie d’objet de type d’objet sera « catégorie » n’est spécifié).  
  
-   `write-summary-report-to:` Spécifie le chemin d’accès à laquelle le rapport sera généré.  
  
    Si seul le chemin d’accès du dossier est indiqué, puis de fichiers par nom **DataMigrationReport&lt;n&gt;. XML** est créé. (attribut facultatif)  
  
    La création de rapports a deux sous-catégories supplémentaires :  
  
    -   `report-errors` (= « true/false », valeur par défaut en tant que « false » (attributs facultatifs))  
  
    -   `verbose` (= « true/false », valeur par défaut en tant que « false » (attributs facultatifs))  
  
**Exemple de syntaxe :**  
  
```xml  
<migrate-data  
  
  write-summary-report-to="<file-name/folder-name>"  
  
  report-errors="<true/false>" verbose="<true/false>">  
  
    <metabase-object object-name="<object-name>"/>  
  
    <metabase-object object-name="<object-name>"/>  
  
    <metabase-object object-name="<object-name>"/>  
  
    <data-migration-connection  
  
      source-use-last-used="true"/source-server="<server-unique-name>"  
  
      target-use-last-used="true"/target-server="<server-unique-name>"/>  
  
</migrate-data>  
```  
ou  
  
```xml  
<migrate-data  
  
  object-name="<object-name>"  
  
  object-type="<object-category>"  
  
  write-summary-report-to="<file-name/folder-name>"  
  
  report-errors="<true/false>" verbose="<true/false>"/>  
```  
  
## <a name="migration-preparation-command"></a>Commande de préparation de migration  
La commande de préparation de Migration lance un mappage de schéma entre les bases de données source et cible.  
  
> [!NOTE]  
> La sortie de console par défaut définissant pour les commandes de migration est le rapport de sortie « Complète » avec aucun rapport d’erreur détaillé : résumé uniquement sur le nœud racine d’arborescence objet source.  
  
### <a name="map-schema"></a>schéma de mappage  
Cette commande fournit le mappage du schéma de la base de données source vers le schéma cible.  
  
-   `source-schema` Spécifie le schéma source pour effectuer la migration.  
  
-   `sql-server-schema` Spécifie le schéma cible à laquelle le schéma source est migré.  
  
**Exemple de syntaxe :**  
  
```xml  
<map-schema source-schema="<source-schema>"  
  
sql-server-schema="<target-schema>"/>  
```  
  
## <a name="manageability-commands"></a>Commandes de la facilité de gestion  
Les commandes de la facilité de gestion permettent de synchroniser les objets de base de données cible avec la base de données source.  
  
> [!NOTE]  
> La sortie de console par défaut définissant pour les commandes de migration est le rapport de sortie « Complète » avec aucun rapport d’erreur détaillé : résumé uniquement sur le nœud racine d’arborescence objet source.  
  
### <a name="synchronize-target"></a>synchroniser la cible  
Cette commande synchronise les objets cibles avec la base de données cible.  
 
Si cette commande est exécutée sur la base de données source, une erreur s’est produite.  
  
Si la connexion de base de données cible n’est pas effectuée avant d’exécuter cette commande, ou la connexion au serveur de base de données cible échoue pendant l’exécution de la commande, une erreur est générée et l’application console se ferme.  
  
-   `object-name:` Spécifie l’ou les objets cible pris en compte pour la synchronisation avec la base de données cible (prend en charge les noms de l’objet ou un nom d’objet de groupe).  
  
-   `object-type:` Spécifie le type de l’objet appelé dans l’attribut de nom de l’objet (si la catégorie d’objet de type d’objet sera « catégorie » n’est spécifié).  
  
-   `on-error:` Spécifie s’il faut spécifier des erreurs de synchronisation comme des avertissements ou erreurs. Options disponibles pour l’erreur :  
  
    -   total de rapports en tant qu’avertissement  
  
    -   rapport-chaque-sous-avertissement  
  
    -   Échec-script  
  
-   `report-errors-to:` Spécifie l’emplacement du rapport d’erreurs pour l’opération de synchronisation (attribut facultatif). Si seul le chemin d’accès de dossier est indiqué, puis de fichiers par nom **TargetSynchronizationReport.XML** est créé.  
  
**Exemple de syntaxe :**  
  
```xml  
<synchronize-target  
  
object-name="<object-name>"  
  
on-error="<report-total-as-warning/  
  
report-each-as-warning/  
  
fail-script>" (optional)  
  
  report-errors-to="<file-name/folder-name>"        (optional)  
  
/>  
```  
ou  
  
```xml  
<synchronize-target  
  
  object-name="<object-name>"  
  
  object-type="<object-category>"/>  
```  
ou  
  
```xml  
<synchronize-target>  
  
  <metabase-object object-name="<object-name>"/>  
  
  <metabase-object object-name="<object-name>"/>  
  
  <metabase-object object-name="<object-name>"/>  
  
</synchronize-target>  
```  
  
### <a name="refresh-from-database"></a>actualisation de base de données  
Cette commande actualise les objets de la source à partir de la base de données.  
  
Si cette commande est exécutée sur la base de données cible, une erreur est générée.  
  
Cette commande nécessite un ou plusieurs nœuds de la métabase en tant que paramètre de ligne de commande.  
  
-   `object-name:` Spécifie l’ou les objets source pris en compte pour l’actualisation à partir de la base de données source (prend en charge les noms de l’objet ou un nom d’objet de groupe).  
  
-   `object-type:` Spécifie le type de l’objet spécifié dans l’attribut de nom de l’objet (si la catégorie d’objet de type d’objet sera « catégorie » n’est spécifié).  
  
-   `on-error:` Spécifie s’il faut appeler les erreurs d’actualisation, comme des avertissements ou erreurs. Options disponibles pour l’erreur :  
  
    -   total de rapports en tant qu’avertissement  
  
    -   rapport-chaque-sous-avertissement  
  
    -   Échec-script  
  
-   `report-errors-to:` Spécifie l’emplacement du rapport d’erreurs pour l’opération d’actualisation (attribut facultatif). Si seul le chemin d’accès de dossier est indiqué, puis de fichiers par nom **SourceDBRefreshReport.XML** est créé.  
  
**Exemple de syntaxe :**  
  
```xml  
<refresh-from-database  
  
  object-name="<object-name>"  
  
  on-error="<report-total-as-warning/  
  
             report-each-as-warning/  
  
             fail-script>"              (optional)  
  
  report-errors-to="<file-name/folder-name>"        (optional)  
  
/>  
```  
ou  
  
```xml  
<refresh-from-database  
  
  object-name="<object-name>"  
  
  object-type="<object-category>" />  
```  
ou  
  
```xml  
<refresh-from-database>  
  
  <metabase-object object-name="<object-name>"/>  
  
</refresh-from-database>  
```  
  
## <a name="script-generation-commands"></a>Les commandes de génération de script  
Les commandes de génération du Script effectuent deux tâches : elles permettent d’enregistrer la sortie dans un fichier de script de la console, et ils enregistrent la sortie de T-SQL dans la console ou un fichier basé sur le paramètre que vous spécifiez.  
  
### <a name="save-as-script"></a>en tant que script de sauvegarde  
Cette commande est utilisée pour enregistrer les Scripts des objets dans un fichier mentionné lorsque la métabase = cible. Il s’agit d’une alternative à la commande de synchronisation dans la mesure où nous obtenir les scripts et d’exécuter le même sur la base de données cible.  
  
Cette commande nécessite un ou plusieurs nœuds de la métabase en tant que paramètre de ligne de commande.  
  
-   `object-name:` Spécifie l’ou les objets dont les scripts doivent être enregistrés (prend en charge les noms de l’objet ou un nom d’objet de groupe).  
  
-   `object-type:` Spécifie le type de l’objet appelé dans l’attribut de nom de l’objet (si la catégorie d’objet est spécifié, le type d’objet sera « catégorie »).  
  
-   `metabase:` Spécifie s’il s’agit de la source ou cible de la métabase.  
  
-   `destination:` Spécifie le chemin d’accès ou le dossier dans lequel le script doit être enregistré. Si le nom de fichier n’est pas spécifié, un nom de fichier dans le format (valeur de l’attribut object_name) .out doivent être fourni.
  
-   `overwrite:` Si la valeur est true, puis il remplace le nom de fichier même s’il existe. Il peut avoir les valeurs (true/false).  
  
**Exemple de syntaxe :**  
  
```xml  
<save-as-script  
  
  metabase="<source/target>"  
  
  object-name="<object-name>"  
  
  object-type="<object-category>"  
  
  destination="<file-name/folder-name>"  
  
  overwrite="<true/false>"   (optional)  
  
/>  
```  
ou  
  
```xml  
<save-as-script  
  
  metabase="<source/target>"  
  
  destination="<file-name/folder-name>"  
  
    <metabase-object object-name="<object-name>"  
  
                     object-type="<object-category>"/>  
  
</save-as-script>  
```  
  
### <a name="convert-sql-statement"></a>instruction CONVERT-sql
Cette commande convertit l’instruction SQL.  
  
-   `context` Spécifie le nom du schéma.  
  
-   `destination` Spécifie si la sortie doit être stockée dans un fichier.  
  
    Si cet attribut n’est pas spécifié, l’instruction T-SQL convertie s’affiche sur la console. (attribut facultatif)  
  
-   `conversion-report-folder` Spécifie le dossier dans lequel le rapport d’évaluation peut être stocké. (attribut facultatif)  
  
-   `conversion-report-overwrite` Spécifie s’il faut remplacer le dossier de rapport d’évaluation s’il existe déjà.  
  
    **Valeur par défaut :** false. (attribut facultatif)  
  
-   `write-converted-sql-to` Spécifie le fichier (ou) le chemin d’accès du dossier pour lequel le code T-SQL converti doit être stocké. Lorsqu’un chemin d’accès du dossier est spécifié avec la `sql-files` attribut, chaque fichier source a une cible correspondante fichier T-SQL créé sous le dossier spécifié. Lorsqu’un chemin d’accès du dossier est spécifié avec la `sql` attribut, le code T-SQL converti est écrit dans un fichier nommé Result.out sous le dossier spécifié.  
  
-   `sql` Spécifie les instructions sql de Sybase à convertir, une ou plusieurs instructions peuvent être séparées par un « ; »  
  
-   `sql-files` Spécifie le chemin d’accès des fichiers sql qui doit être converti en code T-SQL.  
  
-   `write-summary-report-to` Spécifie le chemin d’accès où le rapport de synthèse est généré. Si seul le chemin d’accès du dossier est indiqué, puis de fichiers par nom **ConvertSQLReport.XML** est créé. (attribut facultatif)  
  
    La création de rapports de synthèse a deux sous-catégories supplémentaires, à savoir :  
  
    -   erreurs de rapport (= « true/false », la valeur par défaut en tant que « false » (attributs facultatifs)).  
  
    -   verbose (= « true/false », valeur par défaut en tant que « false » (attributs facultatifs)).  
  
Cette commande nécessite un ou plusieurs nœuds de la métabase en tant que paramètre de ligne de commande.  
  
**Exemple de syntaxe :**  
  
```  
<convert-sql-statement  
  
       context="<database-name>.<schema-name>"  
  
        conversion-report-folder="<folder-name>"  
  
        conversion-report-overwrite="<true/false>"  
  
        write-summary-report-to="<file-name/folder-name>"   (optional)  
  
        verbose="<true/false>"   (optional)  
  
        report-errors="<true/false>"   (optional)  
  
        destination="<stdout/file>"   (optional)  
  
        write-converted-sql-to ="<file-name/folder-name>"  
  
        sql="SELECT 1 FROM DUAL;">  
  
    <output-window suppress-messages="<true/false>" />  
  
</convert-sql-statement>  
```  
ou  
  
```  
<convert-sql-statement  
  
         context="<database-name>.<schema-name>"  
  
         conversion-report-folder="<folder-name>"  
  
         conversion-report-overwrite="<true/false>"  
  
         write-summary-report-to="<file-name/folder-name>"   (optional)  
  
         verbose="<true/false>"   (optional)  
  
         report-errors="<true/false>"   (optional)  
  
         destination="<stdout/file>"   (optional)  
  
         write-converted-sql-to ="<file-name/folder-name>"  
  
         sql-files="<folder-name>\*.sql"  
  
/>  
```  
ou  
  
```  
<convert-sql-statement  
  
         context="<database-name>.<schema-name>"  
  
         conversion-report-folder="<folder-name>"  
  
         conversion-report-overwrite="<true/false>"  
  
         sql-files="<folder-name>\*.sql"  
  
/>  
```  
  
## <a name="next-steps"></a>Étapes suivantes  
Pour plus d’informations sur les options de ligne de commande, consultez [des options de ligne de commande de la Console de SSMA (AccessToSQL)](../access/command-line-options-in-ssma-console-accesstosql.md).  
  
Pour plus d’informations sur un exemple de fichier de script de console, consultez [fonctionne avec les exemples de fichiers de Script Console &#40;SybaseToSQL&#41;](../../ssma/sybase/working-with-the-sample-console-script-files-sybasetosql.md)  
  
L’étape suivante varie selon les spécifications de votre projet :  
  
-   Pour spécifier un mot de passe ou d’exportation / importation des mots de passe, consultez [la gestion des mots de passe &#40;SybaseToSQL&#41;](../../ssma/sybase/managing-passwords-sybasetosql.md).  
  
-   Pour la génération de rapports, consultez [génération de rapports &#40;SybaseToSQL&#41;](../../ssma/sybase/generating-reports-sybasetosql.md).  
  
-   Pour résoudre les problèmes dans la console, consultez [dépannage &#40;SybaseToSQL&#41;](../../ssma/sybase/troubleshooting-sybasetosql.md).  
  
