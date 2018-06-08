---
title: Établissement de connexions sécurisées dans ADOMD.NET | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: adomd
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 0d9151dd1072f165010eeed7ae065ea62f36296e
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="connections-in-adomdnet---establishing-secure-connections"></a>Connexions dans ADOMD.NET - établissement de connexions sécurisées
  Lorsque vous utilisez une connexion dans ADOMD.NET, la méthode de sécurité qui est utilisée pour la connexion dépend de la valeur de la **ProtectionLevel** propriété de chaîne de connexion utilisée lorsque vous appelez le <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection.Open%2A> méthode de la <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection>.  
  
 Le **ProtectionLevel** propriété offre quatre niveaux de sécurité : non authentifié, authentifié, signé et chiffré. Le tableau suivant décrit ces différents niveaux de sécurité.  
  
> [!NOTE]  
>  Si vous choisissez d'utiliser le regroupement de connexions de base de données, la base de données ne sera pas en mesure de gérer la sécurité. La raison en est que le regroupement de connexions de base de données exige que la chaîne de connexion soit identique aux connexions du pool. Par conséquent, vous devez gérer la sécurité ailleurs.  
  
|Niveau de sécurité|Valeur de ProtectionLevel|  
|--------------------|---------------------------|  
|*connexion non authentifiée*<br /> Une connexion non authentifiée n'assure aucune forme d'authentification. Ce type de connexion est le plus largement pris en charge, mais également le moins sûr.|**Aucun**|  
|*connexion authentifiée*<br /> Une connexion authentifiée authentifie l'utilisateur qui établit la connexion, mais elle ne sécurise pas les autres communications. Ce type de connexion s'avère utile dans la mesure où vous pouvez établir l'identité de l'utilisateur ou de l'application qui se connecte à une source de données analytiques.|**Se connecter**|  
|*connexion signée*<br /> Une connexion signée authentifie l'utilisateur qui demande la connexion et vérifie que les transmissions ne sont pas modifiées. Ce type de connexion est utile lorsque l'authenticité des données transférées doit être vérifiée. Toutefois, une connexion signée ne fait qu'empêcher la modification du contenu du paquet de données. Le contenu peut toujours être consulté en transit.<br /><br /><br /><br /> Notez qu’une connexion signée est uniquement pris en charge par le fournisseur XML for Analysis fourni par [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].|**L’intégrité de PKT** ou **PktIntegrity**|  
|*connexion chiffrée*<br /> Une connexion chiffrée est le type de connexion utilisé par défaut par ADOMD.NET. Ce type de connexion authentifie l'utilisateur qui demande la connexion et chiffre également les données transmises. La connexion chiffrée est le type de connexion le plus sûr qui puisse être créé par ADOMD.NET. Le contenu du paquet de données ne pouvant ni être consulté ni modifié, les données sont protégées pendant le transit.<br /><br /><br /><br /> Une connexion chiffrée est uniquement pris en charge par le fournisseur XML for Analysis fourni par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].|**PKT Privacy** ou **PktPrivacy**|  
  
 Toutefois, les niveaux de sécurité disponibles varient en fonction du type de connexion :  
  
-   Une connexion TCP peut utiliser n'importe lequel des quatre niveaux de sécurité. En fait, quand elle est utilisée conjointement avec la sécurité intégrée Windows, la connexion TCP constitue la méthode la plus sûre de connexion à une source de données analytiques.  
  
-   Une connexion HTTP ne peut être qu'une connexion authentifiée. Par conséquent, le **ProtectionLevel** propriété doit être définie sur **connexion**.  
  
-   Une connexion HTTPS ne peut être qu'une connexion chiffrée. Par conséquent, le **ProtectionLevel** propriété doit être définie sur **Pkt confidentialité** ou **PktPrivacy**.  
  
## <a name="securing-tcp-connections"></a>Sécurisation des connexions TCP  
 Pour une connexion TCP, la **ProtectionLevel** propriété prend en charge les quatre niveaux de sécurité, comme indiqué dans le tableau suivant.  
  
|Valeur de ProtectionLevel|À utiliser avec une connexion TCP ?|Résultats|  
|---------------------------|------------------------------|-------------|  
|**Aucun**|Oui|Indique une connexion non authentifiée.<br /><br /> Bien qu'un flux TCP soit demandé par le fournisseur, aucune forme d'authentification n'est effectuée au niveau de l'utilisateur qui demande le flux.|  
|**Se connecter**|Oui|Indique une connexion authentifiée.<br /><br /> Un flux TCP est demandé par le fournisseur, puis le contexte de sécurité de l’utilisateur qui demande le flux est authentifié sur le serveur : si l’authentification réussit, aucune autre action n’est effectuée. Si l'authentification échoue, l'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection> se déconnecte de la source de données multidimensionnelles et une exception est levée.<br /><br /> Le contexte de sécurité utilisé pour authentifier la connexion est supprimé de suite après l'authentification, qu'elle ait réussi ou échoué.|  
|**L’intégrité de PKT** ou **PktIntegrity**|Oui|Indique une connexion signée.<br /><br /> Un flux TCP est demandé par le fournisseur et le contexte de sécurité de l'utilisateur qui demande le flux est authentifié sur le serveur :<br /><br /> <br /><br /> Si l'authentification réussit, l'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection> ferme le flux TCP existant et ouvre un flux TCP signé pour gérer toutes les demandes. Chaque demande de données ou de métadonnées est authentifiée en utilisant le contexte de sécurité qui a servi à ouvrir la connexion. En outre, chaque paquet est signé numériquement pour s'assurer que la charge utile du paquet TCP n'a pas été modifiée de quelque manière que ce soit.<br /><br /> <br /><br /> Si l'authentification échoue, l'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection> se déconnecte de la source de données multidimensionnelles et une exception est levée.|  
|**PKT Privacy** ou **PktPrivacy**|Oui|Indique une connexion chiffrée.<br /><br /> <br /><br /> Notez que vous pouvez également spécifier une connexion chiffrée en ne définissant ne pas la **ProtectionLevel** propriété dans la chaîne de connexion.<br /><br /> <br /><br /> Un flux TCP est demandé par le fournisseur et le contexte de sécurité de l'utilisateur demandant le flux est authentifié sur le serveur :<br /><br /> <br /><br /> Si l'authentification réussit, l'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection> ferme le flux TCP existant et ouvre un flux TCP chiffré pour gérer toutes les demandes. Chaque demande de données ou de métadonnées est authentifiée en utilisant le contexte de sécurité qui a servi à ouvrir la connexion. En outre, la charge utile de chaque paquet TCP est chiffrée selon la méthode de chiffrement la plus élevée prise en charge à la fois par le fournisseur et par la source de données multidimensionnelles.<br /><br /> <br /><br /> Si l'authentification échoue, l'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection> se déconnecte de la source de données multidimensionnelles et une exception est levée.|  
  
### <a name="using-windows-integrated-security-for-the-connection"></a>Utilisation de la sécurité intégrée Windows pour la connexion  
 La sécurité intégrée Windows est le moyen le plus sûr d'établir et de sécuriser une connexion à une instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. La sécurité intégrée Windows ne révèle pas les informations d'identification de sécurité (p.ex., le nom ou le mot de passe d'un utilisateur) au cours du processus d'authentification. À la place, elle utilise l'identificateur de sécurité du processus en cours d'exécution pour établir l'identité. Pour la plupart des applications clientes, cet identificateur de sécurité représente l'identité de l'utilisateur connecté.  
  
 Pour utiliser la sécurité intégrée Windows, les paramètres suivants sont nécessaires dans la chaîne de connexion :  
  
-   Pour le **sécurité intégrée** propriété, ne pas définir cette propriété ou définissez cette propriété sur **SSPI**.  
  
    > [!NOTE]  
    >  Sécurité intégrée de Windows est disponible uniquement pour les connexions TCP car les connexions HTTP doivent utiliser le **base** définition pour le **sécurité intégrée** propriété.  
  
-   Pour le **ProtectionLevel** propriété, définissez cette propriété sur **Connect**, **Pkt intégrité**, ou **Pkt confidentialité**.  
  
## <a name="securing-http-connections"></a>Sécurisation des connexions HTTP  
 HTTPS et SSL (Secure Sockets Layer) peuvent être utilisés pour sécuriser de façon externe les communications HTTP avec une source de données analytiques.  
  
 Parce qu'un fournisseur XMLA utilise uniquement le protocole HTTP sécurisé, une connexion HTTP dans ADOMD.NET doit être une connexion signée, comme l'illustre le tableau suivant.  
  
|Valeur de ProtectionLevel|Utilisation avec HTTP ou HTTPS|  
|---------------------------|----------------------------|  
|**Aucun**|non|  
|**Se connecter**|HTTP|  
|**L’intégrité de PKT** ou **PktIntegrity**|non|  
|**PKT Privacy** ou **PktPrivacy**|HTTPS|  
  
### <a name="opening-a-secure-http-connection"></a>Ouverture d'une connexion HTTP sécurisée  
 L’exemple suivant montre comment utiliser ADOMD.NET pour ouvrir une connexion HTTP pour le **AdventureWorksAS** exemple [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] base de données :  
  
```vb  
Public Function GetAWEncryptedConnection( _  
    Optional ByVal serverName As String = "https:\\localhost\isapy\msmdpump.dll") _  
    As AdomdConnection  
  
    Dim strConnectionString As String = ""  
    Dim objConnection As New AdomdConnection  
  
    Try  
        ' To establish an encrypted connection, set the   
        ' ProtectionLevel setting to PktPrivacy.  
        strConnectionString = "DataSource=" & serverName & ";" & _  
            "Catalog=AdventureWorksAS;" & _  
            "ProtectionLevel=PktPrivacy;"  
  
        ' Note that username and password are not supplied here.  
        ' The current security context is used for authentication  
        ' purposes.  
  
        objConnection.ConnectionString = strConnectionString  
        objConnection.Open()  
    Catch ex As Exception  
        objConnection = Nothing  
        Throw ex  
    Finally  
        ' Return the encrypted connection.  
        GetAWEncryptedConnection = objConnection  
    End Try  
End Function  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Établissement de connexions dans ADOMD.NET](../../analysis-services/multidimensional-models-adomd-net-client/connections-in-adomd-net.md)  
  
  
