---
title: Format de chaîne de connexion et les attributs | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- connection strings [ODBC], ODBC driver for Oracle
- ODBC driver for Oracle [ODBC], connection strings
ms.assetid: 0c360112-8720-4e54-a1a6-b9b18d943557
caps.latest.revision: 9
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 258391853e03879b6b970347c8ca3f6f2e0eacc9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connection-string-format-and-attributes"></a>Attributs et le Format de chaîne de connexion
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Au lieu de cela, utilisez le pilote ODBC fourni par Oracle.  
  
 Au lieu d’utiliser une boîte de dialogue, certaines applications peuvent nécessiter une chaîne de connexion qui spécifie les informations de connexion de source de données. La chaîne de connexion est composée d’un nombre d’attributs qui spécifient comment un pilote se connecte à une source de données. Un attribut identifie une information spécifique que le pilote doit savoir avant d’établir la connexion de source de données approprié. Chaque pilote peut avoir un autre ensemble d’attributs, mais le format de chaîne de connexion est toujours le même. Une chaîne de connexion a le format suivant :  
  
```  
"DSN=data-source-name[;SERVER=value] [;PWD=value] [;UID=value] [;<Attribute>=<value>]"  
```  
  
> [!NOTE]  
>  Le pilote Microsoft ODBC pour Oracle prend en charge le format de chaîne de connexion de la première version du pilote utilisé `CONNECTSTRING`= à la place de `SERVER=`.  
  
 Si vous vous connectez à un fournisseur de source de données qui prend en charge l’authentification Windows, vous devez spécifier `Trusted_Connection=yes` au lieu des informations d’ID et mot de passe utilisateur dans la chaîne de connexion.  
  
 Vous devez spécifier la source de données nom si vous ne spécifiez pas l’UID, PWD, SERVER (ou CONNECTSTRING) et les attributs de pilote. Toutefois, tous les autres attributs sont facultatifs. Si vous ne spécifiez pas un attribut, cet attribut par défaut est celle spécifiée dans l’onglet approprié de la source de données de la **administrateur de sources de données ODBC** boîte de dialogue. La valeur d’attribut peut respecter la casse.  
  
 Les attributs de la chaîne de connexion sont les suivantes :  
  
|Attribut| Description|Valeur par défaut|  
|---------------|-----------------|-------------------|  
|DSN|Le nom de source de données répertoriés dans l’onglet pilotes de la **administrateur de sources de données ODBC** boîte de dialogue.|""|  
|PWD|Le mot de passe pour le serveur Oracle que vous souhaitez accéder. Ce pilote prend en charge les limitations Oracle place sur les mots de passe.|""|  
|SERVER|La chaîne de connexion pour le serveur Oracle que vous souhaitez accéder.|""|  
|UID|Le nom d’utilisateur de serveur Oracle. Selon votre système, cet attribut ne peut pas être facultatif, c'est-à-dire que certaines bases de données et les tables peuvent nécessiter cet attribut pour des raisons de sécurité.<br /><br /> Utilisez « / » pour utiliser Oracle d’exploitation de l’authentification du système.|""|  
|BUFFERSIZE|La taille du tampon optimal utilisée lors de l’extraction des colonnes.<br /><br /> Le pilote optimise l’extraction afin qu’une opération d’extraction à partir du serveur Oracle renvoie suffisamment de lignes pour remplir une mémoire tampon de cette taille. De plus grandes valeurs ont tendance à augmenter les performances si vous lisez une grande quantité de données.|65535|  
|SYNONYMCOLUMNS|Lorsque cette valeur est true (1), un appel de () API SQLColumn renvoie des informations de colonne. Sinon, (de) SQLColumn retourne uniquement les colonnes pour les tables et vues. Le pilote ODBC pour Oracle fournit un accès plus rapide lorsque cette valeur n’est pas définie.|1|  
|REMARKS|Lorsque cette valeur est true (1), le pilote retourne les colonnes de la section Notes pour le [SQLColumns](../../odbc/microsoft/level-1-api-functions-odbc-driver-for-oracle.md) jeu de résultats. Le pilote ODBC pour Oracle fournit un accès plus rapide lorsque cette valeur n’est pas définie.|0|  
|StdDayOfWeek|Applique la norme ODBC pour la valeur scalaire DAYOFWEEK. Par défaut cette est activée, mais les utilisateurs qui ont besoin de la version localisée peuvent modifier le comportement pour utiliser tout élément retourné par Oracle.|1|  
|GuessTheColDef|Spécifie si le pilote doit retourner une valeur différente de zéro pour le *cbColDef* argument de **SQLDescribeCol**. S’applique uniquement aux colonnes où il n’est aucune échelle définis par Oracle, tel que calculé numériques colonnes et les colonnes définies en tant que nombre sans échelle ni précision. A **SQLDescribeCol** appeler renvoie 130 pour la précision lorsque Oracle ne fournit pas ces informations.|0|  
  
 Par exemple, une chaîne de connexion qui se connecte à la source de données MyDataSource en utilisant le serveur MyOracleServerOracle et le monIdUtilisateur utilisateur Oracle serait :  
  
```  
"DSN={MyDataSource};UID={MyUserID};PWD={MyPassword};SERVER={MyOracleServer}"  
```  
  
 Une chaîne de connexion qui se connecte à la source de données MyOtherDataSource à l’aide de l’authentification du système d’exploitation et le serveur MyOtherOracleServerOracle serait :  
  
```  
"DSN=MyOtherDataSource;UID=/;PWD=;SERVER=MyOtherOracleServer"  
```
