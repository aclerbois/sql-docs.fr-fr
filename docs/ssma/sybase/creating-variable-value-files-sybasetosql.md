---
title: Création de fichiers de la valeur de la Variable (SybaseToSQL) | Documents Microsoft
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
- Sybase Console,Creating Variable Value Files
- Sybase Console,Variable Value File Validation
ms.assetid: 395be464-4b19-44f7-91e5-b8876d6743dc
caps.latest.revision: 15
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: ad9aec22da1234c19e9124eefb4ebbfca50cfc55
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="creating-variable-value-files-sybasetosql"></a>Création de fichiers de la valeur de la Variable (SybaseToSQL)
Fichier de la valeur de variable est un fichier XML contenant les valeurs de paramètre de commandes, comme le nom du serveur source ou de destination qui changent fréquemment à partir de la migration d’un serveur vers un autre. En cas d’un grand nombre de migrations de la base de données, plusieurs fichiers de variables pour stocker la valeur de chaque serveur source seront créés et référencés dans un fichier de script principal avec le **– v** passer à la ligne de commande. Cela permet de conserver des valeurs statiques dans plusieurs fichiers de script avec les valeurs des variables dans plusieurs fichiers de variable.  
  
> [!NOTE]  
> 1.  Les noms de variables ont le préfixe et suffixe avec un symbole $ (dollar). Si les variables ne sont pas attribués une valeur dans le fichier de la valeur de la variable, vous rencontrerez une erreur lors de l’analyse du fichier de script résultant de bloquer le processus d’exécution de console.  
> 2.  The escape character for **$** is **$$**. Si la valeur d’une valeur d’un paramètre de variable ou statique contient **$** symbole (dollar), puis **$$** doit être spécifié pour le traiter comme un caractère au lieu d’une variable.  
> 3.  À des fins de facilité de maintenance, les variables peuvent être déclarées à l’intérieur de `‘variable-group’` variables définies par des éléments pour une séparation logique de l’utilisateur.  L’utilisation de cet élément n’est pas obligatoire.  
  
**Exemples :**  
  
**Exemple 1 :**  
  
```xml  
<!--Sample of variable value file commands-->  
  
<variables>  
  
  <variable-group name="ProjectSpecs">  
  
    <variable name="$project_folder$" value="<project-folder>"/>  
  
    <variable name="$project_name$" value="<project-name>"/>  
  
    <variable name="$project_overwrite$" value="<true/false>"/>  
  
    <variable name="$project_type$" value="<project-type>"/>  
  
  </variable-group>  
  
</variables>  
```  
**Exemple 2 :**  
  
```xml  
<!--Sample of variable value file commands-->  
  
<variables>  
  
  <variable-group name="SQLServerParams">  
  
    <variable-group name="SqlServerConnectionParams">  
  
      <variable name="$TargetUserName$" value="<user-name>"/>  
  
      <variable name="$TargetServerName$" value="<server-name>"/>  
  
      <variable name="$TargetDB$" value="<database-name>"/>  
  
      <variable name="$TargetPassword$" value="<password>"/>  
  
      <variable name="$TrustedConnection$" value="<true/false>"/>  
  
    </variable-group>  
  
    <variable-group name="SqlServerObjectParams">  
  
      <variable name="$ObjectName1$" value="<object-name>"/>  
  
      <variable name="$ObjectName2$" value="<object-name>"/>  
  
    </variable-group>  
  
  </variable-group>  
  
</variables>  
```  
  
## <a name="variable-value-file-validation"></a>Validation des fichiers de valeur de la variable  
L’utilisateur peut valider facilement son fichier de la valeur de la variable par rapport au fichier de définition de schéma **ConsoleScriptVariablesSchema.xsd** disponibles dans le dossier « Schémas ».  
  
## <a name="next-step"></a>Étape suivante  
L’étape suivante d’exploitation de la console est [création des fichiers de connexion serveur &#40;SybaseToSQL&#41;](../../ssma/sybase/creating-the-server-connection-files-sybasetosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Création des fichiers de serveur (Sybase)](http://msdn.microsoft.com/en-us/35ef396f-9f98-429d-9fc5-4f413d08fb37)  
  
