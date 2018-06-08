---
title: Codes de retour | Documents Microsoft
description: Codes de retour
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: ole-db-errors
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- OLE DB error handling, return codes
- OLE DB Driver for SQL Server, errors
- failed function [OLE DB]
- successful function [OLE DB]
- S_FALSE
- IS_ERROR macro
- DB_S_ERRORSOCCURRED return
- return codes [OLE DB]
- S_OK
- FAILED macro
- errors [OLE DB], return codes
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: 18564da5e47a6b28b92841163847bfd0b8a28aa0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="return-codes"></a>Codes de retour
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Au niveau le plus élémentaire, une fonction membre réussit ou échoue. À un niveau plus précis, une fonction peut réussir, mais son succès peut ne pas être ce que le développeur d'applications prévoyait.  
  
 Pour plus d’informations sur les codes de retour OLE DB, consultez [Codes de retour (OLE DB)](http://go.microsoft.com/fwlink/?LinkId=101631).  
  
 Lorsqu’un pilote OLE DB pour la fonction membre de SQL Server retourne S_OK, la fonction a réussi.  
  
 Lorsqu’un pilote OLE DB pour la fonction membre de SQL Server ne retourne pas S_OK, les macros de IS_ERROR de décompactage de HRESULT OLE/COM n’a pas pu peuvent déterminer la réussite ou l’échec d’une fonction globale.  
  
 Si FAILED ou IS_ERROR retourne la valeur TRUE, le pilote OLE DB pour le consommateur de SQL Server est assuré que l’exécution d’une fonction membre a échoué. Lorsque FAILED ou IS_ERROR retourne FALSE et que HRESULT n’est pas égal S_OK, le pilote OLE DB pour SQL Server consommateur est assuré de la fonction a réussi dans un sens. Le consommateur peut récupérer des informations détaillées sur ce retour « réussi avec informations » dans le pilote OLE DB pour les interfaces d’erreur SQL Server. Dans le cas où une fonction échoue clairement (la macro FAILED retourne TRUE), informations d’erreur étendues sont également disponibles dans le pilote OLE DB pour les interfaces d’erreur SQL Server.  
  
 Pilote OLE DB pour les consommateurs de SQL Server rencontrent généralement le retour HRESULT de DB_S_ERRORSOCCURRED « réussi avec informations ». En général, les fonctions membres qui retournent DB_S_ERRORSOCCURRED définissent un ou plusieurs paramètres qui remettent les valeurs d'état au consommateur. Aucune information d’erreur ne peut-être être disponible au consommateur autre que celle retournée dans les paramètres de la valeur d’état, les consommateurs doivent implémenter la logique d’application pour récupérer les valeurs d’état lorsqu’elles sont disponibles.  
  
 Le pilote OLE DB pour les fonctions de membre SQL Server ne renvoient pas le code de réussite S_FALSE. Tous les pilote OLE DB pour les fonctions de membre SQL Server retournent toujours S_OK pour indiquer une réussite.  
  
## <a name="see-also"></a>Voir aussi  
 [Erreurs](../../oledb/ole-db-errors/errors.md)  
  
  
