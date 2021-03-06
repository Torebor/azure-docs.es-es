---
title: Reglas de firewall de Azure SQL Database | Microsoft Docs
description: "Obtenga información acerca de cómo configurar un firewall de base de datos SQL mediante las reglas de firewall de nivel de servidor y de nivel de base de datos, para administrar el acceso."
keywords: firewall de base de datos
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: ac57f84c-35c3-4975-9903-241c8059011e
ms.service: sql-database
ms.custom: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 04/10/2017
ms.author: rickbyh
ms.translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: 744ad6cfc15453e1db7a012eebe09ceba226fde9
ms.contentlocale: es-es
ms.lasthandoff: 04/15/2017


---
# <a name="azure-sql-database-server-level-and-database-level-firewall-rules"></a>Reglas de firewall de nivel de servidor y de nivel de base de datos de Azure SQL Database 

Base de datos SQL de Microsoft Azure ofrece un servicio de base de datos relacional para Azure y otras aplicaciones basadas en Internet. Para ayudar a proteger los datos, los firewalls impiden todo acceso al servidor de bases de datos, excepto a aquellos equipos a los que haya concedido permiso. Asimismo, otorgan acceso a las bases de datos según la dirección IP de origen de cada solicitud.

## <a name="overview"></a>Información general

Inicialmente, todo el acceso mediante instrucciones Transact-SQL al servidor SQL de Azure está bloqueado por el firewall. Para comenzar a usar Azure SQL Server, debe especificar una o varias reglas de firewall de nivel de servidor que permitan acceder a Azure SQL Server. Use las reglas de firewall para especificar qué intervalos de direcciones IP de Internet se permiten y si las aplicaciones de Azure pueden tratar de conectarse al servidor SQL de Azure.

Para conceder selectivamente el acceso a solo una de las bases de datos en el servidor SQL de Azure, debe crear una regla de nivel de base de datos para la base de datos necesaria. Especifique un intervalo de direcciones IP para la regla de firewall de base de datos que quede fuera del intervalo de direcciones IP especificado en la regla de firewall de nivel de servidor. Además, asegúrese de que la dirección IP del cliente se encuentre en el intervalo especificado en la regla de nivel de base de datos.

Los intentos de conexión desde Internet y Azure deben atravesar primero el firewall antes de poder alcanzar SQL Database y el servidor SQL de Azure, tal y como se muestra en el siguiente diagrama:

   ![Diagrama donde se describe la configuración del firewall.][1]

* **Reglas de firewall de nivel de servidor:** estas reglas permiten a los clientes acceder a todo el servidor SQL de Azure; es decir, a todas las bases de datos que se encuentren en el mismo servidor lógico. Estas reglas se almacenan en la base de datos **maestra** . Las reglas de firewall de nivel de servidor pueden configurarse a través del Portal o mediante instrucciones Transact-SQL. Para poder crear reglas de firewall en el nivel del servidor mediante Azure Portal o PowerShell, debe ser el propietario o un colaborador de la suscripción. Para crear reglas de firewall en el nivel de servidor mediante Transact-SQL, debe conectarse a una instancia de SQL Database con el inicio de sesión principal del nivel de servidor o como administrador de Azure Active Directory (lo que significa que la regla de firewall del nivel de servidor debe crearla primero un usuario con permisos en el nivel de Azure).
* **Reglas de firewall de nivel de base de datos:** estas reglas permiten a los clientes acceder a determinadas bases de datos dentro del mismo servidor lógico. Puede crear estas reglas para cada base de datos (incluida la base datos **maestra**), y se almacenan en las bases de datos individuales. Las reglas de firewall de nivel de base de datos solo pueden configurarse mediante instrucciones Transact-SQL, pero solo después de haber configurado el primer firewall de nivel de servidor. Si especifica un intervalo de direcciones IP en la regla de firewall de nivel de base de datos que se encuentra fuera del intervalo especificado en la regla de firewall de nivel de servidor, solo los clientes que tengan direcciones IP en el intervalo de nivel de base de datos pueden tener acceso a la base de datos. Puede tener un máximo de 128 reglas de firewall de nivel de base de datos para una base de datos. Solo se pueden crear reglas de firewall de nivel de base de datos para las bases de datos de usuario y maestras y administrarse a través de Transact-SQL. Para más información sobre cómo configurar las reglas de firewall de nivel de base de datos, consulte el ejemplo más adelante en este artículo y vea [sp_set_database_firewall_rule (Azure SQL Database)](https://msdn.microsoft.com/library/dn270010.aspx).

**Recomendación** : Microsoft recomienda usar reglas de firewall de nivel de base de datos siempre que sea posible con el fin de mejorar la seguridad y hacer que la base de datos sea más portátil. Utilice reglas de firewall de nivel de servidor para administradores y cuando tenga muchas bases de datos con los mismos requisitos de acceso y no quiera dedicar tiempo a configurar individualmente cada una de ellas.

> [!Note]
> Para más información acerca de bases de datos portátiles en el contexto de la continuidad empresarial, consulte [Requisitos de autenticación para la recuperación ante desastres](sql-database-geo-replication-security-config.md).
>

### <a name="connecting-from-the-internet"></a>Conexión desde Internet

Cuando un equipo intenta conectarse al servidor de bases de datos desde Internet, el firewall comprueba la dirección IP de origen de la solicitud con las reglas de firewall de nivel de base de datos, para la base de datos que está solicitando la conexión:

* Si la dirección IP de la solicitud está comprendida en uno de los intervalos especificados en las reglas de firewall de nivel de base de datos, la conexión se concede a la instancia de SQL Database que contiene la regla.
* Si la dirección IP de la solicitud no está comprendida en uno de los intervalos especificados en la regla de firewall de nivel de base de datos, se comprueban las reglas de firewall de nivel de servidor. Si la dirección IP de la solicitud está comprendida en uno de los intervalos especificados en las reglas de firewall de nivel de servidor, la conexión se concede. Las reglas de firewall de nivel de servidor se aplican a todas las instancias de SQL Database en SQL Server de Azure.  
* Si la dirección IP de la solicitud no se encuentra dentro de los intervalos especificados en cualquiera de las reglas de firewall de nivel de base de datos o de servidor, la solicitud de conexión genera un error.

> [!NOTE]
> Para obtener acceso a la Base de datos SQL de Azure desde el equipo local, asegúrese de que el firewall de su red y el equipo local permite la comunicación saliente en el puerto TCP 1433.
> 

### <a name="connecting-from-azure"></a>Conexión desde Azure
Para permitir que las aplicaciones de Azure se conecten al servidor SQL de Azure, deben habilitarse las conexiones de Azure. Cuando una aplicación desde Azure intenta conectarse a su servidor de s de datos, el firewall comprueba que se permiten las conexiones de Azure. Una configuración del firewall con dirección inicial y final igual a 0.0.0.0 indica que se permiten estas conexiones. Si no se permite el intento de conexión, la solicitud no alcanza el servidor de Base de datos SQL de Azure.

> [!IMPORTANT]
> Esta opción configura el firewall para permitir todas las conexiones de Azure, incluidas las de las suscripciones de otros clientes. Al seleccionar esta opción, asegúrese de que los permisos de usuario y el inicio de sesión limiten el acceso solamente a los usuarios autorizados.
> 

## <a name="creating-and-managing-firewall-rules"></a>Creación y administración de reglas de firewall
La primera configuración del firewall de nivel de servidor puede crearse mediante [Azure Portal](https://portal.azure.com/) o mediante programación con [Azure PowerShell](https://msdn.microsoft.com/library/azure/dn546724.aspx), la [CLI de Azure](/cli/azure/sql/server/firewall-rule#create) o la [API de REST](https://msdn.microsoft.com/library/azure/dn505712.aspx). Las reglas de firewall de nivel de servidor posteriores se pueden crear y administrar mediante estos métodos, y mediante Transact-SQL. 
> [!IMPORTANT]
> Las reglas de firewall de nivel de base de datos solo pueden crearse y administrarse mediante Transact-SQL. 
>

Para mejorar el rendimiento, las reglas de firewall de nivel de servidor se almacenan temporalmente en caché en el nivel de base de datos. Para actualizar la memoria caché, consulte [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx). 

### <a name="azure-portal"></a>Portal de Azure

Para establecer una regla de firewall de nivel de servidor en Azure Portal, puede ir a la página de información general de Azure SQL Database o a la página de información general del servidor lógico de la base de datos de Azure.

> [!TIP]
> Para consultar un tutorial, vea [Creación de una instancia de Azure SQL Database en Azure Portal](sql-database-get-started-portal.md).
>

**Desde la página de información general de la base de datos**

1. Para establecer una regla de firewall de nivel de servidor desde la página de información general de la base de datos, haga clic en **Establecer el firewall del servidor** en la barra de herramientas, como se muestra en la siguiente imagen: se abre la página **Configuración del firewall** del servidor de SQL Database.

      ![regla de firewall del servidor](./media/sql-database-get-started-portal/server-firewall-rule.png) 

2. Haga clic en **Agregar IP de cliente** en la barra de herramientas para agregar la dirección IP del equipo que está usando actualmente y, luego, haga clic en **Guardar**. Se creará una regla de firewall de nivel de servidor para la dirección IP actual.

      ![establecer regla de firewall del servidor](./media/sql-database-get-started-portal/server-firewall-rule-set.png) 

**Desde la página de información general del servidor**

Se abre la página de información general del servidor, que muestra el nombre completo del servidor (por ejemplo, **mynewserver20170403.database.windows.net**) y proporciona opciones para otras configuraciones.

1. Para establecer una regla de nivel de servidor desde la página de información general del servidor, haga clic en **Firewall** en el menú izquierdo en Configuración, tal y como se explica en la siguiente imagen: 

     ![información general del servidor lógico](./media/sql-database-migrate-your-sql-server-database/logical-server-overview.png)

2. Haga clic en **Agregar IP de cliente** en la barra de herramientas para agregar la dirección IP del equipo que está usando actualmente y, luego, haga clic en **Guardar**. Se creará una regla de firewall de nivel de servidor para la dirección IP actual.

     ![establecer regla de firewall del servidor](./media/sql-database-migrate-your-sql-server-database/server-firewall-rule-set.png)

### <a name="transact-sql"></a>Transact-SQL
| Vista de catálogo o un procedimiento almacenado | Level | Descripción |
| --- | --- | --- |
| [sys.firewall_rules](https://msdn.microsoft.com/library/dn269980.aspx) |Server |Muestra las reglas de firewall de nivel de servidor actuales |
| [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) |Server |Crea o actualiza las reglas de firewall de nivel de servidor |
| [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx) |Server |Elimina las reglas de firewall de nivel de servidor |
| [sys.database_firewall_rules](https://msdn.microsoft.com/library/dn269982.aspx) |Base de datos |Muestra las reglas de firewall de nivel de base de datos actuales |
| [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) |Base de datos |Crea o actualiza las reglas de firewall de nivel de base de datos |
| [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx) |Bases de datos |Elimina las reglas de firewall de nivel de base de datos |


En los ejemplos siguientes se revisan las reglas existentes, se habilita un intervalo de direcciones IP en el servidor Contoso y se elimina una regla de firewall:
   
```sql
SELECT * FROM sys.firewall_rules ORDER BY name;
```
  
A continuación, agregue una regla de firewall.
   
```sql
EXECUTE sp_set_firewall_rule @name = N'ContosoFirewallRule',
   @start_ip_address = '192.168.1.1', @end_ip_address = '192.168.1.10'
```

Para eliminar una regla de firewall de nivel de servidor, ejecute el procedimiento almacenado sp_delete_firewall_rule. En el ejemplo siguiente se elimina la regla llamada ContosoFirewallRule:
   
```sql
EXECUTE sp_delete_firewall_rule @name = N'ContosoFirewallRule'
```   

### <a name="azure-powershell"></a>Azure PowerShell
| Cmdlet | Level | Description |
| --- | --- | --- |
| [Get-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546731.aspx) |Server |Devuelve las reglas de firewall de nivel de servidor actuales |
| [New-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546724.aspx) |Server |Crear una regla de firewall de nivel de servidor |
| [Set-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546739.aspx) |Server |Actualiza las propiedades de una regla de firewall de nivel de servidor existente |
| [Remove-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546727.aspx) |Server |Elimina las reglas de firewall de nivel de servidor |


En el ejemplo siguiente se establece una regla de firewall de nivel de servidor mediante PowerShell:

```powershell
New-AzureRmSqlServerFirewallRule -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress "0.0.0.0" -EndIpAddress "0.0.0.1"
```

> [!TIP]
> Para consultar ejemplos de PowerShell en el contexto de un inicio rápido, vea [Creación de una base de datos con PowerShell](sql-database-get-started-powershell.md) y [Creación de una instancia única de SQL Database y configuración de una regla de firewall mediante Powershell](scripts/sql-database-create-and-configure-database-powershell.md).
>

### <a name="azure-cli"></a>CLI de Azure
| Cmdlet | Level | Descripción |
| --- | --- | --- |
| [az sql server firewall create](/cli/azure/sql/server/firewall-rule#create) | Crea una regla de firewall para permitir el acceso a todas las bases de datos SQL en el servidor desde el intervalo de direcciones IP especificado.|
| [az sql server firewall delete](/cli/azure/sql/server/firewall-rule#delete)| Elimina una regla de firewall.|
| [az sql server firewall list](/cli/azure/sql/server/firewall-rule#list)| Enumera las reglas de firewall.|
| [az sql server firewall rule show](/cli/azure/sql/server/firewall-rule#show)| Muestra los detalles de una regla de firewall.|
| [ax sql server firewall rule update](/cli/azure/sql/server/firewall-rule#update)| Actualiza una regla de firewall.

En el ejemplo siguiente se establece una regla de firewall de nivel de servidor mediante la CLI de Azure: 

```azurecli-interactive
az sql server firewall-rule create --resource-group myResourceGroup --server $servername \
    -n AllowYourIp --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.1
```

> [!TIP]
> Para consultar un ejemplo de la CLI de Azure en el contexto de un inicio rápido, vea [Creación de una base de datos con la CLI de Azure](sql-database-get-started-cli.md) y [Cree una base de datos SQL única y configure una regla de firewall mediante la CLI de Azure](scripts/sql-database-create-and-configure-database-cli.md).
>

### <a name="rest-api"></a>API de REST
| API | Level | Description |
| --- | --- | --- |
| [Enumerar reglas de firewall](https://msdn.microsoft.com/library/azure/dn505715.aspx) |Server |Muestra las reglas de firewall de nivel de servidor actuales |
| [Crear regla de firewall](https://msdn.microsoft.com/library/azure/dn505712.aspx) |Server |Crea o actualiza las reglas de firewall de nivel de servidor |
| [Establecer regla de firewall](https://msdn.microsoft.com/library/azure/dn505707.aspx) |Server |Actualiza las propiedades de una regla de firewall de nivel de servidor existente |
| [Eliminar regla de firewall](https://msdn.microsoft.com/library/azure/dn505706.aspx) |Server |Elimina las reglas de firewall de nivel de servidor |

## <a name="server-level-firewall-rule-versus-a-database-level-firewall-rule"></a>Regla de firewall de nivel de servidor frente a una regla de firewall de nivel de base de datos
P: ¿Los usuarios de una base de datos deben estar completamente aislados de otra base de datos?   
  Si es así, conceda acceso usando reglas de firewall de nivel de base de datos. Esto evita usar reglas de firewall de nivel de servidor, que permiten el acceso a través del firewall a todas las bases de datos, lo que reduce la solidez de sus defensas.   
 
P: ¿Los usuarios de las direcciones IP requieren acceso a todas las bases de datos?   
  Utilice reglas de firewall de nivel de servidor para reducir el número de veces que se deben configurar las reglas de firewall.   

P: ¿La persona o el equipo que configura las reglas de firewall solo tiene acceso mediante la API de REST, PowerShell o Azure Portal?   
  Debe usar reglas de firewall de nivel de servidor. Las reglas de firewall de nivel de base de datos solo pueden configurarse mediante Transact-SQL.  

P: ¿Se prohíbe que la persona o el equipo que configura las reglas de firewall tenga permiso de alto nivel en el nivel de base de datos?   
  Use reglas de firewall de nivel de servidor. La configuración de reglas de firewall de nivel de base de datos mediante Transact-SQL requiere, al menos, permiso `CONTROL DATABASE` en el nivel de base de datos.  

P: ¿La persona o el equipo que configura o audita las reglas de firewall administra de forma centralizada las reglas de firewall para muchas bases de datos (quizás 100)?   
  Esta decisión depende de sus necesidades y del entorno. Las reglas de firewall de nivel de servidor pueden ser más fáciles de configurar, pero con scripting se pueden configurar reglas en el nivel de base de datos. Incluso si usa las reglas de firewall de nivel de servidor, podría necesitar auditar las reglas de firewall de base de datos para ver si los usuarios con permiso `CONTROL` en la base de datos han creado reglas de firewall de nivel de base de datos.   

P: ¿Puedo usar una combinación de reglas de firewall de nivel de servidor y de base de datos?   
  Sí. Algunos usuarios, por ejemplo, los administradores, pueden necesitar reglas de firewall de nivel de servidor. Otros usuarios, como los usuarios de una aplicación de base de datos, pueden necesitar reglas de firewall de nivel de base de datos.   

## <a name="troubleshooting-the-database-firewall"></a>Solución de problemas del firewall de la base de datos
Tenga en cuenta los siguientes puntos cuando el acceso al servicio de Base de datos SQL de Microsoft Azure no se comporte de la manera prevista:

* **Configuración del firewall local:** antes de que el equipo pueda tener acceso a la Base de datos SQL de Azure, puede que necesite crear una excepción de firewall en el equipo para el puerto TCP 1433. Si está realizando conexiones dentro del límite de la nube de Azure, es posible que tenga que abrir puertos adicionales. Para más información, vea la sección **SQL Database: fuera frente a dentro**: del artículo [Puertos más allá de 1433 para ADO.NET 4.5 y SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).
* **Traducción de direcciones de red (NAT):** debido a la NAT, la dirección IP usada por su equipo para conectarse a Azure SQL Database puede diferir de la que se muestra en los valores de la configuración de IP del equipo. A fin de ver la dirección IP usada por su equipo para conectarse a Azure, inicie sesión en el portal y vaya a la pestaña **Configurar** del servidor que hospede su base de datos. En la sección **Direcciones IP permitidas**, verá la sección **Dirección IP del cliente actual**. Haga clic en **Agregar** en la opción **Direcciones IP permitidas** para permitir que este equipo tenga acceso al servidor.
* **Los cambios realizados en la lista de permitidos no han surtido efecto todavía:** puede haber un retraso de hasta cinco minutos hasta que los cambios en la configuración del firewall de Azure SQL Database surtan efecto.
* **El inicio de sesión no está autorizado o se ha usado una contraseña incorrecta:** si un inicio de sesión no tiene permisos en el servidor de Azure SQL Database o la contraseña usada es incorrecta, se denegará la conexión al servidor de Azure SQL Database. La creación de una configuración de firewall solo ofrece a los clientes una oportunidad de intentar conectarse al servidor; cada cliente debe ofrecer las credenciales de seguridad necesarias. Para obtener más información sobre la preparación de inicios de sesión, vea Administración de bases de datos, inicios de sesión y usuarios en la Base de datos SQL de Azure.
* **Dirección IP dinámica:** si tiene una conexión a Internet con una direccionamiento IP dinámica y tiene problemas al acceder al firewall, podría intentar una de las siguientes soluciones:
  
  * Pida a su proveedor de acceso a Internet (ISP) el intervalo de direcciones IP asignado a los equipos cliente que acceden al servidor de Azure SQL Database y agréguelo como regla de firewall.
  * Obtenga el direccionamiento IP estático en su lugar para los equipos cliente y luego agregue las direcciones IP como reglas de firewall.

## <a name="next-steps"></a>Pasos siguientes

- Para consultar un inicio rápido sobre cómo crear una base de datos y una regla de firewall de nivel de servidor, vea [Creación de una instancia de Azure SQL Database](sql-database-get-started-portal.md).
- Si desea obtener ayuda para conectarse a una base de datos SQL de Azure desde aplicaciones de código abierto o de terceros, consulte [Ejemplos de código de inicio rápido de cliente para Base de datos SQL](https://msdn.microsoft.com/library/azure/ee336282.aspx).
- Para obtener información sobre los puertos adicionales que puede necesitar abrir, vea la sección **SQL Database: fuera frente a dentro**: del artículo [Puertos más allá de 1433 para ADO.NET 4.5 y SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md)
- Para obtener información general sobre la seguridad de Azure SQL Database, vea [Protección de bases de datos SQL](sql-database-security-overview.md).

<!--Image references-->
[1]: ./media/sql-database-firewall-configure/sqldb-firewall-1.png

