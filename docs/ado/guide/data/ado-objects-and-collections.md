---
title: Collections et les objets ADO | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- ADO, objects and collections
ms.assetid: 7a745aae-9372-49b6-8dae-b9c93e5f3216
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 03671824ef02a7316398f1d9b8d51d57845be736
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ado-objects-and-collections"></a>Collections et les objets ADO
ADO comprend les objets suivants de neuf et quatre regroupements.  
  
|Objet ou la Collection| Description|  
|--------------------------|-----------------|  
|**Connexion** objet|Représente une session unique avec une source de données. Dans le cas d’un système de base de données client/serveur, il peut être équivalente à une connexion réseau réelle au serveur. Selon les fonctionnalités prises en charge par le fournisseur, certaines collections, des méthodes ou propriétés d’un **connexion** objet n’est peut-être pas disponible.|  
|Objet**Command** |Permet de définir une commande spécifique, comme une requête SQL, destinée à s’exécuter sur une source de données.|  
|**Jeu d’enregistrements** objet|Représente l’ensemble d’enregistrements à partir d’une table de base ou les résultats d’une commande exécutée. Tous les **Recordset** objets sont constitués d’enregistrements (lignes) et des champs (colonnes).|  
|**Enregistrement** objet|Représente une ligne unique de données, à partir d’un **Recordset** ou à partir du fournisseur. Cet enregistrement peut représenter un enregistrement de base de données ou un autre type d’objet tel qu’un fichier ou répertoire, en fonction de votre fournisseur.|  
|**Flux** objet|Représente un flux de données binaire ou texte. Par exemple, un document XML peut être chargé dans un flux d’entrée ou retournée à partir de certains fournisseurs, comme les résultats d’une requête de commande. A **flux** objet peut être utilisé pour manipuler des champs ou enregistrements contenant ces flux de données.|  
|**Paramètre** objet|Représente un paramètre ou un argument associé à un **commande** objet basé sur une procédure stockée ou une requête paramétrable.|  
|**Champ** objet|Représente une colonne de données avec un type de données commun. Chaque **champ** objet correspond à une colonne dans la **Recordset**.|  
|**Propriété** objet|Représente une caractéristique d’un objet ADO qui est défini par le fournisseur. Objets ADO possèdent deux types de propriétés : intégrées et dynamiques. Les propriétés intégrées sont les propriétés implémentées dans ADO et immédiatement disponibles pour tout nouvel objet. Le **propriété** objet est un conteneur pour les propriétés dynamiques, définies par le fournisseur sous-jacent.|  
|Objet**Error** |Contient des détails sur les erreurs d’accès aux données qui se rapportent à une seule opération impliquant le fournisseur.|  
|**Champs** collection|Contient tous les **champ** les objets d’un **Recordset** ou **enregistrement** objet.|  
|**Propriétés** collection|Contient tous les **propriété** objets pour une instance spécifique d’un objet.|  
|**Paramètres** collection|Contient tous les **paramètre** les objets d’un **commande** objet.|  
|**Erreurs** collection|Contient tous les **erreur** objets créés en réponse à une erreur liée au fournisseur.|  
  
## <a name="see-also"></a>Voir aussi  
 [Modèle objet ADO](../../../ado/reference/ado-api/ado-object-model.md)
