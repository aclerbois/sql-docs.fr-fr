---
title: Prise en main de SSMA pour Oracle Console (OracleToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-oracle
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Oracle Console, Console Output Conventions
- Oracle Console, Launching Console
ms.assetid: 667a5e4a-6848-4973-a72d-1287f64718ac
caps.latest.revision: 31
author: Shamikg
ms.author: Shamikg
manager: v-thobro
ms.openlocfilehash: 95da4265bcaf56f7abbd5e445d93760790752b22
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="getting-started-with-ssma--for-oracle-console-oracletosql"></a>Prise en main de SSMA pour Oracle Console (OracleToSQL)
Cette section décrit la procédure pour lancer et commencer à l’application de console Oracle. Dans la liste, qu’elle contient, sont les conventions utilisées dans une fenêtre de sortie de Console de SSMA classique.  
  
## <a name="launching-ssma-console"></a>Lancer la Console SSMA  
Utilisez les étapes suivantes pour démarrer l’application de console SSMA :  
  
1.  Accédez à **Démarrer** et pointez sur **tous les programmes**.  
  
2.  Cliquez sur le  **[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] l’Assistant Migration de l’invite de commandes Oracle** contextuel.  
  
    Il affiche le menu de l’utilisation de SSMA Console et `(/? Help)`, pour vous aider à démarrer avec l’application console.  
  
## <a name="procedure-for-using-the-ssma-console"></a>Procédure d’utilisation de la Console SSMA  
Une fois que la console est lancée correctement sur votre système Windows, vous pouvez utiliser les étapes suivantes pour travailler dessus :  
  
1.  Configurer la Console de SSMA via les fichiers de script. Pour plus d’informations sur cette section, consultez [création de fichiers de Script &#40;OracleToSQL&#41; ](../../ssma/oracle/creating-script-files-oracletosql.md) .  
  
2.  [Création de fichiers de la valeur de la Variable &#40;OracleToSQL&#41;](../../ssma/oracle/creating-variable-value-files-oracletosql.md)  
  
3.  [Création des fichiers de connexion serveur &#40;OracleToSQL&#41;](../../ssma/oracle/creating-the-server-connection-files-oracletosql.md)  
  
4.  [L’exécution de la Console SSMA &#40;OracleToSQL&#41; ](../../ssma/oracle/executing-the-ssma-console-oracletosql.md) selon les besoins de votre projet  
  
Fonctionnalités supplémentaires :  
  
1.  [Spécifiez un mot de passe](http://msdn.microsoft.com/en-us/8c7d9f8e-06bb-476c-bbd2-15b61d5bba3c) et exporter / importer sur d’autres ordinateurs de la fenêtre  
  
2.  [Générer des rapports](http://msdn.microsoft.com/en-us/ccad6262-01e1-447a-bd2b-c105154c80ce) pour afficher le code xml détaillé rapports pour évaluation /conversion et migration des données de sortie. Rapports d’erreur détaillé peuvent également être générées pour les commandes d’actualisation et la synchronisation.  
  
## <a name="ssma-console-output-conventions"></a>Conventions de sortie de Console SSMA  
Lors de l’exécution des commandes de script SSMA et des options, le programme de console affiche les résultats et les messages (information, erreur, etc.) à l’utilisateur sur la console ou si nécessaire, redirige vers un fichier de sortie xml. Chaque type de message dans la sortie est signalé par une couleur unique. Par exemple, le message de couleur blanche indique les commandes du fichier de script. celui de couleur verte représente une invite de commandes pour l’entrée d’utilisateur et ainsi de suite.  
  
![Output_Oracle de Console SSMA](../../ssma/db2/media/ssmaconsoleoutput_oracle.jpg "Output_Oracle de Console SSMA")  
  
Interprétation de couleur de la sortie de console dans le tableau suivant :  
  
|Color| Description|  
|---------|---------------|  
|Rouge|Erreur irrécupérable lors de l’exécution|  
|Gris|Cachet de date et heure, le message à l’utilisateur|  
|White|Commandes du fichier de script, le type de message|  
|Jaune|Avertissement|  
|Vert|Invite d’entrée d’utilisateur|  
|Cyan|Début, fin et le résultat d’une opération|  
  
## <a name="see-also"></a>Voir aussi  
[L’installation de SSMA pour Oracle](http://msdn.microsoft.com/en-us/9211013a-ab24-4c52-9b26-87994b35e502)  
  
