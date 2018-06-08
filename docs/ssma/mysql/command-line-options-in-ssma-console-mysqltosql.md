---
title: Options de ligne de commande dans la Console SSMA (MySQLToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-mysql
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
- Command line options, help option
- Command line options, log file option
- Command line options, project environment folder option
- Command line options, script file option
- Command line options, secure password option
- Command line options, SecurePassword help option
- Command line options, server connection file option
- Command line options, variable value file option
- Command line options, XML output option
ms.assetid: a2310b10-68ad-4285-a08b-c8694cf84416
caps.latest.revision: 12
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 1c7d3f83668df0413b1898d888385fed2a183f00
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="command-line-options-in-ssma-console-mysqltosql"></a>Options de ligne de commande dans la Console SSMA (MySQLToSQL)
Microsoft vous fournit un options de ligne de commande ensemble robuste pour exécuter et contrôler les activités SSMA. Les sections qui en découlent décrit en détail le même.  
  
## <a name="command-line-options-in-ssma-console"></a>Options de ligne de commande dans la Console SSMA  
Décrites dans ce document est la console des options de commande.  
  
Pour les besoins de cette section, le terme 'option' est également appelé 'switch'.  
  
Options ne respectent pas la casse et peut commencer par '**-**'ou',**/**' caractères.  
  
Si les options sont spécifiées, il devient obligatoire pour spécifier les paramètres d’option correspondante.  
  
Les paramètres d’option doivent être séparés par le caractère de l’option par un espace blanc.  
  
**Exemples de syntaxe :**  
  
`C:\> SSMAforMySQLConsole.EXE -s scriptfile`  
  
`C:\> SSMAforMySQLConsole.EXE -s “C:\Program Files\Microsoft SQL Server Migration Assistant for MySQL\Sample Console Scripts\AssessmentReportGenerationSample.xml” –v “C:\Program Files\Microsoft SQL Server Migration Assistant for MySQL\Sample Console Scripts\VariableValueFileSample.xml” –c “C:\Program Files\Microsoft SQL Server Migration Assistant for MySQL\Sample Console Scripts\ServersConnectionFileSample.xml”`  
  
Les noms de dossier ou un fichier contenant des espaces doivent être spécifiés entre guillemets doubles.  
  
La sortie des entrées de ligne de commande et les messages d’erreur sont stockés dans STDOUT ou dans un fichier spécifié.  
  
### <a name="script-file-option-sscript"></a>Option de fichier de script : – s/script  
Un commutateur obligatoire, le chemin d’accès/nom de fichier de script spécifie le script de séquences de commande doit être exécutée par SSMA.  
  
**Exemples de syntaxe :**  
  
`C:\>SSMAforMySQLConsole.EXE –s “C:\Program Files\Microsoft SQL Server Migration Assistant for MySQL\Sample Console Scripts\ConversionAndDataMigrationSample.xml”`  
  
### <a name="variable-value-file-option-vvariable"></a>Variable valeur Option de fichier : la variable ou – v  
Ce fichier comprend des variables utilisées dans le fichier de script. Il s’agit d’un commutateur facultatif. Si les variables ne sont pas déclarés dans le fichier de variable et utilisés dans le fichier de script, l’application génère une erreur et termine l’exécution de la console.  
  
**Exemples de syntaxe :**  
  
Variables définies dans plusieurs fichiers de valeur de la variable, par exemple, un avec une valeur par défaut et l’autre avec une valeur spécifique de l’instance le cas échéant. Le dernier fichier de variable spécifié dans les arguments de ligne de commande prend la préférence, au cas où une duplication des variables :  
  
`C:\>SSMAforMySQLConsole.EXE -s`  
  
`“C:\Program Files\Microsoft SQL Server Migration Assistant for MySQL\Sample Console Scripts\ConversionAndDataMigrationSample.xml” –v c:\migration`  
  
`projects\global_variablevaluefile.xml –v “c:\migrationprojects\instance_variablevaluefile.xml”`  
  
### <a name="server-connection-file-option-cserverconnection"></a>Option de fichier de connexion de serveur : – c/serverconnection  
Ce fichier contient des informations de connexion de serveur pour chaque serveur. Chaque définition de serveur est identifiée par un ID de serveur unique. Les ID de serveur sont référencés dans le fichier de script pour la connexion des commandes similaires.  
  
Définition de serveur peut être une partie du fichier de connexion de serveur et/ou le fichier de script. Id du serveur dans le fichier de script est prioritaire sur le fichier de connexion de serveur, au cas où une duplication des id de serveur.  
  
**Exemples de syntaxe :**  
  
ID de serveur sont utilisés dans le fichier de script et elles sont définies dans un fichier de connexion de serveur distinct, fichier de connexion de serveur utilise des variables qui sont définies dans le fichier de la valeur de la variable :  
  
`C:\>SSMAforMySQLConsole.EXE –s “C:\Program Files\Microsoft SQL Server Migration Assistant for MySQL\Sample Console Scripts\ConversionAndDataMigrationSample.xml”  –v`  
  
`c:\SsmaProjects\myvaluefile1.xml –c`  
  
`c:\SsmaProjects\myserverconnectionsfile1.xml`  
  
Définition de serveur est incorporée dans le fichier de script :  
  
`C:\>SSMAforMySQLConsole.EXE –s “C:\Program Files\Microsoft SQL Server Migration Assistant for MySQL\Sample Console Scripts\ConversionAndDataMigrationSample.xml”`  
  
### <a name="xml-output-option--xxmloutput-xmloutputfile"></a>Option de sortie XML : - x / xmloutput [xmloutputfile]  
Cette commande est utilisée pour la sortie des messages de sortie de commande au format xml à la console ou dans un fichier xml.  
  
Il existe deux options pour xmloutput, notamment..,:  
  
-   Si le chemin d’accès est fourni après le commutateur xmloutput la sortie est redirigée vers le fichier.  
  
    **Exemple de syntaxe :**  
  
    `C:\>SSMAforMySQLConsole.EXE –s`  
  
    `“C:\Program Files\Microsoft SQL Server Migration Assistant for MySQL\Sample Console Scripts\ConversionAndDataMigrationSample.xml”  –x d:\xmloutput\project1output.xml`  
  
-   Si aucun chemin d’accès n’est fourni après le commutateur xmloutput la xmlout s’affiche sur la console.  
  
    **Exemple de syntaxe :**  
  
    `C:\>SSMAforMySQLConsole.EXE –s “C:\Program Files\Microsoft SQL Server Migration Assistant for MySQL\Sample Console Scripts\ConversionAndDataMigrationSample.xml”  –xmloutput`  
  
### <a name="log-file-option-llog"></a>Option de fichier journal : – l/journal  
Toutes les opérations dans l’application de Console SSMA enregistrées dans un fichier journal. Il s’agit d’un commutateur facultatif. Si un fichier journal et son chemin d’accès sont spécifiés sur la ligne de commande, le journal est généré dans l’emplacement spécifié. Sinon, il obtient généré dans son emplacement par défaut.  
  
**Exemple de syntaxe :**  
  
`C:\>SSMAforMySQLConsole.EXE`  
  
`“C:\Program Files\Microsoft SQL Server Migration Assistant for MySQL\Sample Console Scripts\ConversionAndDataMigrationSample.xml”  –l c:\SsmaProjects\migration1.log`  
  
### <a name="project-environment-folder-option-eprojectenvironment"></a>Option de dossier d’environnement de projet : – e/projectenvironment  
Cela indique le dossier de paramètres d’environnement projet pour le projet SSMA actuel. Ce commutateur est facultatif.  
  
**Exemple de syntaxe :**  
  
`C:\>SSMAforMySQLConsole.EXE –s`  
  
`“C:\Program Files\Microsoft SQL Server Migration Assistant for MySQL\Sample Console Scripts\ConversionAndDataMigrationSample.xml”  –e c:\SsmaProjects\CommonEnvironment`  
  
### <a name="secure-password-option-psecurepassword"></a>Option de mot de passe sécurisée : – p/securepassword  
Cette option indique le mot de passe chiffré pour les connexions au serveur. Il diffère de toutes les autres options : l’option exécute n’importe quel script ni permet de toutes les activités liées à la migration, mais vous aide à gérer le chiffrement de mot de passe pour les connexions du serveur utilisé dans le projet de migration.  
  
Vous ne pouvez pas entrer de toute autre option ou mot de passe en tant que le paramètre de ligne de commande. Dans le cas contraire, il génère une erreur. Pour plus d’informations, reportez-vous à la [la gestion des mots de passe](http://msdn.microsoft.com/en-us/4ffbc587-ea3f-49ad-bc42-a654f672325e) section.  
  
Les options de sous-section suivantes sont prises en charge pour `–p/securepassword`:  
  
-   Pour ajouter le mot de passe protégé stockage pour un ID de serveur spécifié ou pour tous les ID de serveur définis dans le fichier de connexion de serveur. -Option, ci-dessous, les mises à jour de remplacement du mot de passe s’il existe déjà :  
  
    `-p|-securepassword -a|add    {"<server_id>[, .n]"|all}` `-c|-serverconnection <server-connection-file> [-v|variable <variable-value-file>]``[-o|overwrite]`  
  
    `-p|-securepassword -a|add    {"<server_id>[, .n]"|all}``-s|-script <server-connection-file> [-v|variable <variable-value-file>] [-o|overwrite]`  
  
-   Pour supprimer le mot de passe chiffré de stockage protégé de l’ID de serveur spécifié ou pour tous les ID de serveur :  
  
    `–p/securepassword –r/remove {<server_id> [, …n] | all}`  
  
-   Pour afficher une liste d’ID de serveur pour lequel le mot de passe est chiffré :  
  
    `–p/securepassword –l/list`  
  
-   Pour exporter les mots de passe stockés dans le stockage protégé vers un fichier chiffré. Ce fichier est chiffré avec la phrase secrète spécifiée par l’utilisateur.  
  
    `–p/securepassword –e/export {<server-id> [, …n] | all} <encrypted-password -file>`  
  
-   Le chiffré-fichier qui a été précédemment exporté est importé dans un stockage local protégé à l’aide de la phrase secrète spécifiée par l’utilisateur. Une fois que le fichier est déchiffré, il est stocké dans un nouveau fichier, qui à son tour, est chiffré sur l’ordinateur local.  
  
    `–p/securepassword –i/import {<server-id> [, …n] | all} <encrypted-password -file>`  
  
    Plusieurs ID de serveur peut être spécifiés à l’aide de virgules de séparation.  
  
### <a name="help-option-help"></a>Option d’aide : – ? /help  
Affiche le résumé de la syntaxe des options de la Console de SSMA :  
  
`C:\>SSMAforMySQLConsole.EXE -?`  
  
Pour un affichage sous forme de tableau des options de ligne de commande de Console de SSMA, reportez-vous à [annexe - 1 &#40;MySQLToSQL&#41;](../../ssma/mysql/appendix-1-mysqltosql.md).  
  
### <a name="securepassword-help-option-securepassword--help"></a>Option de l’aide de SecurePassword : – securepassword- ? /Help  
Affiche le résumé de la syntaxe des options de la Console de SSMA :  
  
`C:\>SSMAforMySQLConsole.EXE -securepassword -?`  
  
Pour un affichage sous forme de tableau des options de ligne de commande de Console de SSMA, reportez-vous à [annexe - 1 &#40;MySQLToSQL&#41;](../../ssma/mysql/appendix-1-mysqltosql.md)  
  
### <a name="next-step"></a>Étape suivante  
L’étape suivante varie selon les spécifications de votre projet :  
  
-   Pour spécifier un mot de passe ou d’exportation / importation des mots de passe, consultez [la gestion des mots de passe &#40;MySQLToSQL&#41;](../../ssma/mysql/managing-passwords-mysqltosql.md).  
  
-   Pour la génération de rapports, consultez [génération de rapports &#40;MySQLToSQL&#41;](../../ssma/mysql/generating-reports-mysqltosql.md).  
  
-   Pour résoudre les problèmes dans la console, consultez [dépannage &#40;MySQLToSQL&#41;](../../ssma/mysql/troubleshooting-mysqltosql.md).  
  
