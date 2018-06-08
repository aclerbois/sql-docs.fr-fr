---
title: 'Étape 1 : Configurer l’environnement de développement Python pyodbc | Documents Microsoft'
ms.custom: ''
ms.date: 08/08/2017
ms.prod: sql
ms.prod_service: connectivity
ms.component: python
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 74e69704-e63c-450b-9207-5c1491d0e0f5
caps.latest.revision: 2
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 253a4f16b5e5319ff4d805a8fb16114f534bee02
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="step-1-configure-development-environment-for-pyodbc-python-development"></a>Étape 1 : Configurer l’environnement de développement pour pyodbc développement de Python

## <a name="windows"></a>Windows  
Se connecter à la base de données SQL à l’aide de Python - pyodbc sur Windows :
  
1. **Télécharger le programme d’installation de Python**  
  Si votre ordinateur ne dispose pas de Python Veuillez l’installer. Atteindre le [page de téléchargement de Python](https://www.python.org/downloads/windows/) et téléchargez le programme d’installation approprié. Pour exemple, si vous êtes sur un ordinateur 64 bits, téléchargez le programme d’installation de Python 2.7 ou 3.5 (x 64).  
  
2. **Installer Python** une fois le programme d’installation téléchargé, procédez comme suit : un. Double-cliquez sur le fichier pour démarrer le programme d’installation. B. Sélectionnez votre langue et j’accepte les termes du contrat. c. Suivez les instructions à l’écran et Python doit être installé sur votre ordinateur. d. Vous pouvez vérifier qui est Python est installé en accédant à C:\Python27 ou C:\Python35 et exécuter python - v ou pier - v (3.x) 
      
3. [**Installer le pilote ODBC de Microsoft**](../../sql-connection-libraries.md#anchor-20-drivers-relational-access)
  
4. **Ouvrez cmd.exe en tant qu’administrateur**     

5. **Installer pyodbc l’utilisation de pip - Gestionnaire de package de Python**
```  
> cd C:\Python27\Scripts>  
> pip install pyodbc  
```  

  
## <a name="linux"></a>Linux 
Se connecter à la base de données SQL à l’aide de Python - pyodbc sur Ubuntu et RedHat :
  
1. **Ouvrez Terminal Server**  

2. **Installer Microsoft ODBC Driver 13 for Linux** pour Ubuntu 15.04 + 
``` 
> sudo su  
> wget https://gallery.technet.microsoft.com/ODBC-Driver-13-for-Ubuntu-b87369f0/file/154097/2/installodbc.sh  
> sh installodbc.sh  
```   

  Pour RedHat 6,7 
``` 
> sudo su 
> wget https://gallery.technet.microsoft.com/ODBC-Driver-13-for-SQL-8d067754/file/153653/4/install.sh 
> sh install.sh 
```  
  
3.  **Installer pyodbc**  
```  
> sudo -H pip install pyodbc
```
