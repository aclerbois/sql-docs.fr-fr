---
title: Retour des paramètres de tableau à partir de procédures stockées | Documents Microsoft
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
- stored procedures [ODBC], ODBC driver for Oracle
- ODBC driver for Oracle [ODBC], stored procedures
ms.assetid: 2018069b-da5d-4cee-a971-991897d4f7b5
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 6f67a9044dcd95b2b652c310e066843b7aa5d1ab
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="returning-array-parameters-from-stored-procedures"></a>Retourner les paramètres de tableau à partir de procédures stockées
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Au lieu de cela, utilisez le pilote ODBC fourni par Oracle.  
  
 Dans Oracle 7.3, il n’existe aucun moyen pour accéder à un Type d’enregistrement PL/SQL, à l’exception à partir d’un programme PL/SQL. Si une fonction ou procédure empaquetée possède un argument formel est défini comme un Type d’enregistrement PL/SQL, il n’est pas possible de lier cet argument formel en tant que paramètre. Utilisez le type de TABLE de PL/SQL dans le pilote Microsoft ODBC pour Oracle pour appeler des paramètres de tableau à partir de procédures contenant les séquences d’échappement correcte.  
  
 Pour appeler la procédure, utilisez la syntaxe suivante :  
  
```  
{call  <package-name>.<proc-or-func>;  
(..., {resultset <max-records-requested> ,<formal-array-param_1>,;  
 <formal-array-param_2>,...,<formal-array-param_n> }, ... ) }  
```  
  
> [!NOTE]  
>  Le \<a demandé des enregistrements max > paramètre doit être supérieur ou égal au nombre de lignes présentes dans le jeu de résultats. Sinon, Oracle retourne une erreur qui est passée à l’utilisateur par le pilote.  
>   
>  Les enregistrements PL/SQL ne peut pas être utilisés en tant que paramètres de tableau. Chaque paramètre de tableau peut représenter qu’une seule colonne d’une table de base de données.  
  
 L’exemple suivant définit un package contenant deux procédures qui retournent des jeux de résultats, puis fournit deux façons de retourner des jeux de résultats à partir du package.  
  
## <a name="package-definition"></a>Définition du package :  
  
```  
CREATE OR REPLACE PACKAGE SimplePackage AS  
  
TYPE t_id is TABLE of  NUMBER(5)  
    INDEX BY BINARY_INTEGER;  
  
TYPE t_Course is TABLE of VARCHAR2(10)  
    INDEX BY BINARY_INTEGER;  
  
TYPE t_Dept is TABLE of VARCHAR2(5)  
    INDEX BY BINARY_INTEGER;  
  
PROCEDURE proc1  
   (  
   o_id             OUT    t_id,  
   ao_course       OUT    t_Course,  
   ao_dept         OUT    t_Dept  
   );  
  
TYPE t_pk1Type1 IS TABLE OF VARCHAR2(100) INDEX BY BINARY_INTEGER;  
TYPE t_pk1Type2 IS TABLE OF NUMBER INDEX BY BINARY_INTEGER;  
PROCEDURE proc2  
   (  
   i_Arg1         IN    NUMBER,  
   ao_Arg2         OUT   t_pk1Type1,  
   ao_Arg3         OUT   t_pk1Type2  
   );  
  
END SimplePackage;  
  
CREATE OR REPLACE PACKAGE BODY SimplePackage AS  
    PROCEDURE  proc1 ( o_id OUT t_id,  
    ao_course OUT t_Course, ao_dept OUT t_Dept   ) AS  
    BEGIN  
          o_id(1):= 200;  
          ao_course(1) :=  'M101';  
          ao_dept(1) :=  'EEE' ;  
  
          o_id(2) := 201;  
          ao_course(2) :=  'PHY320';  
          ao_dept(2) :=  'ECE' ;  
  
     END proc1;  
PROCEDURE proc2  
   (  
   i_Arg1         IN    NUMBER,  
   ao_Arg2         OUT   t_pk1Type1,  
   ao_Arg3         OUT   t_pk1Type2  
   )  
AS  
   i   NUMBER;  
BEGIN  
   FOR i IN 1 .. i_Arg1 LOOP  
      ao_Arg2(i) := 'Row Number ' || to_char(i);  
   END LOOP;  
   FOR i IN 1 .. i_Arg1 LOOP  
      ao_Arg3(i) := i;  
   END LOOP;  
END proc2;  
END SimplePackage;  
```  
  
#### <a name="to-invoke-procedure-proc1"></a>Pour appeler la procédure PROC1  
  
1.  Renvoyer toutes les colonnes dans un seul jeu de résultats :  
  
    ```  
    {call SimplePackage.Proc1( {resultset  3, o_id , ao_course, ao_dept  } ) }  
    ```  
  
2.  Chaque colonne de retour comme un seul jeu de résultats :  
  
    ```  
    {call SimplePackage.Proc1( {resultset 3, o_id},  {resultset 3, ao_course}, {resultset 3, ao_dept} ) }  
    ```  
  
     Cela retourne trois jeux de résultats, un pour chaque colonne.  
  
#### <a name="to-invoke-procedure-proc2"></a>Pour appeler la procédure PROC2  
  
1.  Renvoyer toutes les colonnes dans un seul jeu de résultats :  
  
    ```  
    {call SimplePackage.Proc2( 5 , {resultset  5, ao_Arg2, ao_Arg3} ) }  
    ```  
  
2.  Chaque colonne de retour comme un seul jeu de résultats :  
  
    ```  
    {call SimplePackage.Proc2( 5 , {resultset 5, ao_Arg2}, {resultset 5, ao_Arg3} ) }  
    ```  
  
 Assurez-vous que vos applications extraire tous les jeux de résultats à l’aide de la [SQLMoreResults](../../odbc/microsoft/level-2-api-functions-odbc-driver-for-oracle.md) API. Pour plus d’informations, reportez-vous à la *de référence du programmeur ODBC*.  
  
> [!NOTE]  
>  Dans le pilote ODBC pour Oracle version 2.0, les fonctions Oracle qui retournent des tableaux de PL/SQL ne peut pas être utilisées pour renvoyer des jeux de résultats.
