---
title: Messages de diagnostic | Documents Microsoft
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
- diagnostic information [ODBC], diagnostic messages messages
- error messages [ODBC], diagnostic messages
- diagnostic messages [ODBC]
ms.assetid: 98027871-9901-476e-a722-ee58b7723c1f
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 9ea827e040afa43762edc32c5a4c28de5581b94e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="diagnostic-messages"></a>Messages de diagnostic
Un message de diagnostic est retourné avec chaque SQLSTATE. La même valeur SQLSTATE est souvent retournée avec un nombre de messages différents. Par exemple, SQLSTATE 42000 (syntaxe ou violation d’accès) est retournée pour la plupart des erreurs dans la syntaxe SQL. Toutefois, chaque erreur de syntaxe est susceptible d’être décrite par un autre message.  
  
 Exemples de messages de diagnostic sont répertoriées dans la colonne d’erreur dans la table de SQLSTATE dans l’annexe A et dans chaque fonction. Bien que les pilotes peuvent retourner ces messages, ils sont plus susceptibles de renvoyer le message leur est passée à la source de données.  
  
 Applications affichent généralement des messages de diagnostic à l’utilisateur, ainsi que les chaînes SQLSTATE et code d’erreur natif. Cela permet à l’utilisateur et le service de support technique déterminer la cause des problèmes. Informations sur les composant incorporées dans le message sont particulièrement utiles pour cette opération.  
  
 Messages de diagnostic proviennent de sources de données et les composants d’une connexion ODBC, tels que le Gestionnaire de pilotes, les passerelles et les pilotes. En règle générale, les sources de données ne gèrent pas directement ODBC. Par conséquent, si un composant dans une connexion ODBC reçoit un message à partir d’une source de données, elle doit identifier la source de données comme la source du message. Il doit également s’identifier en tant que le composant qui a reçu le message.  
  
 Si la source d’une erreur ou un avertissement est un composant lui-même, le message de diagnostic doive expliquer ceci. Par conséquent, le texte des messages a deux formats. Pour les erreurs et avertissements qui n’apparaissent pas dans une source de données, le message de diagnostic doit utiliser ce format :  
  
 **[** *identificateur du fournisseur* **] [** *identificateur de composant ODBC* **]** *texte composant fourni*  
  
 Pour les erreurs et avertissements qui se produisent dans une source de données, le message de diagnostic doit utiliser ce format :  
  
 **[** *identificateur du fournisseur* **] [** *identificateur de composant ODBC* **] [** *identificateur de source de données*  **]** *données source-fourni en texte intégral*  
  
 Le tableau suivant indique la signification de chaque élément.  
  
|Élément|Signification|  
|-------------|-------------|  
|*Identificateur du fournisseur*|Identifie le fournisseur du composant dans lequel l’erreur ou l’avertissement s’est produite ou qui a reçu l’erreur ou avertissement directement à partir de la source de données.|  
|*Identificateur de composant ODBC*|Identifie le composant dans lequel l’erreur ou l’avertissement s’est produite ou qui a reçu l’erreur ou avertissement directement à partir de la source de données.|  
|*identificateur de source de données*|Identifie la source de données. Pour les pilotes basée sur le fichier, il s’agit généralement d’un format de fichier, tels que les pilotes basés sur SGBD pour Xbase [1], il s’agit du produit du SGBD.|  
|*composant fourni en texte intégral*|Généré par le composant ODBC.|  
|*données source-fourni en texte intégral*|Généré par la source de données.|  
  
 [1] dans ce cas, le pilote agit en tant que le pilote et la source de données.  
  
 Crochets (**[]**) doit être inclus dans le message et n’indiquent pas les éléments facultatifs.
