---
title: Ajout et modification des données Sources à l’aide du programme d’installation | Documents Microsoft
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
- data sources [ODBC], adding
- editing data sources [ODBC], ODBC driver for Oracle
- adding data sources [ODBC], ODBC driver for Oracle
- data sources [ODBC], changing
- data sources [ODBC], ODBC driver for Oracle
- ODBC driver for Oracle [ODBC], adding data sources
ms.assetid: 54b2d61d-6ce5-45af-a776-e03180470ecf
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 87a099890327cd4a01d5cd36cd15fc5adcaca4b7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="adding-and-modifying-data-sources-using-setup"></a>Ajout et modification de Sources de données à l’aide du programme d’installation
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Au lieu de cela, utilisez le pilote ODBC fourni par Oracle.  
  
 Une source de données identifie un chemin d’accès aux données qui peuvent inclure une bibliothèque réseau, serveur, base de données et d’autres attributs : dans ce cas, la source de données est le chemin d’accès à une base de données Oracle. Pour vous connecter à une source de données, le Gestionnaire de pilotes vérifie le Registre Windows pour les informations de connexion spécifiques.  
  
 L’entrée de Registre créée par l’administrateur de Source de données ODBC est utilisée par les pilotes ODBC et de gestionnaire de pilotes ODBC. Cette entrée contient des informations sur chaque source de données et son pilote associé. Avant de vous connecter à une source de données, ses informations de connexion doivent être ajoutées au Registre.  
  
 Pour ajouter et configurer des sources de données, utilisez la [administrateur de sources de données ODBC](../../odbc/admin/odbc-data-source-administrator.md). L’administrateur ODBC met à jour vos informations de connexion de source de données. Lorsque vous ajoutez des sources de données, l’administrateur ODBC met à jour les informations du Registre pour vous.  
  
### <a name="to-add-a-data-source-for-windows"></a>Pour ajouter une source de données pour Windows  
  
1.  Ouvrez l’administrateur de Source de données ODBC.  
  
2.  Dans la boîte de dialogue Administrateur de sources de données ODBC, cliquez sur Ajouter. La boîte de dialogue Créer une nouvelle Source de données s’affiche.  
  
3.  Sélectionnez Microsoft ODBC pour Oracle et puis cliquez sur Terminer. Microsoft ODBC pour la boîte de dialogue Oracle le programme d’installation s’affiche.  
  
4.  Dans la zone Nom de Source de données, tapez le nom de la source de données que vous souhaitez accéder. Il peut être n’importe quel nom que vous choisissez.  
  
5.  Dans la zone Description, tapez la description du pilote. Ce champ facultatif décrit le pilote de base de données que la source de données se connecte à. Il peut être n’importe quel nom que vous choisissez.  
  
6.  Dans la zone Nom d’utilisateur, tapez votre nom d’utilisateur de base de données (votre ID utilisateur de base de données).  
  
7.  Dans la zone serveur, tapez l’Alias de la base de données ou se connecter à la chaîne pour le moteur de serveur Oracle que vous souhaitez accéder.  
  
8.  Cliquez sur OK pour ajouter cette source de données.  
  
> [!NOTE]  
>  La boîte de dialogue de Sources de données s’affiche, et l’administrateur ODBC met à jour les informations du Registre. L’utilisateur nom et la chaîne que vous avez tapé de connexion deviennent les valeurs de connexion par défaut pour cette source de données lorsque vous vous connectez à ce dernier.  
  
1.  Cliquez sur Créer des Options plus spécifications sur le pilote ODBC pour le programme d’installation Oracle :  
  
    -   **Traduction** , cliquez sur Sélectionner pour choisir un convertisseur de données chargé. La valeur par défaut est \<pas de convertisseur >.  
  
    -   **Performances** — l’inclure la section Notes dans les fonctions de catalogue case à cocher spécifie si le pilote retourne les colonnes de la section Notes pour le [SQLColumns](../../odbc/microsoft/level-1-api-functions-odbc-driver-for-oracle.md) jeu de résultats. Le pilote ODBC pour Oracle fournit un accès plus rapide lorsque cette valeur n’est pas définie.  
  
         Les synonymes à inclure dans la case à cocher colonnes SQL Spécifie si le pilote retourne des informations sur les colonnes. **Taille de mémoire tampon** spécifie la taille, en octets, allouée pour recevoir les données extraites. Le pilote optimise l’extraction afin qu’une opération d’extraction à partir du serveur Oracle renvoie suffisamment de lignes pour remplir une mémoire tampon de la taille spécifiée. De plus grandes valeurs ont tendance à augmenter les performances lors de la récupération d’une grande quantité de données.  
  
    -   **Personnalisation** — l’appliquer Standard ODBC DayOfWeek case à cocher spécifie si le jeu de résultats est conforme au format de jour de la semaine spécifié ODBC (dimanche = 1 ; Samedi = 7). Si cette case à cocher est désactivée, la valeur de Oracle spécifique aux paramètres régionaux est retournée.  
  
         La fonction SQLDescribeCol **retourne toujours une valeur de précision** case à cocher spécifie si le pilote doit retourner une valeur différente de zéro pour le *cbColDef* argument de **SQLDescribeCol**. Cet attribut de chaîne de connexion s’applique uniquement aux colonnes où il n’est aucune échelle définis par Oracle, tel que calculé numériques colonnes et les colonnes définies en tant que nombre sans échelle ni précision. A **SQLDescribeCol** appeler renvoie 130 pour la précision lorsque Oracle ne fournit pas ces informations. Si cette case à cocher est désactivée, le pilote retourne 0 pour ces types de colonnes.  
  
2.  Cliquez sur Ajouter pour ajouter une autre source de données, ou cliquez sur Fermer pour quitter.  
  
### <a name="to-modify-a-data-source-for-windows"></a>Pour modifier une source de données pour Windows  
  
1.  Ouvrez l’administrateur de Source de données ODBC. Cliquez sur l’onglet approprié de la source de données.  
  
2.  Sélectionnez la source de données Oracle que vous souhaitez modifier, puis cliquez sur Configurer. Microsoft ODBC pour la boîte de dialogue Oracle le programme d’installation s’affiche.  
  
3.  Modifiez les champs de source de données applicable, puis cliquez sur OK.  
  
 Lorsque vous avez terminé de modifier les informations contenues dans cette boîte de dialogue, l’administrateur ODBC met à jour les informations du Registre.
