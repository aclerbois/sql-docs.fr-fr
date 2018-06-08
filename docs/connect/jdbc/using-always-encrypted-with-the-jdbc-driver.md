---
title: À l’aide d’Always Encrypted avec le pilote JDBC | Documents Microsoft
ms.custom: ''
ms.date: 3/14/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 271c0438-8af1-45e5-b96a-4b1cabe32707
caps.latest.revision: 64
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 7c53479e3e94206645382e0c7b2d930a0b63075f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-always-encrypted-with-the-jdbc-driver"></a>À l’aide d’Always Encrypted avec le pilote JDBC
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Cette page fournit des informations sur la façon de développer des applications Java à l’aide de [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md) et le pilote Microsoft JDBC 6.0 (ou version ultérieure) pour SQL Server.

Always Encrypted permet aux clients de chiffrer les données sensibles et de ne jamais révéler les données ou les clés de chiffrement dans SQL Server ou de la base de données SQL Azure. Un chiffrement intégral activé pilote, tels que le pilote Microsoft JDBC 6.0 (ou version ultérieure) pour SQL Server, permet d’obtenir ce comportement par chiffrement et déchiffrement des données sensibles dans l’application cliente de façon transparente. Le pilote détermine automatiquement la requête les paramètres correspondent aux colonnes de base de données Always Encrypted et chiffre les valeurs de ces paramètres avant de les envoyer à SQL Server ou de la base de données SQL Azure. De même, il déchiffre de manière transparente les données récupérées dans les colonnes de base de données chiffrées, qui figurent dans les résultats de la requête. Pour plus d’informations, consultez [Always Encrypted (moteur de base de données)](../../relational-databases/security/encryption/always-encrypted-database-engine.md) et [toujours chiffré référence des API pour le pilote JDBC](../../connect/jdbc/always-encrypted-api-reference-for-the-jdbc-driver.md).

## <a name="prerequisites"></a>Configuration requise
- Assurez-vous que Microsoft JDBC Driver 6.0 (ou version ultérieure) pour SQL Server est installé sur votre ordinateur de développement. 
- Téléchargez et installez les fichiers de stratégie Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction.  Veillez à lire le fichier Readme inclus dans le fichier zip pour des instructions d’installation et les détails pertinents sur les problèmes d’importation/exportation possibles.  

    - Si vous utilisez le mssql-jdbc-X.X.X.jre7.jar ou sqljdbc41.jar, les fichiers de stratégie peuvent être téléchargés à partir de [téléchargement des fichiers de stratégie juridiction Java Cryptography Extension (JCE) Unlimited 7](http://www.oracle.com/technetwork/java/javase/downloads/jce-7-download-432124.html)

    - Si vous utilisez le mssql-jdbc-X.X.X.jre8.jar ou sqljdbc42.jar, les fichiers de stratégie peuvent être téléchargés à partir de [téléchargement des fichiers de stratégie juridiction Java Cryptography Extension (JCE) Unlimited 8](http://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html)

    - Si vous utilisez le mssql-jdbc-X.X.X.jre9.jar, aucun fichier de stratégie ne doit être téléchargée. La stratégie de juridiction dans Java 9 par défaut est un nombre illimité de niveau de chiffrement.

## <a name="working-with-column-master-key-stores"></a>Utilisation des magasins de clé principale de colonne
Pour chiffrer ou déchiffrer des données pour les colonnes chiffrées, SQL Server gère les clés de chiffrement de colonne. Les clés de chiffrement de colonne sont stockées sous forme chiffrée dans les métadonnées de la base de données. Chaque clé de chiffrement de colonne a une clé principale de colonne correspondante qui est utilisée pour chiffrer la clé de chiffrement de colonne. Les métadonnées de la base de données ne contient pas les clés principales de colonne. Ces clés sont uniquement stockées par le client. Toutefois, les métadonnées de la base de données contient plus d’informations sur l’emplacement des clés principales de la colonne par rapport au client. Par exemple, les métadonnées de la base de données peuvent dire que le magasin de clés contenant une clé principale de colonne est le magasin de certificats Windows et le certificat spécifique utilisé pour chiffrer et déchiffrer se trouve à un emplacement spécifique dans le magasin de certificats Windows. Si le client a accès à ce certificat dans le magasin de certificats Windows, il peut obtenir le certificat. Le certificat peut ensuite être utilisé pour déchiffrer la clé de chiffrement de colonne. Cette clé de chiffrement peut être utilisé pour déchiffrer ou de chiffrer les données pour les colonnes chiffrées qui utilisent cette clé de chiffrement de colonne.

Le pilote JDBC Microsoft pour SQL Server communique avec un magasin de clés à l’aide d’un fournisseur de magasin de clés principales de colonne, qui est une instance d’une classe dérivé de **SQLServerColumnEncryptionKeyStoreProvider**.

### <a name="using-built-in-column-master-key-store-providers"></a>Utilisation des fournisseurs de magasin de clés principales de colonne intégrés
Le pilote JDBC Microsoft pour SQL Server est fourni avec les fournisseurs de magasins de clé principale de la colonne intégrés suivants. Certaines de ces fournisseurs sont pré-enregistré avec les noms des fournisseurs spécifiques (utilisés pour rechercher le fournisseur) en fonction des informations d’identification supplémentaires ou de l’inscription explicite.

| Classe |  Description | Nom de fournisseur (pour la recherche) |Pré-enregistré ?|
|:---|:---|:---|:---|
|**SQLServerColumnEncryptionAzureKeyVaultProvider**| Un fournisseur pour un magasin de clés pour le coffre de clés Azure.| AZURE_KEY_VAULT|non|
|**SQLServerColumnEncryptionCertificateStoreProvider**| Un fournisseur pour le magasin de certificats Windows.|MSSQL_CERTIFICATE_STORE|Oui
|**SQLServerColumnEncryptionJavaKeyStoreProvider**| Un fournisseur pour le keystore Java|MSSQL_JAVA_KEYSTORE|Oui|

Pour les fournisseurs les magasins de clés, il est inutile d’apporter des modifications de code d’application à utiliser ces fournisseurs, mais notez les éléments suivants :

- Vous (ou votre administrateur) doivent s’assurer que le nom du fournisseur configuré dans les métadonnées de clé principale de colonne est correct et que le chemin d’accès de la clé principale de la colonne est compatible avec le format de chemin de la clé est valide pour un fournisseur donné. Il est recommandé de configurer les clés à l’aide des outils, tels que SQL Server Management Studio, ce qui génère automatiquement les noms de fournisseur valide et les chemins de clés lors de l’émission de l’instruction CREATE COLUMN MASTER KEY (Transact-SQL).
- Assurez-vous que votre application peut accéder à la clé dans le magasin de clés. Cette tâche peut impliquer l’octroi de votre application à accéder à la clé ou le magasin de clés, selon le magasin de clés, ou effectuer d’autres étapes de configuration du magasin de clés spécifique. Par exemple, pour l’utilisation de la SQLServerColumnEncryptionJavaKeyStoreProvider, vous devez fournir l’emplacement et le mot de passe de combinaison de touches dans les propriétés de connexion. 

Tous ces fournisseurs de magasins de clés sont décrits plus en détail dans les sections qui suivent. Vous devez uniquement implémenter un fournisseur de magasin de clés pour utiliser le chiffrement intégral.

### <a name="using-azure-key-vault-provider"></a>Utilisation du fournisseur Azure Key Vault
Coffre de clés Azure est une option pratique pour stocker et gérer des clés principales de colonne pour Always Encrypted (en particulier si votre application est hébergée dans Azure). Le pilote JDBC Microsoft pour SQL Server inclut un fournisseur intégré, SQLServerColumnEncryptionAzureKeyVaultProvider, pour les applications qui ont des clés stockées dans le coffre de clés Azure. Le nom de ce fournisseur est AZURE_KEY_VAULT. Pour pouvoir utiliser le fournisseur de magasins d’Azure Key Vault, un développeur d’applications a besoin créer l’archivage et les clés dans le coffre de clés Azure et créer un enregistrement d’application dans Azure Active Directory. L’application inscrite doit être accordé obtenir, déchiffrer, Encrypt, désencapsuler une clé, encapsulez la clé et vérifiez les autorisations dans les stratégies d’accès définis pour le coffre de clés créé pour une utilisation avec Always Encrypted. Pour plus d’informations sur la façon de configurer le coffre de clés et de créer une clé principale de colonne, consultez [Azure Key Vault – étape par étape](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step/) et [création des clés de principales de colonne dans le coffre de clés Azure](../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md#creating-column-master-keys-in-azure-key-vault).

Pour les exemples de cette page, si vous avez créé un coffre de clés Azure basée sur clé principale de colonne et de clé de chiffrement de colonne à l’aide de SQL Server Management Studio, le script T-SQL pour recréer les peut ressembler à cet exemple avec ses propres **clé_ Chemin d’accès** et **ENCRYPTED_VALUE**:

```
CREATE COLUMN MASTER KEY [MyCMK]
WITH
(
    KEY_STORE_PROVIDER_NAME = N'AZURE_KEY_VAULT',
    KEY_PATH = N'https://<MyKeyValutName>.vault.azure.net:443/keys/Always-Encrypted-Auto1/c61f01860f37302457fa512bb7e7f4e8'
)

CREATE COLUMN ENCRYPTION KEY [MyCEK]
WITH VALUES
(
    COLUMN_MASTER_KEY = [MyCMK],
    ALGORITHM = 'RSA_OAEP',
    ENCRYPTED_VALUE = 0x01BA000001680074507400700073003A002F002F006400610076006...
)
```

Pour utiliser le coffre de clés Azure, les applications clientes doivent instancier le SQLServerColumnEncryptionAzureKeyVaultProvider et l’inscrire avec le pilote.

Voici un exemple d’initialisation SQLServerColumnEncryptionAzureKeyVaultProvider :  

```
SQLServerColumnEncryptionAzureKeyVaultProvider akvProvider = new SQLServerColumnEncryptionAzureKeyVaultProvider(clientID, clientKey);
```

**clientID** est l’ID d’Application d’un enregistrement de l’application dans une instance d’Azure Active Directory. **clientKey** est un mot de passe de clé inscrit sous cette Application, qui fournit l’accès aux API pour Azure Key Vault.

Une fois que l’application crée une instance de SQLServerColumnEncryptionAzureKeyVaultProvider, l’application doit inscrire l’instance avec le pilote à l’aide de la méthode sqlserverconnection.registercolumnencryptionkeystoreproviders (). Il est fortement recommandé que l’instance est inscrite en utilisant le nom de recherche par défaut, AZURE_KEY_VAULT, ce qui peut être obtenu en appelant l’API SQLServerColumnEncryptionAzureKeyVaultProvider.getName(). En utilisant le nom par défaut vous permettra d’utiliser des outils tels que SQL Server Management Studio ou PowerShell pour configurer et gérer des clés Always Encrypted (les outils permet d’utiliser le nom par défaut pour générer l’objet de métadonnées pour la clé principale de colonne). L’exemple suivant illustre l’inscription du fournisseur Azure Key Vault. Pour plus d’informations sur la méthode sqlserverconnection.registercolumnencryptionkeystoreproviders (), consultez [toujours chiffré référence des API pour le pilote JDBC](../../connect/jdbc/always-encrypted-api-reference-for-the-jdbc-driver.md).

```
Map<String, SQLServerColumnEncryptionKeyStoreProvider> keyStoreMap = new HashMap<String, SQLServerColumnEncryptionKeyStoreProvider>();
keyStoreMap.put(akvProvider.getName(), akvProvider);
SQLServerConnection.registerColumnEncryptionKeyStoreProviders(keyStoreMap);
```

> [!IMPORTANT]
>  Si vous utilisez le fournisseur de magasin de clés Azure Key Vault, l’implémentation de coffre de clés Azure du pilote JDBC présente des dépendances sur ces bibliothèques (à partir de GitHub) qui doivent être inclus avec votre application :
>
>  [azure-sdk-for-java](https://github.com/Azure/azure-sdk-for-java)
>
>  [bibliothèques Azure bibliothèque Active Directory pour java](https://github.com/AzureAD/azure-activedirectory-library-for-java)
>
> Pour obtenir un exemple montrant comment inclure ces dépendances dans un projet Maven, consultez [télécharger ADAL4J et AKV dépendances avec Apache Maven](https://github.com/Microsoft/mssql-jdbc/wiki/Download-ADAL4J-And-AKV-Dependencies-with-Apache-Maven)

### <a name="using-windows-certificate-store-provider"></a>À l’aide du fournisseur de magasin de certificats Windows
Le SQLServerColumnEncryptionCertificateStoreProvider peut servir à stocker des clés principales de colonne dans le magasin de certificats Windows. Pour créer la clé principale de colonne et le chiffrement de colonne clées définitions dans la base de données, utilisez l’Assistant chiffrement intégral de SQL Server Management Studio (SSMS) ou autres outils pris en charge. L’Assistant même peut être utilisé pour générer un certificat auto-signé dans le magasin de certificats Windows utilisables comme clé principale de colonne pour les données toujours chiffrées. Pour plus d’informations sur la clé principale de colonne et la syntaxe T-SQL clée de chiffrement de colonne, consultez [CREATE COLUMN MASTER KEY](../../t-sql/statements/create-column-master-key-transact-sql.md) et [CREATE COLUMN ENCRPTION KEY](../../t-sql/statements/create-column-encryption-key-transact-sql.md) respectivement.

Le nom de la SQLServerColumnEncryptionCertificateStoreProvider est MSSQL_CERTIFICATE_STORE et peut être interrogé par les API getName() de l’objet fournisseur. Il est automatiquement enregistré par le pilote et peut être utilisé en toute transparence sans aucune modification de l’application.

Pour les exemples de cette page, si vous avez créé un magasin de certificats Windows basée sur clé principale de colonne et de clé de chiffrement de colonne à l’aide de SQL Server Management Studio, le script T-SQL pour recréer les peut ressembler à cet exemple avec son propre spécifiques **KEY_PATH** et **ENCRYPTED_VALUE**:

```
CREATE COLUMN MASTER KEY [MyCMK]
WITH
(
    KEY_STORE_PROVIDER_NAME = N'MSSQL_CERTIFICATE_STORE',
    KEY_PATH = N'CurrentUser/My/A2A91F59C461B559E4D962DA9D2BC6131B32CB91'
)

CREATE COLUMN ENCRYPTION KEY [MyCEK]
WITH VALUES
(
    COLUMN_MASTER_KEY = [MyCMK],
    ALGORITHM = 'RSA_OAEP',
    ENCRYPTED_VALUE = 0x016E000001630075007200720065006E0074007500730065007200...
)
```

> [!IMPORTANT]
> Alors que les autres fournisseurs de magasins de clés dans cet article sont disponibles sur toutes les plateformes prises en charge par le pilote, l’implémentation SQLServerColumnEncryptionCertificateStoreProvider du pilote JDBC est disponible sur les systèmes d’exploitation Windows uniquement. Il comporte une dépendance sur le fichier sqljdbc_auth.dll qui est disponible dans le package de pilotes. Pour utiliser ce fournisseur, copiez le fichier sqljdbc_auth.dll dans un répertoire sur le chemin d’accès de système de Windows sur l’ordinateur où est installé le pilote JDBC. Vous pouvez également définir la propriété système java.libary.path afin de spécifier le répertoire du fichier sqljdbc_auth.dll. Si vous exécutez une machine virtuelle Java (JVM) 32 bits, utilisez le fichier sqljdbc_auth.dll dans le dossier x86, même si la version du système d'exploitation est x64. Si vous exécutez une machine virtuelle Java (JVM) 64 bits sur un processeur x64, utilisez le fichier sqljdbc_auth.dll dans le dossier x64. Par exemple, si vous utilisez la machine virtuelle Java 32 bits et le pilote JDBC est installé dans le répertoire par défaut, vous pouvez spécifier l’emplacement de la DLL à l’aide de l’argument suivant de la machine virtuelle (VM) au démarrage de l’application Java : `-Djava.library.path=C:\Microsoft JDBC Driver <version> for SQL Server\sqljdbc_<version>\enu\auth\x86`

### <a name="using-java-key-store-provider"></a>À l’aide du fournisseur de magasin de clés de Java
Le pilote JDBC est fourni avec une implémentation de fournisseur de magasin de clés intégrée pour le magasin de clés de Java. Si le **keyStoreAuthentication** propriété de chaîne de connexion est présente dans la chaîne de connexion et il est défini sur « JavaKeyStorePassword », le pilote instancie automatiquement et enregistre le fournisseur de magasin de clés de Java. Le nom du fournisseur de magasin de clés de Java est MSSQL_JAVA_KEYSTORE. Ce nom peut également être interrogé à l’aide de l’API SQLServerColumnEncryptionJavaKeyStoreProvider.getName(). 

Il existe trois propriétés de chaîne de connexion qui permettent une application cliente spécifier les informations d’identification que le pilote doit s’authentifier auprès du magasin de clés de Java. Le pilote initialise le fournisseur selon les valeurs de ces trois propriétés dans la chaîne de connexion.

**keyStoreAuthentication :** identifie le magasin de clés Java à utiliser. Avec le pilote Microsoft JDBC 6.0 et versions ultérieures pour SQL Server, vous pouvez authentifier pour le magasin de clés Java uniquement par le biais de cette propriété. Pour le magasin de clés Java, la valeur de cette propriété doit être `JavaKeyStorePassword`.

**keyStoreLocation :** le chemin d’accès au fichier de magasin de clés Java qui stocke la clé principale de colonne. Le chemin d’accès inclut le nom de fichier du magasin de clés.

**keyStoreSecret :** le secret/mot de passe à utiliser pour le keystore ainsi que pour la clé. Pour utiliser le magasin de clés Java, le magasin de clés et le mot de passe de clé doivent être le même.

Voici un exemple de fournir ces informations d’identification dans la chaîne de connexion :

```
String connectionString = "jdbc:sqlserver://localhost;user=<user>;password=<password>;columnEncryptionSetting=Enabled;keyStoreAuthentication=JavaKeyStorePassword;keyStoreLocation=<path_to_the_keystore_file>;keyStoreSecret=<keystore_key_password>";
```

Vous pouvez également obtenir ou définir ces paramètres à l’aide de l’objet SQLServerDataSource. Pour plus d’informations, consultez [toujours chiffré référence des API pour le pilote JDBC](../../connect/jdbc/always-encrypted-api-reference-for-the-jdbc-driver.md).

Le pilote JDBC instancie automatiquement le SQLServerColumnEncryptionJavaKeyStoreProvider lorsque ces informations d’identification sont présentes dans les propriétés de connexion.

### <a name="creating-a-column-master-key-for-the-java-key-store"></a>Création d’une clé principale de colonne pour le magasin de clés de Java
Le SQLServerColumnEncryptionJavaKeyStoreProvider peut être utilisé avec les types de magasins de clés JKS ou PKCS12. Pour créer ou importer une clé à utiliser avec ce fournisseur utiliser Java [keytool](http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html) utilitaire. La clé doit avoir le même mot de passe en tant que le magasin de clés. Voici un exemple montrant comment créer une clé publique et sa clé privée associée à l’aide de l’utilitaire keytool :

```
keytool -genkeypair -keyalg RSA -alias AlwaysEncryptedKey -keystore keystore.jks -storepass mypassword -validity 360 -keysize 2048 -storetype jks
```

Cette commande crée une clé publique et il est encapsulé dans un X.509 auto-signé certificat, qui est stocké dans le magasin de clés 'keystore.jks', ainsi que sa clé privée associée. Cette entrée dans le magasin de clés est identifiée par l’alias 'AlwaysEncryptedKey'.

Voici un exemple de ce même en utilisant un type de magasin PKCS12 :

```
keytool -genkeypair -keyalg RSA -alias AlwaysEncryptedKey -keystore keystore.pfx -storepass mypassword -validity 360 -keysize 2048 -storetype pkcs12 -keypass mypassword
```

Si le magasin de clés est de type PKCS12, l’utilitaire keytool ne pas invite à entrer un mot de passe de clé et le mot de passe de clé doit être fourni avec l’option de - keypass en tant que le SQLServerColumnEncryptionJavaKeyStoreProvider nécessite que le magasin de clés et la clé possèdent le même mot de passe.

Vous pouvez également exporter un certificat à partir du magasin de certificats Windows au format .pfx et l’utiliser avec le SQLServerColumnEncryptionJavaKeyStoreProvider. Le certificat exporté ne peut également être importé dans le magasin de clés Java comme un type de magasin de clés JKS.

Après avoir créé l’entrée keytool, crée les métadonnées de clé principale de colonne dans la base de données, ce qui nécessite le nom de fournisseur de magasin de clés et le chemin de clé. Pour plus d’informations sur la création des métadonnées de clé principale de colonne, consultez [CREATE COLUMN MASTER KEY](../../t-sql/statements/create-column-master-key-transact-sql.md). Pour SQLServerColumnEncryptionJavaKeyStoreProvider, le chemin de clé est simplement l’alias de la clé et le nom de la SQLServerColumnEncryptionJavaKeyStoreProvider est 'MSSQL_JAVA_KEYSTORE'. Vous pouvez également interroger ce nom à l’aide de l’API publique de getName() de la classe SQLServerColumnEncryptionJavaKeyStoreProvider. 

La syntaxe T-SQL pour la création de la clé principale de colonne est la suivante :

```
CREATE COLUMN MASTER KEY [<CMK_name>]
WITH
(
    KEY_STORE_PROVIDER_NAME = N'MSSQL_JAVA_KEYSTORE',
    KEY_PATH = N'<key_alias>'
)
```

Pour le 'AlwaysEncryptedKey' créé ci-dessus, la définition de clé principale de colonne serait :

```
CREATE COLUMN MASTER KEY [MyCMK]
WITH
(
    KEY_STORE_PROVIDER_NAME = N'MSSQL_JAVA_KEYSTORE',
    KEY_PATH = N'AlwaysEncryptedKey'
)
```

> [!NOTE]
> La gestion de SQL Server intégrée fonctionnalité Studio Impossible de créer les définitions de clé principale de colonne pour le magasin de clés de Java. Les commandes T-SQL doivent être utilisés par programme.

### <a name="creating-a-column-encryption-key-for-the-java-key-store"></a>Création d’une clé de chiffrement de colonne pour le magasin de clés de Java
SQL Server Management Studio ou tout autre outil ne peut pas être utilisé pour créer des clés de chiffrement à l’aide des clés principales de colonne dans le magasin de clés Java de colonne. L’application cliente doit créer la clé de chiffrement de colonne par programmation à l’aide de la classe SQLServerColumnEncryptionJavaKeyStoreProvider. Pour plus d’informations, consultez [à l’aide de fournisseurs de magasins de clé principale de colonne pour la configuration par programme des clés](#using-column-master-key-store-providers-for-programmatic-key-provisioning).

### <a name="implementing-a-custom-column-master-key-store-provider"></a>Implémentation d’un fournisseur de magasin de clés principales de colonne personnalisé
Si vous souhaitez stocker des clés principales de colonne dans un magasin de clés n’est pas pris en charge par un fournisseur existant, vous pouvez implémenter un fournisseur personnalisé en étendant la classe SQLServerColumnEncryptionKeyStoreProvider et l’inscription du fournisseur à l’aide de la Méthode Sqlserverconnection.registercolumnencryptionkeystoreproviders ().

```
public class MyCustomKeyStore extends SQLServerColumnEncryptionKeyStoreProvider{  
    private String name = "MY_CUSTOM_KEYSTORE";

    public void setName(String name)
    {
        this.name = name;
    }

    public String getName()
    {
        return name;
    }

    public byte[] encryptColumnEncryptionKey(String masterKeyPath, String encryptionAlgorithm, byte[] plainTextColumnEncryptionKey)
    {
        // Logic for encrypting the column encryption key
    }

    public byte[] decryptColumnEncryptionKey(String masterKeyPath, String encryptionAlgorithm, byte[] encryptedColumnEncryptionKey)
    {
        // Logic for decrypting the column encryption key
    }
}
```

Inscrivez le fournisseur :

```
SQLServerColumnEncryptionKeyStoreProvider storeProvider = new MyCustomKeyStore();
Map<String, SQLServerColumnEncryptionKeyStoreProvider> keyStoreMap = new HashMap<String, SQLServerColumnEncryptionKeyStoreProvider>();
keyStoreMap.put(storeProvider.getName(), storeProvider);
SQLServerConnection.registerColumnEncryptionKeyStoreProviders(keyStoreMap);
```

## <a name="using-column-master-key-store-providers-for-programmatic-key-provisioning"></a>Utilisation des fournisseurs de magasin de clés principales de colonne pour la mise en service des clés par programmation
Pour accéder à des colonnes chiffrées, le pilote JDBC Microsoft pour SQL Server en toute transparence par la recherche et appelle le fournisseur de magasins de clé principale de colonne de droite pour déchiffrer les clés de chiffrement de colonne. En règle générale, un code d’application normal n’appelle pas directement les fournisseurs de magasin de clés principales de colonne. Toutefois, vous pouvez instancier et appeler un fournisseur par programmation pour configurer et gérer des clés Always Encrypted. Cette étape peut être effectuée pour générer une clé de chiffrement de colonne chiffrée et de déchiffrer une clé de chiffrement de colonne en tant que partie rotation de clé principale de la colonne, par exemple. Pour plus d’informations, consultez [Vue d’ensemble de la gestion des clés pour Always Encrypted](../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md).

Si vous utilisez un fournisseur de magasin de clés personnalisés, mise en œuvre de vos propres outils de gestion de clés peut être requis. Lorsque vous utilisez des clés stockées dans le magasin de certificats Windows ou dans le coffre de clés Azure, vous pouvez utiliser des outils existants, tels que SQL Server Management Studio ou PowerShell, pour gérer et configurer des clés. Lorsque vous utilisez des clés stockées dans le magasin de clés Java, vous devez configurer les clés par programmation. L’exemple suivant illustre l’utilisation de la classe SQLServerColumnEncryptionJavaKeyStoreProvider pour chiffrer la clé avec une clé stockée dans le magasin de clés de Java.

```
import java.sql.*;
import javax.xml.bind.DatatypeConverter;
import com.microsoft.sqlserver.jdbc.SQLServerColumnEncryptionJavaKeyStoreProvider;
import com.microsoft.sqlserver.jdbc.SQLServerColumnEncryptionKeyStoreProvider;
import com.microsoft.sqlserver.jdbc.SQLServerException;

/**
 * This program demonstrates how to create a column encryption key programmatically for the Java Key Store.
 */
public class AlwaysEncrypted
{
    // Alias of the key stored in the keystore.
    private static String keyAlias = "<proide key alias>";

    // Name by which the column master key will be known in the database.
    private static String columnMasterKeyName = "MyCMK";

    // Name by which the column encryption key will be known in the database.
    private static String columnEncryptionKey = "MyCEK";

    // The location of the keystore.
    private static String keyStoreLocation = "C:\\Dev\\Always Encrypted\\keystore.jks";

    // The password of the keystore and the key.
    private static char[] keyStoreSecret = "********".toCharArray();

    /**
     * Name of the encryption algorithm used to encrypt the value of
     * the column encryption key. The algorithm for the system providers must be RSA_OAEP.
     */
    private static String algorithm = "RSA_OAEP";

    public static void main(String[] args)
    {
        String connectionString = GetConnectionString();
        try
        {
            // Note: if you are not using try-with-resources statements (as here),
            // you must remember to call close() on any Connection, Statement,
            // ResultSet objects that you create.

            // Open a connection to the database.
            Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");
            try (Connection sourceConnection = DriverManager.getConnection(connectionString))
            {
                // Instantiate the Java Key Store provider.
                SQLServerColumnEncryptionKeyStoreProvider storeProvider =
                        new SQLServerColumnEncryptionJavaKeyStoreProvider(
                                keyStoreLocation,
                                keyStoreSecret);

                byte [] encryptedCEK=getEncryptedCEK(storeProvider);

                /**
                 * Create column encryption key
                 * For more details on the syntax, see:
                 * https://docs.microsoft.com/sql/t-sql/statements/create-column-encryption-key-transact-sql
                 * Encrypted column encryption key first needs to be converted into varbinary_literal from bytes, 
                 * for which DatatypeConverter.printHexBinary is used
                 */
                String createCEKSQL = "CREATE COLUMN ENCRYPTION KEY "
                        + columnEncryptionKey
                        + " WITH VALUES ( "
                        + " COLUMN_MASTER_KEY = "
                        + columnMasterKeyName
                        + " , ALGORITHM =  '"
                        + algorithm
                        + "' , ENCRYPTED_VALUE =  0x"
                        + DatatypeConverter.printHexBinary(encryptedCEK)
                        + " ) ";

                try (Statement cekStatement = sourceConnection.createStatement())
                {
                    cekStatement.executeUpdate(createCEKSQL);
                    System.out.println("Column encryption key created with name : " + columnEncryptionKey);
                }
            }
        }
        catch (Exception e)
        {
            // Handle any errors that may have occurred.
            e.printStackTrace();
        }
    }

    // To avoid storing the sourceConnection String in your code,
    // you can retrieve it from a configuration file.
    private static String GetConnectionString()
    {
        // Create a variable for the connection string.
        String connectionUrl = "jdbc:sqlserver://localhost:1433;" +
                "databaseName=ae2;user=sa;password=********;";

        return connectionUrl;
    }

    private static byte[] getEncryptedCEK(SQLServerColumnEncryptionKeyStoreProvider storeProvider) throws SQLServerException
    {
        /**
         * Following arguments needed by SQLServerColumnEncryptionJavaKeyStoreProvider
         * 1) keyStoreLocation :
         *      Path where keystore is located, including the keystore file name.
         * 2) keyStoreSecret :
         *      Password of the keystore and the key.
         */
        String plainTextKey = "You need to give your plain text";

        // plainTextKey has to be 32 bytes with current algorithm supported
        byte[] plainCEK = plainTextKey.getBytes();

        // This will give us encrypted column encryption key in bytes
        byte[] encryptedCEK = storeProvider.encryptColumnEncryptionKey(
                keyAlias,
                algorithm,
                plainCEK);

        return encryptedCEK;
    }
}
```

## <a name="enabling-always-encrypted-for-application-queries"></a>Activation d’Always Encrypted pour les requêtes d’application
Le plus simple pour activer le chiffrement des paramètres et le déchiffrement des résultats de requête qui ciblent des colonnes chiffrées consiste en définissant la valeur de la **columnEncryptionSetting** mot clé de chaîne de connexion à **activé** .

La chaîne de connexion suivante est un exemple d’activation du chiffrement intégral dans le pilote JDBC :

```
String connectionString = "jdbc:sqlserver://localhost;user=<user>;password=<password>;databaseName=<database>;columnEncryptionSetting=Enabled;";
SQLServerConnection connection = (SQLServerConnection) DriverManager.getConnection(connectionString);
```

Le code suivant est un exemple équivalent à l’aide de l’objet SQLServerDataSource :

```
SQLServerDataSource ds = new SQLServerDataSource();
ds.setServerName("localhost");
ds.setUser("<user>");
ds.setPassword("<password>");
ds.setDatabaseName("<database>");
ds.setColumnEncryptionSetting("Enabled");
SQLServerConnection con = (SQLServerConnection) ds.getConnection();
```

Always Encrypted peut également être activé pour les requêtes individuelles. Pour plus d’informations, consultez [contrôle de l’impact sur les performances d’Always Encrypted](#controlling-the-performance-impact-of-always-encrypted). Activation du chiffrement intégral n’est pas suffisant pour le chiffrement ou le déchiffrement réussisse. Vous devez également vérifier ce qui suit :
- L’application dispose des autorisations de base de données *VIEW ANY COLUMN MASTER KEY DEFINITION* et *VIEW ANY COLUMN ENCRYPTION KEY DEFINITION* qui sont nécessaires pour accéder aux métadonnées des clés Always Encrypted dans la base de données. Pour plus d’informations, consultez [autorisations dans Always Encrypted (moteur de base de données)](../../relational-databases/security/encryption/always-encrypted-database-engine.md#database-permissions).
- L’application peut accéder à la clé principale de colonne qui protège les clés de chiffrement de colonne, lequel chiffrent les colonnes de la base de données interrogées. Pour utiliser le fournisseur de magasin de clés de Java, vous devez fournir les informations d’identification supplémentaires dans la chaîne de connexion. Pour plus d’informations, consultez [fournisseur de magasin de clés à l’aide Java](#using-java-key-store-provider).

### <a name="configuring-how-javasqltime-values-are-sent-to-the-server"></a>Configurer la façon dont les valeurs java.sql.Time sont envoyées au serveur
Le **sendTimeAsDatetime** propriété de connexion est utilisée pour configurer la façon dont la valeur java.sql.Time est envoyée au serveur. Lorsque cette propriété a la valeur false, la valeur d’heure est envoyée en tant que type de temps SQL Server. Lorsque cette propriété a la valeur true, l’heure d’envoi de la valeur en un type datetime. Si une colonne time est chiffrée, le **sendTimeAsDatetime** propriété doit être false, comme des colonnes chiffrées ne prennent pas en charge la conversion à partir de l’heure en datetime. Notez également que cette propriété est par la valeur true par défaut, lors de l’utilisation des colonnes de temps chiffrée, vous devrez affecter la valeur false. Dans le cas contraire, le pilote lève une exception. À compter de la version 6.0 du pilote, la classe SQLServerConnection a deux méthodes pour configurer la valeur de cette propriété par programmation :
 
* setSendTimeAsDatetime void publique (boolean sendTimeAsDateTimeValue)
* public boolean getSendTimeAsDatetime()

Pour plus d’informations sur cette propriété, consultez [java.sql.Time configurer comment les valeurs sont envoyées au serveur](configuring-how-java-sql-time-values-are-sent-to-the-server.md).

### <a name="configuring-how-string-values-are-sent-to-the-server"></a>Configurer comment les valeurs de chaîne sont envoyés au serveur
Le **sendStringParametersAsUnicode** propriété de connexion est utilisée pour configurer la façon dont les valeurs de chaîne sont envoyés à SQL Server. Si la valeur true, des paramètres de chaîne est envoyée au serveur au format Unicode. Si la valeur false, paramètres de chaîne est envoyée dans un format non Unicode, tels que MBCS, Unicode ou ASCII. La valeur par défaut de cette propriété a la valeur true. Lorsque Always Encrypted est activé et une colonne char/varchar/varchar(max) est chiffrée, la valeur de **sendStringParametersAsUnicode** doit être définie sur false. Si cette propriété est définie sur true, le pilote lève une exception lors du déchiffrement des données d’une colonne chiffrée char/varchar/varchar(max) qui comporte des caractères Unicode. Pour plus d’informations sur cette propriété, consultez [définissant les propriétés de connexion](../../connect/jdbc/setting-the-connection-properties.md).
  
## <a name="retrieving-and-modifying-data-in-encrypted-columns"></a>Extraction et modification de données dans des colonnes chiffrées
Une fois que vous activez Always Encrypted pour les requêtes de l’application, vous pouvez utiliser l’API JDBC standard pour récupérer ou modifier des données dans les colonnes de la base de données chiffrée. Si votre application dispose des autorisations de base de données requis et peut accéder à la clé principale de colonne, le pilote chiffre tous les paramètres de requête qui ciblent des colonnes chiffrées et seront déchiffrer les données qui sont récupérées à partir des colonnes chiffrées.

Si Always Encrypted n’est pas activé, les requêtes ayant des paramètres qui ciblent des colonnes chiffrées échouent. Requêtes peuvent toujours récupérer des données des colonnes chiffrées, que la requête n’a aucun paramètre ciblant des colonnes chiffrées. Toutefois, le pilote ne tente pas de déchiffrer les valeurs extraites des colonnes chiffrées et l’application reçoit des données chiffrées binaires (en tant que tableaux d’octets).

Le tableau suivant récapitule le comportement des requêtes selon que le chiffrement intégral est activé ou non :

|Caractéristique de la requête | Always Encrypted est activé et l’application peut accéder aux clés et à leurs métadonnées|Always Encrypted est activé et l’application ne peut pas accéder aux clés et à leurs métadonnées | Always Encrypted est désactivé|
|:---|:---|:---|:---|
| Requêtes avec des paramètres ciblant des colonnes chiffrées. | Des valeurs de paramètres sont chiffrées en toute transparence. | Erreur | Erreur|
| Requêtes qui récupèrent des données des colonnes chiffrées sans paramètres ciblant des colonnes chiffrées.| Les résultats de colonnes chiffrées sont déchiffrés de manière transparente. L’application reçoit des valeurs de texte en clair des types de données JDBC correspondant aux types SQL Server configurées pour les colonnes chiffrées. | Erreur | Les résultats des colonnes chiffrées ne sont pas déchiffrés. L’application reçoit des valeurs chiffrées sous la forme de tableaux d’octets (byte[]).

### <a name="inserting-and-retrieving-encrypted-data-examples"></a>Insertion et la récupération des exemples de données chiffrées 
Les exemples suivants illustrent la récupération et la modification de données dans des colonnes chiffrées. Les exemples supposent que la table cible avec le schéma suivant et les colonnes SSN et BirthDate chiffrées. Si vous avez configuré une clé principale de colonne nommée « MyCMK » et une clé de chiffrement de colonne nommée « MyCEK » (comme décrit dans les sections de fournisseurs de magasins de clés précédent), vous pouvez créer la table à l’aide de ce script :

```
CREATE TABLE [dbo].[Patients]([PatientId] [int] IDENTITY(1,1),
 [SSN] [char](11) COLLATE Latin1_General_BIN2
 ENCRYPTED WITH (ENCRYPTION_TYPE = DETERMINISTIC,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = MyCEK) NOT NULL,
 [FirstName] [nvarchar](50) NULL,
 [LastName] [nvarchar](50) NULL,
 [BirthDate] [date]
 ENCRYPTED WITH (ENCRYPTION_TYPE = RANDOMIZED,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = MyCEK) NOT NULL
 PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY])
 GO
```

Pour chaque exemple de code Java, vous devrez insérer du code de magasin de clés spécifiques à l’emplacement indiqué.

Si vous utilisez un fournisseur de magasin de clés Azure Key Vault :

```
    String clientID = "<Azure Application ID>";
    String clientKey = "<Azure Application API Key Password>";
    SQLServerColumnEncryptionAzureKeyVaultProvider akvProvider = new SQLServerColumnEncryptionAzureKeyVaultProvider(clientID, clientKey);
    Map<String, SQLServerColumnEncryptionKeyStoreProvider> keyStoreMap = new HashMap<String, SQLServerColumnEncryptionKeyStoreProvider>();
    keyStoreMap.put(akvProvider.getName(), akvProvider);
    SQLServerConnection.registerColumnEncryptionKeyStoreProviders(keyStoreMap);
    String connectionString = "jdbc:sqlserver://localhost:1433;databaseName=Clinic;user=sa;password=******;columnEncryptionSetting=Enabled;";
```

Si vous utilisez un fournisseur de magasin de clés du magasin de certificats Windows :

```
    String connectionString = "jdbc:sqlserver://localhost:1433;databaseName=Clinic;user=sa;password=******;columnEncryptionSetting=Enabled;";
```

Si vous utilisez un fournisseur de magasin de clés de magasin de clés Java :

```
    String connectionString = "jdbc:sqlserver://localhost:1433;databaseName=Clinic;user=sa;password=******;columnEncryptionSetting=Enabled;keyStoreAuthentication=JavaKeyStorePassword;keyStoreLocation=<path to jks or pfx file>;keyStoreSecret=<keystore secret/password>";
```

### <a name="inserting-data-example"></a>Exemple d’insertion de données
Cet exemple insère une ligne dans la table Patients. Notez les éléments suivants :
- L’exemple de code ne contient aucun élément spécifique au chiffrement. Le pilote JDBC Microsoft pour SQL Server détecte automatiquement et chiffre les paramètres qui ciblent des colonnes chiffrées. Ce comportement rend le chiffrement transparent pour l’application.
- Les valeurs insérées dans des colonnes de la base de données, y compris les colonnes chiffrées, sont passés comme paramètres à l’aide de SQLServerPreparedStatement. Lors de l’utilisation de paramètres est facultative lors de l’envoi des valeurs pour les colonnes non chiffrées (même si, il est fortement recommandé, car elle contribue à empêcher l’injection SQL), il est nécessaire pour les valeurs qui ciblent des colonnes chiffrées. Si les valeurs insérées dans les colonnes chiffrées ont été passés en tant que littéraux incorporés dans l’instruction de requête, la requête échoue, car le pilote ne serait pas en mesure de déterminer les valeurs de colonnes chiffrées cibles et il ne serait pas chiffrer les valeurs. Par conséquent, le serveur les rejettera en les considérant comme incompatibles avec les colonnes chiffrées.
- Toutes les valeurs sont imprimées par le programme sera en texte brut, comme le pilote JDBC Microsoft pour SQL Server déchiffre de manière transparente les données récupérées à partir des colonnes chiffrées.
- Si vous effectuez une recherche à l’aide d’une clause WHERE, la valeur utilisée dans la clause WHERE doit être passé en tant que paramètre afin que le pilote peut chiffrer de manière transparente avant de l’envoyer à la base de données. Dans l’exemple suivant, le SSN est passé en tant que paramètre, mais le nom est passé en tant que littéral comme LastName n’est pas chiffré.
- La méthode setter utilisée pour le paramètre ciblant la colonne SSN est setString(), qui est mappé avec le type de données SQL Server char/varchar. Si, pour ce paramètre, la méthode d’accesseur Set utilisée a été setNString(), qui est mappée vers nchar/nvarchar, la requête échoue, car Always Encrypted ne prend pas en charge les conversions de valeurs nchar/nvarchar chiffrées en valeurs char/varchar chiffrées.

```
try
{
    <Insert keystore-specific code here>

    Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");
    try (Connection sourceConnection = DriverManager.getConnection(connectionString))
    {
        String insertRecord="INSERT INTO [dbo].[Patients] VALUES (?, ?, ?, ?)";
        try (PreparedStatement insertStatement = sourceConnection.prepareStatement(insertRecord))
        {
            insertStatement.setString(1, "795-73-9838");
            insertStatement.setString(2, "Catherine");
            insertStatement.setString(3, "Abel");
            insertStatement.setDate(4, Date.valueOf("1996-09-10"));
            insertStatement.executeUpdate();
            System.out.println("1 record inserted.\n");
        }
    }
}
catch (Exception e)
{
    e.printStackTrace();
}
```

### <a name="retrieving-plaintext-data-example"></a>Exemple de données de texte en clair d’extraction
L’exemple suivant illustre le filtrage des données en fonction de valeurs chiffrées et la récupération des données en texte clair à partir des colonnes chiffrées. Notez les éléments suivants :
- La valeur utilisée dans la clause WHERE pour filtrer sur la colonne SSN doit être passé en tant que paramètre afin que le pilote JDBC Microsoft pour SQL Server peut chiffrer de manière transparente avant de l’envoyer à la base de données.
- Toutes les valeurs sont imprimées par le programme sera en texte brut, comme le pilote JDBC Microsoft pour SQL Server déchiffre de manière transparente les données récupérées à partir des colonnes SSN et BirthDate.

> [!NOTE]
> Si les colonnes sont chiffrées à l’aide du chiffrement déterministe, les requêtes peuvent effectuer des comparaisons d’égalité sur ces derniers. Pour plus d’informations, consultez [le chiffrement en sélectionnant déterministe ou aléatoire de Always Encrypted (moteur de base de données)](../../relational-databases/security/encryption/always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption).

```
try
{
    <Insert keystore-specific code here>

    Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");
    try (Connection sourceConnection = DriverManager.getConnection(connectionString))
    {
        String filterRecord="SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE SSN = ?;";
    
        try (PreparedStatement selectStatement = sourceConnection.prepareStatement(filterRecord))
        {
            selectStatement.setString(1, "795-73-9838");
            ResultSet rs = selectStatement.executeQuery();
            while(rs.next())
            {
                System.out.println("SSN: " +rs.getString("SSN") +
                    ", FirstName: " + rs.getString("FirstName") +
                    ", LastName:"+ rs.getString("LastName")+
                    ", Date of Birth: " + rs.getString("BirthDate"));
            }
        }
    }
}
catch (Exception e)  
{  
    e.printStackTrace();  
}
```
  
### <a name="retrieving-encrypted-data-example"></a>Exemple de la récupération des données chiffrées
Si Always Encrypted n’est pas activé, une requête peut toujours récupérer des données à partir de colonnes chiffrées, tant qu’aucun de ses paramètres ne ciblent des colonnes chiffrées.

L’exemple suivant illustre la récupération des données chiffrées binaires à partir des colonnes chiffrées. Notez les éléments suivants :
- Étant donné que le chiffrement intégral n’est pas activé dans la chaîne de connexion, la requête retournera des valeurs SSN et BirthDate chiffrées en tant que tableaux d’octets (le programme convertit les valeurs en chaînes).
- Une requête qui récupère des données à partir de colonnes chiffrées lorsqu’Always Encrypted est désactivé peut avoir des paramètres, tant qu’aucun d’eux ne cible une colonne chiffrée. Les filtres de requête suivants par nom, qui n’est pas chiffrée dans la base de données. Si la requête filtre par SSN ou BirthDate, la requête échoue.

```
try
{
    String connectionString  = "jdbc:sqlserver://localhost:1433;" + "databaseName=Clinic;user=sa;password=******";

    Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");
    try (Connection sourceConnection = DriverManager.getConnection(connectionString))
    {
        String filterRecord="SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE LastName = ?;";

        try (PreparedStatement selectStatement = sourceConnection.prepareStatement(filterRecord))
        {
            selectStatement.setString(1, "Abel");
            ResultSet rs = selectStatement.executeQuery();
            while (rs.next())
            {
                System.out.println("SSN: " + rs.getString("SSN") +
                    ", FirstName: " + rs.getString("FirstName") +
                    ", LastName:"+ rs.getString("LastName") +
                    ", Date of Birth: " + rs.getString("BirthDate"));
            }
        }
    }
}
catch (Exception e)
{
    e.printStackTrace();
}
```

### <a name="avoiding-common-problems-when-querying-encrypted-columns"></a>Éviter les problèmes courants lors de l’interrogation de colonnes chiffrées
Cette section décrit les catégories d’erreurs courantes lors de l’interrogation des colonnes chiffrées à partir des applications Java et quelques instructions sur la façon de les éviter.

### <a name="unsupported-data-type-conversion-errors"></a>Erreurs liées à la conversion de types de données non pris en charge
Always Encrypted ne prend en charge que peu de conversions de types de données chiffrées. Consultez [Always Encrypted (moteur de base de données)](../../relational-databases/security/encryption/always-encrypted-database-engine.md) pour la liste détaillée des conversions de type pris en charge. Voici ce que vous pouvez faire pour éviter les erreurs de conversion de type de données. Assurez-vous que :

- vous utilisez les méthodes d’accesseur set appropriée lors de la transmission de valeurs pour les paramètres qui ciblent des colonnes chiffrées. Assurez-vous que le type de données SQL Server du paramètre est exactement le même que le type de la colonne cible ou une conversion du type de données SQL Server du paramètre de type de cible de la colonne est pris en charge. Méthodes de l’API ont été ajoutées aux classes SQLServerPreparedStatement et SQLServerCallableStatement SQLServerResultSet pour passer des paramètres correspondant aux types de données SQL Server spécifiques. Par exemple, si une colonne n’est pas chiffrée, vous pouvez utiliser la méthode setTimestamp() pour passer un paramètre à un datetime2 ou à une colonne datetime. Mais quand une colonne est chiffrée, vous devez utiliser la méthode exacte représentant le type de la colonne dans la base de données. Par exemple, utilisez setTimestamp() pour passer des valeurs à une colonne chiffrée datetime2 et setDateTime() permet de passer des valeurs à une colonne datetime chiffré. Consultez [toujours chiffré référence des API pour le pilote JDBC](../../connect/jdbc/always-encrypted-api-reference-for-the-jdbc-driver.md) pour une liste complète des nouvelles API.
- La précision et l’échelle des paramètres ciblant les colonnes des types de données SQL Server decimal et numeric sont les mêmes que celles configurées pour la colonne cible. Méthodes de l’API ont été ajoutées aux classes SQLServerPreparedStatement et SQLServerCallableStatement SQLServerResultSet pour accepter la précision et l’échelle en même temps que les valeurs de données pour les paramètres ou des colonnes représentant les types de données decimal et numeric. Consultez [toujours chiffré référence des API pour le pilote JDBC](../../connect/jdbc/always-encrypted-api-reference-for-the-jdbc-driver.md) pour une liste complète des API de nouveau/surchargé.  
- le fractions de seconde/échelle de précision des paramètres ciblant des colonnes de type datetime2, datetimeoffset ou types de données SQL Server n’est pas supérieure à la précision en fractions de seconde / d’échelle pour la colonne cible dans les requêtes qui modifient les valeurs de la colonne cible . Méthodes de l’API ont été ajoutés aux classes SQLServerPreparedStatement et SQLServerCallableStatement SQLServerResultSet pour accepter l’échelle de précision/en fractions de seconde, ainsi que les valeurs de paramètres représentant ces types de données. Pour obtenir une liste complète des API de nouveau/surchargés, consultez [toujours chiffré référence des API pour le pilote JDBC](../../connect/jdbc/always-encrypted-api-reference-for-the-jdbc-driver.md).   

### <a name="errors-due-to-incorrect-connection-properties"></a>Erreurs dues à des propriétés de connexion incorrect
Cette section décrit comment configurer les paramètres de connexion correctement pour utiliser des données Always Encrypted. Étant donné que les types de données chiffrées prennent en charge les conversions limitées, la **sendTimeAsDatetime** et **sendStringParametersAsUnicode** paramètres de connexion nécessitent une configuration appropriée lors de l’utilisation des colonnes chiffrées. Assurez-vous que : 
- [sendTimeAsDatetime](setting-the-connection-properties.md) connexion est défini sur false lorsque vous insérez des données chiffrées des colonnes de temps. Pour plus d’informations, consultez [configurer comment les valeurs java.sql.Time sont envoyées au serveur](configuring-how-java-sql-time-values-are-sent-to-the-server.md).
- [sendStringParametersAsUnicode](setting-the-connection-properties.md) connexion est défini sur true (ou est considérée comme la valeur par défaut) lors de l’insertion des données dans des colonnes de char/varchar/varchar(max) chiffrées.

### <a name="errors-due-to-passing-plaintext-instead-of-encrypted-values"></a>Erreurs dues au passage de texte en clair au lieu de valeurs chiffrées
Les valeurs qui ciblent une colonne chiffrée doivent être chiffrées dans l’application. Une tentative pour l’insertion, de modification ou pour filtrer par une valeur en texte clair sur une colonne chiffrée entraîne une erreur similaire à celui-ci :

```
com.microsoft.sqlserver.jdbc.SQLServerException: Operand type clash: varchar is incompatible with varchar(8000) encrypted with (encryption_type = 'DETERMINISTIC', encryption_algorithm_name = 'AEAD_AES_256_CBC_HMAC_SHA_256', column_encryption_key_name = 'MyCEK', column_encryption_key_database_name = 'ae') collation_name = 'SQL_Latin1_General_CP1_CI_AS'
```

Pour éviter ces erreurs, procédez comme suit :
- Always Encrypted est activé pour les requêtes d’application ciblant des colonnes chiffrées (pour la chaîne de connexion ou d’une requête spécifique).
- vous utilisez des instructions préparées et paramètres à envoyer des données ciblant des colonnes chiffrées. L’exemple suivant montre une requête qui filtre incorrectement par une littéral ou d’une constante sur une colonne chiffrée (SSN), au lieu de passer le littéral à l’intérieur en tant que paramètre. Cette requête échoue :

```
ResultSet rs = connection.createStatement().executeQuery("SELECT * FROM Customers WHERE SSN='795-73-9838'");
```

## <a name="force-encryption-on-input-parameters"></a>Forcer le chiffrement sur les paramètres d’entrée
La fonctionnalité de forcer le chiffrement applique le chiffrement d’un paramètre lors de l’utilisation de Always Encrypted. Si forcer le chiffrement est utilisé et que SQL Server informe le pilote que le paramètre ne doit pas être chiffré, la requête en utilisant le paramètre échoue. Cette propriété fournit une protection supplémentaire contre les attaques au niveau de la sécurité qui impliquent un serveur SQL Server compromis fournissant des métadonnées de chiffrement incorrectes au client, ce qui peut entraîner la divulgation de données. Les méthodes dans les classes SQLServerPreparedStatement et SQLServerCallableStatement et de la mise à jour ensemble *\* méthodes dans la classe SQLServerResultSet sont surchargées afin d’accepter un argument booléen pour spécifier le paramètre de chiffrement de force. Si la valeur de cet argument est false, le pilote ne force pas le chiffrement sur les paramètres. Si forcer le chiffrement est défini à true, la requête paramètre est uniquement envoyé si la colonne de destination est chiffrée et Always Encrypted est activé sur la connexion ou l’instruction. À l’aide de cette propriété fournit une couche supplémentaire de sécurité, d’assurer que le pilote ne pas par erreur envoyer des données vers SQL Server en tant que texte brut lorsqu’elle est censée être chiffrée.

Pour plus d’informations sur les méthodes SQLServerPreparedStatement et SQLServerCallableStatement qui sont surchargées avec le paramètre de chiffrement de force, consultez [toujours chiffré référence des API pour le pilote JDBC](../../connect/jdbc/always-encrypted-api-reference-for-the-jdbc-driver.md)  

## <a name="controlling-the-performance-impact-of-always-encrypted"></a>Contrôle de l’impact sur les performances du chiffrement intégral
Always Encrypted étant une technologie de chiffrement côté client, la majeure partie de la surcharge des performances est observée côté client, pas dans la base de données. Le coût des opérations de chiffrement et le déchiffrement, indépendamment des autres sources de dégradation des performances côté client sont :
- Allers-retours supplémentaires vers la base de données pour récupérer des métadonnées pour les paramètres de requête.
- Appels au magasin de clés principales de colonne pour accéder à une clé principale de colonne.

Cette section décrit les optimisations des performances intégrés dans le pilote JDBC Microsoft pour SQL Server et comment vous pouvez contrôler l’impact de ces deux facteurs sur les performances.

### <a name="controlling-round-trips-to-retrieve-metadata-for-query-parameters"></a>Contrôle des allers-retours vers la base de données en vue de la récupération des métadonnées pour les paramètres de requête
Si Always Encrypted est activé pour une connexion, par défaut, le pilote appelle [sys.sp_describe_parameter_encryption](../../relational-databases/system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md) pour chaque requête paramétrable, en passant l’instruction de requête (sans les valeurs de paramètre) à SQL Server. [Sys.sp_describe_parameter_encryption](../../relational-databases/system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md) analyse l’instruction de requête pour déterminer si tous les paramètres doivent être chiffrés et si tel est le cas, pour chaque celui qu’il retourne les informations relatives au chiffrement qui autorisent le pilote à chiffrer les valeurs de paramètre. Ce comportement garantit un haut niveau de transparence à l’application cliente. Tant que l’application utilise des paramètres pour passer des valeurs qui ciblent des colonnes chiffrées au pilote, l’application (et le développeur d’applications) n’a pas besoin de connaître les requêtes qui accèdent à des colonnes chiffrées.

### <a name="setting-always-encrypted-at-the-query-level"></a>Configuration d’Always Encrypted au niveau de la requête
Pour contrôler l’impact sur les performances de la récupération des métadonnées de chiffrement pour les requêtes paramétrables, vous pouvez activer le chiffrement intégral pour les requêtes au lieu de configurer pour la connexion. Ainsi, vous pouvez vous assurer que sys.sp_describe_parameter_encryption est appelé uniquement pour les requêtes dont les paramètres ciblant des colonnes chiffrées. Toutefois, notez que, ce faisant, vous réduisez la transparence du chiffrement : Si vous modifiez les propriétés de chiffrement de vos colonnes de base de données, vous devrez peut-être modifier le code de votre application pour l’aligner sur les modifications de schéma.

Pour contrôler le comportement d’Always Encrypted des requêtes individuelles, vous devez configurer les objets de l’instruction individuelle en passant un Enum, SQLServerStatementColumnEncryptionSetting, qui spécifie comment les données sont envoyées et reçues lors de la lecture et en écriture colonnes chiffrées pour cette instruction spécifique. Voici quelques conseils utiles :
- Si une application cliente envoie via une connexion de base de données de la plupart des requêtes accèdent à des colonnes chiffrées, utilisez ces instructions :
    - Définir le **columnEncryptionSetting** mot clé de chaîne de connexion à **activé**.
    - Définissez SQLServerStatementColumnEncryptionSetting.Disabled pour les requêtes qui n’accèdent pas à toutes les colonnes chiffrées. Ce paramètre désactive l’appel de sys.sp_describe_parameter_encryption ainsi que sur une tentative pour déchiffrer toutes les valeurs du jeu de résultats.
    - Définissez SQLServerStatementColumnEncryptionSetting.ResultSet pour les requêtes qui n’ont aucun paramètre exigeant un chiffrement mais de récupèrent les données des colonnes chiffrées. Ce paramètre désactive l’appel de sys.sp_describe_parameter_encryption et le chiffrement des paramètres. La requête est alors en mesure de déchiffrer les résultats des colonnes de chiffrement.
- Si la plupart des requêtes, qu'une application cliente envoie via une connexion de base de données n’accèdent pas aux colonnes chiffrées, utilisez ces instructions :
    - Définir le **columnEncryptionSetting** mot clé de chaîne de connexion à **désactivé**.
    - Définissez SQLServerStatementColumnEncryptionSetting.Enabled pour les requêtes qui ont des paramètres qui doivent être chiffrés. Ce paramètre Active l’appel de sys.sp_describe_parameter_encryption ainsi que le déchiffrement des résultats de requête extraites des colonnes chiffrées.
    - Définissez SQLServerStatementColumnEncryptionSetting.ResultSet pour les requêtes qui n’ont aucun paramètre exigeant un chiffrement mais de récupèrent les données des colonnes chiffrées. Ce paramètre désactive l’appel de sys.sp_describe_parameter_encryption et le chiffrement des paramètres. La requête est alors en mesure de déchiffrer les résultats des colonnes de chiffrement.

Les paramètres SQLServerStatementColumnEncryptionSetting ne peut pas être utilisés pour contourner le chiffrement et accéder aux données en texte brut. Pour plus d’informations sur la façon de configurer le chiffrement de colonne dans une instruction, consultez [toujours chiffré référence des API pour le pilote JDBC](../../connect/jdbc/always-encrypted-api-reference-for-the-jdbc-driver.md).  

Dans l’exemple suivant, le chiffrement intégral est désactivé pour la connexion de base de données. La requête sur les problèmes de l’application a un paramètre qui cible la colonne LastName qui n’est pas chiffrée. La requête récupère les données des colonnes SSN et BirthDate qui sont toutes deux chiffrées. Dans ce cas, il n’est pas nécessaire d’appeler sys.sp_describe_parameter_encryption pour récupérer les métadonnées de chiffrement. Toutefois, le déchiffrement des résultats de requête doit être activée afin que l’application puisse recevoir des valeurs en texte clair à partir de deux colonnes chiffrées. Le paramètre SQLServerStatementColumnEncryptionSetting.ResultSet est utilisé pour garantir que.

```
// Assumes the same table definition as in Section "Retrieving and modifying data in encrypted columns"
// where only SSN and BirthDate columns are encrypted in the database.
String connectionUrl = "jdbc:sqlserver://localhost;databaseName=ae;user=sa;password=******;"
        + "keyStoreAuthentication=JavaKeyStorePassword;"
        + "keyStoreLocation=" + keyStoreLocation + ";"
        + "keyStoreSecret=******;";
SQLServerConnection connection = (SQLServerConnection) DriverManager.getConnection(connectionString);

String filterRecord="SELECT FirstName, LastName, SSN, BirthDate FROM " + tblName + " WHERE LastName = ?";
PreparedStatement selectStatement = connection.prepareStatement(
        filterRecord,
        ResultSet.TYPE_FORWARD_ONLY,
        ResultSet.CONCUR_READ_ONLY,
        connection.getHoldability(),
        SQLServerStatementColumnEncryptionSetting.ResultSetOnly);
selectStatement.setString(1, "Abel");
ResultSet rs = selectStatement.executeQuery();
while(rs.next()) {
    System.out.println("First name: " + rs.getString("FirstName"));
    System.out.println("Last name: " + rs.getString("LastName"));
    System.out.println("SSN: " + rs.getString("SSN"));
    System.out.println("Date of Birth: " + rs.getDate("BirthDate"));
}
rs.close();
selectStatement.close();
connection.close();
```

### <a name="column-encryption-key-caching"></a>Mise en cache des clés de chiffrement de colonne
Pour réduire le nombre d’appels à un magasin de clés principales de colonne pour déchiffrer les clés de chiffrement de colonne, le pilote JDBC Microsoft pour SQL Server met en cache les clés de chiffrement de colonne de texte en clair dans la mémoire. Après avoir reçu la valeur de clé de chiffrement de colonne chiffrée à partir des métadonnées de base de données, le pilote tente d’abord de trouver la clé de chiffrement de colonne de texte en clair correspondant à la valeur de clé chiffrée. Le pilote appelle le magasin de clés contenant la clé principale de colonne uniquement si elle ne peut pas trouver la valeur de clé de chiffrement de colonne chiffrée dans le cache.

Vous pouvez configurer une valeur de durée de vie pour les entrées de clé de chiffrement de colonne dans le cache à l’aide de l’API, setColumnEncryptionKeyCacheTtl(), dans la classe SQLServerConnection. La valeur de durée de vie par défaut pour les entrées de clé de chiffrement de colonne dans le cache est de deux heures. Pour désactiver la mise en cache, utilisez la valeur 0. Pour définir une valeur de durée de vie, utilisez l’API suivante :

```
SQLServerConnection.setColumnEncryptionKeyCacheTtl (int columnEncryptionKeyCacheTTL, TimeUnit unit)
```

Par exemple, pour définir une valeur de durée de vie de 10 minutes, utilisez :

```
SQLServerConnection.setColumnEncryptionKeyCacheTtl (10, TimeUnit.MINUTES)
```

Seuls les jours, heures, MINUTES ou secondes sont pris en charge en tant que l’unité de temps.  

## <a name="copying-encrypted-data-using-sqlserverbulkcopy"></a>Copie les données chiffrées à l’aide de SQLServerBulkCopy
Avec SQLServerBulkCopy, vous pouvez copier les données qui sont déjà chiffrées et stockées dans une table à une autre table, sans déchiffrage des données. Pour cela :

- Vérifiez que la configuration du chiffrement de la table cible est identique à celle de la table source. En particulier, les deux tables doivent avoir les mêmes colonnes chiffrées et les colonnes doivent être chiffrées à l’aide des mêmes types de chiffrement et les mêmes clés de chiffrement. Si n’importe quelle colonne cible est chiffrée différemment de sa colonne source correspondante, vous ne serez pas en mesure de déchiffrer les données dans la table cible après l’opération de copie. Les données seront endommagées.
- Configurer les connexions de base de données pour la table source et la table cible sans activer Always Encrypted.
- Définissez l’option allowEncryptedValueModifications. Pour plus d’informations, consultez [à l’aide de copie en bloc avec le pilote JDBC](../../connect/jdbc/using-bulk-copy-with-the-jdbc-driver.md).

> [!NOTE]
> Soyez prudent lorsque vous spécifiez AllowEncryptedValueModifications, car cette option peut endommager la base de données, car le pilote JDBC Microsoft pour SQL Server ne vérifie pas si les données sont réellement chiffrées ou si elle est correctement chiffrée à l’aide de la même chiffrement type, algorithme et clé que la colonne cible.

## <a name="see-also"></a>Voir aussi
[Always Encrypted (moteur de base de données)](../../relational-databases/security/encryption/always-encrypted-database-engine.md)
