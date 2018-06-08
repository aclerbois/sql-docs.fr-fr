---
title: Stream, objet (ADO) | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Stream
helpviewer_keywords:
- Stream object [ADO]
ms.assetid: 0514531f-009d-4519-abc3-d727014a39f1
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 08154406d90345fc995fe6eca512290936ab4d83
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="stream-object-ado"></a>Objet de flux de données (ADO)
Représente un flux de données binaires ou de texte.  
  
 Dans les hiérarchies arborescence comme un système de fichiers ou un système de messagerie, en un [enregistrement](../../../ado/reference/ado-api/record-object-ado.md) peut être un flux de binaire par défaut de bits associée qui contient le contenu du fichier ou du message électronique. A **flux** objet peut être utilisé pour manipuler des champs ou enregistrements contenant ces flux de données. A **flux** objet peut être obtenu de plusieurs manières :  
  
-   À partir d’une URL pointant vers un objet (en général un fichier) contenant des données binaires ou texte. Cet objet peut être un document simple, un **enregistrement** objet représentant un document structuré ou un dossier.  
  
-   En ouvrant la valeur par défaut **flux** objet associé à un **enregistrement** objet. Vous pouvez obtenir le flux par défaut associé à un **enregistrement** objet lors de la **enregistrement** est ouvert pour éliminer un aller-retour pour ouvrir le flux.  
  
-   En instanciant un **flux** objet. Ces **flux** objets peuvent être utilisés pour stocker les données dans le cadre de votre application. Contrairement à un **flux** associé à une URL ou la valeur par défaut **flux** d’un **enregistrement**, une instanciation **flux** ne possède aucune association avec un source sous-jacente par défaut.  
  
 Les méthodes et les propriétés d’un **flux** de l’objet, vous pouvez procédez comme suit :  
  
-   Ouvrir un **flux** à partir de l’objet un **enregistrement** ou l’URL avec le [ouvrir](../../../ado/reference/ado-api/open-method-ado-stream.md) (méthode).  
  
-   Fermer un **flux** avec la [fermer](../../../ado/reference/ado-api/close-method-ado.md) (méthode).  
  
-   Entrer des octets ou texte à un **flux** avec la [écrire](../../../ado/reference/ado-api/write-method.md) et [WriteText](../../../ado/reference/ado-api/writetext-method.md) méthodes.  
  
-   Lire les octets à partir de la **flux** avec la [en lecture](../../../ado/reference/ado-api/read-method.md) et [ReadText](../../../ado/reference/ado-api/readtext-method.md) méthodes.  
  
-   Écrire les **flux** du tampon de données en ADO à l’objet sous-jacent avec le [vider](../../../ado/reference/ado-api/flush-method-ado.md) (méthode).  
  
-   Copier le contenu d’un **flux** vers un autre **flux** avec la [CopyTo](../../../ado/reference/ado-api/copyto-method-ado.md) (méthode).  
  
-   Contrôler la façon dont les lignes sont lues à partir du fichier source avec le [SkipLine](../../../ado/reference/ado-api/skipline-method.md)(méthode) et le [LineSeparator](../../../ado/reference/ado-api/lineseparator-property-ado.md) propriété.  
  
-   Déterminer la position de fin de flux avec le [fin du support](../../../ado/reference/ado-api/eos-property.md)propriété et [SetEOS](../../../ado/reference/ado-api/seteos-method.md) (méthode).  
  
-   Enregistrer et restaurer des données dans les fichiers avec le [SaveToFile](../../../ado/reference/ado-api/savetofile-method.md)et [LoadFromFile](../../../ado/reference/ado-api/loadfromfile-method-ado.md) méthodes.  
  
-   Spécifier le jeu de caractères utilisé pour stocker le **flux** avec la [Charset](../../../ado/reference/ado-api/charset-property-ado.md) propriété.  
  
-   Arrêter asynchrone **flux** opération avec le [Annuler](../../../ado/reference/ado-api/cancel-method-ado.md) (méthode).  
  
-   Déterminer le nombre d’octets dans un **flux** avec la [taille](../../../ado/reference/ado-api/size-property-ado-stream.md) propriété.  
  
-   Contrôler la position actuelle dans un **flux** avec la [Position](../../../ado/reference/ado-api/position-property-ado.md) propriété.  
  
-   Déterminer le type de données dans un **flux** avec la [Type](../../../ado/reference/ado-api/type-property-ado-stream.md) propriété.  
  
-   Déterminer l’état actuel de la **flux** (fermé, ouvrir ou de l’exécution) avec le [état](../../../ado/reference/ado-api/state-property-ado.md) propriété.  
  
-   Spécifier le mode d’accès pour le **flux** avec la [Mode](../../../ado/reference/ado-api/mode-property-ado.md) propriété.  
  
> [!NOTE]
>  URL à l’aide du modèle http appellent automatiquement le [fournisseur Microsoft OLE DB pour la publication Internet](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Pour plus d’informations, consultez [URL absolues et relatives](../../../ado/guide/data/absolute-and-relative-urls.md).  
  
 Le **flux** objet est sécurisé pour le script.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Propriétés de l’objet stream, méthodes et événements](../../../ado/reference/ado-api/stream-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Enregistrements et flux](../../../ado/guide/data/records-and-streams.md)
