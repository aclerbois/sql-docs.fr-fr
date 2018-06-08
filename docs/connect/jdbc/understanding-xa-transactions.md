---
title: Présentation des Transactions XA | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 574e326f-0520-4003-bdf1-62d92c3db457
caps.latest.revision: 80
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: a78fdb7edae90289d64d4c7fdf74ac3a12d4b115
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="understanding-xa-transactions"></a>Présentation des Transactions XA
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] fournit la prise en charge de Java Platform, Enterprise Edition/JDBC 2.0 des transactions distribuées facultatives. Les connexions JDBC obtenues à partir de la [SQLServerXADataSource](../../connect/jdbc/reference/sqlserverxadatasource-class.md) classe peut participer aux environnements tels que les serveurs Java Platform, Enterprise Edition (Java EE) application de traitement des transactions distribuées standard.  
  
> [!WARNING]  
>  Le pilote Microsoft JDBC Driver 4.2 (et versions ultérieures) pour SQL inclut de nouvelles options de délai d’attente pour la fonctionnalité existante pour la restauration automatique des transactions non préparées. Consultez [configuration des paramètres de délai d’attente côté serveur pour la restauration automatique des transactions non préparées](../../connect/jdbc/understanding-xa-transactions.md#BKMK_ServerSide) plus loin dans cette rubrique pour plus de détails.  
  
## <a name="remarks"></a>Notes  
 Les classes pour l'implémentation des transactions distribuées sont les suivantes :  
  
|Classe|Implémentations| Description|  
|-----------|----------------|-----------------|  
|com.microsoft.sqlserver.jdbc.SQLServerXADataSource|javax.sql.XADataSource|Fabrique de classe pour les connexions distribuées.|  
|com.microsoft.sqlserver.jdbc.SQLServerXAResource|javax.transaction.xa.XAResource|Adaptateur de ressources pour le gestionnaire de transactions.|  
  
> [!NOTE]  
>  Les connexions de transactions distribuées XA sont définies par défaut au niveau d'isolation Read Committed.  
  
## <a name="guidelines-and-limitations-when-using-xa-transactions"></a>Recommandations et limitations relatives à l'utilisation de transactions XA  
 Les recommandations supplémentaires suivantes s'appliquent aux transactions fortement couplées :  
  
-   Lorsque vous utilisez des transactions XA avec MS DTC (Microsoft Distributed Transaction Coordinator), vous pouvez remarquer que la version actuelle de MS DTC ne prend pas en charge le comportement de branche XA fortement couplée. Par exemple, MS DTC a un mappage un-à-un entre un ID de transaction de branche XA (XID) et un ID de transaction MS DTC et le travail effectué par les branches XA couplées de manière lâche est isolé.  
  
     Le correctif logiciel disponible sur [MSDTC and Tightly Coupled Transactions](http://support.microsoft.com/kb/938653) permet la prise en charge pour les branches XA étroitement couplées lorsque plusieurs branches XA avec transaction globale même ID (GTRID) sont mappées à un ID de transaction MS DTC unique. Cette prise en charge permet à plusieurs branches XA étroitement couplées voir les modifications par un autre dans le Gestionnaire de ressources, telles que [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
-   A [SSTRANSTIGHTLYCPLD](../../connect/jdbc/reference/sstranstightlycpld-field-sqlserverxaresource.md) indicateur permet aux applications d’utiliser les transactions XA étroitement couplées, qui ont des transaction de branche XA ID (bqual) différents mais le même ID (GTRID) de transaction global et ID de format (FormatID). Pour pouvoir utiliser cette fonctionnalité, vous devez définir le [SSTRANSTIGHTLYCPLD](../../connect/jdbc/reference/sstranstightlycpld-field-sqlserverxaresource.md) sur le paramètre d’indicateurs de la méthode XAResource.start :  
  
    ```  
    xaRes.start(xid, SQLServerXAResource.SSTRANSTIGHTLYCPLD);  
    ```  
  
## <a name="configuration-instructions"></a>Instructions relatives à la configuration  
 Les étapes suivantes sont requises si vous souhaitez utiliser des sources de données XA avec Microsoft Distributed Transaction Coordinator (MS DTC) pour manipuler des transactions distribuées.  
  
> [!NOTE]  
>  Les composants de transaction distribuée JDBC sont inclus dans le répertoire xa de l'installation du pilote JDBC. Ces composants incluent les fichiers xa_install.sql et sqljdbc_xa.dll.  
  
### <a name="running-the-ms-dtc-service"></a>Exécution du service MS DTC  
 Le service MS DTC doit être marquée comme **automatique** dans le Gestionnaire de Service pour vous assurer qu’il s’exécute lorsque le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] service est démarré. Pour activer MS DTC pour les transactions XA, vous devez procéder comme suit :  
  
 Sur Windows Vista et versions ultérieures :  
  
1.  Cliquez sur le **Démarrer** bouton, tapez **dcomcnfg** dans le démarrage **recherche** zone, puis appuyez sur ENTRÉE pour ouvrir **Services de composants**. Vous pouvez également taper %windir%\system32\comexp.msc dans la **rechercher** boîte pour ouvrir **Services de composants**.  
  
2.  Développez Services de composants, Ordinateurs, Poste de travail, puis Distributed Transaction Coordinator.  
  
3.  Avec le bouton droit **DTC Local** , puis sélectionnez **propriétés**.  
  
4.  Cliquez sur le **sécurité** onglet sur le **propriétés DTC Local** boîte de dialogue.  
  
5.  Sélectionnez le **activer les Transactions XA** case à cocher, puis cliquez sur **OK**. Ceci entraînera le redémarrage du service MS DTC.  
  
6.  Cliquez sur **OK** à nouveau pour fermer la **propriétés** boîte de dialogue et fermez **Services de composants**.  
  
7.  Arrêtez et redémarrez [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] pour vous assurer que sa synchronisation avec les modifications de MS DTC.  
  
### <a name="configuring-the-jdbc-distributed-transaction-components"></a>Configuration des composants de transaction distribuée JDBC  
 Vous pouvez configurer les composants de transaction distribuée du pilote JDBC en suivant les étapes suivantes :  
  
1.  Copiez le nouveau fichier sqljdbc_xa.dll à partir du répertoire d’installation du pilote JDBC vers le répertoire Binn de chaque [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ordinateur qui doit participer dans des transactions distribuées.  
  
    > [!NOTE]  
    >  Si vous utilisez des transactions XA avec une version 32 bits [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], utilisez le fichier sqljdbc_xa.dll dans le x86 dossier, même si le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] est installé sur un x64 processeur. Si vous utilisez des transactions XA avec une version 64 bits [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] sur la x64 processeur, utilisez le fichier sqljdbc_xa.dll dans le x64 dossier.  
  
2.  Exécutez le script de base de données xa_install.sql sur chaque [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] instance qui est incluses dans les transactions distribuées. Ce script installe les procédures stockées étendues qui sont appelées par sqljdbc_xa.dll. Ces procédures stockées étendues implémentent la transaction distribuée et XA prend en charge pour le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)]. Vous devez exécuter ce script en tant qu’administrateur de le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] instance.  
  
3.  Pour autoriser un utilisateur spécifique à participer à des transactions distribuées avec le pilote JDBC, ajoutez-le au rôle SqlJDBCXAUser.  
  
 Vous pouvez configurer qu’une seule version de l’assembly sqljdbc_xa.dll sur chaque [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] instance à la fois. Les applications devront peut-être utiliser des versions différentes du pilote JDBC pour se connecter à la même [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] instance à l’aide de la connexion XA. Dans ce cas, sqljdbc_xa.dll, qui est livré avec le pilote JDBC le plus récent, doit être installé sur le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] instance.  
  
 Il existe trois façons de vérifier la version de sqljdbc_xa.dll actuellement installée sur le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] instance :  
  
1.  Ouvrez le répertoire des journaux de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ordinateur qui doit participer dans des transactions distribuées. Sélectionnez et ouvrez le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] fichier « ERRORLOG ». Recherchez l'expression « Using 'SQLJDBC_XA.dll' version ... » dans le fichier ERRORLOG.  
  
2.  Ouvrez le répertoire Binn de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ordinateur qui doit participer dans des transactions distribuées. Sélectionnez l’assembly sqljdbc_xa.dll.  
  
    -   Sur Windows Vista et versions ultérieures : cliquez avec le bouton droit sur sqljdbc_xa.dll, puis sélectionnez Propriétés. Puis cliquez sur le **détails** onglet. Le **Version du fichier** champ indique la version de sqljdbc_xa.dll actuellement installée sur le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] instance.  
  
3.  Définissez la fonctionnalité de journalisation comme indiqué dans l'exemple de code de la prochaine section. Recherchez l'expression « Server XA DLL version:... » dans le fichier journal de sortie.  
  
###  <a name="BKMK_ServerSide"></a> Configuration des paramètres de délai d’attente côté serveur pour la restauration automatique des transactions non préparées  
  
> [!WARNING]  
>  Cette option côté serveur est nouvelle avec Microsoft JDBC Driver 4.2 (et versions ultérieures) pour SQL Server. Pour obtenir le comportement mis à jour, vérifiez que le fichier sqljdbc_xa.dll est mis à jour. Pour plus d’informations sur la définition de délais d’attente du côté client, consultez [XAResource.settransactiontimeout ()](http://docs.oracle.com/javase/8/docs/api/javax/transaction/xa/XAResource.html).  
  
 Il existe deux paramètres du Registre (valeurs DWORD) pour contrôler le comportement du délai d'attente des transactions distribuées :  
  
-   **XADefaultTimeout** (en secondes) : la valeur de délai d’attente par défaut à utiliser lors de l’utilisateur ne spécifie pas un délai d’attente. La valeur par défaut est 0.  
  
-   **XAMaxTimeout** (en secondes) : la valeur maximale du délai d’attente qu’un utilisateur peut définir. La valeur par défaut est 0.  
  
 Ces paramètres sont spécifiques de l'instance SQL Server et doivent être créés sous la clé de Registre suivante :  
  
 HKEY_LOCAL_MACHINE\Software\Microsoft\Microsoft SQL Server\MSSQL\<**version**>.\< *nom_instance*> \XATimeout  
  
> [!NOTE]  
>  Pour SQL Server 32 bits s’exécutant sur les ordinateurs 64 bits, les paramètres de Registre doivent être créés sous la clé suivante : HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Microsoft SQL Server\MSSQL\<version >. < nom_instance > \ XATimeout  
  
 Une valeur de délai d'attente est définie au démarrage de chaque transaction, et la transaction est restaurée par le serveur SQL Server si le délai d'attente expire. Le délai d'attente est déterminé en fonction de ces paramètres de Registre et de ce que l'utilisateur a spécifié via XAResource.setTransactionTimeout(). Voici quelques exemples d'interprétation des valeurs de délai d'attente :  
  
-   XADefaultTimeout = 0, XAMaxTimeout = 0  
  
     Signifie qu'aucun délai d'attente par défaut ne sera utilisé et qu'aucun délai d'attente maximal ne sera appliqué sur les clients. Dans ce cas, les transactions auront un délai d'attente uniquement si le client en définit un à l'aide de XAResource.setTransactionTimeout.  
  
-   XADefaultTimeout = 60, XAMaxTimeout = 0  
  
     Signifie que toutes les transactions auront un délai d'attente de 60 secondes si le client n'en spécifie aucun. Si le client spécifie un délai d'attente, cette valeur sera alors utilisée. Aucune valeur de délai d'attente maximale n'est appliquée.  
  
-   XADefaultTimeout = 30, XAMaxTimeout = 60  
  
     Signifie que toutes les transactions auront un délai d'attente de 30 secondes si le client n'en spécifie aucun. Si le client spécifie un délai d'attente, il sera utilisé s'il est inférieur à 60 secondes (valeur maximale).  
  
-   XADefaultTimeout = 0, XAMaxTimeout = 30  
  
     Signifie que toutes les transactions auront un délai d'attente de 30 secondes (valeur maximale) si le client n'en spécifie aucun. Si le client spécifie un délai d'attente, il sera utilisé s'il est inférieur à 30 secondes (valeur maximale).  
  
### <a name="upgrading-sqljdbcxadll"></a>Mise à niveau du fichier sqljdbc_xa.dll  
 Lorsque vous installez une nouvelle version du pilote JDBC, vous devez utiliser le fichier sqljdbc_xa.dll de la nouvelle version pour mettre à niveau le fichier sqljdbc_xa.dll sur le serveur.  
  
> [!IMPORTANT]  
>  Vous devez mettre à niveau le fichier sqljdbc_xa.dll dans une fenêtre de maintenance ou lorsqu'aucune transaction MS DTC n'est en cours.  
  
1.  Décharger le fichier sqljdbc_xa.dll à l’aide de la [!INCLUDE[tsql](../../includes/tsql_md.md)] commande **DBCC sqljdbc_xa (FREE)**.  
  
2.  Copiez le nouveau fichier sqljdbc_xa.dll à partir du répertoire d’installation du pilote JDBC vers le répertoire Binn de chaque [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ordinateur qui doit participer dans des transactions distribuées.  
  
     La nouvelle DLL sera chargée à l'appel d'une procédure étendue dans sqljdbc_xa.dll. Vous n’avez pas besoin de redémarrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] pour charger les nouvelles définitions.  
  
### <a name="configuring-the-user-defined-roles"></a>Configuration des rôles définis par l'utilisateur  
 Pour autoriser un utilisateur spécifique à participer à des transactions distribuées avec le pilote JDBC, ajoutez-le au rôle SqlJDBCXAUser. Par exemple, utilisez ce qui suit [!INCLUDE[tsql](../../includes/tsql_md.md)] code pour ajouter un utilisateur nommé « shelby » (un utilisateur de connexion standard SQL nommé « shelby ») au rôle SqlJDBCXAUser :  
  
```  
USE master  
GO  
EXEC sp_grantdbaccess 'shelby', 'shelby'  
GO  
EXEC sp_addrolemember [SqlJDBCXAUser], 'shelby'  
```  
  
 Les rôles définis par l'utilisateur SQL sont définis par base de données. Pour des raisons de sécurité, si vous souhaitez créer votre propre rôle, vous devez définir le rôle dans chaque base de données et ajouter les utilisateurs par base de données. Le rôle SqlJDBCXAUser est strictement défini dans la base de données master, car il est utilisé pour accorder l'accès aux procédures stockées étendues SQL JDBC se trouvant dans la base de données master. Vous devrez d'abord accorder un accès à la base de données master à l'utilisateur individuel, puis lui accorder un accès au rôle SqlJDBCXAUser en étant connecté à la base de données master.  
  
## <a name="example"></a>Exemple  
  
```  
import java.net.Inet4Address;  
import java.sql.*;  
import java.util.Random;  
import javax.transaction.xa.*;  
import javax.sql.*;  
import com.microsoft.sqlserver.jdbc.*;  
  
public class testXA {  
  
   public static void main(String[] args) throws Exception {  
  
      // Create variables for the connection string.  
      String prefix = "jdbc:sqlserver://";  
      String serverName = "localhost";  
      int portNumber = 1433;  
      String databaseName = "AdventureWorks";   
      String user = "UserName";   
      String password = "*****";  
      String connectionUrl = prefix + serverName + ":" + portNumber  
         + ";databaseName=" + databaseName + ";user=" + user + ";password=" + password;  
  
      try {  
         // Establish the connection.  
         Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");  
         Connection con = DriverManager.getConnection(connectionUrl);  
  
         // Create a test table.  
         Statement stmt = con.createStatement();  
         try {  
            stmt.executeUpdate("DROP TABLE XAMin");   
         }  
         catch (Exception e) {  
         }  
         stmt.executeUpdate("CREATE TABLE XAMin (f1 int, f2 varchar(max))");  
         stmt.close();  
         con.close();  
  
         // Create the XA data source and XA ready connection.  
         SQLServerXADataSource ds = new SQLServerXADataSource();  
         ds.setUser(user);  
         ds.setPassword(password);  
         ds.setServerName(serverName);  
         ds.setPortNumber(portNumber);  
         ds.setDatabaseName(databaseName);  
         XAConnection xaCon = ds.getXAConnection();  
         con = xaCon.getConnection();  
  
         // Get a unique Xid object for testing.  
         XAResource xaRes = null;  
         Xid xid = null;  
         xid = XidImpl.getUniqueXid(1);  
  
         // Get the XAResource object and set the timeout value.  
         xaRes = xaCon.getXAResource();  
         xaRes.setTransactionTimeout(0);  
  
         // Perform the XA transaction.  
         System.out.println("Write -> xid = " + xid.toString());  
         xaRes.start(xid,XAResource.TMNOFLAGS);  
         PreparedStatement pstmt =   
         con.prepareStatement("INSERT INTO XAMin (f1,f2) VALUES (?, ?)");  
         pstmt.setInt(1,1);  
         pstmt.setString(2,xid.toString());  
         pstmt.executeUpdate();  
  
         // Commit the transaction.  
         xaRes.end(xid,XAResource.TMSUCCESS);  
         xaRes.commit(xid,true);  
  
         // Cleanup.  
         con.close();  
         xaCon.close();  
  
         // Open a new connection and read back the record to verify that it worked.  
         con = DriverManager.getConnection(connectionUrl);  
         ResultSet rs = con.createStatement().executeQuery("SELECT * FROM XAMin");  
         rs.next();  
         System.out.println("Read -> xid = " + rs.getString(2));  
         rs.close();  
         con.close();  
      }   
  
      // Handle any errors that may have occurred.  
      catch (Exception e) {  
         e.printStackTrace();  
      }  
   }  
}  
  
class XidImpl implements Xid {  
  
   public int formatId;  
   public byte[] gtrid;  
   public byte[] bqual;  
   public byte[] getGlobalTransactionId() {return gtrid;}  
   public byte[] getBranchQualifier() {return bqual;}  
   public int getFormatId() {return formatId;}  
  
   XidImpl(int formatId, byte[] gtrid, byte[] bqual) {  
      this.formatId = formatId;  
      this.gtrid = gtrid;  
      this.bqual = bqual;  
   }  
  
   public String toString() {  
      int hexVal;  
      StringBuffer sb = new StringBuffer(512);  
      sb.append("formatId=" + formatId);  
      sb.append(" gtrid(" + gtrid.length + ")={0x");  
      for (int i=0; i<gtrid.length; i++) {  
         hexVal = gtrid[i]&0xFF;  
         if ( hexVal < 0x10 )  
            sb.append("0" + Integer.toHexString(gtrid[i]&0xFF));  
         else  
            sb.append(Integer.toHexString(gtrid[i]&0xFF));  
         }  
         sb.append("} bqual(" + bqual.length + ")={0x");  
         for (int i=0; i<bqual.length; i++) {  
            hexVal = bqual[i]&0xFF;  
            if ( hexVal < 0x10 )  
               sb.append("0" + Integer.toHexString(bqual[i]&0xFF));  
            else  
               sb.append(Integer.toHexString(bqual[i]&0xFF));  
         }  
         sb.append("}");  
         return sb.toString();  
      }  
  
      // Returns a globally unique transaction id.  
      static byte [] localIP = null;  
      static int txnUniqueID = 0;  
      static Xid getUniqueXid(int tid) {  
  
      Random rnd = new Random(System.currentTimeMillis());  
      txnUniqueID++;  
      int txnUID = txnUniqueID;  
      int tidID = tid;  
      int randID = rnd.nextInt();  
      byte[] gtrid = new byte[64];  
      byte[] bqual = new byte[64];  
      if ( null == localIP) {  
         try {  
            localIP = Inet4Address.getLocalHost().getAddress();  
         }  
         catch ( Exception ex ) {  
            localIP =  new byte[] { 0x01,0x02,0x03,0x04 };  
         }  
      }  
      System.arraycopy(localIP,0,gtrid,0,4);  
      System.arraycopy(localIP,0,bqual,0,4);  
  
      // Bytes 4 -> 7 - unique transaction id.  
      // Bytes 8 ->11 - thread id.  
      // Bytes 12->15 - random number generated by using seed from current time in milliseconds.  
      for (int i=0; i<=3; i++) {  
         gtrid[i+4] = (byte)(txnUID%0x100);  
         bqual[i+4] = (byte)(txnUID%0x100);  
         txnUID >>= 8;  
         gtrid[i+8] = (byte)(tidID%0x100);  
         bqual[i+8] = (byte)(tidID%0x100);  
         tidID >>= 8;  
         gtrid[i+12] = (byte)(randID%0x100);  
         bqual[i+12] = (byte)(randID%0x100);  
         randID >>= 8;  
      }  
      return new XidImpl(0x1234, gtrid, bqual);  
   }  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Réalisation de transactions avec le pilote JDBC](../../connect/jdbc/performing-transactions-with-the-jdbc-driver.md)  
  
  
