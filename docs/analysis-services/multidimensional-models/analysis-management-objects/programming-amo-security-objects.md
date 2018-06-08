---
title: Programmation des objets de sécurité AMO | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: amo
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 2765ddac9df7fd406dce4394773da9831a56b281
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="programming-amo-security-objects"></a>Programmation d'objets de sécurité AMO
  Dans [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]la programmation d'objets de sécurité ou l'exécution d'applications qui utilisent des objets de sécurité AMO exige d'être membre du groupe Administrateur du serveur ou du groupe Administrateur de base de données. Administrateur du serveur et Administrateur de base de données représentent des niveaux d'accès fournis par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].  
  
 Dans [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] , l'accès utilisateur à tout objet est obtenu à travers la combinaison des rôles et des autorisations attribués à cet objet. Pour plus d’informations, consultez [Classes de sécurité AMO](../../../analysis-services/multidimensional-models/analysis-management-objects/amo-security-classes.md).  
  
## <a name="role-and-permission-objects"></a>Objets de rôle et d'autorisation  
 Les rôles de serveur contiennent un seul et même rôle dans la collection : le rôle Administrateurs. Il n'est pas possible d'ajouter de nouveaux rôles à la collection des rôles de serveur. L'appartenance au rôle Administrateurs autorise un accès complet à chaque objet du serveur  
  
 Les objets <xref:Microsoft.AnalysisServices.Role> sont créés au niveau de la base de données. La maintenance de rôle revient simplement à ajouter ou supprimer des membres au niveau du rôle et à ajouter ou supprimer des rôles dans l'objet <xref:Microsoft.AnalysisServices.Database>. Un rôle ne peut pas être supprimé si un objet <xref:Microsoft.AnalysisServices.Permission> lui est associé. Pour supprimer un rôle, tous les <xref:Microsoft.AnalysisServices.Permission> des objets dans le <xref:Microsoft.AnalysisServices.Database> objets doivent être recherchés et le <xref:Microsoft.AnalysisServices.Role> supprimé des autorisations, avant le <xref:Microsoft.AnalysisServices.Role> peuvent être supprimés de la <xref:Microsoft.AnalysisServices.Database>.  
  
 Les autorisations définissent les actions pouvant être effectuées sur les objets qui en bénéficient. Les autorisations peuvent être fournies aux objets suivants : <xref:Microsoft.AnalysisServices.Database>, <xref:Microsoft.AnalysisServices.DataSource>, <xref:Microsoft.AnalysisServices.Dimension>, <xref:Microsoft.AnalysisServices.Cube>, <xref:Microsoft.AnalysisServices.MiningStructure> et <xref:Microsoft.AnalysisServices.MiningModel>. La maintenance des autorisations implique d'octroyer ou de révoquer l'accès par la propriété d'accès correspondante. À chaque accès autorisé correspond une propriété qui peut être définie selon le niveau d'accès souhaité. L'accès peut être défini pour les opérations suivantes : Process, ReadDefinition, Read, Write et Administer. L'accès Administer n'est défini qu'au niveau de l'objet <xref:Microsoft.AnalysisServices.Database>. Le niveau de sécurité de l'administrateur de base de données est obtenu lorsque le rôle est accordé avec l'autorisation de base de données Administer.  
  
 L'exemple suivant crée quatre rôles : Database Administrators (Administrateurs de base de données), Processors (Processeurs), Writers (Enregistreurs) et Readers (Lecteurs).  
  
 Les administrateurs de base de données peuvent gérer la base de données fournie.  
  
 Les processeurs peuvent traiter tous les objets d'une base de données et vérifier les résultats. Pour vérifier les résultats, l'accès en lecture à l'objet de base de données doit être explicitement activé sur le cube fourni, car l'autorisation de lecture ne s'applique pas aux objets enfants.  
  
 Les enregistreurs peuvent lire et enregistrer sur le cube fourni, tandis que l'accès aux cellules est limité aux États-Unis (« United States ») dans la dimension « Customer » (Client).  
  
 Les lecteurs peuvent lire sur le cube fourni, tandis que l'accès aux cellules est limité aux États-Unis (« United States ») dans la dimension « Customer » (Client).  
  
```  
static public void CreateRolesAndPermissions(Database db, Cube cube)  
{  
    Role role;  
    DatabasePermission dbperm;  
    CubePermission cubeperm;  
  
    #region Create the Database Administrators role  
  
    // Create the Database Administrators role.  
    role = db.Roles.Add("Database Administrators");  
    role.Members.Add(new RoleMember("")); // e.g. domain\user  
    role.Update();  
  
    // Assign administrative permissions to this role.  
    // Members of this role can perform any operation within the database.  
    dbperm = db.DatabasePermissions.Add(role.ID);  
    dbperm.Administer = true;  
    dbperm.Update();  
  
    #endregion  
  
    #region Create the Processors role  
  
    // Create the Processors role.  
    role = db.Roles.Add("Processors");  
    role.Members.Add(new RoleMember("")); // e.g. myDomain\johndoe  
    role.Update();  
  
    // Assign Read and Process permissions to this role.  
    // Members of this role can process objects in the database and query them to verify results.  
    // Process permission applies to all contained objects, i.e. all dimensions and cubes.  
    // Read permission does not apply to contained objects, so we must assign the permission explicitly on the cubes.  
    dbperm = db.DatabasePermissions.Add(role.ID);  
    dbperm.Read = ReadAccess.Allowed;  
    dbperm.Process = true;  
    dbperm.Update();  
  
    cubeperm = cube.CubePermissions.Add(role.ID);  
    cubeperm.Read = ReadAccess.Allowed;  
    cubeperm.Update();  
  
    #endregion  
  
    #region Create the Writers role  
  
    // Create the Writers role.  
    role = db.Roles.Add("Writers");  
    role.Members.Add(new RoleMember("")); // e.g. redmond\johndoe  
    role.Update();  
  
    // Assign Read and Write permissions to this role.  
    // Members of this role can discover, query and writeback to the Adventure Works cube.  
    // However cell access and writeback is restricted to the United States (in the Customer dimension).  
    dbperm = db.DatabasePermissions.Add(role.ID);  
    dbperm.Read = ReadAccess.Allowed;  
    dbperm.Update();  
  
    cubeperm = cube.CubePermissions.Add(role.ID);  
    cubeperm.Read = ReadAccess.Allowed;  
    cubeperm.Write = WriteAccess.Allowed;  
    cubeperm.CellPermissions.Add(new CellPermission(CellPermissionAccess.Read, "[Customer].[Country-Region].CurrentMember is [Customer].[Country-Region].[Country-Region].&[United States]"));  
    cubeperm.CellPermissions.Add(new CellPermission(CellPermissionAccess.ReadWrite, "[Customer].[Country-Region].CurrentMember is [Customer].[Country-Region].[Country-Region].&[United States]"));  
    cubeperm.Update();  
  
    #endregion  
  
    #region Create the Readers role  
  
    // Create the Readers role.  
    role = db.Roles.Add("Readers");  
    role.Members.Add(new RoleMember("")); // e.g. redmond\johndoe  
    role.Update();  
  
    // Assign Read permissions to this role.  
    // Members of this role can discover and query the Adventure Works cube.  
    // However the Customer dimension is restricted to the United States.  
    dbperm = db.DatabasePermissions.Add(role.ID);  
    dbperm.Read = ReadAccess.Allowed;  
    dbperm.Update();  
  
    cubeperm = cube.CubePermissions.Add(role.ID);  
    cubeperm.Read = ReadAccess.Allowed;  
    Dimension dim = db.Dimensions.GetByName("Customer");  
    DimensionAttribute attr = dim.Attributes.GetByName("Country-Region");  
    CubeDimensionPermission cubedimperm = cubeperm.DimensionPermissions.Add(dim.ID);  
    cubedimperm.Read = ReadAccess.Allowed;  
    AttributePermission attrperm = cubedimperm.AttributePermissions.Add(attr.ID);  
    attrperm.AllowedSet = "{[Customer].[Country-Region].[Country-Region].&[United States]}";  
    cubeperm.Update();  
  
    #endregion  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 <xref:Microsoft.AnalysisServices>   
 [Présentation des Classes AMO](../../../analysis-services/multidimensional-models/analysis-management-objects/amo-classes-introduction.md)   
 [Programmation d’objets AMO sécurité](../../../analysis-services/multidimensional-models/analysis-management-objects/programming-amo-security-objects.md)   
 [Autorisations et droits d’accès & #40 ; Analysis Services - données multidimensionnelles & #41 ;](http://msdn.microsoft.com/library/59fa3573-f985-46cb-8042-7da71bd59a7b)   
 [Architecture logique & #40 ; Analysis Services - données multidimensionnelles & #41 ;](../../../analysis-services/multidimensional-models/olap-logical/understanding-microsoft-olap-logical-architecture.md)   
 [Les objets de base de données & #40 ; Analysis Services - données multidimensionnelles & #41 ;](../../../analysis-services/multidimensional-models/olap-logical/database-objects-analysis-services-multidimensional-data.md)  
  
  
