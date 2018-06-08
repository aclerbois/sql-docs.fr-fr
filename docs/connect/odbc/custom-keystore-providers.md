---
title: Les fournisseurs de magasins de clés personnalisé | Documents Microsoft
ms.custom: ''
ms.date: 07/12/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: a6166d7d-ef34-4f87-bd1b-838d3ca59ae7
caps.latest.revision: 1
ms.author: v-chojas
manager: craigg
author: MightyPen
ms.openlocfilehash: 0d3a3b25ca2ead96d23b0d367ab633d900951de8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="custom-keystore-providers"></a>Fournisseurs de magasins de clés personnalisé
[!INCLUDE[Driver_ODBC_Download](../../includes/driver_odbc_download.md)]

## <a name="overview"></a>Vue d'ensemble

La fonctionnalité de chiffrement de colonne de SQL Server 2016 requiert chiffré colonne clés de chiffrement (ECEKs) enregistrées sur le serveur être récupérés par le client et ensuite déchiffré à des clés de chiffrement de colonne (clés cek) afin d’accéder aux données stockées dans des colonnes chiffrées. ECEKs sont chiffrées par clés principales de colonne (clés de migration certifiables) et la sécurité de la clé CMK est importante de la sécurité de chiffrement de colonne. Par conséquent, la clé CMK doit être stockée dans un emplacement sécurisé ; un fournisseur de magasin de clés de chiffrement de colonne vise à fournir une interface pour permettre le pilote ODBC pour y accéder en toute sécurité stockées les clés de migration certifiables. Pour les utilisateurs avec leur propre stockage sécurisé, l’Interface du fournisseur de magasin de clés personnalisé fournit une infrastructure pour l’implémentation de l’accès pour sécuriser le stockage de la clé CMK pour le pilote ODBC, qui peut ensuite être utilisé pour effectuer le chiffrement de clé et le déchiffrement.

Chaque fournisseur de magasin de clés contient et gère un ou plusieurs clés de migration certifiables, qui sont identifiés par les chemins de clés - chaînes de format défini par le fournisseur. Cette opération, ainsi que l’algorithme de chiffrement, également une chaîne définie par le fournisseur, peut servir pour effectuer le chiffrement d’une clé CEK et le déchiffrement d’un ECEK. L’algorithme, ainsi que le ECEK et le nom du fournisseur, sont stockées dans les métadonnées de chiffrement de la base de données ; consultez [CREATE COLUMN MASTER KEY](../../t-sql/statements/create-column-master-key-transact-sql.md) et [CREATE COLUMN ENCRYPTION KEY](../../t-sql/statements/create-column-encryption-key-transact-sql.md) pour plus d’informations. Par conséquent, les deux opérations fondamentales de la gestion de clés sont :

```
CEK = DecryptViaCEKeystoreProvider(CEKeystoreProvider_name, Key_path, Key_algorithm, ECEK)

-and-

ECEK = EncryptViaCEKeystoreProvider(CEKeyStoreProvider_name, Key_path, Key_algorithm, CEK)
```

où le `CEKeystoreProvider_name` est utilisé pour identifier le fournisseur de magasin de clés de chiffrement (CEKeystoreProvider), colonne spécifique et les autres arguments sont utilisés par le CEKeystoreProvider pour chiffrer/déchiffrer la clé CEK (E). Le nom et le chemin d’accès clé sont fournies par les métadonnées de clé CMK, tandis que l’algorithme et la valeur ECEK sont fournies par les métadonnées de clé CEK. Plusieurs fournisseurs de magasins de clés pouvant être présents en même temps que les ou les fournisseurs intégrés par défaut. Lors d’une opération qui requiert la clé CEK, le pilote utilise les métadonnées de clé CMK pour trouver le fournisseur de magasin de clés approprié par nom et exécute son opération de déchiffrement, qui peut être exprimée en tant que :

```
CEK = CEKeyStoreProvider_specific_decrypt(Key_path, Key_algorithm, ECEK)
```

Bien que le pilote a pas besoin pour chiffrer les clés cek, un outil de gestion de clés peut nécessaire pour implémenter des opérations telles que la création de la clé CMK et la rotation ; Cela nécessite d’effectuer l’opération inverse :

```
ECEK = CEKeyStoreProvider_specific_encrypt(Key_path, Key_algorithm, CEK)
```

### <a name="cekeystoreprovider-interface"></a>Interface de CEKeyStoreProvider

Ce document décrit en détail l’interface CEKeyStoreProvider. Un fournisseur de magasins de clés qui implémente cette interface peut être utilisé par le pilote Microsoft ODBC pour SQL Server. Les implémenteurs de CEKeyStoreProvider peuvent utiliser ce guide pour développer des fournisseurs de magasins de clés personnalisé utilisables par le pilote.

Une bibliothèque de fournisseur de magasin de clés (« bibliothèque de fournisseur ») est une bibliothèque de liens dynamiques qui peut être chargée par le pilote ODBC et contient un ou plusieurs fournisseurs de magasins de clés. Le symbole `CEKeystoreProvider` doit être exporté par une bibliothèque de fournisseur, et l’adresse d’un tableau se terminant par null de pointeurs vers les `CEKeystoreProvider` structures, une pour chaque fournisseur de magasin de clés dans la bibliothèque.

A `CEKeystoreProvider` structure définit les points d’entrée d’un fournisseur de magasin de clés unique :

```
typedef struct CEKeystoreProvider {
    wchar_t *Name;
    int (*Init)(CEKEYSTORECONTEXT *ctx, errFunc *onError);
    int (*Read)(CEKEYSTORECONTEXT *ctx, errFunc *onError, void *data, unsigned int *len);
    int (*Write)(CEKEYSTORECONTEXT *ctx, errFunc *onError, void *data, unsigned int len);
    int (*DecryptCEK)(  CEKEYSTORECONTEXT *ctx,
                        errFunc *onError,
                        const wchar_t *keyPath,
                        const wchar_t *alg,
                        unsigned char *ecek,
                        unsigned short ecekLen,
                        unsigned char **cekOut,
                        unsigned short *cekLen);
    int (*EncryptCEK)(  CEKEYSTORECONTEXT *ctx,
                        errFunc *onError,
                        const wchar_t *keyPath,
                        const wchar_t *alg,
                        unsigned char *cek,
                        unsigned short cekLen,
                        unsigned char **ecekOut,
                        unsigned short *ecekLen);
    void (*Free)();
} CEKEYSTOREPROVIDER;
```

|Nom du champ| Description|
|:--|:--|
|`Name`|Le nom du fournisseur de magasin de clés. Il ne doit pas être identique à n’importe quel autre fournisseur de magasin de clés précédemment chargée par le pilote ou présentes dans cette bibliothèque. Se terminant par null, large-chaîne de caractères.|
|`Init`|Fonction d’initialisation. Si une fonction d’initialisation n’est pas requise, ce champ peut être null.|
|`Read`|Fournisseur read (fonction). Peut être null si non nécessaire.|
|`Write`|Fonction d’écriture fournisseur. Requis si lecture n’est pas null. Peut être null si non nécessaire.|
|`DecryptCEK`|Fonction de déchiffrement ECEK. Cette fonction est la raison de l’existence d’un fournisseur de magasins de clés et ne doit pas être null.|
|`EncryptCEK`|Fonction de chiffrement de clé CEK. Le pilote n’appelle pas cette fonction, mais il est fourni pour permettre l’accès par programme à la création de ECEK par les outils de gestion de clés. Peut être null si non nécessaire.|
|`Free`|Fonction d’arrêt. Peut être null si non nécessaire.|

À l’exception la libre, les fonctions dans cette interface tous ont une paire de paramètres, **ctx** et **onError**. Elle identifie le contexte dans lequel la fonction est appelée alors que ce dernier est utilisé pour signaler les erreurs. Consultez [contextes](#context-association) et [gestion des erreurs](#error-handling) ci-dessous pour plus d’informations.

```
int Init(CEKEYSTORECONTEXT *ctx, errFunc onError);
```
Nom d’espace réservé pour une fonction d’initialisation définis par le fournisseur. Le pilote appelle cette fonction une fois, après un fournisseur a été chargé, mais avant la première fois qu’il doit lui permet d’effectuer le déchiffrement de ECEK ou Read()/Write() des demandes. Cette fonction permet d’effectuer toute initialisation dont il a besoin. 

|Argument| Description|
|:--|:--|
|`ctx`|[Entrée] Contexte de l’opération.|
|`onError`|[Entrée] Fonction de rapport d’erreurs.|
|`Return Value`|De retour différente de zéro pour indiquer une réussite, ou zéro pour indiquer un échec.|

```
int Read(CEKEYSTORECONTEXT *ctx, errFunc onError, void *data, unsigned int *len);
```

Nom d’espace réservé pour une fonction définie par le fournisseur de communication. Le pilote appelle cette fonction lorsque l’application demande à lire les données d’un (précédemment-écrites-to) fournisseur à l’aide de l’attribut de connexion SQL_COPT_SS_CEKEYSTOREDATA, ce qui permet l’application de lire des données arbitraires à partir du fournisseur. Consultez [communique avec les fournisseurs de magasins de clés](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#communicating-with-keystore-providers) pour plus d’informations.

|Argument| Description|
|:--|:--|
|`ctx`|[Entrée] Contexte de l’opération.|
|`onError`|[Entrée] Fonction de rapport d’erreurs.|
|`data`|[Sortie] Pointeur vers une mémoire tampon dans lequel le fournisseur écrit des données à lire par l’application. Cela correspond au champ de données de la structure CEKEYSTOREDATA.|
|`len`|[InOut] Pointeur vers une valeur de longueur ; lors de l’entrée, c’est la longueur maximale de la mémoire tampon de données et le fournisseur ne doit pas écrire plus de * octets len à celui-ci. Au moment du retour, le fournisseur doit mettre à jour * len avec le nombre d’octets réellement écrits.|
|`Return Value`|De retour différente de zéro pour indiquer une réussite, ou zéro pour indiquer un échec.|

```
int Write(CEKEYSTORECONTEXT *ctx, errFunc onError, void *data, unsigned int len);
```
Nom d’espace réservé pour une fonction définie par le fournisseur de communication. Le pilote appelle cette fonction lorsque l’application demande à écrire des données dans un fournisseur à l’aide de l’attribut de connexion SQL_COPT_SS_CEKEYSTOREDATA, permettant à l’application écrire des données arbitraires dans le fournisseur. Consultez [communique avec les fournisseurs de magasins de clés](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#communicating-with-keystore-providers) pour plus d’informations.

|Argument| Description|
|:--|:--|
|`ctx`|[Entrée] Contexte de l’opération.|
|`onError`|[Entrée] Fonction de rapport d’erreurs.|
|`data`|[Entrée] Pointeur vers une mémoire tampon contenant les données pour le fournisseur à lire. Cela correspond au champ de données de la structure CEKEYSTOREDATA. Le fournisseur de ne doit pas supérieur à len octets lu à partir de cette mémoire tampon.|
|`len`|[Entrée] Le nombre d’octets disponibles dans les données. Cela correspond au champ dataSize de la structure CEKEYSTOREDATA.|
|`Return Value`|De retour différente de zéro pour indiquer une réussite, ou zéro pour indiquer un échec.|

```
int (*DecryptCEK)( CEKEYSTORECONTEXT *ctx, errFunc *onError, const wchar_t *keyPath, const wchar_t *alg, unsigned char *ecek, unsigned short ecekLen, unsigned char **cekOut, unsigned short *cekLen);
```
Nom d’espace réservé pour une fonction de déchiffrement ECEK défini par le fournisseur. Le pilote appelle cette fonction pour déchiffrer un ECEK chiffrée par une clé CMK associée à ce fournisseur dans une clé CEK.

|Argument| Description|
|:--|:--|
|`ctx`|[Entrée] Contexte de l’opération.|
|`onError`|[Entrée] Fonction de rapport d’erreurs.|
|`keyPath`|[Entrée] La valeur de la [KEY_PATH](../../t-sql/statements/create-column-master-key-transact-sql.md) attribut de métadonnées pour la clé CMK référencée par le ECEK donné. Se terminant par null large-chaîne de caractères. Cela est destiné à identifier une clé CMK gérée par ce fournisseur.|
|`alg`|[Entrée] La valeur de la [algorithme](../../t-sql/statements/create-column-encryption-key-transact-sql.md) attribut de métadonnées pour le ECEK donné. Se terminant par null large-chaîne de caractères. Cela est destiné à identifier l’algorithme de chiffrement utilisé pour chiffrer le ECEK donné.|
|`ecek`|[Entrée] Pointeur vers le ECEK à déchiffrer.|
|`ecekLen`|[Entrée] Longueur de la ECEK.|
|`cekOut`|[Sortie] Le fournisseur doit allouer de mémoire pour le ECEK déchiffrée et écrire son adresse au pointeur pointé par cekOut. Il doit être possible de ce bloc de mémoire à l’aide de libérer le [LocalFree](https://msdn.microsoft.com/library/windows/desktop/aa366730(v=vs.85).aspx) (Windows) ou libre (fonction) (Linux/Mac). Si aucune mémoire n’a été allouée en raison d’une erreur ou dans le cas contraire, le fournisseur est la valeur * cekOut à un pointeur null.|
|`cekLen`|[Sortie] Le fournisseur d’écrire à l’adresse vers laquelle pointé cekLen la longueur de la ECEK déchiffrée que leur écriture sur ** cekOut.|
|`Return Value`|De retour différente de zéro pour indiquer une réussite, ou zéro pour indiquer un échec.|

```
int (*EncryptCEK)( CEKEYSTORECONTEXT *ctx, errFunc *onError, const wchar_t *keyPath, const wchar_t *alg, unsigned char *cek,unsigned short cekLen, unsigned char **ecekOut, unsigned short *ecekLen);
```
Nom d’espace réservé pour une fonction définis par le fournisseur de chiffrement de clé CEK. Le pilote ne pas appeler cette fonction, ni exposer ses fonctionnalités via l’interface ODBC, mais il est fourni pour permettre l’accès par programme à la création de ECEK par les outils de gestion de clés.

|Argument| Description|
|:--|:--|
|`ctx`|[Entrée] Contexte de l’opération.|
|`onError`|[Entrée] Fonction de rapport d’erreurs.|
|`keyPath`|[Entrée] La valeur de la [KEY_PATH](../../t-sql/statements/create-column-master-key-transact-sql.md) attribut de métadonnées pour la clé CMK référencée par le ECEK donné. Se terminant par null large-chaîne de caractères. Cela est destiné à identifier une clé CMK gérée par ce fournisseur.|
|`alg`|[Entrée] La valeur de la [algorithme](../../t-sql/statements/create-column-encryption-key-transact-sql.md) attribut de métadonnées pour le ECEK donné. Se terminant par null large-chaîne de caractères. Cela est destiné à identifier l’algorithme de chiffrement utilisé pour chiffrer le ECEK donné.|
|`cek`|[Entrée] Pointeur vers la clé CEK à chiffrer.|
|`cekLen`|[Entrée] Longueur de la clé CEK.|
|`ecekOut`|[Sortie] Le fournisseur doit allouer de mémoire pour la clé CEK chiffrée et écrire son adresse au pointeur pointé par ecekOut. Il doit être possible de ce bloc de mémoire à l’aide de libérer le [LocalFree](https://msdn.microsoft.com/library/windows/desktop/aa366730(v=vs.85).aspx) (Windows) ou libre (fonction) (Linux/Mac). Si aucune mémoire n’a été allouée en raison d’une erreur ou dans le cas contraire, le fournisseur est la valeur * ecekOut à un pointeur null.|
|`ecekLen`|[Sortie] Le fournisseur d’écrire à l’adresse vers laquelle pointé ecekLen la longueur de la clé CEK chiffrée qui leur écriture sur ** ecekOut.|
|`Return Value`|De retour différente de zéro pour indiquer une réussite, ou zéro pour indiquer un échec.|

```
void (*Free)();
```
Nom d’espace réservé pour une fonction définie par le fournisseur de l’arrêt. Le pilote peut appeler cette fonction à l’arrêt normal du processus.

> [!NOTE]
> *Chaînes à caractères larges sont des caractères de 2 octets (UTF-16) en raison de la manière dont SQL Server stocke les.*


### <a name="error-handling"></a>Gestion des erreurs

Comme les erreurs peuvent se produire au cours du traitement d’un fournisseur, un mécanisme est fourni pour lui permettre de signaler les erreurs au pilote de manière plus détaillée à une réussite/échec booléenne. De nombreuses fonctions ont une paire de paramètres, **ctx** et **onError**, qui sont utilisés conjointement à cette fin, en plus de la valeur de retour de réussite/échec.

Le **ctx** paramètre identifie le contexte dans lequel une opération de fournisseur se produit.

Le **onError** paramètre pointe vers une fonction de rapport d’erreurs, avec le prototype suivant :

`typedef void errFunc(CEKEYSTORECONTEXT *ctx, const wchar_t *msg, ...);`

|Argument| Description|
|:--|:--|
|`ctx`|[Entrée] Contexte à un rapport sur l’erreur.|
|`msg`|[Entrée] Le message d’erreur à signaler. Chaîne à caractères larges se terminant par null. Pour autoriser des informations paramétrables à présent, cette chaîne peut contenir des séquences de mise en forme insérer sous la forme accepté par le [FormatMessage](https://msdn.microsoft.com/library/windows/desktop/ms679351(v=vs.85).aspx) (fonction). Fonctionnalité étendue peut être spécifiée par ce paramètre, comme décrit ci-dessous.|
|...|[Entrée] Paramètres variadic supplémentaires en fonction des spécificateurs de format du message, comme il convient.|

Pour signaler quand une erreur s’est produite, l’onError fournisseur appelle, en fournissant le paramètre de contexte passé dans la fonction de fournisseur par le pilote et un message d’erreur avec des paramètres facultatifs supplémentaires à mettre en forme qu’elle contient. Le fournisseur peut appeler cette fonction plusieurs fois pour publier plusieurs messages d’erreur consécutives au sein de l’appel d’un fournisseur-fonction. Par exemple :

```
    if (!doSomething(...))
    {
        onError(ctx, L"An error occurred in doSomething.");
        onError(ctx, L"Additional error message with more details.");
        return 0;
    }
```


Le `msg` paramètre est généralement une chaîne à caractères larges, mais des extensions supplémentaires sont disponibles :

En utilisant l’une des valeurs prédéfinies spéciales avec la macro IDS_MSG, messages d’erreur génériques existants et dans un formulaire localisé dans le pilote peuvent être utilisée. Par exemple, si un fournisseur ne peut pas allouer de mémoire, le `IDS_S1_001` message de « Échec d’allocation de mémoire » peut être utilisé :

`onError(ctx, IDS_MSG(IDS_S1_001));`

Pour l’erreur être reconnus par le pilote, la fonction de fournisseur doit retourner une erreur. Lorsque cette opération est effectuée dans le contexte d’une opération d’ODBC, les erreurs validées seront accessibles sur le handle de connexion ou une instruction via le mécanisme de diagnostics ODBC standard (`SQLError`, `SQLGetDiagRec`, et `SQLGetDiagField`).


### <a name="context-association"></a>Association de contexte

Le `CEKEYSTORECONTEXT` structure, en plus de fournir un contexte pour le rappel d’erreur, peut également servir à déterminer le contexte d’ODBC dans lequel s’exécute une opération de fournisseur. Cela permet à un fournisseur associer des données à chacun de ces contextes, par exemple, pour implémenter la configuration de chaque connexion. À cet effet, la structure contient des pointeurs opaques 3 correspondant au contexte de l’environnement, de connexion et d’instruction :

```
typedef struct CEKeystoreContext
{
void *envCtx;
void *dbcCtx;
void *stmtCtx;
} CEKEYSTORECONTEXT;
```
|Champ| Description|
|:--|:--|
|`envCtx`|Contexte d’environnement.|
|`dbcCtx`|Contexte de connexion.|
|`stmtCtx`|Contexte de l’instruction.|

Chacun de ces contextes est une valeur opaque qui, pendant que le handle ODBC correspondant, peut être utilisée comme identificateur unique pour le handle : si gérer *X* associé à la valeur de contexte *Y*, puis Aucun autres environnement, connexion ou une instruction poignées existent simultanément en même temps que *X* aura une valeur de contexte de *Y*, et aucune autre valeur de contexte n’est associé gérer les *X*. Si l’opération de fournisseur accomplie ne dispose pas d’un contexte de handle spécifique (par exemple, les appels de SQLSetConnectAttr pour charger et configurer des fournisseurs, dans laquelle il n’existe aucun descripteur d’instruction) correspondant dans la structure de valeur de contexte est null.


## <a name="example"></a>Exemple

### <a name="keystore-provider"></a>Fournisseur de magasin de clés

Le code suivant est un exemple d’implémentation de fournisseur de magasin de clés minimale.

```
/* Custom Keystore Provider Example

Windows:   compile with cl MyKSP.c /LD /MD /link /out:MyKSP.dll
Linux/Mac: compile with gcc -fshort-wchar -fPIC -o MyKSP.so -shared MyKSP.c

 */

#ifdef _WIN32
#include <windows.h>
#else
#define __stdcall
#endif

#include <stdlib.h>
#include <sqltypes.h>
#include "msodbcsql.h"
#include <sql.h>
#include <sqlext.h>

int __stdcall KeystoreInit(CEKEYSTORECONTEXT *ctx, errFunc *onError) {
    printf("KSP Init() function called\n");
    return 1;
}

static unsigned char *g_encryptKey;
static unsigned int g_encryptKeyLen;

int __stdcall KeystoreWrite(CEKEYSTORECONTEXT *ctx, errFunc *onError, void *data, unsigned int len) {
    printf("KSP Write() function called (%d bytes)\n", len);
    if (len) {
        if (g_encryptKey)
            free(g_encryptKey);
        g_encryptKey = malloc(len);
        if (!g_encryptKey) {
            onError(ctx, L"Memory Allocation Error");
            return 0;
        }
        memcpy(g_encryptKey, data, len);
        g_encryptKeyLen = len;
    }
    return 1;
}

// Very simple "encryption" scheme - rotating XOR with the key
int __stdcall KeystoreDecrypt(CEKEYSTORECONTEXT *ctx, errFunc *onError, const wchar_t *keyPath, const wchar_t *alg,
    unsigned char *ecek, unsigned short ecekLen, unsigned char **cekOut, unsigned short *cekLen) {
    unsigned int i;
    printf("KSP Decrypt() function called (keypath=%S alg=%S ecekLen=%u)\n", keyPath, alg, ecekLen);
    if (wcscmp(keyPath, L"TheOneAndOnlyKey")) {
        onError(ctx, L"Invalid key path");
        return 0;
    }
    if (wcscmp(alg, L"none")) {
        onError(ctx, L"Invalid algorithm");
        return 0;
    }
    if (!g_encryptKey) {
        onError(ctx, L"Keystore provider not initialized with key");
        return 0;
    }
#ifndef _WIN32
    *cekOut = malloc(ecekLen);
#else
    *cekOut = LocalAlloc(LMEM_FIXED, ecekLen);
#endif
    if (!*cekOut) {
        onError(ctx, L"Memory Allocation Error");
        return 0;
    }
    *cekLen = ecekLen;
    for (i = 0; i < ecekLen; i++)
        (*cekOut)[i] = ecek[i] ^ g_encryptKey[i % g_encryptKeyLen];
    return 1;
}

// Note that in the provider interface, this function would be referenced via the CEKEYSTOREPROVIDER
// structure. However, that does not preclude keystore providers from exporting their own functions,
// as illustrated by this example where the encryption is performed via a separate function (with a
// different prototype than the one in the KSP interface.)
#ifdef _WIN32
__declspec(dllexport)
#endif
int KeystoreEncrypt(CEKEYSTORECONTEXT *ctx, errFunc *onError,
    unsigned char *cek, unsigned short cekLen,
    unsigned char **ecekOut, unsigned short *ecekLen) {
    unsigned int i;
    printf("KSP Encrypt() function called (cekLen=%u)\n", cekLen);
    if (!g_encryptKey) {
        onError(ctx, L"Keystore provider not initialized with key");
        return 0;
    }
    *ecekOut = malloc(cekLen);
    if (!*ecekOut) {
        onError(ctx, L"Memory Allocation Error");
        return 0;
    }
    *ecekLen = cekLen;
    for (i = 0; i < cekLen; i++)
        (*ecekOut)[i] = cek[i] ^ g_encryptKey[i % g_encryptKeyLen];
    return 1;
}

CEKEYSTOREPROVIDER MyCustomKSPName_desc = {
    L"MyCustomKSPName",
    KeystoreInit,
    0,
    KeystoreWrite,
    KeystoreDecrypt,
    0
};

#ifdef _WIN32
__declspec(dllexport)
#endif
CEKEYSTOREPROVIDER *CEKeystoreProvider[] = {
    &MyCustomKSPName_desc,
    0
};
```

### <a name="odbc-application"></a>Application ODBC

Le code suivant est une application de démonstration qui utilise le fournisseur de magasin de clés ci-dessus. Lors de son exécution, assurez-vous que la bibliothèque du fournisseur est dans le même répertoire que le binaire de l’application, et que la chaîne de connexion spécifie (ou spécifie une source de données qui le contienne) le `ColumnEncryption=Enabled` paramètre.

```
/*
 Example application for demonstration of custom keystore provider usage

Windows:   compile with cl /MD kspapp.c /link odbc32.lib
Linux/Mac: compile with gcc -o kspapp -fshort-wchar kspapp.c -lodbc -ldl
 
 usage: kspapp connstr

 */

#define KSPNAME L"MyCustomKSPName"
#define PROV_ENCRYPT_KEY "JHKCWYT06N3RG98J0MBLG4E3"

#include <stdio.h>
#include <stdlib.h>
#ifdef _WIN32
#include <windows.h>
#else
#define __stdcall
#include <dlfcn.h>
#endif
#include <sql.h>
#include <sqlext.h>
#include "msodbcsql.h"

/* Convenience functions */

int checkRC(SQLRETURN rc, char *msg, int ret, SQLHANDLE h, SQLSMALLINT ht) {
    if (rc == SQL_ERROR) {
        fprintf(stderr, "Error occurred upon %s\n", msg);
        if (h) {
            SQLSMALLINT i = 0;
            SQLSMALLINT outlen = 0;
            char errmsg[1024];
            while ((rc = SQLGetDiagField(
                ht, h, ++i, SQL_DIAG_MESSAGE_TEXT, errmsg, sizeof(errmsg), &outlen)) == SQL_SUCCESS
                || rc == SQL_SUCCESS_WITH_INFO) {
                fprintf(stderr, "Err#%d: %s\n", i, errmsg);
            }
        }
        if (ret)
            exit(ret);
        return 0;
    }
    else if (rc == SQL_SUCCESS_WITH_INFO && h) {
        SQLSMALLINT i = 0;
        SQLSMALLINT outlen = 0;
        char errmsg[1024];
        printf("Success with info for %s:\n", msg);
        while ((rc = SQLGetDiagField(
            ht, h, ++i, SQL_DIAG_MESSAGE_TEXT, errmsg, sizeof(errmsg), &outlen)) == SQL_SUCCESS
            || rc == SQL_SUCCESS_WITH_INFO) {
            fprintf(stderr, "Msg#%d: %s\n", i, errmsg);
        }
    }
    return 1;
}

void postKspError(CEKEYSTORECONTEXT *ctx, const wchar_t *msg, ...) {
    if (msg > (wchar_t*)65535)
        wprintf(L"Provider emitted message: %s\n", msg);
    else
        wprintf(L"Provider emitted message ID %d\n", msg);
}

int main(int argc, char **argv) {
    char sqlbuf[1024];
    SQLHENV env;
    SQLHDBC dbc;
    SQLHSTMT stmt;
    SQLRETURN rc;
    unsigned char CEK[32];
    unsigned char *ECEK;
    unsigned short ECEKlen;
#ifdef _WIN32
    HMODULE hProvLib;
#else
    void *hProvLib;
#endif
    CEKEYSTORECONTEXT ctx = {0};
    CEKEYSTOREPROVIDER **ppKsp, *pKsp;
    int(__stdcall *pEncryptCEK)(CEKEYSTORECONTEXT *, errFunc *, unsigned char *, unsigned short, unsigned char **, unsigned short *);
    int i;
    if (argc < 2) {
        fprintf(stderr, "usage: kspapp connstr\n");
        return 1;
    }

    /* Load the provider library */
#ifdef _WIN32
    if (!(hProvLib = LoadLibrary("MyKSP.dll"))) {
#else
    if (!(hProvLib = dlopen("./MyKSP.so", RTLD_NOW))) {
#endif
        fprintf(stderr, "Error loading KSP library\n");
        return 2;
    }
#ifdef _WIN32
    if (!(ppKsp = (CEKEYSTOREPROVIDER**)GetProcAddress(hProvLib, "CEKeystoreProvider"))) {
#else
    if (!(ppKsp = (CEKEYSTOREPROVIDER**)dlsym(hProvLib, "CEKeystoreProvider"))) {
#endif
        fprintf(stderr, "The export CEKeystoreProvider was not found in the KSP library\n");
        return 3;
    }
    while (pKsp = *ppKsp++) {
        if (!memcmp(KSPNAME, pKsp->Name, sizeof(KSPNAME)))
            goto FoundProv;
    }
    fprintf(stderr, "Could not find provider in the library\n");
    return 4;
FoundProv:
    if (pKsp->Init && !pKsp->Init(&ctx, postKspError)) {
        fprintf(stderr, "Could not initialize provider\n");
        return 5;
    }
#ifdef _WIN32
    if (!(pEncryptCEK = (LPVOID)GetProcAddress(hProvLib, "KeystoreEncrypt"))) {
#else
    if (!(pEncryptCEK = dlsym(hProvLib, "KeystoreEncrypt"))) {
#endif
        fprintf(stderr, "The export KeystoreEncrypt was not found in the KSP library\n");
        return 6;
    }
    if (!pKsp->Write) {
        fprintf(stderr, "Provider does not support configuration\n");
        return 7;
    }

    /* Configure the provider with the key */
    if (!pKsp->Write(&ctx, postKspError, PROV_ENCRYPT_KEY, strlen(PROV_ENCRYPT_KEY))) {
        fprintf(stderr, "Error writing to KSP\n");
        return 8;
    }

    /* Generate a CEK and encrypt it with the provider */
    srand(time(0) ^ getpid());
    for (i = 0; i < sizeof(CEK); i++)
        CEK[i] = rand();

    if (!pEncryptCEK(&ctx, postKspError, CEK, sizeof(CEK), &ECEK, &ECEKlen)) {
        fprintf(stderr, "Error encrypting CEK\n");
        return 9;
    }

    /* Connect to Server */
    rc = SQLAllocHandle(SQL_HANDLE_ENV, NULL, &env);
    checkRC(rc, "allocating environment handle", 2, 0, 0);
    rc = SQLSetEnvAttr(env, SQL_ATTR_ODBC_VERSION, (SQLPOINTER)SQL_OV_ODBC3, 0);
    checkRC(rc, "setting ODBC version to 3.0", 3, env, SQL_HANDLE_ENV);
    rc = SQLAllocHandle(SQL_HANDLE_DBC, env, &dbc);
    checkRC(rc, "allocating connection handle", 4, env, SQL_HANDLE_ENV);
    rc = SQLDriverConnect(dbc, 0, argv[1], strlen(argv[1]), NULL, 0, NULL, SQL_DRIVER_NOPROMPT);
    checkRC(rc, "connecting to data source", 5, dbc, SQL_HANDLE_DBC);
    rc = SQLAllocHandle(SQL_HANDLE_STMT, dbc, &stmt);
    checkRC(rc, "allocating statement handle", 6, dbc, SQL_HANDLE_DBC);

    /* Create a CMK definition on the server */
    {
        static char cmkSql[] = "CREATE COLUMN MASTER KEY CustomCMK WITH ("
            "KEY_STORE_PROVIDER_NAME = 'MyCustomKSPName',"
            "KEY_PATH = 'TheOneAndOnlyKey')";
        printf("Create CMK: %s\n", cmkSql);
        SQLExecDirect(stmt, cmkSql, SQL_NTS);
    }

    /* Create a CEK definition on the server */
    {
        const char cekSqlBefore[] = "CREATE COLUMN ENCRYPTION KEY CustomCEK WITH VALUES ("
            "COLUMN_MASTER_KEY = CustomCMK,"
            "ALGORITHM = 'none',"
            "ENCRYPTED_VALUE = 0x";
        char *cekSql = malloc(sizeof(cekSqlBefore) + 2 * ECEKlen + 2); /* 1 for ')', 1 for null terminator */
        strcpy(cekSql, cekSqlBefore);
        for (i = 0; i < ECEKlen; i++)
            sprintf(cekSql + sizeof(cekSqlBefore) - 1 + 2 * i, "%02x", ECEK[i]);
        strcat(cekSql, ")");
        printf("Create CEK: %s\n", cekSql);
        SQLExecDirect(stmt, cekSql, SQL_NTS);
        free(cekSql);
#ifdef _WIN32
        LocalFree(ECEK);
#else
        free(ECEK);
#endif
    }

#ifdef _WIN32
    FreeLibrary(hProvLib);
#else
    dlclose(hProvLib);
#endif

    /* Create a table with encrypted columns */
    {
        static char *tableSql = "CREATE TABLE CustomKSPTestTable ("
            "c1 int,"
            "c2 varchar(255) COLLATE Latin1_General_BIN2 ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = CustomCEK, ENCRYPTION_TYPE = DETERMINISTIC, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256'))";
        printf("Create table: %s\n", tableSql);
        SQLExecDirect(stmt, tableSql, SQL_NTS);
    }

    /* Load provider into the ODBC Driver and configure it */
    {
        unsigned char ksd[sizeof(CEKEYSTOREDATA) + sizeof(PROV_ENCRYPT_KEY) - 1];
        CEKEYSTOREDATA *pKsd = (CEKEYSTOREDATA*)ksd;
        pKsd->name = L"MyCustomKSPName";
        pKsd->dataSize = sizeof(PROV_ENCRYPT_KEY) - 1;
        memcpy(pKsd->data, PROV_ENCRYPT_KEY, sizeof(PROV_ENCRYPT_KEY) - 1);
#ifdef _WIN32
        rc = SQLSetConnectAttr(dbc, SQL_COPT_SS_CEKEYSTOREPROVIDER, "MyKSP.dll", SQL_NTS);
#else
        rc = SQLSetConnectAttr(dbc, SQL_COPT_SS_CEKEYSTOREPROVIDER, "./MyKSP.so", SQL_NTS);
#endif
        checkRC(rc, "Loading KSP into ODBC Driver", 7, dbc, SQL_HANDLE_DBC);
        rc = SQLSetConnectAttr(dbc, SQL_COPT_SS_CEKEYSTOREDATA, (SQLPOINTER)pKsd, SQL_IS_POINTER);
        checkRC(rc, "Configuring the KSP", 7, dbc, SQL_HANDLE_DBC);
    }

    /* Insert some data */
    {
        int c1;
        char c2[256];
        rc = SQLBindParameter(stmt, 1, SQL_PARAM_INPUT, SQL_C_LONG, SQL_INTEGER, 0, 0, &c1, 0, 0);
        checkRC(rc, "Binding parameters for insert", 9, stmt, SQL_HANDLE_STMT);
        rc = SQLBindParameter(stmt, 2, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_VARCHAR, 255, 0, c2, 255, 0);
        checkRC(rc, "Binding parameters for insert", 9, stmt, SQL_HANDLE_STMT);
        for (i = 0; i < 10; i++) {
            c1 = i * 10 + i + 1;
            sprintf(c2, "Sample data %d for column 2", i);
            rc = SQLExecDirect(stmt, "INSERT INTO CustomKSPTestTable (c1, c2) values (?, ?)", SQL_NTS);
            checkRC(rc, "Inserting rows query", 10, stmt, SQL_HANDLE_STMT);
        }
        printf("(Encrypted) data has been inserted into the [CustomKSPTestTable]. You may inspect the data now.\n"
            "Press Enter to continue...\n");
        getchar();
    }

    /* Retrieve the data */
    {
        int c1;
        char c2[256];
        rc = SQLBindCol(stmt, 1, SQL_C_LONG, &c1, 0, 0);
        checkRC(rc, "Binding columns for select", 11, stmt, SQL_HANDLE_STMT);
        rc = SQLBindCol(stmt, 2, SQL_C_CHAR, c2, sizeof(c2), 0);
        checkRC(rc, "Binding columns for select", 11, stmt, SQL_HANDLE_STMT);
        rc = SQLExecDirect(stmt, "SELECT c1, c2 FROM CustomKSPTestTable", SQL_NTS);
        checkRC(rc, "Retrieving rows query", 12, stmt, SQL_HANDLE_STMT);
        while (SQL_SUCCESS == (rc = SQLFetch(stmt)))
            printf("Retrieved data: c1=%d c2=%s\n", c1, c2);
        SQLFreeStmt(stmt, SQL_CLOSE);
        printf("Press Enter to clean up and exit...\n");
        getchar();
    }

    /* Clean up */
    {
        SQLExecDirect(stmt, "DROP TABLE CustomKSPTestTable", SQL_NTS);
        SQLExecDirect(stmt, "DROP COLUMN ENCRYPTION KEY CustomCEK", SQL_NTS);
        SQLExecDirect(stmt, "DROP COLUMN MASTER KEY CustomCMK", SQL_NTS);
        printf("Removed table, CEK, and CMK\n");
    }
    SQLDisconnect(dbc);
    SQLFreeHandle(SQL_HANDLE_DBC, dbc);
    SQLFreeHandle(SQL_HANDLE_ENV, env);
    return 0;
}

```

## <a name="see-also"></a>Voir aussi

[Utilisation du chiffrement intégral avec le pilote ODBC](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md)
