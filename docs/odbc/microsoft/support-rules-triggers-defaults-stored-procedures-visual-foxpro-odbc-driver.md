---
title: Prise en charge pour les règles, les déclencheurs, les valeurs par défaut et les procédures stockées | Documents Microsoft
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
- Visual FoxPro ODBC driver [ODBC], stored procedures
- FoxPro ODBC driver [ODBC], commands and functions
- commands for FoxPro ODBC driver [ODBC]
- FoxPro ODBC driver [ODBC], default values
- FoxPro ODBC driver [ODBC], triggers
- stored procedures [ODBC], Visual FoxPro ODBC driver
- Visual FoxPro ODBC driver [ODBC], rules
- FoxPro commands and functions [ODBC]
- rules [ODBC]
- Visual FoxPro ODBC driver [ODBC], default values
- FoxPro ODBC driver [ODBC], rules
- functions [ODBC], Visual FoxPro
- default values [ODBC]
- Visual FoxPro ODBC driver [ODBC], commands and functions
- Visual FoxPro ODBC driver [ODBC], triggers
- FoxPro ODBC driver [ODBC], stored procedures
- Visual FoxPro commands and functions [ODBC]
ms.assetid: e449de20-d6ca-4902-9f8e-814eb6e86650
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ba9045b69420b70328071cbf3859ab29676260b9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="support-for-rules-triggers-default-values-and-stored-procedures-visual-foxpro-odbc-driver"></a>Prise en charge pour les règles, les déclencheurs, les valeurs par défaut et les procédures stockées (le pilote ODBC Visual FoxPro)
Impossible de créer des règles Visual FoxPro, des déclencheurs, des valeurs par défaut ou des procédures stockées à l’aide du pilote ODBC Visual FoxPro. Toutefois, votre application peut interagir avec des procédures stockées, des valeurs par défaut, des déclencheurs ou des règles existantes comme elle insère, met à jour ou supprime des données Visual FoxPro stockées dans une base de données.  
  
 Le tableau suivant répertorie les commandes de Visual FoxPro et les fonctions prises en charge par le pilote ODBC Visual FoxPro lorsque les fonctions ou les commandes existent dans les règles, des déclencheurs, des valeurs par défaut ou des procédures stockées.  
  
 Si votre application interagit avec les données dont les règles, les déclencheurs, les valeurs par défaut, ou d’appellent de procédures stockées toutes les autres commandes de Visual FoxPro ou fonctions, le pilote génère une erreur. Consultez [non pris en charge les commandes FoxPro Visual et fonctions](../../odbc/microsoft/unsupported-visual-foxpro-commands-and-functions-visual-foxpro-odbc-driver.md) pour obtenir la liste des commandes et des fonctions non prises en charge par le pilote.  
  
> [!TIP]  
>  Si vous souhaitez insérer du code conditionnelle dans les règles, des déclencheurs ou des procédures stockées qui détermine les commandes à exécuter lorsqu’elle est appelée par le pilote, vous pouvez utiliser la **(de) VERSION** (fonction). Le **(de) VERSION** fonction retourne « pilote ODBC Visual FoxPro  *\<version >*» lorsqu’elle est appelée par le pilote.  
  
## <a name="visual-foxpro-commands-and-functions-supported-in-rules-triggers-default-values-and-stored-procedures"></a>Les commandes de Visual FoxPro et de fonctions pris en charge dans les règles, les déclencheurs, les valeurs par défaut et les procédures stockées  
  
||||  
|-|-|-|  
|Opérateur de $|Opérateur %|& Commande|  
|& & Commande|* Commande|= Commande|  
  
## <a name="a"></a>Un  
  
||||  
|-|-|-|  
|ABS (fonction))|ACOPY (fonction))|AJOUTER des commandes de la TABLE|  
|ADATABASES (fonction))|ADBOBJECTS (fonction))|À AERROR (fonction))|  
|ADEL (fonction))|AELEMENT (fonction))|ALEN (fonction))|  
|AFIELDS (fonction))|AINS (fonction))|ALTER TABLE - commande SQL|  
|ALIAS (fonction))|ALLTRIM (fonction))|AJOUTER à partir de la commande de tableau|  
|AND (opérateur)|AJOUTER des commandes|AJOUTER la commande de facture|  
|AJOUTER à partir de la commande|AJOUTER les commandes général|ASCAN (fonction))|  
|AJOUTER des commandes de procédures|ASC (fonction))|ASUBSCRIPT (fonction))|  
|ASIN (fonction))|ASORT (fonction))|ATAN (fonction))|  
|À la fonction de)|AT_C (fonction))|ATCLINE (fonction))|  
|ATC (fonction))|ATCC (fonction))|AUSED (fonction))|  
|ATLINE (fonction))|ATN2 (fonction))||  
|Commande moyenne|ACOS (fonction))||  
  
## <a name="b"></a>B  
  
||||  
|-|-|-|  
|Commande BEGIN TRANSACTION|ENTRE la fonction)|BITNOT (fonction))|  
|BITCLEAR (fonction))|BITLSHIFT (fonction))|BITSET (fonction))|  
|BITOR (fonction))|BITRSHIFT (fonction))|Commande vide|  
|BITTEST (fonction))|BITXOR (fonction))||  
|BOF (fonction))|BITAND (fonction))||  
  
## <a name="c"></a>C  
  
||||  
|-|-|-|  
|CALCULER la commande|CANDIDAT (fonction))|CHR (fonction))|  
|CDX (fonction))|CEILING, fonction)|Commandes CLOSE|  
|CHRTRAN (fonction))|CHRTRANC (fonction))|Commande de copie des index|  
|CMONTH (fonction))|Commande CONTINUE|Commande de copie STRUCTURE étendue|  
|COPIER la commande de procédures|COPIER la commande STRUCTURE|Copier vers la commande|  
|COPIER la commande de balise|Copier vers la commande de tableau|CPCONVERT (fonction))|  
|COS () de fonction|NOMBRE de commandes|CTOD (fonction))|  
|CPCURRENT (fonction))|CPDBF (fonction))|CURSORSETPROP (fonction))|  
|CTOT (fonction))|CURSORGETPROP (fonction))||  
|Fonction CURVAL)|CDOW (fonction))||  
  
## <a name="d"></a>D  
  
||||  
|-|-|-|  
|DATE (fonction))|DATETIME (fonction))|DAY (fonction))|  
|Fonction DBC)|DBF (fonction))|DBGETPROP (fonction))|  
|DBUSED (fonction))|Supprimer : la commande SQL|Commande DELETE|  
|SUPPRIMER des commandes de balise|Fonction supprimée)|DÉCROISSANT (fonction))|  
|DIFFÉRENCE (fonction))|Commande de la DIMENSION|Espace disque (fonction))|  
|DMY (fonction))|EFFECTUEZ LES CAS EN COURS... Commande ENDCASE|DO commande|  
|DO WHILE... Commande ENDDO|VALI (fonction))|DTOC (fonction))|  
|DTOR (fonction))|DTO (fonction))|DTOT (fonction))|  
  
## <a name="e"></a>E  
  
||||  
|-|-|-|  
|Fonction vide)|ÉVALUER la fonction)|EXIT (commande)|  
|ERREUR (fonction))|EXP (fonction))||  
|Commande END TRANSACTION|EOF (fonction))||  
  
## <a name="f"></a>F  
  
||||  
|-|-|-|  
|FCOUNT (fonction))|FDATE (fonction))|CHAMP (fonction))|  
|FICHIER (fonction))|Fonction de filtre)|FLDLIST (fonction))|  
|GÉNÉALOGIQUE (fonction))|FLOOR (fonction))|Commande FLUSH|  
|FOR... Commande ENDFOR|POUR la fonction)|TROUVER la fonction)|  
|Commande TABLE libre|FSIZE (fonction))|FTIME (fonction))|  
|FULLPATH (fonction))|Commande de fonction|VC (fonction))|  
  
## <a name="g"></a>G  
  
||||  
|-|-|-|  
|Recueillir des commandes|GETNEXTMODIFIED (fonction))|Commande GO/GOTO|  
|GETFLDSTATE (fonction))|GOMONTH (fonction))||  
|GETCP (fonction))|GETENV (fonction))||  
  
## <a name="h"></a>H  
  
|||  
|-|-|  
|EN-tête (fonction))|HEURE (fonction))|  
  
## <a name="i"></a>I  
  
||||  
|-|-|-|  
|IDXCOLLATE (fonction))|IF... ENDIF commande|IIF (fonction))|  
|INDBC (fonction))|Commande INDEX|LISTE (fonction))|  
|Commande INSERT-SQL|INT (fonction))|ISALPHA (fonction))|  
|ESTVIDE (fonction))|ISDIGIT (fonction))|ISEXCLUSIVE (fonction))|  
|ISLEADBYTE (fonction))|ISLOWER (fonction))|ISNULL (fonction))|  
|ISREADONLY (fonction))|ISUPPER (fonction))||  
  
## <a name="k"></a>K  
  
||||  
|-|-|-|  
|Fonction de la clé)|KEYMATCH (fonction))||  
  
## <a name="l"></a>L  
  
||||  
|-|-|-|  
|Fonction gauche)|LEFTC (fonction))|LIKEC (fonction))|  
|LENC (fonction))|COMME la fonction)|LOCK (fonction))|  
|Commande locale|RECHERCHEZ la commande|Fonction de recherche)|  
|LOG (fonction))|LOG10 (fonction))|LTRIM (fonction))|  
|LOWER, fonction)|Commande LPARAMETERS||  
|LUPDATE (fonction))|LEN (fonction))||  
  
## <a name="m"></a>M  
  
||||  
|-|-|-|  
|Variable de mémoire système _MLINE|MAX (fonction))|Fonction MDX)|  
|MDY (fonction))|MEMLINES (fonction))|MESSAGE (fonction))|  
|MIN (fonction))|Fonction MINUTE)|MLINE (fonction))|  
|Fonction MOD)|MOIS (fonction))|MTON (fonction))|  
  
## <a name="n"></a>N  
  
||||  
|-|-|-|  
|NDX (fonction))|NORMALISER la fonction)|NOT (opérateur)|  
|Commande de Remarque|NTOM (fonction))|NVL (fonction))|  
  
## <a name="o"></a>O  
  
||||  
|-|-|-|  
|Se produit (fonction))|OLDVAL (fonction))|SUR la commande de l’erreur|  
|SUR la combinaison de touches|SUR la fonction)|Commande de base de données ouverte|  
|OR (opérateur)|Fonction d’ordre)|Système d’exploitation (fonction))|  
  
## <a name="p"></a>P  
  
||||  
|-|-|-|  
|Commande PACK|PARAMÈTRES (fonction))|Fonction de paiement)|  
|Commande de paramètres|Fonction principale)|Commande de privé|  
|Fonction de PI)|PROGRAMME (fonction))|Fonction appropriée)|  
|PROCÉDURE, commande|Fonction va)||  
|Commande PUBLIC|(De) PADL &#124; () PADR &#124; les fonctions PADC)||  
  
## <a name="r"></a>R  
  
||||  
|-|-|-|  
|Fonction RAND)|RAT (fonction))|RATC (fonction))|  
|RATLINE (fonction))|RAPPEL de commande|RECCOUNT (fonction))|  
|RECNO (fonction))|RECSIZE (fonction))|Commande régional|  
|RELATION (fonction))|SUPPRIMER la commande de la TABLE|Remplacer (commande)|  
|REMPLACER à partir de la commande de tableau|RÉPLIQUER (fonction))|RÉESSAYEZ la commande|  
|Commande de retour|Fonction RIGHT)|RIGHTC (fonction))|  
|RLOCK (fonction))|Commande de restauration|Fonction ROUND)|  
|RTOD (fonction))|RTRIM (fonction))||  
  
## <a name="s"></a>S  
  
||||  
|-|-|-|  
|ANALYSE EN COURS... Commande ENDSCAN|Commande à nuages de points|S (fonction))|  
|SECONDES (fonction))|Commande de recherche|SEEK (fonction))|  
|Sélectionnez la commande|Fonction de sélection)|Commande SELECT-SQL|  
|Commande de taille de bloc SET|Commande CARRY SET|Commande SET siècle|  
|Commande SET COLLATE|Commande de base de données de jeu|Commande DATE SET|  
|Commande par défaut de SET|Commande SET DELETED|Commande exacte de jeu|  
|Commande exclusif SET|Commande SET contient|Commande de jeu de champs|  
|Commande de filtre de jeu|Commande fixe SET|Commande SET FULLPATH|  
|Commande SET FWEEK|Commande des heures définies|Commande de jeu d’INDEX|  
|Commande LOCK de jeu|Commande SET MULTILOCKS|DÉFINIR le côté de la commande|  
|Commande SET NOCPTRANS|Commande de notification de jeu|Commande NULL SET|  
|Commande d’optimisation de jeu|Commande de commande SET|Commande de chemin d’accès de SET|  
|PROCÉDURE de jeu, commande|Commande RELATION SET|RELATION de l’ensemble de la commande de désactivation|  
|Commande de RETRAITER SET|DÉFINIR la commande Ignorer|Commande SET UDFPARMS|  
|Commande UNIQUE SET|Commande VOLUME SET|SET (fonction))|  
|SETFLDSTATE (fonction))|SIGN (fonction))|SIN (fonction))|  
|Commande Ignorer|Commande de tri|SPACE (fonction))|  
|SQRT (fonction))|Commande de stockage|STR (fonction))|  
|STRCONV (fonction))|STRTRAN (fonction))|STUFF (fonction))|  
|STUFFC (fonction))|SUBSTR (fonction))|SUBSTRC (fonction))|  
|Commande somme|SYS(2011) (fonction)||  
  
## <a name="t"></a>T  
  
||||  
|-|-|-|  
|Variable de mémoire système _TALLY|Variable de mémoire système _TRIGGERLEVEL|TAGCOUNT (fonction))|  
|TABLEUPDATE (fonction))|BALISE (fonction))|CIBLE (fonction))|  
|TAGNO (fonction))|Fonction TAN)|TRIM (fonction))|  
|Fonction de date et heure)|Commande TOTAL|TXNLEVEL (fonction))|  
|TTOC (fonction))|TTOD (fonction))||  
|Fonction TYPE)|TABLEREVERT (fonction))||  
  
## <a name="u"></a>U  
  
||||  
|-|-|-|  
|Fonction UNIQUE)|Commande UNLOCK|Utilisez la commande|  
|Commande de mise à jour|Fonction supérieure)||  
|UTILISER la fonction)|Commande de mise à jour - SQL||  
  
## <a name="v"></a>V  
  
||||  
|-|-|-|  
|VAL (fonction))|VERSION (fonction))||  
  
## <a name="w"></a>W  
  
||||  
|-|-|-|  
|WEEK (fonction))|||  
  
## <a name="y"></a>O  
  
||||  
|-|-|-|  
|ANNÉE (fonction))|||  
  
## <a name="z"></a>Z  
  
||||  
|-|-|-|  
|Commande ZAP|||
