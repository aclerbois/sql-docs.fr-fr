---
title: Définir le Format de texte (pilote du fichier texte) | Documents Microsoft
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
- text format [ODBC]
- text file driver [ODBC], text format
ms.assetid: 3af46dad-52cc-4d5c-a27e-6315d65a74e6
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 4b8d87785ccb63796698e164b36728301d3c35e5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="defining-text-format-text-file-driver"></a>Définir le Format de texte (pilote du fichier texte)
Lorsque le pilote de texte est utilisé, vous pouvez utiliser la **définition du Format texte** boîte de dialogue pour définir le format des colonnes dans un fichier sélectionné. Cette boîte de dialogue vous permet de spécifier le schéma pour chaque table de données. Cette information est écrite dans un fichier Schema.ini dans le répertoire de source de données. Un fichier Schema.ini distinct est créé pour chaque répertoire de source de données de texte.  
  
> [!NOTE]  
>  Le même format de fichier par défaut s’applique à toutes les nouvelles tables de données de texte. Tous les fichiers créés par l’instruction CREATE TABLE héritent de ces mêmes valeurs de format par défaut, qui sont définies en sélectionnant les valeurs de format de fichier dans le **définition du Format texte** boîte de dialogue avec \<par défaut > choisi dans le **Tables** liste. Le pilote de texte ne modifie pas le format d’un fichier texte existant pour faire correspondre le format défini dans cette boîte de dialogue, mais retourne une erreur lorsqu’il utilise le format, par exemple quand il tente de récupérer des données à partir du fichier texte.  
  
 Les options suivantes sont disponibles dans le **définition du Format texte** boîte de dialogue :  
  
|Option|Informations|  
|------------|-----------------|  
|**Ajouter**|Ajoute une colonne en utilisant les valeurs **Type de données**, **nom**, et **largeur** à partir de la boîte de dialogue, et si applicable, le séparateur de Date valeur Schema.ini.|  
|**Caractères**|**ANSI** ou **OEM**. OEM spécifie un jeu de caractères non-ANSI. Par défaut est OEM si le format de l’élément sélectionné dans le **Tables** liste n’a pas été précédemment définie par cette boîte de dialogue.|  
|**En-tête de nom de colonne**|Indique si les colonnes de la première ligne de la table sélectionnée doivent être utilisés comme noms de colonne. Soit **TRUE** ou **FALSE**. Valeur par défaut est FALSE si le format de l’élément sélectionné dans le **Tables** liste n’a pas été précédemment définie par cette boîte de dialogue.|  
|**Columns**|Répertorie les noms de colonne pour chaque colonne dans la table sélectionnée. L’ordre des colonnes reflète l’ordre des colonnes dans la table. Cette liste est activée si un fichier a été sélectionné dans le **Tables** liste.|  
|**Type de données**|Peut être BIT, BYTE, CHAR, devise, DATE, FLOAT, entier, LONGCHAR, SHORT ou unique. Types de données date peuvent être dans les formats suivants : « jj-mmm-aa », « mm-dd-yy », « mmm jj-aa », « aaaa-mm-jj » ou « aaaa-mmm-jj ». « mm » indique les numéros de mois ; « mmm » désigne des lettres pour les mois.|  
|**Délimiteur**|Spécifie le caractère délimiteur personnalisé à utiliser pour séparer les colonnes. Lorsque le **personnalisé délimitée par des** format est sélectionné. Le délimiteur peut être qu’un seul caractère, et guillemets doubles (") ne peut pas servir en tant que le caractère délimiteur. (Le délimiteur ne peut pas être spécifié au format hexadécimal ou décimal).|  
|**Format**|Soit délimitée ou à longueur fixe. S’ils sont délimités, indique le type de délimiteur utilisé : virgule (CSV), onglet ou un caractère spécial (personnalisé). Par défaut est **CSV délimité** si le format de l’élément sélectionné dans le **Tables** liste n’a pas été précédemment définie par cette boîte de dialogue.<br /><br /> Si **Format** est à longueur fixe et **en-tête de colonne nom** est TRUE, la première ligne doit être délimitée par des virgules.|  
|**Estimation**|Génère automatiquement des valeurs de largeur, nom et type de données pour les colonnes de la colonne dans la table sélectionnée en analysant le contenu de la table en fonction de la **Format** zone de sélection. Activé quand le format de table est délimité. Des définis précédemment des colonnes dans le **colonnes** liste sont effacées et remplacées par nouvelles entrées. Si **en-tête de colonne nom** est ne pas sélectionnée, les noms de colonne sont générés automatiquement en tant que « F1 », « F2 » et ainsi de suite. Aucune valeur par défaut n’est affichée dans le **Type de données** boîte.<br /><br /> Cette fonctionnalité fonctionne uniquement sur les colonnes qui sont inférieures à 64 513 octets.|  
|**Modifier**|Modifie la colonne sélectionnée en utilisant les valeurs **Type de données**, **nom**, et **largeur**.|  
|**Nom**|Affiche le nom de la colonne sélectionnée. Peut être utilisé pour spécifier un nouveau nom de colonne pour une colonne existante ou d’une nouvelle colonne.<br /><br /> Si **en-tête de colonne nom** est TRUE, la colonne nom affiché est ignoré.|  
|**Supprimer**|Supprime la colonne sélectionnée.|  
|**Lignes à analyser**|Le nombre de lignes que le programme d’installation ou le pilote analysera lors de la définition des colonnes et types de données de colonne en fonction des données existantes.<br /><br /> Vous pouvez entrer un nombre compris entre 1 et 32 767 pour le nombre de lignes à analyser. Par défaut, cette fonction 25 si le format de l’élément sélectionné dans le **Tables** liste n’a pas été précédemment définie par cette boîte de dialogue. (Un nombre en dehors de la limite retournera une erreur).|  
|**Tables**|Contient une liste de tous les fichiers dans le répertoire sélectionné dans le **le programme d’installation de texte** boîte de dialogue qui correspondent à la liste des extensions spécifiées.<br /><br /> Lorsque \<par défaut > est sélectionnée, et une des conditions suivantes est true, les valeurs des attributs de table dans le **Tables** groupe sont écrits dans Schema.ini (sans les autres entrées dans Schema.ini sont couvertes) :<br /><br /> -Il n’existe aucun Schema.ini dans le répertoire spécifié.<br />-Le fichier Schema.ini existe, mais il n’existe pas dans Schema.ini pour un des fichiers texte (avec l’extension spécifiée) dans le répertoire.<br />-La section d’un fichier texte existe dans Schema.ini, mais le corps est vide.<br /><br /> Lorsque \<par défaut > est sélectionnée, le **colonnes** groupe est désactivé.|  
|**Largeur**|La largeur de la colonne peut être modifiée pour les colonnes CHAR ou LONGCHAR. La largeur par défaut est 1, si le format de l’élément sélectionné dans le **Tables** liste n’a pas été précédemment définie par cette boîte de dialogue.<br /><br /> Pour les autres types de données, la largeur du contrôle est désactivé et aucune valeur affichée.|
