---
title: Propriétés personnalisées des fichiers plats | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: data-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 7f2caeab-784c-4b0c-9b3e-6a88d1ccdbf9
caps.latest.revision: 7
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: c3094ea5f55d6884eae2d0edfd972a2c1162653b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="flat-file-custom-properties"></a>Propriétés personnalisées des fichiers plats
  **Propriétés personnalisées des sources**  
  
 La source de fichier plat comporte des propriétés personnalisées et les propriétés communes à l'ensemble des composants de flux de données.  
  
 Le tableau suivant décrit les propriétés personnalisées de la source de fichier plat. Toutes les propriétés sont en lecture/écriture.  
  
|Nom de la propriété|Type de données|Description|  
|-------------------|---------------|-----------------|  
|FileNameColumnName|String|Nom d'une colonne de sortie qui contient le nom de fichier. Si le nom n'est pas spécifié, aucune colonne de sortie contenant le nom de fichier ne sera générée.<br /><br /> Remarque : cette propriété n’est pas disponible dans **l’Éditeur de source de fichier plat**, mais elle peut être définie à l’aide de **l’éditeur avancé**.|  
|RetainNulls|Booléen|Valeur qui spécifie si les valeurs NULL du fichier source doivent être conservées comme valeurs NULL lorsque les données sont traitées par le moteur du pipeline de transformation des données. La valeur par défaut de cette propriété est **False**.|  
  
 La sortie de la source de fichier plat n'est pas dotée de propriétés personnalisées.  
  
 Le tableau suivant décrit les propriétés personnalisées des colonnes de sortie de la source de fichier plat. Toutes les propriétés sont en lecture/écriture.  
  
|Nom de la propriété|Type de données|Description|  
|-------------------|---------------|-----------------|  
|FastParse|Booléen|Valeur qui indique si la colonne utilise les routines d'analyse fournies par DTS (routines plus rapides mais qui ne tiennent pas compte des paramètres régionaux) ou les routines d'analyse standard qui tiennent compte des paramètres régionaux. Pour plus d'informations, consultez [Fast Parse](http://msdn.microsoft.com/library/6688707d-3c5b-404e-aa2f-e13092ac8d95) et [Standard Parse](http://msdn.microsoft.com/library/dfe835b1-ea52-4e18-a23a-5188c5b6f013). La valeur par défaut de cette propriété est **False**.<br /><br /> Remarque : cette propriété n’est pas disponible dans **l’Éditeur de source de fichier plat**, mais elle peut être définie à l’aide de **l’éditeur avancé**.|  
  
 Pour plus d'informations, consultez [Flat File Source](../../integration-services/data-flow/flat-file-source.md).  
  
 **Propriétés personnalisées des destinations**  
  
 La destination de fichier plat comporte à la fois des propriétés personnalisées et les propriétés communes à l'ensemble des composants de flux de données.  
  
 Le tableau suivant décrit les propriétés personnalisées de la destination de fichier plat. Toutes les propriétés sont en lecture/écriture.  
  
|Nom de la propriété|Type de données|Description|  
|-------------------|---------------|-----------------|  
|En-tête|String|Bloc de texte inséré dans le fichier avant l'écriture des données.<br /><br /> Il est possible de spécifier la valeur de cette propriété en utilisant l'expression d'une propriété.|  
|Remplacer|Booléen|Valeur qui spécifie s'il faut remplacer un fichier de destination existant qui porte le même nom ou lui ajouter des données. La valeur par défaut de cette propriété est **True**.|  
  
 L'entrée et les colonnes d'entrée de la destination de fichier plat ne disposent pas de propriétés personnalisées.  
  
 Pour plus d'informations, consultez [Flat File Destination](../../integration-services/data-flow/flat-file-destination.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Propriétés communes](http://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796)  
  
  
