---
title: En Mode FIPS | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: ''
caps.latest.revision: 1
author: v-nisidh
ms.author: v-nisidh
manager: andrela
ms.openlocfilehash: 48c0ca49b743a012130a46dda30dd5053e52cea3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="fips-mode"></a>En Mode FIPS
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Le pilote JDBC Microsoft pour SQL Server prend en charge *Mode conforme à FIPS 140*. Pour Oracle / machine virtuelle Java Sun, reportez-vous à la [Mode conforme à FIPS 140 pour SunJSSE](https://docs.oracle.com/javase/7/docs/technotes/guides/security/jsse/FIPS.html) section fournie par Oracle pour configurer FIPS activé JVM. 

**Conditions préalables**:
* FIPS configuré JVM
* Certificat SSL approprié.
* Fichiers de la stratégie appropriée. 
* Paramètres de Configuration appropriés. 


## <a name="fips-configured-jvm"></a>FIPS configuré JVM

Pour voir les modules approuvés pour la Configuration de la norme FIPS, reportez-vous à la [validés FIPS 140-1 et les Modules de chiffrement FIPS 140-2](http://csrc.nist.gov/groups/STM/cmvp/documents/140-1/1401val2016.htm). 

Les fournisseurs peuvent avoir des étapes supplémentaires pour configurer la machine virtuelle Java à la norme FIPS.

### <a name="ensure-your-jvm-is-in-fips-mode"></a>Vérifiez que votre machine virtuelle Java est en Mode FIPS
Afin de garantir que votre machine virtuelle Java est FIPS activé, exécutez l’extrait de code suivant : 

````
public boolean isFIPS() throws Exception {
    Provider jsse = Security.getProvider("SunJSSE");
    return jsse != null && jsse.getInfo().contains("FIPS");
}
````

## <a name="appropriate-ssl-certificate"></a>Certificat SSL approprié
Pour pouvoir vous connecter à SQL Server en mode FIPS, un certificat SSL valide est requis. Installer ou l’importer dans le magasin de clés Java sur l’ordinateur client (JVM) où FIPS est activé. Si vous non importer, installer le certificat approprié, vous n’est peut-être pas en mesure de se connecter à SQL Server ne peut pas établir une connexion sécurisée.

### <a name="importing-ssl-certificate-in-java-keystore"></a>L’importation de certificat SSL dans le KeyStore Java
Pour FIPS, très probablement vous devez importer le certificat (.cert) soit PKCS ou dans un format spécifique au fournisseur. L’extrait de code suivant permet d’importer le certificat SSL et stockez-le dans un répertoire de travail avec le format du magasin de clés approprié. _TRUST_STORE_PASSWORD_ est votre mot de passe pour le KeyStore Java. 

````
    public void saveGenericKeyStore(String provider, String trustStoreType, String certName, String certPath) throws KeyStoreException, CertificateException, NoSuchAlgorithmException, NoSuchProviderException, IOException {
        KeyStore ks = KeyStore.getInstance(trustStoreType, provider);
        FileOutputStream os = new FileOutputStream("./MyTrustStore_" + trustStoreType);
        ks.load(null, null);
        ks.setCertificateEntry(certName, getCertificate(certPath));
        ks.store(os, TRUST_STORE_PASSWORD.toCharArray());
        os.flush();
        os.close();
    }

    private Certificate getCertificate(String pathName) throws FileNotFoundException, CertificateException {
        FileInputStream fis = new FileInputStream(pathName);
        CertificateFactory cf = CertificateFactory.getInstance("X.509");
        return cf.generateCertificate(fis);
    }

````


L’exemple suivant importe un certificat SSL de Azure dans un format PKCS12 avec BouncyCastle fournisseur. Le certificat est importé dans le répertoire de travail nommé _MyTrustStore_PKCS12_ à l’aide de l’extrait de code suivant :

` saveGenericKeyStore(BCFIPS, PKCS12, "SQLAzure SSL Certificate Name", "SQLAzure.cer"); `

## <a name="appropriate-policy-files"></a>Fichiers de la stratégie appropriée
Pour certains fournisseurs FIPS, les fichiers JAR stratégie unrestricted est nécessaires. Dans ce cas, Sun / Oracle, téléchargez les Java Cryptography Extension (JCE) illimitées force juridiction stratégie fichiers [JRE 8](http://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html) ou [JRE 7](http://www.oracle.com/technetwork/java/javase/downloads/jce-7-download-432124.html). 

## <a name="appropriate-configuration-parameters"></a>Paramètres de Configuration appropriés
Pour exécuter le pilote JDBC en mode conforme à FIPS, configurez les propriétés de connexion comme indiqué dans le tableau suivant. 

**Propriétés**: 

|Propriété|Type|Par défaut| Description|Remarques|
|---|---|---|---|---|
|encrypt|valeur booléenne [« true / false »]|« false »|FIPS activé JVM chiffrer la propriété doit être **true**||
|TrustServerCertificate|valeur booléenne [« true / false »]|« false »|Pour FIPS, l’utilisateur doit valider la chaîne de certificats, par conséquent, l’utilisateur doit utiliser **« false »** valeur de cette propriété. ||
|trustStore|Chaîne|Null|Votre chemin d’accès fichier Keystore Java où vous avez importé votre certificat. Si vous installez le certificat sur votre système, puis pas nécessaire de passer à quoi que ce soit. Pilote utilise les fichiers cacerts ou jssecacerts.||
|trustStorePassword|Chaîne|Null|Mot de passe utilisé pour vérifier l'intégrité des données trustStore.||
|FIPS|valeur booléenne [« true / false »]|« false »|Fips activé JVM cette propriété doit être **true**|Ajouté dans 6.1.4 (Stable release 6.2.2)||
|fipsProvider|Chaîne|Null|Fournisseur FIPS configuré dans la machine virtuelle Java. Par exemple, BCFIPS ou SunPKCS11-NSS |Ajouté dans 6.1.2 (Stable release 6.2.2), déconseillée dans 6.4.0 - afficher les détails [ici](https://github.com/Microsoft/mssql-jdbc/pull/460).|
|trustStoreType|Chaîne|JKS|Type de magasin de confiance FIPS mode jeu PKCS12 ou type défini par le fournisseur FIPS |Ajouté dans 6.1.2 (Stable release 6.2.2)||



  
