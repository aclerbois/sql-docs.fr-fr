---
title: Application de correctifs logiciels Oracle | Documents Microsoft
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
- ODBC driver for Oracle [ODBC], Oraclesoftware patches
- Oracle software patches [ODBC]
ms.assetid: 1275157b-f4e1-4c24-b273-c02555e261c2
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 773b7f179d70a4ba04b5516f930c4e1a39a31963
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="oracle-software-patches"></a>Application de correctifs logiciels Oracle
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Au lieu de cela, utilisez le pilote ODBC fourni par Oracle.  
  
 Les correctifs pour les produits de serveur Oracle et de son composant de client sont nécessaires au bon fonctionnement de plusieurs produits et technologies Microsoft, y compris le pilote Microsoft ODBC pour Oracle, le fournisseur Microsoft OLE DB pour Oracle, Internet Information Services (IIS), Services de composants (ou Microsoft Transaction Server, si vous utilisez Windows NT), et ainsi de suite.  
  
> [!NOTE]  
>  Les instructions suivantes ne peuvent pas être totalement exactes, car le site FTP d’Oracle est susceptible de changer.  
  
### <a name="to-download-the-oracle-software-patches"></a>Pour télécharger l’application de correctifs logiciels Oracle  
  
1.  Se connecter au site FTP public à oracle-ftp.oracle.com. L’ID d’utilisateur est « anonyme » et le mot de passe est votre adresse de messagerie.  
  
2.  Accédez au répertoire suivant : /server/wgt_tech/server/windowsNT.  
  
3.  Pour télécharger des correctifs les plus pertinents pour Windows 95, Windows 98 et Windows NT et Windows 2000, accédez au sous-répertoire correspondant à votre version d’Oracle : 7.3 ou 8.0. Les deux sous-répertoires sont /73patchsets et /80patchsets.  
  
4.  Pour télécharger des correctifs pour la technologie de réseau d’Oracle, SQL * Net ou Net8, accédez au répertoire suivant : / réseau.  
  
 L’accès à ce site FTP à partir de votre navigateur Web peut ne pas fonctionner. Si vous rencontrez des problèmes, essayez d’utiliser un client FTP « traditionnel », ou utilisez l’invite de commandes DOS.  
  
> [!NOTE]  
>  Oracle résout les bogues dans les versions actuelles et puis de les retrofits à des versions antérieures à l’aide de correctifs logiciels, il est recommandé que vous téléchargez le dernier correctif disponible. Cela est particulièrement vrai pour les composants clients de serveur Oracle. Si vous avez des questions sur l’installation de ces correctifs, contactez le Support de Oracle.
