---
title: Méthode OpenSchema | Documents Microsoft
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
- Connection15::OpenSchema
- Connection15::raw_OpenSchema
helpviewer_keywords:
- OpenSchema method [ADO]
ms.assetid: 850cf3ce-f18f-4e7c-8597-96c1dc504866
caps.latest.revision: 21
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 52eaf3a58ae7f6eeaddecb943b5a129dec5e44e0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="openschema-method"></a>OpenSchema, méthode
Obtient les informations de schéma de base de données à partir du fournisseur.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Set recordset = connection.OpenSchema(QueryType, Criteria, SchemaID)  
```  
  
## <a name="return-value"></a>Valeur retournée  
 Retourne un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet qui contient les informations de schéma. Le **Recordset** s’ouvre comme un curseur statique en lecture seule. Le *QueryType* détermine les colonnes qui apparaissent dans le **Recordset**.  
  
#### <a name="parameters"></a>Paramètres  
 *QueryType*  
 N’importe quel [SchemaEnum](../../../ado/reference/ado-api/schemaenum.md) valeur qui représente le type de requête de schéma à exécuter.  
  
 *Critères*  
 Ce paramètre est facultatif. Un tableau de contraintes de requête pour chaque *QueryType* option, comme indiqué dans [SchemaEnum](../../../ado/reference/ado-api/schemaenum.md).  
  
 *SchemaID*  
 Le GUID pour une requête de schéma de fournisseur non défini par la spécification OLE DB. Ce paramètre est obligatoire si *QueryType* a la valeur **adSchemaProviderSpecific**; sinon, il n’est pas utilisé.  
  
## <a name="remarks"></a>Notes  
 Le **OpenSchema** méthode renvoie des informations autodescriptives sur la source de données, telles que les tables figurant dans la source de données, les colonnes dans les tables, et les types de données pris en charge.  
  
 Le *QueryType* argument est un GUID qui indique les colonnes (schémas) retournés. La spécification OLE DB comporte une liste complète des schémas.  
  
 Le *critères* argument limite les résultats d’une requête de schéma. *Critères* spécifie un tableau de valeurs qui doivent apparaître dans un sous-ensemble correspondant de colonnes, appelées colonnes de contrainte dans la boîte **Recordset**.  
  
 La constante **adSchemaProviderSpecific** est utilisé pour le *QueryType* argument si le fournisseur définit ses propres requêtes de schéma non standard en dehors de celles répertoriées précédemment. Lorsque cette constante est utilisée, le *si IDSchéma* argument est requis pour passer le GUID de la requête de schéma à exécuter. Si *QueryType* a la valeur **adSchemaProviderSpecific** mais *si IDSchéma* n’est pas fourni, une erreur se produit.  
  
 Fournisseurs ne sont pas requis pour prendre en charge toutes les requêtes de schéma standard OLE DB. Plus précisément, uniquement **requêtes adSchemaTables**, **adSchemaColumns**, et **adSchemaProviderTypes** sont requis par la spécification OLE DB. Toutefois, le fournisseur n’est pas requis pour prendre en charge la *critères* contraintes répertoriées précédemment pour les requêtes de schéma.  
  
> [!NOTE]
>  **Utilisation du Service de données à distance** le **OpenSchema** méthode n’est pas disponible sur un côté client [connexion](../../../ado/reference/ado-api/connection-object-ado.md) objet.  
  
> [!NOTE]
>  En Visual Basic, les colonnes qui possèdent un entier non signé de 4 octets (DBTYPE UI4) dans le **Recordset** retourné à partir de la **OpenSchema** méthode sur le **connexion** objet ne peut pas comparé à d’autres variables. Pour plus d’informations sur les types de données OLE DB, consultez [dans OLE DB (OLE DB), les Types de données](http://msdn.microsoft.com/en-us/6039292f-74e0-49b2-b133-17bc117ebf6a) et [Appendix A: Data Types](http://msdn.microsoft.com/en-us/e3a0533a-2196-4eb0-a31e-92fe9556ada6) dans le de Microsoft DB OLE référence du programmeur.  
  
> [!NOTE]
>  **Les utilisateurs de Visual C/C++** lorsque vous n’utilisez ne pas de curseurs côté client, la récupération de la « ORDINAL_POSITION » d’un schéma de la colonne dans ADO renvoie un variant de type VT_R8 dans Windows Data Access Components (Windows DAC) 6.0, tandis que le type utilisé dans MDAC, MDAC 2.8 et MDAC 2.7 2.6 a été VT_I4. Programmes écrits pour MDAC 2.6 recherchent uniquement un variant retourné de type que VT_I4 obtiendriez un zéro pour chaque ordinal de s’exécuter sous Windows DAC 6.0, MDAC 2.8 et MDAC 2.7 sans modification. Cette modification a été effectuée, car le type de données OLE DB retourne est DBTYPE_UI4, et dans le type VT_I4 signé il n’est pas assez d’espace pour contenir toutes les valeurs possibles sans troncation éventuellement qui se produisent et qui provoquent une perte de données.  
  
## <a name="applies-to"></a>S'applique à  
 [Connection, objet (ADO MD)](../../../ado/reference/ado-api/connection-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de méthode OpenSchema (VB)](../../../ado/reference/ado-api/openschema-method-example-vb.md)   
 [Exemple de méthode OpenSchema (VC ++)](../../../ado/reference/ado-api/openschema-method-example-vc.md)   
 [Open (méthode) (connexion ADO)](../../../ado/reference/ado-api/open-method-ado-connection.md)   
 [Open (méthode) (enregistrement ADO)](../../../ado/reference/ado-api/open-method-ado-record.md)   
 [Open (méthode) (jeu d’enregistrements ADO)](../../../ado/reference/ado-api/open-method-ado-recordset.md)   
 [Open (méthode) (flux ADO)](../../../ado/reference/ado-api/open-method-ado-stream.md)   
 [Annexe A : Fournisseurs](../../../ado/guide/appendixes/appendix-a-providers.md)
