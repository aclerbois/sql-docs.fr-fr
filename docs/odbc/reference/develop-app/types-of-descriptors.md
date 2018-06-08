---
title: Types de descripteurs | Documents Microsoft
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
- descriptors [ODBC], types
ms.assetid: ec20e446-e540-41ad-8559-d9c0a5b8358f
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 8834d05eda6393b00f528cb0c662f88e44d4c8a6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="types-of-descriptors"></a>Types de descripteurs
Un descripteur est utilisé pour décrire l’un des éléments suivants :  
  
-   Un ensemble de zéro ou plusieurs paramètres. Un descripteur de paramètre peut être utilisé pour décrire :  
  
    -   Le *tampon de paramètre d’application,* qui contient les arguments dynamiques d’entrée en tant que jeu par l’application ou les arguments de sortie dynamiques après l’exécution d’un **appeler** instruction SQL.  
  
    -   Le *mémoire tampon de paramètre de mise en œuvre*. Pour les arguments d’entrée dynamiques, il contient les mêmes arguments que le tampon de paramètres d’application, après toute conversion de données que l’application peut spécifier. Pour les arguments de sortie dynamiques, contient les arguments retournés, avant toute conversion de données que l’application peut spécifier.  
  
     Pour les arguments d’entrée dynamiques, l’application doit fonctionner sur un descripteur de paramètre d’application avant d’exécuter une instruction SQL qui contient des marqueurs de paramètres dynamiques. Pour les arguments d’entrée et de sortie de dynamiques, l’application peut spécifier différents types de données de celles figurant dans le descripteur de paramètre de mise en œuvre pour atteindre la conversion de données.  
  
-   Une seule ligne de base de données. Un descripteur de ligne peut être utilisé pour décrire :  
  
    -   Le *tampon de ligne d’implémentation,* qui contient la ligne à partir de la base de données. (Ces mémoires tampons sur le plan conceptuel contiennent des données lors de l’écriture à ou lire à partir de la base de données. Toutefois, le formulaire stocké de base de données n’est pas spécifié. Une base de données pourrait effectuer de conversion supplémentaire sur les données à partir de sa forme dans la mémoire tampon d’implémentation.)  
  
    -   Le *tampon de ligne d’application,* qui contient la ligne de données, tel que présenté à l’application, après toute conversion de données que l’application peut spécifier.  
  
     L’application s’exécute sur le descripteur de ligne d’application dans tous les cas où les données des colonnes de la base de données doivent apparaître dans les variables d’application. Pour obtenir une conversion de données de la colonne de données, l’application peut spécifier les types de données différents de ceux dans le descripteur de ligne d’implémentation.  
  
 Les types de descripteur sont résumées dans le tableau suivant.  
  
|Type de tampon|Lignes|Paramètres dynamiques|  
|-----------------|----------|------------------------|  
|**Mémoire tampon d’application**|Descripteur de ligne d’application (ARD)|Descripteur de paramètre d’application (APD)|  
|**Mémoire tampon de mise en œuvre**|Descripteur de ligne d’implémentation (IRD)|Descripteur de paramètre d’implémentation (IPD)|  
  
 Pour le paramètre ou les mémoires tampons de ligne, si l’application spécifie les différents types de données dans les enregistrements correspondants des descripteurs de mise en œuvre et l’application, le pilote effectue la conversion de données lorsqu’il utilise les descripteurs de. Par exemple, il peut convertir des valeurs numériques et de date/heure au format de chaîne de caractères. (Pour les conversions valides, consultez [annexe d : les Types de données](../../../odbc/reference/appendixes/appendix-d-data-types.md).)  
  
 Un descripteur permettre exécuter différents rôles. Instructions différentes peuvent partager le descripteur de l’application alloue explicitement. Un descripteur de ligne dans une seule instruction peut servir d’un descripteur de paramètre dans une autre instruction.  
  
 Il est toujours connue si un descripteur donné est un descripteur de l’application ou un descripteur d’implémentation, même si le descripteur n’a pas encore été utilisé dans une opération de base de données. Pour les descripteurs implicitement alloue de l’implémentation, l’implémentation enregistre la ligne prédéfinie par rapport au handle d’instruction. Un descripteur qui alloue de l’application en appelant **SQLAllocHandle** est un descripteur de l’application.
