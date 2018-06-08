---
title: Historique des pilotes de la base de données bureau | Documents Microsoft
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
- Jet-based ODBC drivers [ODBC], history
- ODBC desktop database drivers [ODBC], history
- desktop database drivers [ODBC], history
ms.assetid: b4a2aff8-bde7-4bd5-8580-bc50f27311c8
caps.latest.revision: 9
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 44d0dd0c1f460bbed2b256d89bb9e986764b5256
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="history-of-the-desktop-database-drivers"></a>Historique des pilotes de base de données de bureau
Le tableau suivant montre l’historique de version des pilotes de base de données de bureau.  
  
|Version|Date de sortie| Description|  
|-------------|------------------|-----------------|  
|1.0|Août 1993|Utiliser le processeur de requêtes SIMBA produit par le logiciel de PageAhead. SIMBA a reçu des appels ODBC et des instructions SQL, le traitement en appels ISAM installables Microsoft Jet et ensuite appelée la couche de distribution Microsoft Jet ISAM pour charger et appeler le pilote ISAM installable approprié.|  
|2|Décembre 1994|Utilisée avec ODBC 2.0, qui a considérablement augmenté la fonctionnalité ODBC. La principale modification dans la version 2.0 était que le moteur de base de données Microsoft Jet remplacé le processeur de requêtes SIMBA. Avec le moteur de base de données Microsoft Jet, les pilotes de base de données de bureau intégré plus étroitement avec la technologie de Microsoft Access et de pilotes ISAM installables de Microsoft Jet. Des améliorations importantes ont été :<br /><br /> -Prise en charge native pour les curseurs de défilement.<br />-Prise en charge native des jointures externes, les jointures hétérogènes et mettre à jour et les transactions.<br />-les versions 32 bits des pilotes pour Microsoft Windows NT.|  
|3|Octobre 1995|Fourni la prise en charge de Windows 95 et Windows NT Workstation ou Server NT 3.51. Seuls les pilotes 32 bits ont été inclus dans cette version. les pilotes 16 bits pour la version 3.1 de Windows ont été supprimés.|  
|3.5|Octobre 1996|Ces pilotes ont été le jeu de caractères de deux octets (DBCS)-activée, ont été mieux adapté pour une utilisation avec les applications Internet que les versions antérieures et prises en charge l’utilisation de noms de sources de données de fichier (DSN). Le pilote Microsoft Access a été publié dans une version RISC pour une utilisation sur les plates-formes Alpha pour Windows 95/98 et Windows NT 3.51 et systèmes d’exploitation ultérieurs.|  
|4.0|1998 à liaison tardive|Prend en charge le format Unicode de moteur Jet Microsoft ainsi que de la compatibilité pour le format ANSI des versions antérieures.|  
  
> [!NOTE]  
>  Les pilotes version3.5 ont été conçus pour fonctionner avec ODBC2. *x*. Bien qu’ils fonctionnent également avec ODBC 3.0, ils ne gèrent pas toutes les fonctions ODBC 3.0. Pour plus d’informations sur le fonctionnement de ces pilotes ODBC 3.0, consultez [la compatibilité descendante et la conformité aux normes](../../odbc/reference/develop-app/backward-compatibility-and-standards-compliance.md).
