---
title: Création de fichiers de la valeur de la Variable (DB2ToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-db2
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
ms.assetid: 122f3fbe-46a0-40df-ac3b-d43bf33d96ba
caps.latest.revision: 5
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: ba8f6733cad8a492009ef845ec713d617a3f1f45
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="creating-variable-value-files-db2tosql"></a>Création de fichiers de la valeur de la Variable (DB2ToSQL)
Fichier de la valeur de variable est un fichier XML contenant les valeurs de paramètre de commandes, comme le nom du serveur source ou de destination qui changent fréquemment à partir de la migration d’un serveur vers un autre. En cas d’un grand nombre de migrations de la base de données, plusieurs fichiers de variables pour stocker la valeur de chaque serveur source seront créés et référencés dans un fichier de script principal avec le **– v** passer à la ligne de commande. Cela permet de conserver des valeurs statiques dans plusieurs fichiers de script avec les valeurs des variables dans plusieurs fichiers de variable.  
  
> [!NOTE]  
> 1.  Les noms de variables ont le préfixe et suffixe avec un symbole $ (dollar). Si les variables ne sont pas attribués une valeur dans le fichier de la valeur de la variable, vous rencontrerez une erreur lors de l’analyse du fichier de script résultant de bloquer le processus d’exécution de console.  
> 2.  The escape character for **$** is **$$**. Si la valeur d’une valeur d’un paramètre de variable ou statique contient **$** symbole (dollar), puis **$$** doit être spécifié pour le traiter comme un caractère au lieu d’une variable.  
> 3.  À des fins de facilité de maintenance, les variables peuvent être déclarées à l’intérieur de `‘variable-group’` variables définies par des éléments pour une séparation logique de l’utilisateur.  L’utilisation de cet élément n’est pas obligatoire.  
  
**Exemples :**  
  
**Exemple 1 :**  
  
```  
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
  
```  
<!--Sample of variable value file commands-->  
  
<variables>  
  
  <variable-group name="SQLServerParams">  
  
    <variable-group name="SqlServerConnectionParams">  
  
      <variable name="$TargetServerName$" value="<server-name>"/>  
  
      <variable name="$TargetDB$" value="<database-name>"/>  
  
      <variable name="$TargetUserName$" value="<user-name>"/>  
  
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
  
## <a name="next-step"></a>Étape suivante  
L’étape suivante d’exploitation de la console est [création des fichiers de connexion serveur &#40;DB2ToSQL&#41;](../../ssma/db2/creating-the-server-connection-files-db2tosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Création des fichiers de connexion de serveur](http://msdn.microsoft.com/en-us/002f129e-0868-48ad-a4b4-c68b5007e12e)  
  
