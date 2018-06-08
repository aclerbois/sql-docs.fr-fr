---
title: 'Étape 1 : Configurer l’environnement de développement pour le développement de Node.js | Documents Microsoft'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.component: node-js
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 2dad01f1-fadf-4ac9-9b4d-26be3d301886
caps.latest.revision: 17
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 313adb458786f009cd2cbd4fa86c09d7d5302b29
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="step-1--configure-development-environment-for-nodejs-development"></a>Étape 1 : Configurer l’environnement de développement pour le développement de Node.js
Vous devez configurer votre environnement de développement avec les composants requis pour développer une application à l’aide du pilote Node.js pour SQL Server.  La méthode la plus courante consiste à utiliser le Gestionnaire de package de Node (npm) pour installer le module tedious, mais vous pouvez télécharger le module tedious directement au [Github](https://github.com/pekim/tedious) si vous préférez.  
  
Notez que le pilote Node.js utilise le protocole TDS, qui est activé par défaut dans SQL Server et la base de données SQL Azure.  Aucune configuration supplémentaire n’est requise.  
  
## <a name="windows"></a>Windows  
  
1. **Installer le runtime Node.js et le gestionnaire de paquets npm**  
A. Accédez à [Node.js](https://nodejs.org/en/download/)  
B. Cliquez sur le lien de msi du programme d’installation Windows approprié.   
c. Une fois téléchargé, exécutez le fichier msi pour installer Node.js  
  
2. **Ouvrez cmd.exe**  
  
3. **Créer un répertoire projet** pour y accéder.    
```  
> mkdir HelloWorld  
> cd HelloWorld  
```  
4. **Créez un projet de node.**  Pour conserver les valeurs par défaut lors de la création de votre projet, appuyez sur entrée jusqu'à ce que le projet est créé. À la fin de cette étape, vous devez voir un fichier package.json dans votre répertoire de projet.  
```  
> npm init  
```  
  
5. **Installez le module tedious dans votre projet.**  Il s’agit de l’implémentation du protocole TDS, qui le pilote utilise pour communiquer avec SQL Server.  
```  
> npm install tedious  
```  
  
## <a name="ubuntu-linux"></a>Ubuntu Linux  
  
1.  **Ouvrez Terminal Server**  
  
2. **Installer le runtime de Node.js**  
```  
>sudo apt-get install node  
```  
3. **Installez npm (Gestionnaire de package de node)**  
```  
> sudo apt-get install npm  
```  
4. **Créer un répertoire projet** pour y accéder.    
```  
> mkdir HelloWorld  
> cd HelloWorld  
```  
  
5. **Créez un projet de node.**  Pour conserver les valeurs par défaut lors de la création de votre projet, appuyez sur entrée jusqu'à ce que le projet est créé. À la fin de cette étape, vous devez voir un fichier package.json dans votre répertoire de projet.  
```  
> sudo npm init  
```  
  
6. **Installez le module tedious dans votre projet.**  Il s’agit de l’implémentation du protocole TDS, qui le pilote utilise pour communiquer avec SQL Server.  
```  
> sudo npm install tedious  
```  
  
## <a name="mac"></a>Mac  
  
1. **Installer le runtime Node.js et le gestionnaire de paquets npm**  
A. Accédez à [Node.js](https://nodejs.org/en/download/)  
B. Cliquez sur le lien du programme d’installation du système d’exploitation Mac approprié.  
c. Une fois téléchargé, exécutez le dmg pour installer Node.js  
  
2. **Ouvrez Terminal Server**  
  
3. **Créer un répertoire projet** pour y accéder.    
```  
> mkdir HelloWorld  
> cd HelloWorld  
```  
  
4. **Créez un projet de node.**  Pour conserver les valeurs par défaut lors de la création de votre projet, appuyez sur entrée jusqu'à ce que le projet est créé. À la fin de cette étape, vous devez voir un fichier package.json dans votre répertoire de projet.  
```  
> npm init  
```  
  
5. **Installez le module tedious dans votre projet.**  Il s’agit de l’implémentation du protocole TDS, qui le pilote utilise pour communiquer avec SQL Server.  
```  
> npm install tedious  
```  
  
