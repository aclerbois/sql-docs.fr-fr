---
title: Nom de classement Windows (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- Windows collations [SQL Server]
- names [SQL Server], collations
- collations [SQL Server], Windows collations
- Collation Designator
ms.assetid: acceef84-2c68-46e2-a021-be019b7ab14e
caps.latest.revision: 43
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: de2d55bbe2dafaa02886a1e0deeb675a43469e60
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="windows-collation-name-transact-sql"></a>Nom de classement Windows (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Spécifie le nom de classement Windows dans la clause COLLATE dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le nom de classement Windows est composé d'un indicateur de classement et de styles de comparaison.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
<Windows_collation_name> :: =   
CollationDesignator_<ComparisonStyle>  
  
<ComparisonStyle> :: =   
{ CaseSensitivity_AccentSensitivity  [ _KanatypeSensitive ] [ _WidthSensitive ]    
}  
| { _BIN | _BIN2 }  
```  
  
## <a name="arguments"></a>Arguments  
 *CollationDesignator*  
 Spécifie les règles de classement de base utilisées par le classement Windows. Les règles de classement de base incluent les éléments suivants :  
  
-   les règles de tri appliquées lorsque le tri de dictionnaire est spécifié. Les règles de tri sont basées sur l'alphabet ou la langue ;  
  
-   la page de codes utilisée pour stocker les données caractères non-Unicode.  
  
 Exemples :  
  
-   Latin1_General ou French : ces deux ensembles de caractères s'appuient sur la page de codes 1252.  
  
-   Turkish : utilise la page de code 1254.  
  
 *CaseSensitivity*  
 **CI** ne respecte pas la casse, contrairement à **CS**.  
  
 *AccentSensitivity*  
 **AI** ne respecte pas les accents, contrairement à **AS**.  
  
 *KanatypeSensitive*  
 **Omitted** ne respecte pas les caractères Kana alors que **KS** les respecte.  
  
 *WidthSensitivity*  
 **Omitted** ne tient pas compte des largeurs, contrairement à **WS**.  
  
 **BIN**  
 Indique l'ordre de tri binaire et assurant la compatibilité descendante à utiliser.  
  
 **BIN2**  
 Indique l'ordre de tri binaire utilisant la sémantique de comparaison des points de code.  
  
## <a name="remarks"></a>Notes   
 Selon la version des classements, certains points de code peuvent être non définis. Par exemple, comparez :  
  
```  
SELECT LOWER(nchar(504) COLLATE Latin1_General_CI_AS);   
SELECT LOWER (nchar(504) COLLATE Latin1_General_100_CI_AS);  
GO  
```  
  
 La première ligne retourne un caractère majuscule lorsque le classement est Latin1_General_CI_AS, car ce point de code est non défini dans ce classement.  
  
 Lors de l'utilisation de certaines langues, il peut être essentiel d'éviter les classements anciens. C'est par exemple le cas pour le télougou.  
  
 Dans certains cas, les classements Windows et les classements [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent générer différents plans de requête pour la même requête.  
  
## <a name="examples"></a>Exemples  
 Voici quelques exemples de noms de classements Windows :  
  
-   **Latin1_General_100_**  
  
 Le classement utilise les règles de tri du dictionnaire général Latin1 et établit un mappage à la page de codes 1252. Non-respect de la casse (CI) et respect des accents (AS). Le classement utilise les mappages et les règles de tri du dictionnaire général Latin1 et établit un mappage à la page de codes 1252. Affiche le numéro de version du classement s'il s'agit d'un classement Windows : _90 ou _100. Non-respect de la casse (CI) et respect des accents (AS).  
  
-   **Estonian_CS_AS**  
  
     Ce classement utilise les règles de tri du dictionnaire estonien, page de codes 1257. Respect de la casse et des accents.  
  
-   **Latin1_General_BIN**  
  
     Ce classement utilise la page de codes 1252 et les règles de tri binaire. Les règles de tri du dictionnaire général Latin1 sont ignorées.  
  
## <a name="windows-collations"></a>Classements Windows  
 Pour énumérer les classements Windows pris en charge par votre d'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], exécutez la requête suivante.  
  
```  
SELECT * FROM sys.fn_helpcollations() WHERE name NOT LIKE 'SQL%';  
```  
  
 Le tableau suivant répertorie tous les classements Windows pris en charge dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
|Paramètres régionaux Windows|Classement version 100|Classement version 90|  
|--------------------|---------------------------|--------------------------|  
|Alsacien (France)|Latin1_General_100_|Non disponible|  
|Amharique (Éthiopie)|Latin1_General_100_|Non disponible|  
|Arménien (Arménie)|Cyrillic_General_100_|Non disponible|  
|Assamais (Inde)|Assamese_100_ <sup>1</sup>|Non disponible|  
|Bachkir (Russie)|Bashkir_100_|Non disponible|  
|Basque (Basque)|Latin1_General_100_|Non disponible|  
|Bengali (Bangladesh)|Bengali_100_<sup>1</sup>|Non disponible|  
|Bengali (India)|Bengali_100_<sup>1</sup>|Non disponible|  
|Bosniaque (Bosnie-Herzégovine, cyrillique)|Bosnian_Cyrillic_100_|Non disponible|  
|Bosniaque (Bosnie-Herzégovine, latin)|Bosnian_Latin_100_|Non disponible|  
|Breton (France)|Breton_100_|Non disponible|  
|Chinese (Macao SAR)|Chinese_Traditional_Pinyin_100_|Non disponible|  
|Chinese (Macao SAR)|Chinese_Traditional_Stroke_Order_100_|Non disponible|  
|Chinese (Singapore)|Chinese_Simplified_Stroke_Order_100_|Non disponible|  
|Corse (France)|Corsican_100_|Non disponible|  
|Croate (Bosnie-Herzégovine, latin)|Croatian_100_|Non disponible|  
|Dari (Afghanistan)|Dari_100_|Non disponible|  
|Anglais (Inde)|Latin1_General_100_|Non disponible|  
|Anglais (Malaisie)|Latin1_General_100_|Non disponible|  
|Anglais (Singapour)|Latin1_General_100_|Non disponible|  
|Filipino (Philippines)|Latin1_General_100_|Non disponible|  
|Frison (Pays-Bas)|Frisian_100_|Non disponible|  
|Géorgien (Géorgie)|Cyrillic_General_100_|Non disponible|  
|Groenlandais (Groenland)|Danish_Greenlandic_100_|Non disponible|  
|Goudjrati (Inde)|Indic_General_100_<sup>1</sup>|Indic_General_90_|  
|Haoussa (Nigeria, latin)|Latin1_General_100_|Non disponible|  
|Hindi (Inde)|Indic_General_100_<sup>1</sup>|Indic_General_90_|  
|Igbo (Nigeria)|Latin1_General_100_|Non disponible|  
|Inuktitut (Canada, latin)|Latin1_General_100_|Non disponible|  
|Inuktitut (syllabique, Canada)|Latin1_General_100_|Non disponible|  
|Irlandais (Irlande)|Latin1_General_100_|Non disponible|  
|Japonais (Japon XJIS)|Japanese_XJIS_100_|Japanese_90_, Japanese_|  
|Japonais (Japon)|Japanese_Bushu_Kakusu_100_|Non disponible|  
|Kannada (Inde)|Indic_General_100_<sup>1</sup>|Indic_General_90_|  
|Khmer (Cambodge)|Khmer_100_<sup>1</sup>|Non disponible|  
|Quiché (Guatemala)|Modern_Spanish_100_|Non disponible|  
|Kinyarwanda (Rwanda)|Latin1_General_100_|Non disponible|  
|Konkani (Inde)|Indic_General_100_<sup>1</sup>|Indic_General_90_|  
|Lao (RDP Lao)|Lao_100_<sup>1</sup>|Non disponible|  
|Bas-sorabe (Allemagne)|Latin1_General_100_|Non disponible|  
|Luxembourgeois (Luxembourg)|Latin1_General_100_|Non disponible|  
|Malayalam (Inde)|Indic_General_100_<sup>1</sup>|Non disponible|  
|Maltais (Malte)|Maltese_100_|Non disponible|  
|Maori (Nouvelle-Zélande)|Maori_100_|Non disponible|  
|Mapudungun (Chili)|Mapudungan_100_|Non disponible|  
|Marathe (Inde)|Indic_General_100_<sup>1</sup>|Indic_General_90_|  
|Mohawk (Canada)|Mohawk_100_|Non disponible|  
|Mongol (République populaire de Chine)|Cyrillic_General_100_|Non disponible|  
|Népalais (Népal)|Nepali_100_<sup>1</sup>|Non disponible|  
|Norvégien (bokmål, Norvège)|Norwegian_100_|Non disponible|  
|Norvégien (Nynorsk, Norvège)|Norwegian_100_|Non disponible|  
|Occitan (France)|French_100_|Non disponible|  
|Oriya (Inde)|Indic_General_100_<sup>1</sup>|Non disponible|  
|Pachtou (Afghanistan)|Pashto_100_<sup>1</sup>|Non disponible|  
|Persan (Iran)|Persian_100_|Non disponible|  
|Pendjabi (Inde)|Indic_General_100_<sup>1</sup>|Indic_General_90_|  
|Quechua (Bolivie)|Latin1_General_100_|Non disponible|  
|Quechua (Équateur)|Latin1_General_100_|Non disponible|  
|Quechua (Pérou)|Latin1_General_100_|Non disponible|  
|Romanche (Suisse)|Romansh_100_|Non disponible|  
|Same d'Inari (Finlande)|Sami_Sweden_Finland_100_|Non disponible|  
|Same de Lule (Norvège)|Sami_Norway_100_|Non disponible|  
|Same de Lule (Suède)|Sami_Sweden_Finland_100_|Non disponible|  
|Same du nord (Finlande)|Sami_Sweden_Finland_100_|Non disponible|  
|Same du nord (Norvège)|Sami_Norway_100_|Non disponible|  
|Same du nord (Suède)|Sami_Sweden_Finland_100_|Non disponible|  
|Same de Skolt (Finlande)|Sami_Sweden_Finland_100_|Non disponible|  
|Same du sud (Norvège)|Sami_Norway_100_|Non disponible|  
|Same du sud (Suède)|Sami_Sweden_Finland_100_|Non disponible|  
|Sanskrit (Inde)|Indic_General_100_<sup>1</sup>|Indic_General_90_|  
|Serbe (Bosnie-Herzégovine, cyrillique)|Serbian_Cyrillic_100_|Non disponible|  
|Serbe (Bosnie-Herzégovine, latin)|Serbian_Latin_100_|Non disponible|  
|Serbe (Serbie, cyrillique)|Serbian_Cyrillic_100_|Non disponible|  
|Serbe (latin, Serbie)|Serbian_Latin_100_|Non disponible|  
|Sesotho sa Leboa/Sotho du Nord (Afrique du Sud)|Latin1_General_100_|Non disponible|  
|Setswana/Tswana (Afrique du Sud)|Latin1_General_100_|Non disponible|  
|Cinghalais (Sri Lanka)|Indic_General_100_<sup>1</sup>|Non disponible|  
|Swahili (Kenya)|Latin1_General_100_|Non disponible|  
|Syriaque (Syrie)|Syriac_100_<sup>1</sup>|Syriac_90_|  
|Tadjik (Tadjikistan)|Cyrillic_General_100_|Non disponible|  
|Tamazight (Algérie, latin)|Tamazight_100_|Non disponible|  
|Tamoul (Inde)|Indic_General_100_<sup>1</sup>|Indic_General_90_|  
|Télougou (Inde)|Indic_General_100_<sup>1</sup>|Indic_General_90_|  
|Tibétain (RPC)|Tibetan_100_<sup>1</sup>|Non disponible|  
|Turkmène (Turkménistan)|Turkmen_100_|Non disponible|  
|Ouïgour (RPC)|Uighur_100_|Non disponible|  
|Haut-sorabe (Allemagne)|Upper_Sorbian_100_|Non disponible|  
|Ourdou (Pakistan)|Urdu_100_|Non disponible|  
|Gallois (Royaume-Uni)|Welsh_100_|Non disponible|  
|Wolof (Sénégal)|French_100_|Non disponible|  
|Xhosa (Afrique du Sud)|Latin1_General_100_|Non disponible|  
|Iakoute (Russie)|Yakut_100_|Non disponible|  
|Yi (RPC)|Latin1_General_100_|Non disponible|  
|Yorouba (Nigeria)|Latin1_General_100_|Non disponible|  
|Zoulou (Afrique du Sud)|Latin1_General_100_|Non disponible|  
|Déconseillé, non disponible au niveau serveur dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] ou les versions ultérieures|Hindi|Hindi|  
|Déconseillé, non disponible au niveau serveur dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] ou les versions ultérieures|Korean_Wansung_Unicode|Korean_Wansung_Unicode|  
|Déconseillé, non disponible au niveau serveur dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] ou les versions ultérieures|Lithuanian_Classic|Lithuanian_Classic|  
|Déconseillé, non disponible au niveau serveur dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] ou les versions ultérieures|Macedonian|Macedonian|  
  
 <sup>1</sup> Les classements Windows Unicode seulement ne peuvent être appliqués qu’à des données de niveau colonne ou de niveau expression. Ils ne peuvent pas être utilisés en tant que classements de serveur ou de base de données.  
  
 <sup>2</sup>Comme le classement chinois (Taiwan), le chinois (Macao (R.A.S.)) utilise les règles du chinois simplifié ; contrairement au chinois (Taiwan), il utilise la page de codes 950.  
  
## <a name="see-also"></a> Voir aussi  
 [Prise en charge d'Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md)   
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [Constantes &#40;Transact-SQL&#41;](../../t-sql/data-types/constants-transact-sql.md)   
 [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-sql-server-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [table &#40;Transact-SQL&#41;](../../t-sql/data-types/table-transact-sql.md)   
 [sys.fn_helpcollations &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md)  
  
  
