---
title: Mise à jour des données dans les curseurs SQL Server | Documents Microsoft
description: Mise à jour des données dans les curseurs de SQL Server
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: ole-db-rowsets
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- updating data [SQL Server]
- isolation levels [SQL Server]
- delayed update mode [OLE DB]
- immediate update mode [OLE DB]
- cursors [OLE DB]
- data updates [SQL Server], OLE DB
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: 06f02dbd2376ffcdb08f932245bb098ae64c53ad
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="updating-data-in-sql-server-cursors"></a>Mise à jour des données dans les curseurs SQL Server
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Lors de l’extraction et la mise à jour des données via [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] curseurs, un pilote OLE DB pour SQL Server de l’application consommateur est liée par les mêmes considérations et les contraintes qui s’appliquent à toute autre application cliente.  
  
 Seules les lignes des curseurs [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] participent à un contrôle de simultanéité d'accès aux données. Lorsque le consommateur demande un ensemble de lignes modifiable, le contrôle de simultanéité est vérifié par DBPROP_LOCKMODE. Pour modifier le niveau de contrôle d'accès simultané, le consommateur définit la propriété DBPROP_LOCKMODE avant d'ouvrir l'ensemble de lignes.  
  
 Les niveaux d'isolation de la transaction peuvent provoquer des décalages significatifs dans la position des lignes si la conception de l'application cliente permet aux transactions de demeurer ouvertes sur une longue période de temps. Par défaut, le pilote OLE DB pour SQL Server utilise le niveau d’isolation de lecture validée spécifié par DBPROPVAL_TI_READCOMMITTED. Le pilote OLE DB pour SQL Server prend en charge l’isolation de lecture erronée lors de la concurrence de l’ensemble de lignes est en lecture seule. Par conséquent, le consommateur peut demander un niveau supérieur d'isolation dans un ensemble de lignes modifiable, mais il ne peut pas demander avec succès un niveau inférieur.  
  
## <a name="immediate-and-delayed-update-modes"></a>Modes de mises à jour immédiat et différé  
 En mode de mise à jour immédiate, chaque appel à **IRowsetChange::SetData** provoque un aller-retour vers le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Si le consommateur apporte plusieurs modifications à une seule ligne, il est plus efficace de soumettre toutes les modifications avec une seule **SetData** appeler.  
  
 En mode de mise à jour différée, un aller-retour vers le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour chaque ligne indiquée dans le *cRows* et *rghRows* paramètres de **IRowsetUpdate::Update**.  
  
 Dans l'un et l'autre mode, un aller-retour représente une transaction distincte quand aucun objet de transaction n'est ouvert pour l'ensemble de lignes.  
  
 Lorsque vous utilisez **IRowsetUpdate::Update**, le pilote OLE DB pour SQL Server tente de traiter chaque ligne indiquée. Une erreur se produit en raison de valeurs de données, longueur ou état non valides pour toute ligne n’arrête pas le pilote OLE DB pour le traitement de SQL Server. La totalité des autres lignes prenant part à la mise à jour peut être modifiée. Le consommateur doit examiner le *prgRowStatus* tableau pour déterminer l’échec d’une ligne spécifique lorsque le pilote OLE DB pour SQL Server retourne DB_S_ERRORSOCCURRED.  
  
 Un consommateur ne doit pas présumer que les lignes sont traitées selon un ordre spécifique. Si un consommateur a besoin d'un traitement ordonné de la modification des données sur plusieurs lignes, le consommateur doit établir cet ordre dans la logique de l'application et ouvrir une transaction pour encadrer le processus.  
  
## <a name="see-also"></a>Voir aussi  
 [Mise à jour des données dans les ensembles de lignes](../../oledb/ole-db-rowsets/updating-data-in-rowsets.md)  
  
  
