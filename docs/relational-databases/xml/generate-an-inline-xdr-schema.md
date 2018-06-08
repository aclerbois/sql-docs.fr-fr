---
title: Générer un schéma XDR en ligne | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: xml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- XDR schemas [SQL Server]
- inline XDR schema generation [SQL Server]
- XMLDATA option
- FOR XML clause, inline XDR schema generation
ms.assetid: 2a40d617-9724-4f7d-80a4-a85c702f14d0
caps.latest.revision: 32
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 7aec8be09b1ecd5faed258532d6e770c8720308a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="generate-an-inline-xdr-schema"></a>Générer un schéma XDR en ligne
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  La directive **XMLDATA** de FOR XML retourne un schéma XDR en ligne avec le résultat de la requête. Toutefois, le schéma XDR ne prend pas en charge tous les nouveaux types de données et autres améliorations introduites dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et les versions ultérieures. Au lieu de cela, vous pouvez demander un schéma XSD en ligne à l’aide de [la directive XMLSCHEMA](../../relational-databases/xml/generate-an-inline-xsd-schema.md).  
  
> [!IMPORTANT]  
>  La directive XMLDATA de l'option FOR XML est déconseillée. Utilisez la génération XSD en mode RAW et AUTO. Il n'y a aucun remplacement pour la directive XMLDATA en mode EXPLICIT. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 Notez également les éléments suivants concernant la prise en charge de schéma XDR en ligne :  
  
-   Si le résultat de la requête FOR XML contient des colonnes de type **xml** et que vous demandez un schéma XDR en ligne, une erreur est retournée. XDR en ligne ne prend pas en charge ces types.  
  
-   Les types **(n)varchar(max)** et **(n)varbinary(max)** seront mappés respectivement à **(n)varchar(n)** et **varbinary(n)**.  
  
-   Quand le mode de compatibilité a la valeur 90 ou une valeur supérieure, les valeurs **timestamp** sont considérées comme des données **varbinary(8)** , sont traitées comme des données binaires et sont retournées dans le résultat de la manière suivante :  
  
    -   L’encodage en base 64 est utilisé quand **binary base64** est spécifié.  
  
    -   L’encodage des URL est utilisé en mode AUTO quand **binary base64** n’est pas spécifié.  
  
  
