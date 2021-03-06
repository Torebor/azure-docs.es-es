---
title: "VS Code: conexión con Azure SQL Database y consulta de datos | Microsoft Docs"
description: "Aprenda a conectarse a SQL Database en Azure con Visual Studio Code. A continuación, ejecute instrucciones de Transact-SQL (T-SQL) para consultar y editar los datos."
metacanonical: 
keywords: "conexión a sql database"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 676bd799-a571-4bb8-848b-fb1720007866
ms.service: sql-database
ms.custom: quick start manage
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 04/17/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 8c4e33a63f39d22c336efd9d77def098bd4fa0df
ms.openlocfilehash: 45405c7bb9993d1fd529b25b599c3cd7f459843c
ms.lasthandoff: 04/20/2017


---
# <a name="azure-sql-database-use-visual-studio-code-to-connect-and-query-data"></a>Azure SQL Database: uso de Visual Studio Code para conectar y consultar datos

[Visual Studio Code](https://code.visualstudio.com/docs) es un editor de código gráfico para Linux, Mac OS y Windows que admite extensiones, incluidas la [extensión mssql](https://aka.ms/mssql-marketplace) para realizar consultas en Microsoft SQL Server, Azure SQL Database y SQL Data Warehouse. Este inicio rápido muestra cómo usar Visual Studio Code para conectarse a una base de datos de SQL Azure Database, para después usar las instrucciones Transact-SQL para consultar, insertar, actualizar y eliminar datos en la base de datos.

En esta guía de inicio rápido se utilizan como punto de partida los recursos creados en una de las siguientes guías:

- [Creación de la base de datos: Azure Portal](sql-database-get-started-portal.md)
- [Creación de la base de datos: CLI](sql-database-get-started-cli.md)

Antes de empezar, asegúrese de que tiene instalada la versión más reciente de [Visual Studio Code](https://code.visualstudio.com/Download) y cargue la [extensión mssql](https://aka.ms/mssql-marketplace). Para obtener instrucciones de instalación para la extensión mssql, vea [Install VS Code](https://docs.microsoft.com/sql/linux/sql-server-linux-develop-use-vscode#install-vs-code) (Instalación de VS Code) y [mssql for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql) (mssql para Visual Studio Code). 

## <a name="configure-vs-code"></a>Configuración del código de VS 

### <a name="mac-os"></a>**Mac OS**
Para macOS, debe instalar OpenSSL, que es un requisito previo para DotNet Core que la extensión mssql utiliza. Abra el terminal y escriba los siguientes comandos para instalar **brew** y **OpenSSL**. 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install openssl
mkdir -p /usr/local/lib
ln -s /usr/local/opt/openssl/lib/libcrypto.1.0.0.dylib /usr/local/lib/
ln -s /usr/local/opt/openssl/lib/libssl.1.0.0.dylib /usr/local/lib/
```

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**

No se necesita ninguna configuración especial.

### <a name="windows"></a>**Windows**

No se necesita ninguna configuración especial.

## <a name="get-connection-information"></a>Obtención de información sobre la conexión

Obtención de la información de conexión necesaria para conectarse a Azure SQL Database. En los procedimientos siguientes, necesitará el nombre completo del servidor, el nombre de la base de datos y la información de inicio de sesión.

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com/).
2. Seleccione **Bases de datos SQL** en el menú de la izquierda y haga clic en la base de datos en la página **Bases de datos SQL**. 
3. En la página **Introducción** de la base de datos, revise el nombre completo del servidor tal como se muestra en la siguiente imagen. Mantenga el puntero sobre el nombre del servidor hasta que aparezca la opción **Haga clic para copiar**.

   ![información sobre la conexión](./media/sql-database-connect-query-ssms/connection-information.png) 

4. Si ha olvidado la información de inicio de sesión para el servidor de Azure SQL Database, navegue a la página del servidor de SQL Database para ver el nombre del Administrador de servidor y, si es necesario, restablecer la contraseña. 

## <a name="set-language-mode-to-sql"></a>Definición del modo de lenguaje en SQL

El modo de lenguaje está establecido en **SQL** en Visual Studio Code para habilitar comandos mssql y T-SQL IntelliSense.

1. Abra una nueva ventana de Visual Studio Code. 

2. Haga clic en **Texto sin formato** en la esquina inferior derecha de la barra de estado.
3. En menú desplegable **Seleccionar modo de lenguaje** que se abre, escriba **SQL** y, a continuación, presione **ENTRAR** para establecer el modo de lenguaje en SQL. 

   ![Modo de lenguaje SQL](./media/sql-database-connect-query-vscode/vscode-language-mode.png)

## <a name="connect-to-your-database-in-the-sql-database-logical-server"></a>Conexión con la base de datos en el servidor lógico de SQL Database

Use Visual Studio Code para establecer una conexión con el servidor de Azure SQL Database.

> [!IMPORTANT]
> Antes de continuar, asegúrese de que están preparados el servidor, la base de datos y la lista de información de inicio de sesión. Cuando comience a escribir la información de perfil de conexión, si cambia el foco de Visual Studio Code, tendrá que reiniciar la creación del perfil de conexión.
>

1. En VS Code, presione **CTRL + MAYÚS + P** (o **F1**) para abrir la paleta de comandos.

2. Escriba **sqlcon** y presione **ENTRAR**.

3. Presione **ENTRAR** para seleccionar **Create Connection Profile** (Crear perfil de conexión). Al hacerlo, se creará un perfil de conexión para la instancia de SQL Server.

4. Siga las indicaciones para especificar las propiedades de conexión para el nuevo perfil de conexión. Después de especificar cada valor, presione **ENTRAR** para continuar. 

   En la tabla siguiente se describen las propiedades del perfil de conexión.

   | Configuración | Descripción |
   |-----|-----|
   | **Nombre del servidor** | Escriba el nombre completo del servidor, como **mynewserver20170313.database.windows.net** |
   | **Nombre de la base de datos** | Escriba el nombre de la base de datos, como **mySampleDatabase** |
   | **Autenticación** | Seleccione Inicio de sesión de SQL. |
   | **Nombre de usuario** | Escriba la cuenta de administrador del servidor. |
   | **Contraseña (Inicio de sesión de SQL)** | Escriba la contraseña de la cuenta de administrador del servidor. | 
   | **¿Guardar la contraseña?** | Seleccione **Sí** o **No**. |
   | **[Opcional] Enter a name for this profile** (Escribir un nombre para este perfil) | Escriba un nombre de perfil de conexión, como **mySampleDatabase**. 

5. Presione la tecla **ESC** para cerrar el mensaje de información que indica que el perfil se ha creado y conectado.

6. Compruebe la conexión en la barra de estado.

   ![Estado de la conexión](./media/sql-database-connect-query-vscode/vscode-connection-status.png)

## <a name="query-data"></a>Datos de consulta

Utilice el código siguiente para consultar los 20 primeros productos por categoría con la instrucción Transact-SQL [SELECT](https://msdn.microsoft.com/library/ms189499.aspx).

1. En la ventana del **editor**, escriba la siguiente consulta en la ventana de consulta vacía:

   ```sql
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM [SalesLT].[ProductCategory] pc
   JOIN [SalesLT].[Product] p
   ON pc.productcategoryid = p.productcategoryid;
   ```

2. Presione **CTRL + MAYÚS + E** para recuperar datos de las tablas Product y ProductCategory.

    ![Consultar](./media/sql-database-connect-query-vscode/query.png)

## <a name="insert-data"></a>Insertar datos

Utilice el código siguiente para insertar un nuevo producto en la tabla SalesLT.Product con la instrucción Transact-SQL [INSERT](https://msdn.microsoft.com/library/ms174335.aspx).

1. En la ventana del **editor**, elimine la consulta anterior y escriba la siguiente consulta:

   ```sql
   INSERT INTO [SalesLT].[Product]
           ( [Name]
           , [ProductNumber]
           , [Color]
           , [ProductCategoryID]
           , [StandardCost]
           , [ListPrice]
           , [SellStartDate]
           )
     VALUES
           ('myNewProduct'
           ,123456789
           ,'NewColor'
           ,1
           ,100
           ,100
           ,GETDATE() );
   ```

2. Presione **CTRL + MAYÚS + E** para insertar una nueva fila en la tabla Product.

## <a name="update-data"></a>Actualización de datos

Utilice el código siguiente para actualizar el nuevo producto que ha agregado anteriormente con la instrucción Transact-SQL [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx).

1.  En la ventana del **editor**, elimine la consulta anterior y escriba la siguiente consulta:

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. Presione **CTRL + MAYÚS + E** para actualizar la fila especificada en la tabla Product.

## <a name="delete-data"></a>Eliminación de datos

Utilice el código siguiente para eliminar el nuevo producto que ha agregado anteriormente con la instrucción Transact-SQL [DELETE](https://msdn.microsoft.com/library/ms189835.aspx).

1. En la ventana del **editor**, elimine la consulta anterior y escriba la siguiente consulta:

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. Presione **CTRL + MAYÚS + E** para eliminar la fila especificada en la tabla Product.

## <a name="next-steps"></a>Pasos siguientes

- Para conectarse y consultar mediante SQL Server Management Studio, consulte el artículo de [Conexión y consultas con SSMS](sql-database-connect-query-ssms.md).
- Para conectarse y consultar con .NET, vea [Conexión y consultas con .NET](sql-database-connect-query-dotnet.md).
- Para conectarse y consultar con PHP, vea [Conexión y consultas con PHP](sql-database-connect-query-php.md).
- Para conectarse y consultar con Node.js, vea [Conexión y consultas con Node.js](sql-database-connect-query-nodejs.md).
- Para conectarse y consultar con Java, vea [Conexión y consultas con Java](sql-database-connect-query-java.md).
- Para conectarse y consultar con Python, vea [Conexión y consultas con Python](sql-database-connect-query-python.md).
- Para conectarse y consultar con Ruby, vea [Conexión y consultas con Ruby](sql-database-connect-query-ruby.md).

