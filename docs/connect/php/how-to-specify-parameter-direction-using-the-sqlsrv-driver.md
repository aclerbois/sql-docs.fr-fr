---
title: 'Comment : spécifier la Direction du paramètre à l’aide du pilote SQLSRV | Documents Microsoft'
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
- stored procedure support
ms.assetid: 1209eeca-df75-4283-96dc-714f39956b95
caps.latest.revision: 16
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 048b2961d933787cbdd35a95f676e602c37ee9f9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="how-to-specify-parameter-direction-using-the-sqlsrv-driver"></a>Procédure : spécifier la direction du paramètre à l’aide du pilote SQLSRV
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Cette rubrique décrit l’utilisation du pilote SQLSRV pour spécifier la direction du paramètre quand vous appelez une procédure stockée. La direction du paramètre est spécifiée lorsque vous construisez un tableau de paramètres (étape 3) qui est passé à [sqlsrv_query](../../connect/php/sqlsrv-query.md) ou [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md).  
  
### <a name="to-specify-parameter-direction"></a>Pour spécifier la direction du paramètre  
  
1.  Définissez une requête Transact-SQL qui appelle une procédure stockée. Utilisez des points d’interrogation (?) plutôt que les paramètres à passer à la procédure stockée. Par exemple, cette chaîne appelle une procédure stockée (UpdateVacationHours) qui accepte deux paramètres :  
  
    ```  
    $tsql = "{call UpdateVacationHours(?, ?)}";  
    ```  
  
    > [!NOTE]  
    > Appeler les procédures stockées à l’aide de la syntaxe canonique est la pratique recommandée. Pour plus d’informations sur la syntaxe canonique, consultez [appel d’une procédure stockée](../../relational-databases/native-client-odbc-stored-procedures/calling-a-stored-procedure.md).  
  
2.  Initialisez ou mettez à jour les variables PHP qui correspondent aux espaces réservés dans la requête Transact-SQL. Par exemple, le code suivant initialise les deux paramètres de la procédure stockée UpdateVacationHours :  
  
    ```  
    $employeeId = 101;  
    $usedVacationHours = 8;  
    ```  
  
    > [!NOTE]  
    > Les variables initialisées ou mises à jour avec la valeur **null**, **DateTime**ou des types de flux ne peuvent pas être utilisées comme paramètres de sortie.  
  
3.  Utilisez vos variables PHP de l’étape 2 pour créer ou mettre à jour un tableau de valeurs de paramètre correspondant, dans l’ordre, aux espaces réservés de paramètre dans la chaîne Transact-SQL. Spécifiez la direction de chaque paramètre dans le tableau. La direction de chaque paramètre est déterminée dans un des deux façons : par défaut (pour les paramètres d’entrée) ou à l’aide de **SQLSRV_PARAM_\***  constantes (pour les paramètres de sortie et bidirectionnels). Par exemple, le code suivant spécifie le paramètre *$employeeId* comme paramètre d’entrée et le paramètre *$usedVacationHours* comme paramètre bidirectionnel :  
  
    ```  
    $params = array(  
                     array($employeeId, SQLSRV_PARAM_IN),  
                     array($usedVacationHours, SQLSRV_PARAM_INOUT)  
                    );  
    ```  
  
    Pour comprendre la syntaxe à utiliser pour spécifier la direction du paramètre en général, supposons que *$var1*, *$var2*et *$var3* correspondent respectivement au paramètre d’entrée, de sortie et bidirectionnel. Vous pouvez spécifier la direction du paramètre de l’une des manières suivantes :  
  
    -   Implicitement spécifier le paramètre d’entrée, spécifiez explicitement le paramètre de sortie et spécifiez explicitement le paramètre bidirectionnel :  
  
        ```  
        array(   
               array($var1),  
               array($var2, SQLSRV_PARAM_OUT),  
               array($var3, SQLSRV_PARAM_INOUT)  
               );  
        ```  
  
    -   Spécifiez explicitement le paramètre d’entrée, spécifiez explicitement le paramètre de sortie et spécifiez explicitement le paramètre bidirectionnel :  
  
        ```  
        array(   
               array($var1, SQLSRV_PARAM_IN),  
               array($var2, SQLSRV_PARAM_OUT),  
               array($var3, SQLSRV_PARAM_INOUT)  
               );  
        ```  
  
4.  Exécutez la requête avec [sqlsrv_query](../../connect/php/sqlsrv-query.md) ou [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md) et [sqlsrv_execute](../../connect/php/sqlsrv-execute.md). Par exemple, le code suivant utilise la connexion *$conn* pour exécuter la requête *$tsql* avec les valeurs de paramètre spécifiées dans *$params*:  
  
    ```  
    sqlsrv_query($conn, $tsql, $params);  
    ```  
  
## <a name="see-also"></a>Voir aussi  
[Guide pratique pour récupérer des paramètres de sortie à l’aide du pilote SQLSRV](../../connect/php/how-to-retrieve-output-parameters-using-the-sqlsrv-driver.md)

[How to: Retrieve Input and Output Parameters Using the SQLSRV Driver](../../connect/php/how-to-retrieve-input-and-output-parameters-using-the-sqlsrv-driver.md)  
  
