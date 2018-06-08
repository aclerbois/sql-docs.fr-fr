---
title: Redistribution d’ADOMD.NET | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: adomd
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: aef9cf75270e8fccf9572669ad0487fce96c0c61
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="redistributing-adomdnet"></a>Redistribution d'ADOMD.NET
  Lorsque vous écrivez une application qui utilise ADOMD.NET, vous devez redistribuer la version appropriée d'ADOMD.NET en même temps que votre application. Pour redistribuer ADOMD.NET, incluez le programme d'installation ADOMD.NET dans le programme d'installation de votre application.  
  
 Le programme d'installation d'ADOMD.NET, ainsi que la dernière version d'ADOMD.NET, sont disponibles dans le Centre de téléchargement Microsoft dans le cadre de SQL Server Feature Pack.  
  
 Le programme d’installation ADOMD.NET installe les fichiers ADOMD.NET dans \< *lecteur système*> : \Program Files\Microsoft.NET\ADOMD.NET\\*numéro de version*.  
  
 Après avoir inclus le programme d'installation ADOMD.NET, faites en sorte que le programme d'installation de votre application lance le programme d'installation ADOMD.NET et installez ADOMD.NET. En outre, selon votre environnement, vous devrez peut-être veiller à ce que les assemblys associés soient approuvés par SQL Server.  
  
 Pour plus d'informations, consultez :  
  
 [Feature Pack pour Microsoft SQL Server](http://go.microsoft.com/fwlink/?LinkId=389949)  
  
 [Microsoft Connect : Dépendances ADOMD.NET](http://go.microsoft.com/fwlink/?LinkId=389950)  
  
## <a name="see-also"></a>Voir aussi  
 [Programmation du Client ADOMD.NET](../../../analysis-services/multidimensional-models-adomd-net-client/adomd-net-client-programming.md)   
 [Programmation du serveur ADOMD.NET](../../../analysis-services/multidimensional-models-adomd-net-server/adomd-net-server-programming.md)  
  
  
