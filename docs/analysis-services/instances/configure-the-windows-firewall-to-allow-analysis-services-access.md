---
title: Configurer le pare-feu Windows pour autoriser l’accès à Analysis Services | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 5145bda99e7c5518c3904e51485c4053d33a4a58
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="configure-the-windows-firewall-to-allow-analysis-services-access"></a>Configurer le pare-feu Windows pour autoriser l'accès à Analysis Services
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Une première étape essentielle pour mettre [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ou [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] à disposition sur le réseau consiste à déterminer si vous devez débloquer des ports dans un pare-feu. La plupart des installations nécessitent la création d'au moins une règle de trafic entrant dans le pare-feu qui autorise les connexions à [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
 Les conditions de configuration du pare-feu varient selon le mode d'installation de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]:  
  
-   Ouvrez le port TCP 2383 lors de l'installation d'une instance par défaut ou de la création d'un cluster de basculement [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .  
  
-   Ouvrez le port TCP 2382 lors de l'installation d'une instance nommée. Les instances nommées utilisent les affectations de ports dynamiques. En tant que service de découverte pour Analysis Services, le service SQL Server Browser écoute le port TCP 2382 et redirige la demande de connexion vers le port utilisé par [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
-   Ouvrez le port TCP 2382 lors de l'installation de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] en mode SharePoint pour prendre en charge [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 2013. Dans [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 2013, l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] est externe à SharePoint. Les demandes entrantes à destination de l’instance nommée «[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]» proviennent des applications Web SharePoint via une connexion réseau, qui nécessite un port ouvert. Comme avec d'autres instances nommées [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , créez une règle de trafic entrant pour le service SQL Server Browser sur le port TCP 2382 pour permettre l'accès à [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)].  
  
-   Pour [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 2010, n'ouvrez pas les ports du Pare-feu Windows. En tant que complément de SharePoint, le service utilise les ports configurés pour SharePoint et établit uniquement des connexions locales à l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] qui charge et interroge les modèles de données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
-   Pour les instances [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] s'exécutant sur des ordinateurs virtuels Windows Azure, utilisez d'autres instructions pour configurer l'accès au serveur. Consultez [Business Intelligence de SQL Server dans les ordinateurs virtuels Windows Azure](http://msdn.microsoft.com/library/windowsazure/jj992719.aspx).  
  
 Bien que l’instance par défaut de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] écoute le port TCP 2383, vous pouvez configurer le serveur pour écouter sur un port fixe différent, la connexion au serveur au format suivant : \<nom_serveur > :\<numéro_port >.  
  
 Seul un port TCP peut être utilisé par une instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Sur des ordinateurs disposant de plusieurs cartes réseau ou de plusieurs adresses IP, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] écoute un port TCP à la recherche de toutes les adresses IP affectées ou affectées comme alias à l'ordinateur. Si vous avez des exigences particulières concernant plusieurs ports, pensez à configurer [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour l'accès HTTP. Vous pouvez ensuite configurer plusieurs points de terminaison HTTP sur les ports que vous choisissez. Consultez [Configurer l’accès HTTP à Analysis Services sur Internet Information Services &#40;IIS&#41; 8.0](../../analysis-services/instances/configure-http-access-to-analysis-services-on-iis-8-0.md).  
  
 Cette rubrique contient les sections suivantes :  
  
-   [Vérifier les paramètres des ports et du pare-feu utilisés par Analysis Services](#bkmk_checkport)  
  
-   [Configurer le pare-feu Windows pour une instance par défaut d'Analysis Services](#bkmk_default)  
  
-   [Configurer l'accès au Pare-feu Windows pour une instance nommée Analysis Services](#bkmk_named)  
  
-   [Configuration de port pour un cluster Analysis Services](#bkmk_cluster)  
  
-   [Configuration de port pour PowerPivot pour SharePoint](#bkmk_powerpivot)  
  
-   [Utiliser un port fixe pour une instance par défaut ou nommée d'Analysis Services](#bkmk_fixed)  
  
 Pour plus d’informations sur les paramètres par défaut du Pare-feu Windows et pour obtenir une description des ports TCP qui affectent le moteur de base de données, Analysis Services, Reporting Services et Integration Services, consultez [Configurer le Pare-feu Windows pour autoriser l’accès à SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).  
  
##  <a name="bkmk_checkport"></a> Vérifier les paramètres des ports et du pare-feu utilisés par Analysis Services  
 Sur les systèmes d'exploitation Microsoft Windows pris en charge par [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], le Pare-feu Windows est activé par défaut et bloque les connexions à distance. Vous devez ouvrir manuellement un port dans le Pare-feu pour autoriser les demandes entrantes à destination d'Analysis Services. Le programme d'installation de SQL Server n'effectue pas cette étape pour vous.  
  
 Le paramétrage des ports s'effectue dans le fichier msmdsrv.ini et dans SQL Server Management Studio, dans la page Propriétés générales d'une instance d'Analysis Services. Si la valeur de **Port** est un entier positif, le service écoute un port fixe. Si la valeur de **Port** est 0, le service écoute le port 2383 s'il s'agit de l'instance par défaut ou un port affecté dynamiquement s'il s'agit d'une instance nommée.  
  
 Les affectations de ports dynamiques ne sont utilisées que par les instances nommées. Le service **MSOLAP$InstanceName** détermine le port à utiliser lorsqu'il démarre. Pour connaître le port qu'utilise effectivement une instance nommée, vous pouvez procéder comme suit :  
  
-   Démarrez le Gestionnaire des tâches, puis cliquez sur **Services** pour obtenir le PID de **MSOLAP$InstanceName**.  
  
-   Exécutez **netstat –ao –p TCP** à partir de la ligne de commande pour afficher les informations de port TCP relatives à ce PID.  
  
-   Vérifier le port à l’aide de SQL Server Management Studio et connectez-vous à un serveur Analysis Services dans ce format : \<IPAddress > :\<numéro_port >.  
  
 Bien qu'une application puisse être à l'écoute d'un port spécifique, les connexions échouent si un pare-feu bloque l'accès. Pour que des connexions atteignent une instance nommée d'Analysis Services, vous devez débloquer l'accès à msmdsrv.exe ou au port fixe du pare-feu sur lequel s'effectue l'écoute. Les sections suivantes de cette rubrique fournissent des instructions à cet effet.  
  
 Pour vérifier si des paramètres de pare-feu sont déjà définis pour Analysis Services, utilisez Pare-feu Windows avec fonctions avancées de sécurité dans le Panneau de configuration. La page Pare-feu du dossier Analyse affiche la liste complète des règles définies pour le serveur local.  
  
 Notez que pour [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], toutes les règles de pare-feu doivent être définies manuellement. Bien que [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et SQL Server Browser réservent les ports 2382 et 2383, ni le programme d'installation de SQL Server, ni les outils de configuration ne définissent de règles de pare-feu qui vous donnent accès à l'un ou l'autre des ports ou aux fichiers exécutables du programme.  
  
##  <a name="bkmk_default"></a> Configurer le pare-feu Windows pour une instance par défaut d'Analysis Services  
 L'instance par défaut de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] écoute le port TCP 2383. Si vous avez installé l'instance par défaut et souhaitez utiliser ce port, il vous suffit de débloquer l'accès entrant au port TCP 2383 dans le Pare-feu Windows pour permettre l'accès distant à l'instance par défaut de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Si vous avez installé l'instance par défaut mais souhaitez configurer le service de façon à écouter un port fixe, consultez [Utiliser un port fixe pour une instance par défaut ou nommée d'Analysis Services](#bkmk_fixed) , plus loin dans cette rubrique.  
  
 Pour vérifier si le service s'exécute en tant qu'instance par défaut (MSSQLServerOLAPService), vérifiez le nom du service dans le Gestionnaire de configuration SQL Server. Une instance par défaut d’Analysis Services est toujours répertoriée sous le nom **SQL Server Analysis Services (MSSQLSERVER)**.  
  
> [!NOTE]  
>  Les différents systèmes d'exploitation Windows fournissent d'autres outils pour configurer le Pare-feu Windows. Dans leur majorité, ces outils vous donnent le choix entre ouvrir un port spécifique ou un exécutable de programme. À moins d'avoir une raison particulière de spécifier l'exécutable du programme, nous vous recommandons de spécifier le port.  
  
 Quand vous spécifiez une règle de trafic entrant, veillez à adopter une convention d’affectation de noms qui vous permette par la suite de retrouver facilement ces règles (par exemple, **SQL Server Analysis Services (TCP-entrant) 2383**).  
  
#### <a name="windows-firewall-with-advanced-security"></a>Pare-feu Windows avec fonctions avancées de sécurité  
  
1.  Sur Windows 7 ou Windows Vista, dans le Panneau de configuration, cliquez sur **Système et sécurité**, sélectionnez **Pare-feu Windows**, puis cliquez sur **Paramètres avancés**. Sous Windows Server 2008 ou 2008 R2, ouvrez Outils d'administration et cliquez sur **Pare-feu Windows avec fonctions avancées de sécurité**. Sous Windows Server 2012, ouvrez la page Applications et entrez **Pare-feu Windows**.  
  
2.  Cliquez avec le bouton droit sur **Règles de trafic entrant** et sélectionnez **Nouvelle règle**.  
  
3.  Dans Type de règle, cliquez sur **Port** , puis sur **Suivant**.  
  
4.  Dans Protocole et ports, sélectionnez **TCP** et tapez **2383** dans **Ports locaux spécifiques**.  
  
5.  Dans Action, cliquez sur **Autoriser la connexion** , puis sur **Suivant**.  
  
6.  Dans Profil, désactivez les emplacements réseau qui ne s'appliquent pas, puis cliquez sur **Suivant**.  
  
7.  Dans Nom, tapez un nom descriptif pour cette règle (par exemple, **SQL Server Analysis Services (tcp-entrant) 2383**), puis cliquez sur **Terminer**.  
  
8.  Pour vérifier que les connexions distantes sont bien activées, ouvrez SQL Server Management Studio ou Excel sur un autre ordinateur et connectez-vous à [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , en spécifiant le nom de réseau du serveur dans **Nom du serveur**.  
  
    > [!NOTE]  
    >  Les autres utilisateurs n'auront pas accès à ce serveur, à moins que vous ne leur accordiez les autorisations nécessaires. Pour plus d’informations, consultez [Autorisation de l’accès à des objets et des opérations &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/authorizing-access-to-objects-and-operations-analysis-services.md).  
  
#### <a name="netsh-advfirewall-syntax"></a>Syntaxe Netsh AdvFirewall  
  
-   La commande suivante crée une règle de trafic entrant qui autorise les demandes entrantes sur le port TCP 2383.  
  
    ```  
    netsh advfirewall firewall add rule name="SQL Server Analysis Services inbound on TCP 2383" dir=in action=allow protocol=TCP localport=2383 profile=domain  
    ```  
  
##  <a name="bkmk_named"></a> Configurer l'accès au Pare-feu Windows pour une instance nommée Analysis Services  
 Les instances nommées de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] peuvent écouter soit un port fixe, soit un port affecté dynamiquement, tandis que le service SQL Server Browser fournit les informations de connexion actuelles au sujet du service au moment de la connexion.  
  
 Le service SQL Server Browser écoute le port TCP 2382. UDP n'est pas utilisé. Le protocole TCP est le seul protocole de communication utilisé par [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
 Choisissez l'une des approches suivantes pour activer l'accès à distance à une instance nommée d'Analysis Services :  
  
-   Utilisez les affectations de port dynamiques, ainsi que le service SQL Server Browser. Débloquez le port utilisé par le service SQL Server Browser dans le Pare-feu Windows. Se connecter au serveur au format suivant : \<nom_serveur >\\< nom_instance\>.  
  
-   Utilisez conjointement un port fixe et le service SQL Server Browser. Cette approche vous permet de vous connectez à l’aide de ce format : \<nom_serveur >\\< nom_instance\>, identique à l’approche d’affectation de port dynamique, sauf que, dans ce cas, le serveur écoute sur un port fixe. Dans ce cas, le service SQL Server Browser fournit un nom de résolution à l'instance Analysis Services qui écoute le port fixe. Pour utiliser cette approche, configurez le serveur de façon à ce qu'il écoute un port fixe, débloquez l'accès à ce port, puis débloquez l'accès au port utilisé par le service SQL Server Browser.  
  
 Le service SQL Server Browser est uniquement utilisé avec les instances nommées, et jamais avec l'instance par défaut. Le service est automatiquement installé et activé, dès que vous installez l'une des fonctionnalités SQL Server en tant qu'instance nommée. Si vous choisissez une approche qui nécessite le service SQL Server Browser, assurez-vous qu'il est toujours activé et lancé sur votre serveur.  
  
 Si vous ne pouvez pas utiliser le service SQL Server Browser, vous devez affecter un port fixe dans la chaîne de connexion, qui ignore la résolution des noms de domaine. En l'absence du service SQL Server Browser, toutes les connexions client doivent inclure le numéro de port dans la chaîne de connexion (par exemple, AW-SRV01:54321).  
  
 **Option n°1 : utilisez les affectations de port dynamiques et débloquez l'accès au service SQL Server Browser**  
  
 Les affectations de ports dynamiques pour les instances nommées d'Analysis Services sont établies par le service **MSOLAP$InstanceName** lorsqu'il démarre. Par défaut, le service revendique le premier numéro de port disponible qu'il trouve, et utilise un numéro de port différent chaque fois qu'il est redémarré.  
  
 Le service SQL Server Browser assure la résolution des noms d'instances. Si vous utilisez des affectations de port dynamiques avec une instance nommée, il sera toujours nécessaire de débloquer le port TCP 2382 pour le service SQL Server Browser.  
  
> [!NOTE]  
>  Le service SQL Server Browser écoute le port UDP 1434 et le port TCP 2382, pour le moteur de base de données et Analysis Services, respectivement. Si vous avez déjà débloqué le port UDP 1434 pour le service SQL Server Browser, il vous faut néanmoins toujours débloquer le port TCP 2382 pour Analysis Services.  
  
#### <a name="windows-firewall-with-advanced-security"></a>Pare-feu Windows avec fonctions avancées de sécurité  
  
1.  Sur Windows 7 ou Windows Vista, dans le Panneau de configuration, cliquez sur **Système et sécurité**, sélectionnez **Pare-feu Windows**, puis cliquez sur **Paramètres avancés**. Sous Windows Server 2008 ou 2008 R2, ouvrez Outils d'administration et cliquez sur **Pare-feu Windows avec fonctions avancées de sécurité**. Sous Windows Server 2012, ouvrez la page Applications et entrez **Pare-feu Windows**.  
  
2.  Pour débloquer l’accès au service SQL Server Browser, cliquez avec le bouton droit sur **Règles de trafic entrant** , puis sélectionnez **Nouvelle règle**.  
  
3.  Dans Type de règle, cliquez sur **Port** , puis sur **Suivant**.  
  
4.  Dans Protocole et ports, sélectionnez **TCP** et saisissez **2382** dans **Ports locaux spécifiques**.  
  
5.  Dans Action, cliquez sur **Autoriser la connexion** , puis sur **Suivant**.  
  
6.  Dans Profil, désactivez les emplacements réseau qui ne s'appliquent pas, puis cliquez sur **Suivant**.  
  
7.  Dans Nom, tapez un nom descriptif pour cette règle (par exemple, **SQL Server Browser Service (tcp-entrant) 2382**), puis cliquez sur **Terminer**.  
  
8.  Pour vérifier que les connexions distantes sont activées, ouvrez SQL Server Management Studio ou Excel sur un autre ordinateur et connectez-vous à Analysis Services en spécifiant le nom réseau du serveur et le nom de l’instance au format suivant : \<nom_serveur >\\< nom_instance\>. Par exemple, sur un serveur nommé **AW-SRV01** avec une instance nommée **Finance**, le nom du serveur sera **AW-SRV01\Finance**.  
  
 **Option n°2 : utilisez un port fixe pour une instance nommée**  
  
 L'alternative consiste à désigner un port fixe et à débloquer l'accès à ce port. L'avantage de cette approche est qu'elle offre de plus grandes possibilités en termes d'audit si vous autorisez l'accès au fichier exécutable de programme. L'utilisation d'un port fixe est de ce fait l'approche recommandée pour accéder à toute instance Analysis Services.  
  
 Pour affecter un port fixe, suivez les instructions de la section [Utiliser un port fixe pour une instance par défaut ou nommée d'Analysis Services](#bkmk_fixed) de cette rubrique, puis revenez à la présente section pour débloquer le port.  
  
#### <a name="windows-firewall-with-advanced-security"></a>Pare-feu Windows avec fonctions avancées de sécurité  
  
1.  Sur Windows 7 ou Windows Vista, dans le Panneau de configuration, cliquez sur **Système et sécurité**, sélectionnez **Pare-feu Windows**, puis cliquez sur **Paramètres avancés**. Sous Windows Server 2008 ou 2008 R2, ouvrez Outils d'administration et cliquez sur **Pare-feu Windows avec fonctions avancées de sécurité**. Sous Windows Server 2012, ouvrez la page Applications et entrez **Pare-feu Windows**.  
  
2.  Pour débloquer l’accès à Analysis Services, cliquez avec le bouton droit sur **Règles de trafic entrant** , puis sélectionnez **Nouvelle règle**.  
  
3.  Dans Type de règle, cliquez sur **Port** , puis sur **Suivant**.  
  
4.  Dans Protocole et ports, sélectionnez **TCP** et indiquez le port fixe dans **Ports locaux spécifiques**.  
  
5.  Dans Action, cliquez sur **Autoriser la connexion** , puis sur **Suivant**.  
  
6.  Dans Profil, désactivez les emplacements réseau qui ne s'appliquent pas, puis cliquez sur **Suivant**.  
  
7.  Dans Nom, tapez un nom descriptif pour cette règle (par exemple, **SQL Server Analysis Services sur port 54321**), puis cliquez sur **Terminer**.  
  
8.  Pour vérifier que les connexions distantes sont activées, ouvrez SQL Server Management Studio ou Excel sur un autre ordinateur et connectez-vous à Analysis Services en spécifiant le nom réseau du serveur et le numéro de port au format suivant : \<nom_serveur > :\<numéro_port >.  
  
#### <a name="netsh-advfirewall-syntax"></a>Syntaxe Netsh AdvFirewall  
  
-   Les commandes suivantes créent des règles de trafic entrant qui débloquent le port TCP 2382 pour le service SQL Server Browser et débloquent le port fixe que vous avez spécifié pour l'instance Analysis Services. Vous pouvez exécuter l'une ou l'autre pour permettre l'accès à une instance nommée d'Analysis Services.  
  
     Dans cet exemple de commande, le port fixe est le 54321. Veillez à le remplacer par le port effectivement utilisé sur votre système.  
  
    ```  
    netsh advfirewall firewall add rule name="SQL Server Analysis Services (tcp-in) on 54321" dir=in action=allow protocol=TCP localport=54321 profile=domain  
    ```  
  
    ```  
    netsh advfirewall firewall add rule name="SQL Server Browser Services inbound on TCP 2382" dir=in action=allow protocol=TCP localport=2382 profile=domain  
    ```  
  
##  <a name="bkmk_fixed"></a> Utiliser un port fixe pour une instance par défaut ou nommée d'Analysis Services  
 Cette section explique comment configurer le service pour l'écoute d'un port fixe. L'utilisation d'un port fixe est la plus courante lorsqu'Analysis Services est installé en tant qu'instance nommée, cependant, vous pouvez également utiliser cette approche si les besoins de l'entreprise et les besoins en matière de sécurité nécessitent l'utilisation de ports non affectés par défaut.  
  
 Notez que l'utilisation d'un port fixe modifie la syntaxe de connexion de l'instance par défaut, puisqu'il vous faudra ajouter le numéro de port après le nom du serveur. Par exemple, la connexion à une instance locale par défaut d'Analysis Services écoutant le port 54321 dans SQL Server Management Studio nécessite que vous tapiez localhost:54321 comme nom de serveur dans la boîte de dialogue Se connecter au serveur de Management Studio.  
  
 Si vous utilisez une instance nommée, vous pouvez affecter un port fixe sans aucune modification à la façon dont vous spécifiez le nom du serveur (en particulier, vous pouvez utiliser \<nom_serveur\nom_instance > pour vous connecter à une instance nommée à l’écoute sur un port fixe). Cela fonctionne uniquement si le service SQL Server Browser est en cours d'exécution et si vous avez débloqué le port que le service écoute. Service SQL Server Browser fournit la redirection vers le port fixe selon \<nomserveur\nominstance >. Tant que vous ouvrez des ports à la fois pour le service SQL Server Browser et pour l'instance nommée d'Analysis Services à l'écoute du port fixe, le service SQL Server Browser assure la résolution de la connexion en instance nommée.  
  
1.  Identifiez un port TCP/IP disponible à utiliser.  
  
     Pour afficher la liste des ports réservés et enregistrés que vous ne devez pas utiliser, consultez les [numéros de port (IANA)](http://go.microsoft.com/fwlink/?LinkID=198469)(en anglais). Pour afficher la liste des ports déjà utilisés par votre système, ouvrez une fenêtre d'invite de commandes et tapez **netstat –a –p TCP** pour afficher la liste des ports TCP ouverts sur le système.  
  
2.  Après avoir déterminé quel port utiliser, spécifiez-le en modifiant le paramètre de configuration **Port** dans le fichier msmdsrv.ini ou dans la page Propriétés générales d'une instance d'Analysis Services dans SQL Server Management Studio.  
  
3.  Redémarrage du service.  
  
4.  Configurez le Pare-feu Windows pour débloquer le port TCP spécifié. Si vous utilisez un port fixe pour une instance nommée, débloquez le port TCP spécifié pour cette instance et le port TCP 2382 pour le service SQL Server Browser.  
  
5.  Vérifiez en vous connectant localement (dans Management Studio), puis à distance à partir d'une application cliente sur un autre ordinateur. Pour utiliser Management Studio, connectez-vous à une instance par défaut d’Analysis Services en spécifiant un nom de serveur au format suivant : \<nom_serveur > :\<numéro_port >. Pour une instance nommée, spécifiez le nom du serveur en tant que \<nom_serveur >\\< nom_instance\>.  
  
##  <a name="bkmk_cluster"></a> Configuration de port pour un cluster Analysis Services  
 Un cluster de basculement [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] écoute toujours sur le port TCP 2383, que vous l'ayez installé comme instance par défaut ou comme instance nommée. Les affectations de ports dynamiques ne sont pas utilisées par [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] lorsqu'il est installé sur un cluster de basculement Windows. Veillez à ouvrir le port TCP 2383 sur chaque nœud en exécutant [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans le cluster. Pour plus d'informations sur le clustering [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], consultez [Procédure : mettre en cluster SQL Server Analysis Services](http://go.microsoft.com/fwlink/p/?LinkId=396548).  
  
##  <a name="bkmk_powerpivot"></a> Configuration de port pour PowerPivot pour SharePoint  
 L'architecture du serveur pour [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] est fondamentalement différente selon la version de SharePoint que vous utilisez.  
  
 **SharePoint 2013**  
  
 Dans SharePoint 2013, Excel Services redirige les demandes de modèles de données Power Pivot, qui sont ensuite chargés sur une instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] hors de l'environnement SharePoint. Les connexions suivent le modèle par défaut, où une bibliothèque cliente Analysis Services sur un ordinateur local envoie une demande de connexion à une instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] distante dans le même réseau.  
  
 Étant donné que [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] installe toujours [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] en tant qu'instance nommée, vous devez supposer le service SQL Server Browser et les affectations de ports dynamiques. Comme indiqué précédemment, le service SQL Server Browser écoute le port TCP 2382 à la recherche des demandes de connexion envoyées aux instances nommées [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , qui redirigent la demande au port actuel.  
  
 Notez qu'Excel Services dans SharePoint 2013 ne prend pas en charge la syntaxe de connexion de port fixe ; par conséquent, veillez à ce que le service SQL Server Browser soit accessible.  
  
 **SharePoint 2010**  
  
 Si vous utilisez SharePoint 2010, il n'est pas nécessaire que vous ouvriez des ports dans le Pare-feu Windows. SharePoint ouvre les ports nécessaires et les compléments, tels que [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint, fonctionnent dans l’environnement SharePoint. Dans une installation [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint 2010, le service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] a l’usage exclusif de l’instance du service SQL Server Analysis Services ([!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]) local, qui est installée avec elle sur le même ordinateur. Il utilise des connexions locales, et non réseau, pour accéder au service du moteur Analysis Services qui charge, interroge et traite les données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] sur le serveur SharePoint. Les demandes de données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] aux applications clientes sont routées via les ports ouverts par le programme d’installation de SharePoint (en particulier, les règles de trafic entrant sont définies de façon à autoriser l’accès à SharePoint – 80, à l’Administration centrale de SharePoint v4, aux services web SharePoint et à SPUserCodeV4). Étant donné que les services Web [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] fonctionnent dans une batterie de serveurs SharePoint, les règles de pare-feu SharePoint suffisent pour l’accès à distance aux données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] d’une batterie de serveurs SharePoint.  
  
## <a name="see-also"></a>Voir aussi  
 [Service SQL Server Browser &#40;moteur de base de données et SSAS&#41;](../../database-engine/configure-windows/sql-server-browser-service-database-engine-and-ssas.md)   
 [Démarrer, arrêter, suspendre, reprendre, redémarrer le moteur de base de données, SQL Server Agent ou le service SQL Server Browser](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)   
 [Configurer un pare-feu Windows pour accéder au moteur de base de données](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)  
  
  
