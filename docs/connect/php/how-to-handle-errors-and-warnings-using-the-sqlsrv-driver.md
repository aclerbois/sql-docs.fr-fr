---
title: 'Comment : gérer les erreurs et avertissements à l’aide du pilote SQLSRV | Documents Microsoft'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.component: php
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- errors and warnings
ms.assetid: fa231d60-4c06-4137-89e8-097c28638c5d
caps.latest.revision: 18
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 1f16a1b5af939ce7880a3f775631763c75e768b9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="how-to-handle-errors-and-warnings-using-the-sqlsrv-driver"></a>Procédure : gérer les erreurs et avertissements à l’aide du pilote SQLSRV
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Par défaut, le pilote SQLSRV traite les avertissements comme des erreurs ; un appel à une **sqlsrv** fonction qui génère une erreur ou un avertissement retourne **false**. Cette rubrique montre comment désactiver ce comportement par défaut et comment gérer les avertissements séparément des erreurs.  
  
> [!NOTE]  
> Il existe quelques exceptions au comportement par défaut consistant à traiter les avertissements comme des erreurs. Les avertissements qui correspondent aux valeurs SQLSTATE 01000, 01001, 01003 et 01S02 ne sont jamais traités comme des erreurs.  
  
## <a name="example"></a>Exemple  
L’exemple de code suivant utilise deux fonctions définies par l’utilisateur, **DisplayErrors** et **DisplayWarnings**, pour gérer les erreurs et avertissements. L’exemple montre comment gérer séparément les avertissements et les erreurs en procédant comme suit :  
  
1.  Désactive le comportement par défaut consistant à traiter les avertissements comme des erreurs.  
  
2.  Crée une procédure stockée qui met à jour les heures de congé d’un employé et retourne les heures de congé restantes en tant que paramètre de sortie. Quand les heures de congé disponibles d’un employé sont inférieures à zéro, la procédure stockée imprime un avertissement.  
  
3.  Met à jour les heures de congé de plusieurs employés en appelant la procédure stockée pour chacun d’eux et affiche les messages qui correspondent aux avertissements et erreurs qui se produisent.  
  
4.  Affiche les heures de congé restantes de chaque employé.  
  
Dans le premier appel à une **sqlsrv** (fonction) ([sqlsrv_configure](../../connect/php/sqlsrv-configure.md)), les avertissements sont traités comme des erreurs. Étant donné que les avertissements sont ajoutés à la collection d’erreurs, il est inutile de vérifier les avertissements séparément des erreurs. Dans les appels ultérieurs aux fonctions **sqlsrv** , en revanche, les avertissements ne sont pas traités comme des erreurs, vous devez donc vérifier explicitement et les avertissements et les erreurs.  
  
Notez également que l’exemple de code vérifie les erreurs après chaque appel à une fonction **sqlsrv** . Il s’agit de la pratique recommandée.  
  
Cet exemple suppose que SQL Server et le [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) base de données sont installés sur l’ordinateur local. Toute la sortie est écrite dans la console quand l’exemple est exécuté à partir de la ligne de commande. Quand l’exemple est exécuté sur une nouvelle installation de la base de données AdventureWorks, il génère trois avertissements et deux erreurs. Les deux premiers avertissements sont des avertissements standard émis quand vous vous connectez à une base de données. Le troisième avertissement se produit parce que les heures de congé disponibles d’un employé sont mises à jour vers une valeur inférieure à zéro. Les erreurs se produisent parce que les heures de congé disponibles d’un employé sont mises à jour vers une valeur inférieure à -40 heures, ce qui constitue une violation d’une contrainte sur la table.  
  
```  
<?php  
/* Turn off the default behavior of treating errors as warnings.  
Note: Turning off the default behavior is done here for demonstration  
purposes only. If setting the configuration fails, display errors and  
exit the script. */  
if( sqlsrv_configure("WarningsReturnAsErrors", 0) === false)  
{  
     DisplayErrors();  
     die;  
}  
  
/* Connect to the local server using Windows Authentication and   
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$connectionInfo = array("Database"=>"AdventureWorks");  
$conn = sqlsrv_connect( $serverName, $connectionInfo);  
  
/* If the connection fails, display errors and exit the script. */  
if( $conn === false )  
{  
     DisplayErrors();  
     die;  
}  
/* Display any warnings. */  
DisplayWarnings();  
  
/* Drop the stored procedure if it already exists. */  
$tsql1 = "IF OBJECT_ID('SubtractVacationHours', 'P') IS NOT NULL  
                DROP PROCEDURE SubtractVacationHours";  
$stmt1 = sqlsrv_query($conn, $tsql1);  
  
/* If the query fails, display errors and exit the script. */  
if( $stmt1 === false)  
{  
     DisplayErrors();  
     die;  
}  
/* Display any warnings. */  
DisplayWarnings();  
  
/* Free the statement resources. */  
sqlsrv_free_stmt( $stmt1 );  
  
/* Create the stored procedure. */  
$tsql2 = "CREATE PROCEDURE SubtractVacationHours  
                  @EmployeeID int,  
                  @VacationHours smallint OUTPUT  
              AS  
                  UPDATE HumanResources.Employee  
                  SET VacationHours = VacationHours - @VacationHours  
                  WHERE EmployeeID = @EmployeeID;  
                  SET @VacationHours = (SELECT VacationHours    
                                       FROM HumanResources.Employee  
                                       WHERE EmployeeID = @EmployeeID);  
              IF @VacationHours < 0   
              BEGIN  
                PRINT 'WARNING: Vacation hours are now less than zero.'  
              END;";  
$stmt2 = sqlsrv_query( $conn, $tsql2 );  
  
/* If the query fails, display errors and exit the script. */  
if( $stmt2 === false)  
{  
     DisplayErrors();  
     die;  
}  
/* Display any warnings. */  
DisplayWarnings();  
  
/* Free the statement resources. */  
sqlsrv_free_stmt( $stmt2 );  
  
/* Set up the array that maps employee ID to used vacation hours. */  
$emp_hrs = array (7=>4, 8=>5, 9=>8, 11=>50);  
  
/* Initialize variables that will be used as parameters. */  
$employeeId = 0;  
$vacationHrs = 0;  
  
/* Set up the parameter array. */  
$params = array(  
                 array(&$employeeId, SQLSRV_PARAM_IN),  
                 array(&$vacationHrs, SQLSRV_PARAM_INOUT)  
                );  
  
/* Define and prepare the query to substract used vacation hours. */  
$tsql3 = "{call SubtractVacationHours(?, ?)}";  
$stmt3 = sqlsrv_prepare($conn, $tsql3, $params);  
  
/* If the statement preparation fails, display errors and exit the script. */  
if( $stmt3 === false)  
{  
     DisplayErrors();  
     die;  
}  
/* Display any warnings. */  
DisplayWarnings();  
  
/* Loop through the employee=>vacation hours array. Update parameter  
 values before statement execution. */  
foreach(array_keys($emp_hrs) as $employeeId)  
{  
     $vacationHrs = $emp_hrs[$employeeId];  
     /* Execute the query.  If it fails, display the errors. */  
     if( sqlsrv_execute($stmt3) === false)  
     {  
          DisplayErrors();  
          die;  
     }  
     /* Display any warnings. */  
     DisplayWarnings();  
  
     /*Move to the next result returned by the stored procedure. */  
     if( sqlsrv_next_result($stmt3) === false)  
     {  
          DisplayErrors();  
          die;  
     }  
     /* Display any warnings. */  
     DisplayWarnings();  
  
     /* Display updated vacation hours. */  
     echo "EmployeeID $employeeId has $vacationHrs ";  
     echo "remaining vacation hours.\n";  
}  
  
/* Free the statement and connection resources. */  
sqlsrv_free_stmt( $stmt3 );  
sqlsrv_close( $conn );  
  
/* ------------- Error Handling Functions --------------*/  
function DisplayErrors()  
{  
     $errors = sqlsrv_errors(SQLSRV_ERR_ERRORS);  
     foreach( $errors as $error )  
     {  
          echo "Error: ".$error['message']."\n";  
     }  
}  
  
function DisplayWarnings()  
{  
     $warnings = sqlsrv_errors(SQLSRV_ERR_WARNINGS);  
     if(!is_null($warnings))  
     {  
          foreach( $warnings as $warning )  
          {  
               echo "Warning: ".$warning['message']."\n";  
          }  
     }  
}  
?>  
```  
  
## <a name="see-also"></a>Voir aussi  
[Procédure : configurer la gestion des erreurs et avertissements à l’aide du pilote SQLSRV](../../connect/php/how-to-configure-error-and-warning-handling-using-the-sqlsrv-driver.md)

[Informations de référence sur l’API du pilote SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)  
  
