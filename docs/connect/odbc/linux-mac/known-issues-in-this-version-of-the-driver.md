---
title: Problèmes connus dans cette Version du pilote pour SQL Server | Documents Microsoft
ms.custom: ''
ms.date: 02/14/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- known issues
caps.latest.revision: 30
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 9cd3bb6f733b9d9cac1dc3973e65199c9357bbbb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="known-issues-in-this-version-of-the-driver"></a>Problèmes connus dans cette version du pilote

[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

Cet article contient une liste des problèmes connus avec Microsoft ODBC Driver 13, 13.1 et 17 pour SQL Server sur Linux et macOS.

D’autres problèmes seront publiés sur le [blog de l’équipe du pilote Microsoft ODBC](http://blogs.msdn.com/b/sqlnativeclient/).  

- Windows, Linux et macOS convertir différemment des caractères à partir de la zone d’utilisation privée (PUA) ou des caractères (EUDC). Conversions effectuées sur le serveur dans [!INCLUDE[tsql](../../../includes/tsql_md.md)] utilisent la bibliothèque de conversion Windows. Les conversions dans le pilote utilisent les bibliothèques de conversion Windows, Linux ou macOS. Chaque bibliothèque peut produire des résultats différents lors de l’exécution de ces conversions. Pour plus d’informations, consultez [End-User-Defined and Private Use Area Characters](http://msdn.microsoft.com/library/dd317802.aspx).

- Si le client l’encodage est UTF-8, le Gestionnaire de pilotes ne pas toujours correctement convertit UTF-8 en UTF-16. Actuellement, une altération des données se produit lorsqu’un ou plusieurs caractères dans la chaîne ne sont pas des caractères UTF-8 valides. Les caractères ASCII sont correctement mappés. Le Gestionnaire de pilotes tente de cette conversion lors de l’appel des versions SQLCHAR de l’API ODBC (par exemple, SQLDriverConnectA). Le gestionnaire de pilotes n’essaie pas d’effectuer cette conversion lors de l’appel des versions SQLWCHAR de l’API ODBC (par exemple, SQLDriverConnectW).  

- Le *ColumnSize* paramètre de **SQLBindParameter** fait référence au nombre de caractères dans le type SQL, tandis que *BufferLength* est le nombre d’octets dans l’application mémoire tampon. Toutefois, si le type de données SQL est `varchar(n)` ou `char(n)`, l’application lie le paramètre en tant que SQL_C_CHAR ou SQL_C_VARCHAR et l’encodage de caractères du client est UTF-8, vous obtiendrez une erreur « données de chaîne, troncation à droite » à partir d’au pilote, même si le valeur de *ColumnSize* est aligné avec la taille du type de données sur le serveur. Cette erreur se produit, car les conversions entre les encodages de caractères peuvent modifier la longueur des données. Par exemple, une apostrophe droite caractères (U + 2019) est encodé en CP-1252 comme l’octet 0 x 92, mais en UTF-8 en tant que la séquence de 3 octets 0xe2 0x80 0x99.

Par exemple, si votre encodage est UTF-8 et que vous spécifiez 1 pour les deux *BufferLength* et *ColumnSize* dans **SQLBindParameter** pour un paramètre-out, puis essaiera à récupérer le caractère précédent est stocké dans un `char(1)` colonne sur le serveur (à l’aide de CP-1252), le pilote tente de convertir de l’encodage UTF-8 3 octets, mais ne peut pas tenir le résultat dans une mémoire tampon sur 1 octet. Dans l’autre sens, elle compare *ColumnSize* avec la *BufferLength* dans **SQLBindParameter** avant d’effectuer la conversion entre les pages de codes différentes sur le client et serveur. Étant donné qu’une *ColumnSize* de 1 est inférieure à une *BufferLength* de 3 (par exemple), le pilote génère une erreur. Pour éviter cette erreur, assurez-vous que la longueur des données après que la conversion s’adapte à la mémoire tampon spécifiée ou la colonne. Notez que *ColumnSize* ne peut pas être supérieure à 8 000 octets pour la `varchar(n)` type.

## <a name="see-also"></a>Voir aussi  
[Instructions de programmation](../../../connect/odbc/linux-mac/programming-guidelines.md)  
[Notes de publication](../../../connect/odbc/linux-mac/release-notes.md)  

