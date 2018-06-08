---
title: ALTER TABLE - commande SQL | Documents Microsoft
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
- alter table [ODBC]
ms.assetid: 3a01a291-f4d9-43bc-a725-5a95546ff364
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 255ce0a4e9e06f2cdd20dd8d1e707b7f7823e6bd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="alter-table---sql-command"></a>ALTER TABLE - commande SQL
Par programme modifie la structure d’une table.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
ALTER TABLE TableName1  
   ADD | ALTER [COLUMN] FieldName1  
      FieldType [(nFieldWidth [, nPrecision])]  
      [NULL | NOT NULL]  
      [CHECK lExpression1 [ERROR cMessageText1]]  
      [DEFAULT eExpression1]  
      [PRIMARY KEY | UNIQUE]  
      [REFERENCES TableName2 [TAG TagName1]]  
      [NOCPTRANS]  
 - Or -  
ALTER TABLE TableName1  
   ALTER [COLUMN] FieldName2  
      [NULL | NOT NULL]  
      [SET DEFAULT eExpression2]  
      [SET CHECK lExpression2 [ERROR cMessageText2]]  
      [DROP DEFAULT]  
      [DROP CHECK]  
 - Or -  
ALTER TABLE TableName1  
   [DROP [COLUMN] FieldName3]  
   [SET CHECK lExpression3 [ERROR cMessageText3]]  
   [DROP CHECK]  
   [ADD PRIMARY KEY eExpression3 TAG TagName2]  
   [DROP PRIMARY KEY]  
   [ADD UNIQUE eExpression4 [TAG TagName3]]  
   [DROP UNIQUE TAG TagName4]  
   [ADD FOREIGN KEY [eExpression5] TAG TagName4  
      REFERENCES TableName2 [TAG TagName5]]  
   [DROP FOREIGN KEY TAG TagName6 [SAVE]]  
   [RENAME COLUMN FieldName4 TO FieldName5]  
   [NOVALIDATE]  
```  
  
## <a name="arguments"></a>Arguments  
 *TableName1*  
 Spécifie le nom de la table dont la structure est modifiée.  
  
 Ajouter [colonne] *Nom_champ1*  
 Spécifie le nom du champ à ajouter.  
  
 ALTER [colonne] *Nom_champ1*  
 Spécifie le nom d’un champ existant à modifier.  
  
 *FieldType* [( *nFieldWidth* [, *nPrecision*]])  
 Spécifie le type de champ, de la largeur de champ et de la précision de champ (nombre de décimales) pour un champ de nouvel ou modifié.  
  
 *FieldType* est une lettre unique qui indique au champ [type de données](../../odbc/microsoft/visual-foxpro-field-data-types.md). Certains types de données de champ requièrent que vous spécifiez *nFieldWidth* ou *nPrecision* ou les deux.  
  
 *nFieldWidth* et *nPrecision* sont ignorés pour D, G, I, L, M, P, T et Y types. Par défaut, *nPrecision* n’est zéro (aucun décimales) si *nPrecision* n’est pas inclus pour les types B, F ou N.  
  
 NULL &#124; NOT NULL  
 Autorise ou empêche des valeurs null dans le champ.  
  
 Si vous omettez la valeur NULL et NOT NULL, la valeur actuelle de SET NULL détermine si les valeurs null sont autorisées dans le champ. Toutefois, si vous omettez la valeur NULL et non NULL et incluez la clé primaire ou une clause UNIQUE, le paramètre actuel de la valeur NULL est ignoré et le champ n’est pas NULL par défaut.  
  
 Vérifiez *lExpression1*  
 Spécifie une règle de validation pour le champ. *lExpression1* doit correspondre à une expression logique et peut être une fonction définie par l’utilisateur ou une procédure stockée. Chaque fois qu’un enregistrement vide est ajouté, la règle de validation est activée. Une erreur est générée si la règle de validation n’autorise pas une valeur de champ dans un enregistrement ajouté.  
  
 ERREUR *cMessageText1*  
 Spécifie le message d’erreur affiché lorsque la règle de validation de champ génère une erreur.  
  
 Par défaut *eExpression1*  
 Spécifie une valeur par défaut pour le champ. Type de données de *eExpression1* doit être le même que le type de données pour le champ.  
  
 PRIMARY KEY  
 Crée une balise de l’index principal. La balise index porte le même nom que le champ.  
  
 UNIQUE  
 Crée une balise d’index candidats portant le même nom que le champ.  
  
> [!NOTE]  
>  Candidat les index (en incluant l’option UNIQUE, fournie pour la compatibilité ANSI dans l’instruction ALTER TABLE ou CREATE TABLE) diffèrent des index créés à l’aide de l’option UNIQUE dans la commande de l’INDEX. Un index créé à l’aide de UNIQUE dans la commande de l’INDEX autorise les clés d’index en double ; index candidats n’autorisent pas les clés d’index en double.  
  
 Les valeurs NULL et les enregistrements en double ne sont pas autorisés dans un champ qui est utilisé pour un index primaire ou candidate.  
  
 Si vous créez un nouveau champ à l’aide d’ajouter une colonne, Visual FoxPro pas génère une erreur si vous créez un index primaire ou candidate pour un champ qui prend en charge les valeurs null. Toutefois, Visual FoxPro génère une erreur si vous essayez d’entrer une valeur null ou en double dans un champ qui est utilisé pour un index primaire ou candidate.  
  
 Si vous modifiez un champ existant et le serveur principal ou expression d’index candidats se compose de champs dans la table, Visual FoxPro vérifie dans les champs pour voir si elles contiennent des valeurs null ou les enregistrements en double. Dans le cas, Visual FoxPro génère une erreur et la table n’est pas modifiée.  
  
 RÉFÉRENCES *TableName2* balise *TagName1*  
 Spécifie la table parente à laquelle une relation permanente est établie. BALISE *TagName1* spécifie la balise d’index de la table parente sur lequel repose la relation. Noms de balise d’index peuvent contenir jusqu'à 10 caractères.  
  
 NOCPTRANS  
 Empêche la traduction à une autre page de codes pour les champs de type caractère et Mémo. Si la table est convertie en une autre page de codes, les champs pour lesquels NOCPTRANS a été spécifié ne sont pas traduits. NOCPTRANS peut être spécifié uniquement pour les champs de type caractère et Mémo.  
  
 L’exemple suivant crée une table intitulée mytable qui contient deux champs de type caractère et les deux champs de type memo. Le deuxième champ de caractère et le deuxième champ de type memo, memo2, char2 incluent NOCPTRANS pour empêcher la traduction.  
  
```  
CREATE TABLE mytable (char1 C(10), char2 C(10) NOCPTRANS,;  
   memo1 M, memo2 M NOCPTRANS)  
```  
  
 ALTER [colonne] *Nom_champ2*  
 Spécifie le nom d’un champ existant à modifier.  
  
 DÉFINIR par défaut *eExpression2*  
 Spécifie une nouvelle valeur par défaut pour un champ existant. Type de données de *eExpression2* doit être le même que le type de données pour le champ.  
  
 VÉRIFICATION du jeu *lExpression2*  
 Spécifie une nouvelle règle de validation pour un champ existant. *lExpression2* doit correspondre à une expression logique et peut être une fonction définie par l’utilisateur ou une procédure stockée.  
  
 ERREUR *cMessageText2*  
 Spécifie le message d’erreur affiché lorsque la règle de validation de champ génère une erreur. Le message s’affiche uniquement lorsque les données sont modifiées dans une fenêtre Parcourir ou modifier.  
  
 DROP DEFAULT  
 Supprime la valeur par défaut pour un champ existant.  
  
 VÉRIFICATION DE LA LISTE  
 Supprime la règle de validation pour un champ existant.  
  
 DROP [colonne] *FieldName3*  
 Spécifie un champ à supprimer de la table. Suppression d’un champ de la table supprime également la valeur de la valeur par défaut et la règle de validation de champ.  
  
 Si les expressions d’index clé ou un déclencheur font référence au champ, les expressions deviennent non valides lorsque le champ est supprimé. Dans ce cas, une erreur n’est pas générée lorsque le champ est supprimé, mais les expressions de clé ou un déclencheur index non valide génère des erreurs au moment de l’exécution.  
  
 VÉRIFICATION du jeu *lExpression3*  
 Spécifie la règle de validation de table. *lExpression3* doit correspondre à une expression logique et peut être une fonction définie par l’utilisateur ou une procédure stockée.  
  
 ERREUR *cMessageText3*  
 Spécifie le message d’erreur affiché lorsque la règle de validation de table génère une erreur. Le message s’affiche uniquement lorsque les données sont modifiées dans une fenêtre Parcourir ou modifier.  
  
 VÉRIFICATION DE LA LISTE  
 Supprime la règle de validation de la table.  
  
 CLÉ primaire d’ajouter *eExpression3*balise *TagName2*  
 Ajoute un index primaire à la table. *eExpression3* Spécifie l’expression de clé de l’index principal, et *TagName2* Spécifie le nom de la balise de l’index principal. Noms de balise d’index peuvent contenir jusqu'à 10 caractères. Si balise *TagName2* est omis et *eExpression3* est un champ unique, la balise de l’index primaire porte le même nom que le champ spécifié dans *eExpression3*.  
  
 SUPPRIMER LA CLÉ PRIMAIRE  
 Supprime l’index primaire et son étiquette de l’index. Une table peut avoir qu’une clé primaire, il n’est pas nécessaire de spécifier le nom de la clé primaire. Suppression de l’index principal supprime également les relations persistantes basées sur la clé primaire.  
  
 Ajouter UNIQUE *eExpression4*[balise *TagName3*]  
 Ajoute un index candidats à la table. *eExpression4* Spécifie l’expression d’index clé candidate, et *TagName3* Spécifie le nom de la balise d’index candidats. Noms de balise d’index peuvent contenir jusqu'à 10 caractères. Si vous omettez la balise *TagName3* et si *eExpression4* est un champ unique, la balise d’index candidat a le même nom que le champ spécifié dans *eExpression4*.  
  
 BALISE UNIQUE de dépôt *TagName4*  
 Supprime l’index de candidat et sa balise de l’index. Comme une table peut avoir plusieurs clés candidates, vous devez spécifier le nom de la balise d’index candidats.  
  
 ADD FOREIGN KEY [ *eExpression5*] balise *TagName4*  
 Ajoute un index (non essentielles) étrangère à la table. *eExpression5* Spécifie l’expression de clé étrangère d’index, et *TagName4* Spécifie le nom de la balise index étranger. Noms de balise d’index peuvent contenir jusqu'à 10 caractères.  
  
 RÉFÉRENCES *TableName2*[balise *TagName5*]  
 Spécifie la table parente à laquelle une relation permanente est établie. Include (balise) *TagName5* pour établir une relation basée sur une balise d’index existante pour la table parente. Noms de balise d’index peuvent contenir jusqu'à 10 caractères. Si vous omettez la balise *TagName5*, la relation est établie à l’aide de la balise d’index primaire de la table parente.  
  
 BALISE de clé étrangère DROP *TagName6*[Enregistrer]  
 Supprime une clé étrangère dont la balise index est *TagName6*. Si vous omettez les enregistrer, la balise de l’index est supprimée de l’index structurel. Inclure les enregistrer pour empêcher la suppression de la balise de l’index de l’index structurel.  
  
 COLONNE de changement de nom *FieldName4*à *FieldName5*  
 Vous permet de modifier le nom d’un champ dans la table. *FieldName4* Spécifie le nom du champ qui est renommé. *FieldName5* Spécifie le nouveau nom du champ.  
  
> [!CAUTION]  
>  Soyez vigilant lorsque vous renommez des champs de la table, car les expressions d’index, les règles de validation de champ et de table, les commandes et les fonctions peuvent référencer les noms de champ d’origine.  
  
 NOVALIDATE  
 Spécifie que Visual FoxPro autorise les modifications à apporter à la structure de la table ; ces modifications peuvent violer l’intégrité des données dans la table. Par défaut, Visual FoxPro empêche l’apport de modifications qui enfreignent l’intégrité des données dans la table de ALTER TABLE. Inclure NOVALIDATE pour remplacer ce comportement par défaut.  
  
## <a name="remarks"></a>Notes  
 ALTER TABLE peut être utilisée pour modifier la structure d’une table qui n’a pas été ajoutée à une base de données. Toutefois, Visual FoxPro génère une erreur si vous incluez par défaut, FOREIGN KEY, PRIMARY KEY, les références, ou de clauses SET lors de la modification d’une table indépendante.  
  
 ALTER TABLE peut reconstruire la table par la création d’un nouvel en-tête de table et ajout d’enregistrements à l’en-tête du tableau. Par exemple, la modification d’un champ de type ou de largeur peut entraîner la table à reconstruire.  
  
 Une fois une table est reconstruite, les règles de validation de champ sont exécutés pour tous les champs dont la largeur ou type sont modifiés. Si vous modifiez le type ou la largeur de n’importe quel champ de la table, la règle de la table est exécutée.  
  
 Si vous modifiez des règles de validation de champ ou une table pour une table comportant des enregistrements, Visual FoxPro teste le nouveau champ ou une table de règles de validation sur les données existantes et émet un avertissement sur la première occurrence d’une règle de validation de champ ou une table ou d’une violation du déclencheur.  
  
 Si la table que vous modifiez est dans une base de données, ALTER TABLE - SQL requiert une utilisation exclusive de la base de données. Pour ouvrir une base de données pour une utilisation exclusive, inclure exclusif d’ouvrir la base de données.  
  
## <a name="see-also"></a>Voir aussi  
 [CRÉER la TABLE - commande SQL](../../odbc/microsoft/create-table-sql-command.md)   
 [INDEX, commande](../../odbc/microsoft/index-command.md)
