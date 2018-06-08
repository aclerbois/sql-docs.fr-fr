---
title: Fermeture de la connexion (méthode), exemple de propriété Table Type (VC ++) | Documents Microsoft
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
dev_langs:
- C++
helpviewer_keywords:
- Type property [ADOX], VC++ example
- Close method [ADOX], VC++ example
ms.assetid: d0e250aa-fc57-4fd3-9610-d64f50c5507f
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 63760975712c15318331db4b4ad0a001a3232da2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connection-close-method-table-type-property-example-vc"></a>Méthode de fermeture de connexion, exemple de propriété Table Type (VC ++)
Définition de la [ActiveConnection](../../../ado/reference/adox-api/activeconnection-property-adox.md) propriété **rien** doit « fermer » le catalogue. Collections associées seront vides. Tous les objets qui ont été créés à partir des objets de schéma dans le catalogue sont orphelins. Toutes les propriétés sur les objets qui ont été mis en cache seront toujours disponibles, mais la tentative de lecture des propriétés qui requièrent un appel au fournisseur échouera.  
  
```  
// BeginCloseConnectionCpp.cpp  
// compile with: /EHsc  
#import "msado15.dll" rename("EOF", "EndOfFile")  
#import "msadox.dll" no_namespace  
  
#include "iostream"  
using namespace std;  
  
// Function declarations  
inline void TESTHR(HRESULT x) {if FAILED(x) _com_issue_error(x);};  
void CloseConnectionByNothingX();  
void CloseConnectionX();  
  
int main() {  
   if ( FAILED(::CoInitialize(NULL) ) )  
      return -1;  
  
   CloseConnectionByNothingX();  
   CloseConnectionX();  
  
   ::CoUninitialize();  
}  
  
void CloseConnectionByNothingX() {  
   HRESULT hr = S_OK;  
  
   // Define and initialize ADOX object pointers, in ADODB namespace.  
   _CatalogPtr m_pCatalog = NULL;  
   _TablePtr m_pTable = NULL;  
  
   // Define ADODB object pointers  
   ADODB::_ConnectionPtr m_pCnn = NULL;  
  
   // Define other variables  
   _variant_t vIndex = (short) 0;  
  
   try {  
      TESTHR(hr = m_pCnn.CreateInstance(__uuidof(ADODB::Connection)));  
      TESTHR(hr = m_pCatalog.CreateInstance(__uuidof(Catalog)));  
  
      m_pCnn->Open("Provider='Microsoft.JET.OLEDB.4.0';Data Source= 'c:\\Northwind.mdb';", "", "", NULL);  
      m_pCatalog->PutActiveConnection(_variant_t((IDispatch *)m_pCnn));  
      m_pTable = m_pCatalog->Tables->GetItem(vIndex);  
  
      // Cache m_pTable.Type info  
      cout << m_pTable->Type << endl;  
  
      _variant_t vCnn;  
      vCnn.vt = VT_DISPATCH;  
      vCnn.pdispVal = NULL;  
      m_pCatalog->PutActiveConnection(vCnn);  
  
      // m_pTable is orphaned, will succeed if this was cached  
      cout << m_pTable->Type << endl;  
  
      cout << m_pTable->Columns->GetItem(vIndex)->DefinedSize << endl;  
      // Previous line will fail if this info has not been cached  
   }  
   catch(_com_error &e) {  
      // Notify the user of errors if any.  
      _bstr_t bstrSource(e.Source());  
      _bstr_t bstrDescription(e.Description());  
  
      printf("\nError\n\tSource :  %s \n\tdescription : %s \n", (LPCSTR)bstrSource, (LPCSTR)bstrDescription);  
   }  
   catch(...) {  
      cout << "Error occured in CloseConnectionByNothingX...." << endl;  
   }  
}  
  
void CloseConnectionX() {  
   HRESULT hr = S_OK;  
  
   // Define ADOX object pointers, initialize pointers. These are in ADODB  namespace.  
  
   _CatalogPtr m_pCatalog = NULL;  
   _TablePtr m_pTable = NULL;  
  
   // Define ADODB object pointers  
   ADODB::_ConnectionPtr m_pCnn = NULL;  
  
   // Define other variables  
   _variant_t vIndex = (short) 0;  
   try {  
      TESTHR(hr = m_pCnn.CreateInstance(__uuidof(ADODB::Connection)));  
      m_pCnn->Open("Provider='Microsoft.JET.OLEDB.4.0';Data Source= 'c:\\Northwind.mdb';", "", "", NULL);  
  
      TESTHR(hr = m_pCatalog.CreateInstance(__uuidof(Catalog)));  
      m_pCatalog->PutActiveConnection(_variant_t((IDispatch *)m_pCnn));  
  
      m_pTable = m_pCatalog->Tables->GetItem(vIndex);  
  
      // Cache m_pTable.Type info  
      cout << m_pTable->Type << endl;  
  
      m_pCnn->Close();  
  
      // m_pTable is orphaned, will succeed if this was cached  
      cout << m_pTable->Type << endl;  
  
      cout << m_pTable->Columns->GetItem(vIndex)->DefinedSize << endl;  
      // Previous line will fail if this info has not been cached  
   }  
   catch(_com_error &e) {  
      // Notify the user of errors if any.  
      _bstr_t bstrSource(e.Source());  
      _bstr_t bstrDescription(e.Description());  
  
      printf("\nError\n\tSource :  %s \n\tdescription : %s \n", (LPCSTR)bstrSource, (LPCSTR)bstrDescription);  
   }  
   catch(...) {  
      cout << "Error occured in CloseConnectionX...." << endl;  
   }  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ActiveConnection, propriété (ADOX)](../../../ado/reference/adox-api/activeconnection-property-adox.md)
