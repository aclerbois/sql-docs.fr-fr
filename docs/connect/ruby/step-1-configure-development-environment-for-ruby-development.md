---
title: 'Étape 1 : Configurer l’environnement de développement pour le développement Ruby | Documents Microsoft'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.component: ruby
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 8cdbadeb-f640-406c-977c-d2d44b7b5368
caps.latest.revision: 9
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: c8b4cd36a78952e8d783e79388ede49287e5e0f7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="step-1-configure-development-environment-for-ruby-development"></a>Étape 1 : Configurer l’environnement de développement pour le développement Ruby
Vous devez configurer votre environnement de développement avec les composants requis pour développer une application à l’aide du pilote Ruby pour SQL Server.    
  
Notez que le pilote Ruby utilise le protocole TDS, qui est activé par défaut dans SQL Server et la base de données SQL Azure.  Aucune configuration supplémentaire n’est requise.  
  
  
## <a name="windows"></a>Windows  
  
1.  **Télécharger le programme d’installation Ruby**  
Si votre ordinateur ne dispose pas de Ruby Veuillez l’installer. Pour les nouveaux utilisateurs ruby, nous vous recommandons de qu'utiliser programmes d’installation 2.2.X Ruby. Ils fournissent une langue stable et une liste complète des packages (gems) qui sont compatibles et mis à jour. Atteindre le [page de téléchargement Ruby](http://rubyinstaller.org/downloads/) et téléchargez le programme d’installation 2.1.x approprié. Pour exemple, si vous êtes sur un ordinateur 64 bits, téléchargez le programme d’installation Ruby 2.1.6 (x 64).   
  
2.  **Installer Ruby**  
Une fois le programme d’installation téléchargé, procédez comme suit :  
A. Double-cliquez sur le fichier pour démarrer le programme d’installation.  
B. Sélectionnez votre langue et j’accepte les termes du contrat.  
c.  Dans l’écran Paramètres d’installation, sélectionnez les cases à cocher en regard de ces deux exécutables Ruby d’ajouter à vos fichiers .rb et .rbw chemin d’accès et les associer avec cette installation Ruby.  
  
3.  **Télécharger le Kit de développement Ruby**  
Télécharger le Kit de développement à partir de la page RubyInstaller  
  
4.  **Installer le Kit de développement Ruby**  
Une fois le téléchargement terminé, procédez comme suit :  
A. Double-cliquez sur le fichier. Vous devez where extraire les fichiers.  
B. Cliquez sur le bouton «... » et sélectionnez « C:\DevKit ». Vous devrez probablement d’abord créer ce dossier en cliquant sur « Créer un nouveau dossier ».  
c. Cliquez sur « OK » et puis « extraire », pour extraire les fichiers.  
  
5. **Ouvrez cmd.exe**  
  
6. **Initialiser le Kit de développement Ruby**  
```  
> chdir C:\DevKit  
> ruby dk.rb init  
> ruby dk.rb install  
```  
  
7.  **Installer TinyTDS marque**  
```  
> gem inst tiny_tds
```  
  
## <a name="ubuntu-linux"></a>Ubuntu Linux  
  
1. **Ouvrez Terminal Server**  
  
2. **Installez le Gestionnaire de versions Ruby (rvm) et les composants requis**  
```  
> sudo apt-get --assume-yes update  
> command curl -sSL https://rvm.io/mpapis.asc | gpg --import -  
> curl -L https://get.rvm.io | bash -s stable  
> source ~/.rvm/scripts/rvm  
```  
   
3. **Permet d’installer Ruby rvm**  
Par exemple, installez la version 2.3.0 de Ruby :  
```  
> rvm install 2.3.0  
> rvm use 2.3.0 --default  
> ruby -v  
```  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Vérifiez que la sortie de la dernière commande indique la que version 2.3.0 sont en cours d’exécution.  
  
4.  **Installer FreeTDS**  
```  
> sudo apt-get --assume-yes install freetds-dev freetds-bin  
```  
  
5.  **Installer TinyTDS**  
```  
> gem install tiny_tds  
```  
  
## <a name="mac"></a>Mac  
  
Notez que Mac OS X a déjà Ruby préinstallé, que le système d’exploitation a une dépendance.    
  
1.  **Ouvrez Terminal Server**  
  
2. **Installer le Gestionnaire de package Homebrew**  
```  
> ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"  
```  
  
3.  **Installer FreeTDS**  
```  
> brew install FreeTDS  
```  
  
4.  **Installer TinyTDS marque**  
```  
> gem install tiny_tds  
```
