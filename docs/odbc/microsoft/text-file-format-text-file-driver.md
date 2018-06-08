---
title: Format de fichier texte (pilote du fichier texte) | Documents Microsoft
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
- delimited text lines
- fixed-width text files
- text format [ODBC]
- text file driver [ODBC], text format
ms.assetid: f53cd4b5-0721-4562-a90f-4c55e6030cb9
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d2f0de1d7b5ca14c5ae51cd057244d0c3252780a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="text-file-format-text-file-driver"></a>Format de fichier texte (pilote du fichier texte)
Le pilote ODBC texte prend en charge les deux fichiers texte délimités et à largeur fixe. Un fichier texte se compose d’une ligne d’en-tête facultatif et zéro ou plusieurs lignes de texte.  
  
 Bien que la ligne d’en-tête utilise le même format que les autres lignes dans le fichier texte, le pilote ODBC texte interprète les entrées de ligne d’en-tête en tant que noms de colonne, pas les données.  
  
 Une ligne de texte délimité contient une ou plusieurs valeurs de données séparées par des délimiteurs : un délimiteur personnalisé, des tabulations ou des virgules. Le même séparateur doit être utilisé dans le fichier. Les valeurs de données de type NULL sont signalées par les deux séparateurs dans une ligne sans données entre eux. Chaînes de caractères dans une ligne de texte délimité peuvent être placés entre guillemets doubles ( » »). Sans vides peuvent se produire avant ou après les valeurs délimitées.  
  
 La largeur de chaque entrée de données dans une ligne de texte de largeur fixe est spécifiée dans un schéma. Les valeurs de données de type NULL sont signalées par des espaces.  
  
 Les tables sont limitées à un maximum de 255 champs. Les noms de champ sont limités à 64 caractères, et les largeurs de champs sont limités à 32 766 caractères. Les enregistrements sont limités à 65 000 octets.  
  
 Un fichier texte peut être ouvert uniquement pour un seul utilisateur. Plusieurs utilisateurs ne sont pas pris en charge.  
  
 La grammaire suivante, écrite pour les programmeurs, définit le format d’un fichier texte qui peut être lu par le pilote de texte ODBC :  
  
|Format|Représentation|  
|------------|--------------------|  
|Non-italique|Caractères qui doivent être écrits comme indiqué|  
|*Italique*|Arguments sont définis ailleurs dans la grammaire|  
|crochets ([])|Éléments facultatifs|  
|accolades ({})|Une liste de choix s’excluent mutuellement|  
|barres verticales (&#124;)|Options mutuellement exclusives distinctes|  
|points de suspension (...)|Éléments qui peuvent être répétés une ou plusieurs fois|  
  
 Le format d’un fichier texte est :  
  
```  
text-file ::=  
   [delimited-header-line] [delimited-text-line]... end-of-file |  
   [fixed-width-header-line] [fixed-width-text-line]... end-of-file  
delimited-header-line ::= delimited-text-line  
delimited-text-line ::=  
   blank-line |  
   delimited-data [delimiter delimited-data]... end-of-line  
fixed-width-header-line ::= fixed-width-text-line  
fixed-width-text-line ::=  
   blank-line |  
   fixed-width-data [fixed-width-data]... end-of-line  
end-of-file ::= <EOF>  
blank-line ::= end-of-line  
delimited-data ::= delimited-string | number | date | delimited-null  
fixed-width-data ::= fixed-width-string | number | date | fixed-width-null  
```  
  
> [!NOTE]  
>  La largeur de chaque colonne dans un fichier texte de largeur fixe est spécifiée dans le fichier Schema.ini.  
  
```  
  
      end-of-line ::= <CR> | <LF> | <CR><LF>  
delimited-string ::= unquoted-string | quoted-stringunquoted-string ::= [character | digit] [character | digit | quote-character]...  
quoted-string ::=  
   quote-character  
   [character | digit | delimiter | end-of-line | embedded-quoted-string]...  
   quote-characterembedded-quoted-string ::=   quote-characterquote-character  
   [character | digit | delimiter | end-of-line]  
   quote-characterquote-characterfixed-width-string ::= [character | digit | delimiter | quote-character] ...  
character ::= any character except:  
   delimiterdigitend-of-fileend-of-linequote-characterdigit ::= 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9  
delimiter ::= , | <TAB> |   
custom-delimitercustom-delimiter ::= any character except:  
   end-of-fileend-of-linequote-character  
```  
  
> [!NOTE]  
>  Le délimiteur dans un fichier texte délimité personnalisé est spécifié dans le fichier Schema.ini.  
  
```  
quote-character ::= "  
number ::= exact-number | approximate-number  
exact-number ::= [+ | -] {unsigned-integer[.unsigned-integer] |  
   unsigned-integer. |  
   .unsigned-integer}  
approximate-number ::= exact-number{e | E}[+ | -]unsigned-integer  
unsigned-integer ::= {digit}...  
date ::=  
   mm date-separator dd date-separator yy |  
   mmm date-separator dd date-separator yy |  
   dd date-separator mmm date-separator yy |  
   yyyy date-separator mm date-separator dd |  
   yyyy date-separator mmm date-separator dd  
mm ::= digit [digit]  
dd ::= digit [digit]  
yy ::= digit digit  
yyyy ::= digit digit digit digit  
mmm ::= Jan | Feb | Mar | Apr | May | Jun | Jul | Aug | Sep | Oct | Nov | Dec  
date-separator ::= - | / | .  
delimited-null ::=  
```  
  
> [!NOTE]  
>  Pour les fichiers délimités, une valeur NULL est représentée par aucune des données entre les deux séparateurs.  
  
```  
fixed-width-null ::= <SPACE>...  
```  
  
> [!NOTE]  
>  Pour les fichiers à largeur fixe, une valeur NULL est représentée par des espaces.
