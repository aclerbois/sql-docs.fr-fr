---
title: L’installation du logiciel (ODBC) | Documents Microsoft
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
- ODBC driver for Oracle [ODBC], installing
- installing ODBC driver for Oracle [ODBC]
ms.assetid: dfac8ade-eebe-4ebe-a199-feb740ed5bae
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 831716bbbf9c006a651b6ef82a3241d3cece381f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="installing-the-software-odbc"></a>L’installation du logiciel (ODBC)
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Au lieu de cela, utilisez le pilote ODBC fourni par Oracle.  
  
 Le pilote ODBC pour Oracle est un des composants d’accès aux données. Il accompagne d’autres composants ODBC, telles que l’administrateur de Source de données ODBC et doit déjà être installé. Le pilote peut également être trouvé sous « Pilotes et autres téléchargements » sur le site Web de Services de Support technique Microsoft en ligne sur www.microsoft.com.  
  
 Logiciel réseau doit être installé en fonction de sa propre documentation. Le pilote ODBC pour Oracle ne requiert aucune considération particulière installation tant que le logiciel réseau est prise en charge.  
  
 Logiciel Oracle doit être installé en fonction de sa propre documentation. Le pilote ODBC pour Oracle ne requiert généralement aucune considération particulière installation tant que le pilote prend en charge la version. Toutefois, pour préserver la compatibilité de produits, vous devez installer le pilote ODBC pour Oracle dernier pour vous assurer la dernière version du pilote. Oracle gère un site FTP public où il publie, entre autres choses, des correctifs pour les produits de serveur Oracle et le composant client qui est fourni avec les produits serveur. Ces correctifs sont nécessaires au bon fonctionnement de plusieurs produits et technologies Microsoft. Pour plus d’informations sur ce site, consultez [application de correctifs logiciels Oracle](../../odbc/microsoft/oracle-software-patches.md).  
  
> [!CAUTION]  
>  L’installation du logiciel Oracle sur MDAC/Windows DAC peut remplacer les versions actuelles de MDAC. Si des problèmes surviennent à l’aide des composants ODBC, réinstallez MDAC.
