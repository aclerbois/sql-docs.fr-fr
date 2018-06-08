---
title: Création de fichiers de Script (AccessToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-access
ms.custom: ''
ms.date: 08/17/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
ms.assetid: 64dfe192-965c-49d4-a3ea-848fbc5f619f
caps.latest.revision: 21
author: Shamikg
ms.author: Shamikg
manager: murato
ms.openlocfilehash: 5d1b0fda2c6c822d6d080bb2cf48958964e230ed
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="creating-script-files-accesstosql"></a>Création de fichiers de script (AccessToSQL)
La première étape avant le lancement de l’application de console SSMA est à créer le fichier de script et, si nécessaire de créer le fichier de la valeur de la variable et le fichier de connexion de serveur.  
  
Le fichier de script peut être divisé en trois sections, notamment..,:  
  
1.  **config :** permet à l’utilisateur définir les paramètres de configuration pour l’application console.  
  
2.  **serveurs :** permet à l’utilisateur définir des définitions de serveur de la source/cible. Cela peut également être dans un fichier de connexion de serveur distinct.  
  
3.  **commandes de script :** permet à l’utilisateur d’exécuter des commandes de flux de travail SSMA.  
  
Chaque section est décrite en détail ci-dessous :  
  
## <a name="configuring-access-console-settings"></a>Configuration des paramètres de la console accès  
Les configurations d’un script sont affichent dans le fichier de script de console.  
  
Si un des éléments sont spécifié dans le nœud de configuration, ils sont définis comme par exemple, le paramètre global, elles sont applicables pour toutes les commandes de script. Ces éléments de configuration peuvent également être définies dans chaque commande dans la section de la commande de script si l’utilisateur souhaite remplacer le paramètre global.  
  
Les options configurables par l’utilisateur sont les suivantes :  
  
1.  **Fournisseur de fenêtre de sortie :** si l’attribut de messages supprimer a la valeur 'true', la commande spécifique messages n’a pas s’affichent sur la console. La description des attributs est répertoriée ci-dessous :  
  
    -   destination : Spécifie si la sortie doit obtenir imprimé à un fichier ou à stdout. Il a la valeur false par défaut.  
  
    -   nom de fichier : le chemin d’accès du fichier (facultatif).  
  
    -   messages supprimer : supprime les messages sur la console. Cela est « false » par défaut.  
  
    **Exemple :**  
  
    ```xml  
    <output-providers>  
  
      <output-window  
  
        suppress-messages="<true/false>"   (optional)  
  
        destination="<file/stdout>"        (optional)  
  
        file-name="<file-name>"            (optional)  
  
       />  
  
    </output-providers>  
    ```  
    *Ou*  
  
    ```xml  
    <…All commands…>  
  
      <output-window  
  
         suppress-messages="<true/false>"   (optional)  
  
         destination="<file/stdout>"        (optional)  
  
         file-name="<file-name>"            (optional)  
  
       />  
  
    </…All commands…>  
    ```  
  
2.  **Fournisseur de connexion de la Migration de données :** qui spécifie le serveur de source/cible est à prendre en considération pour la migration de données.  Utilisé en source-utilisation-dernier indique que le dernier serveur source utilisé est utilisé pour la migration des données. De même utilisés en cible-utilisation-dernier indique que le dernier serveur cible utilisé est utilisé pour la migration des données. L’utilisateur peut également spécifier le serveur (source ou cible) en utilisant les attributs source-serveur ou le serveur cible.  
  
    Qu’un seul ou l’autre attribut spécifié peut être utilisé par exemple :  
  
    - utilisé en source-utilisation-dernier = « true » (par défaut) ou serveur de source = « source_servername »  
  
    - utilisé en cible-utilisation-dernier = « true » (par défaut) ou serveur à la cible = « target_servername »  
  
    **Exemple :**  
  
    ```xml  
    <output-providers>  
  
      <data-migration-connection   source-use-last-used="true"  
  
                                   target-server="target_1"/>  
  
    </output-providers>  
    ```  
    *Ou*  
  
    ```xml  
    <migrate-data>  
  
      <data-migration-connection   source-server="source_1"  
  
                                   target-use-last-used="true"/>  
  
    </migrate-data>  
    ```  
  
3.  **Popup d’entrée utilisateur :** Cela permet la gestion des erreurs, lorsque les objets sont chargés à partir de la base de données. L’utilisateur fournit les modes d’entrée, et en cas d’erreur, la console se déroule comme utilisateur spécifie.  
  
    Les modes sont les suivantes :  
  
    -   **Demandez à-utilisateur -** invite l’utilisateur à continue('yes') ou d’erreur (« non »).  
  
    -   **erreur -** la console affiche une erreur et interrompt l’exécution.  
  
    -   **continuer-** la console se poursuit avec l’exécution.  
  
    Le mode par défaut est **erreur**.  
  
    **Exemple :**  
  
    ```xml  
    <output-providers>  
  
      <user-input-popup mode="<ask-user/continue/error>"/>  
  
    </output-providers>  
    ```  
    *Ou*  
  
    ```xml  
    <!-- Connect to target database -->  
  
    <connect-target-database server="target_0">  
  
      <user-input-popup mode="<ask-user/continue/error>"/>  
  
    </connect-target-database>  
    ```  
  
4.  **Reconnectez le fournisseur :** Cela permet à l’utilisateur définir les paramètres de reconnexion en cas d’échecs de connexion. Cela peut être défini pour les serveurs source et cible.  
  
    Les modes de reconnexion sont :  
  
    -   reconnecter-last-utilisé-serveur : si la connexion n’est pas active, il essaie de se reconnecter au dernier serveur utilisé au plus 5 fois.  
  
    -   générer une erreur : si la connexion n’est pas active, une erreur est générée.  
  
    Le mode par défaut est **génèrent une erreur**.  
  
    **Exemple :**  
  
    ```xml  
    <output-providers>  
  
     <reconnect-manager on-source-reconnect="<reconnect-to-last-used-server/generate-an-error>"  
  
                        on-target-reconnect="<reconnect-to-last-used-server/generate-an-error>"/>  
  
    </output-providers>  
    ```  
    *Ou*  
  
    ```xml  
    <!--synchronization-->  
  
    <synchronize-target>  
  
      <reconnect-manager on-target-reconnect="reconnect-to-last-used-server"/>  
  
    </synchronize-target>  
    ```  
    *Ou*  
  
    ```xml  
    <!--data migration-->  
  
    <migrate-data server="target_0">  
  
      <reconnect-manager  
  
        on-source-reconnect="reconnect-to-last-used-server"  
  
        on-target-reconnect="generate-an-error"/>  
  
    </migrate-data>  
    ```  
  
5.  **Fournisseur de remplacement de convertisseur :** Cela permet à l’utilisateur gérer les objets qui sont déjà présents sur la cible de la métabase. Les actions possibles sont les suivantes :  
  
    -   Erreur : la console affiche une erreur et interrompt l’exécution.  
  
    -   remplacer : remplace les valeurs d’objet existantes. Cette action est effectuée par défaut.  
  
    -   Ignorer : la console ignore les objets qui existent déjà sur la base de données  
  
    -   utilisateur demander : invite l’utilisateur pour l’entrée ('yes' / 'non')  
  
    **Exemple :**  
  
    ```xml  
    <output-providers>  
  
      <object-overwrite action="<error|skip|overwrite|ask-user>"/>  
  
    </output-providers>  
    ```  
    *Ou*  
  
    ```xml  
    <convert-schema object-name="ssma.TT1">  
  
      <object-overwrite action="<error|skip|overwrite|ask-user>"/>  
  
    </convert-schema>  
    ```  
  
6.  **Fournisseur de configuration requise a échoué :** Cela permet à l’utilisateur gérer toutes les conditions requises pour le traitement d’une commande. Par défaut, le mode strict est 'false'. Si elle est définie à 'true', une exception généré pour le non-respect de la configuration requise.  
  
    **Exemple :**  
  
    ```xml  
    <output-providers>  
  
      <prerequisites strict-mode="<true|false>"/>  
  
    </output-providers>  
    ```  
  
7.  **Opération d’arrêt :** pendant l’opération intermédiaire, si l’utilisateur veut arrêter l’opération, puis **'Ctrl + C'** raccourci clavier peut être utilisé. SSMA pour Access Console attend que l’opération se termine et termine l’exécution de la console.  
  
    Si l’utilisateur veut arrêter l’exécution immédiatement, puis, **'Ctrl + C'** raccourcis peuvent appuyer à nouveau un arrêt brutal de l’application Console de SSMA.  
  
8.  **Fournisseur de progression :** indique la progression de chaque commande de la console. Il est désactivé par défaut. Les attributs de rapport de progression comprennent :  
  
    -   inactif  
  
    -   chaque 1 %  
  
    -   chaque 2 %  
  
    -   chaque 5 %  
  
    -   chaque 10 %  
  
    -   chaque 20 %  
  
    **Exemple :**  
  
    ```xml  
    <output-providers>  
  
     <progress-reporting enable="<true|false>"           (optional)  
  
                         report-messages="<true|false>"  (optional)  
  
                         report-progress="every-1%|every-2%|every-5%|every-10%|every-20%|off" (optional)/>  
  
    </output-providers>  
    ```  
    *Ou*  
  
    ```xml  
    <…All commands…>  
  
      <progress-reporting  
  
        enable="<true|false>"              (optional)  
  
        report-messages="<true|false>"     (optional)  
  
        report-progress="every-1%|every-2%|every-5%|every-10%|every-20%|off"     (optional)/>  
  
    </…All commands…>  
    ```  
  
9. **Détail de l’enregistreur d’événements :** jeux de connecter le niveau de détail. Cela correspond à l’option de toutes les catégories dans l’interface utilisateur. Par défaut, le niveau de détail du journal est « erreur ».  
  
    Les options au niveau de l’enregistreur d’événements sont les suivantes :  
  
    -   Erreur irrécupérable : uniquement-erreur irrécupérable sont enregistrés.  
  
    -   Erreur : seuls les messages d’erreur et erreur irrécupérable sont enregistrés.  
  
    -   Avertissement : tous les niveaux, à l’exception des messages de débogage et les informations sont enregistrées.  
  
    -   Info : tous les niveaux, sauf les messages de débogage sont enregistrés.  
  
    -   déboguer : tous les niveaux de messages consignés.  
  
    > [!NOTE]  
    > Messages obligatoires sont enregistrés à n’importe quel niveau.  
  
    **Exemple :**  
  
    ```xml  
    <output-providers>  
  
      <log-verbosity level="fatal-error/error/warning/info/debug"/>  
  
    </output-providers>  
    ```  
    *Ou*  
  
    ```xml  
    <…All commands…>  
  
      <log-verbosity level="fatal-error/error/warning/info/debug"/>  
  
    </…All commands…>  
    ```  
  
10. **Mot de passe chiffré remplacement :** si 'true', le mot de passe spécifié dans la section de définition de serveur du fichier de connexion du serveur ou dans le fichier de script, stockée dans le mot de passe chiffré protégées stockage si des remplacements existe. Si aucun mot de passe n’est spécifié en texte clair, l’utilisateur est invité à entrer le mot de passe.  
  
    Ici, deux cas se présentent :  
  
    1.  Si remplacer l’option est **false**, l’ordre de recherche sera protégé de stockage -&gt;fichier de Script -&gt;fichier de connexion de serveur -&gt; inviter l’utilisateur.  
  
    2.  Si remplacer l’option est **true**, l’ordre de recherche sera fichier de Script -&gt;fichier de connexion de serveur -&gt;inviter l’utilisateur.  
  
    **Exemple :**  
  
    ```xml  
    <output-providers>  
  
      <encrypted-password override="<true/false>"/>  
  
    </output-providers>  
    ```  
  
L’option non configurable est :  
  
-   **Nombre maximal de tentatives de reconnexion :** lorsqu’une connexion établie expire ou s’arrête en raison d’une défaillance du réseau, le serveur est requis pour être reconnecté. Les tentatives de reconnexion sont autorisées à un maximum de **5** nouvelles tentatives, la console effectue automatiquement la reconnexion. La fonctionnalité de reconnexion automatique permet de réduire votre effort de réexécuter le script.  
  
## <a name="server-connection-parameters"></a>Paramètres de connexion de serveur  
Paramètres de connexion de serveur peuvent être définis dans le fichier de script ou dans le fichier de connexion de serveur. Reportez-vous à la [création des fichiers de connexion serveur &#40;AccessToSQL&#41; ](../../ssma/access/creating-the-server-connection-files-accesstosql.md) section pour plus d’informations.  
  
## <a name="script-commands"></a>Commandes de script  
Le fichier de script contient une séquence de commandes de flux de travail de migration dans le format XML. L’application de console SSMA traite la migration dans l’ordre les commandes figurant dans le fichier de script.  
  
Par exemple, une migration de données par défaut d’une table spécifique dans une base de données Access suit la hiérarchie de : base de données -&gt; Table.  
  
Lorsque toutes les commandes dans le fichier de script sont exécutées avec succès, l’application de console SSMA se termine et retourne le contrôle à l’utilisateur. Le contenu d’un fichier de script est plus ou moins statique avec les informations sur les variables contenues dans un [les fichiers de valeurs de Variable](http://msdn.microsoft.com/808595c3-8ef1-40bd-a93e-5cf237950e08) ou, dans une section distincte dans le fichier de script pour les valeurs des variables.  
  
**Exemple :**  
  
```xml  
<!--Sample of script file commands -->  
  
<ssma-script-file>  
  
  <script-commands>  
  
    <create-new-project project-folder="$project_folder$"  
  
                        project-name="$project_name$"  
  
                        overwrite-if-exists="true"/>  
  
    <connect-source-database server="source_2"/>  
  
    <save-project/>  
  
    <close-project/>  
  
  </script-commands>  
  
</ssma-script-file>  
```  
Modèles composé de 3 fichiers de script (pour l’exécution de divers scénarios), fichier de la valeur de la variable et un fichier de connexion de serveur sont fournies dans le dossier d’exemples de Scripts de Console du répertoire du produit :  
  
-   AssessmentReportGenerationSample.xml  
  
-   ConversionAndDataMigrationSample.xml  
  
-   VariableValueFileSample.xml  
  
-   ServersConnectionFileSample.xml  
  
Vous pouvez exécuter les modèles (fichiers) après avoir modifié les paramètres affichés dans celui-ci, de pertinence.  
  
Vous trouverez la liste complète des commandes de script dans [l’exécution de la Console SSMA &#40;AccessToSQL&#41;](../../ssma/access/executing-the-ssma-console-accesstosql.md)  
  
## <a name="script-file-validation"></a>Validation des fichiers de script  
L’utilisateur peut valider facilement son fichier de script par rapport au fichier de définition de schéma **'A2SSConsoleScriptSchema.xsd'** disponibles dans le dossier « Schémas ».  
  
## <a name="next-step"></a>Étape suivante
L’étape suivante d’exploitation de la console est [création de fichiers de valeur Variable &#40;AccessToSQL&#41;](../../ssma/access/creating-variable-value-files-accesstosql.md).  
  
## <a name="see-also"></a>Voir aussi  
[Création de fichiers de la valeur de la Variable &#40;AccessToSQL&#41;](../../ssma/access/creating-variable-value-files-accesstosql.md)  
  
