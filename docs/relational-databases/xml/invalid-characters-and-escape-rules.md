---
title: Caractères non valides et règles d’échappement | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: xml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- FOR XML clause, invalid characters
- FOR XML clause, escape rules
ms.assetid: f2e9b997-f400-4963-b225-59d46c6b93e8
caps.latest.revision: 17
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 6188f8359cb662ce7dc168d5e0745ebc8f53b722
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="invalid-characters-and-escape-rules"></a>Caractères non valides et règles d'échappement
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Cette rubrique décrit comment les caractères XML non valides sont contrôlés par la clause FOR XML, et répertorie les règles d'échappement pour les caractères qui ne sont pas valides dans les noms XML.  
  
## <a name="for-xml-and-invalid-characters"></a>FOR XML et les caractères non valides  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] convertit en entités les caractères XML non valides quand ils sont retournés dans des requêtes FOR XML qui n’utilisent pas la directive TYPE.  
  
 Bien que les analyseurs XML 1.0 génèrent des erreurs d'analyse, que ces caractères soient ou non codés sous forme d'entité, le codage d'entité est plus en conformité avec XML 1.1. Le codage d'entité est aussi en meilleure adéquation avec les futures versions de la norme XML. De plus, il facilite le débogage puisque le point de code du caractère incorrect est visible.  
  
 Pour les utilisateurs d'outils XML, il n'existe aucun moyen de contourner le problème puisque l'analyseur XML échouera quoi qu'il arrive au point où se présentent les caractères non valides dans le flux de données. Si vous utilisez des outils non XML, cette modification risque de vous imposer la mise à jour de votre logique de programmation afin de rechercher ces caractères sous la forme de leurs codes d'entité.  
  
 Les caractères d'espace suivants sont codés différemment dans les requêtes FOR XML afin de les garder présents tout au long des va-et-vient :  
  
-   Dans le contenu d’élément et d’attribut : **hex(0D)** (retour chariot)  
  
-   Dans le contenu d’attribut : **hex(09)** (tabulation), **hex(0A)** (saut de ligne)  
  
 Ces caractères sont conservés dans la sortie et ne seront pas normalisés par un analyseur.  
  
## <a name="escape-rules"></a>Règles d'échappement  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Les noms contenant des caractères non valides pour les noms XML (comme les espaces) sont convertis en noms XML de telle façon que les caractères non valides soient convertis en codages d’entités numériques placés dans une séquence d’échappement.  
  
 Seuls deux caractères non alphabétiques sont autorisés au début d'un nom XML : les deux-points (:) et le trait de soulignement (_). Dans la mesure où les deux-points (:) sont déjà réservés aux espaces de noms, le trait de soulignement (_) est utilisé comme caractère d'échappement. Les règles d'échappement utilisées pour l'encodage sont les suivantes :  
  
-   Tout caractère UCS-2 qui n’est pas un caractère valide pour un nom XML (selon la spécification XML 1.0), est placé dans une séquence d’échappement sous la forme _xHHHH\_. La chaîne HHHH désigne le code hexadécimal UCS-2 à quatre chiffres du caractère dans l'ordre du bit le plus significatif en premier. Par exemple, le nom de la table **Order Details** est encodé sous la forme Order_x0020_Details.  
  
-   Les caractères qui n’entrent pas dans le domaine UCS-2 (les ajouts UCS-4 correspondant à la plage comprise entre U+00010000 et U+0010FFFF) sont encodés sous la forme _xHHHHHHHH\_, où HHHHHHHH représente l'encodage UCS-4 hexadécimal à huit chiffres du caractère, en cas d'utilisation du mode de compatibilité ascendante avec SQL Server 2000. Dans tous les autres cas, les caractères sont encodés sous la forme _xHHHHHH\_de façon à respecter la norme ISO.  
  
-   Le caractère de soulignement n'a pas besoin d'être placé dans une séquence d'échappement, sauf s'il est suivi du caractère x. Par exemple, le nom de la table **Order_Details** n’est pas encodé.  
  
-   Dans les identificateurs, les deux-points (:) ne sont pas placés dans une séquence d'échappement pour que les noms d'élément ou d'attribut des espaces de noms puissent être générés par la requête FOR XML. Par exemple, la requête suivante génère un attribut d'espace de noms comportant deux-points (:) dans le nom :  
  
    ```  
    SELECT 'namespace-urn' as 'xmlns:namespace',   
     1 as 'namespace:a'   
    FOR XML RAW  
    ```  
  
     La requête produit le résultat suivant :  
  
    ```  
    <row xmlns:namespace="namespace-urn" namespace:a="1"/>  
    ```  
  
     Notez qu'il est quand même préférable d'utiliser WITH XMLNAMESPACES pour ajouter des espaces de noms XML.  
  
## <a name="see-also"></a> Voir aussi  
 [FOR XML &#40;SQL Server&#41;](../../relational-databases/xml/for-xml-sql-server.md)  
  
  
