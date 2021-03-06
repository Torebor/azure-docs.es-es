---
title: "Tutorial: Integración de Azure Active Directory con AnswerHub | Microsoft Docs"
description: "Aprenda a usar AnswerHub con Azure Active Directory para habilitar el inicio de sesión único, el aprovisionamiento automatizado, etc."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 818b91d7-01df-4b36-9706-f167c710a73c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/17/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: ff8a2bc6350be35fa368e11d2ebd4a994f01cedc
ms.openlocfilehash: af99741f5f5f8b2fb1c4fc8975571c65c2fc98c6
ms.lasthandoff: 02/28/2017


---
# <a name="tutorial-azure-active-directory-integration-with-answerhub"></a>Tutorial: Integración de Azure Active Directory con AnswerHub
El objetivo de este tutorial es mostrar la integración de Azure y [AnswerHub](http://www.dzonesoftware.com/products/answerhub-question-answer-software).  
En la situación descrita en este tutorial se supone que ya cuenta con los elementos siguientes:

* Una suscripción de Azure válida
* Una suscripción habilitada para el inicio de sesión único en [AnswerHub](http://www.dzonesoftware.com/products/answerhub-question-answer-software)

Después de completar este tutorial, los usuarios de Azure AD que ha asignado a AnswerHub podrán acceder a la aplicación mediante el inicio de sesión único (SSO) en el sitio de la compañía de AnswerHub (inicio de sesión iniciado por el proveedor de servicios) o con la información del artículo de [introducción al panel de acceso](active-directory-saas-access-panel-introduction.md).

La situación descrita en este tutorial consta de los siguientes bloques de creación:

1. Habilitación de la integración de aplicaciones para AnswerHub
2. Configuración del inicio de sesión único (SSO)
3. Configuración del aprovisionamiento de usuario
4. Asignación de usuarios

![Escenario](./media/active-directory-saas-answerhub-tutorial/IC785165.png "Escenario")

## <a name="enable-the-application-integration-for-answerhub"></a>Habilitación de la integración de aplicaciones para AnswerHub
El objetivo de esta sección es describir cómo se habilita la integración de aplicaciones para AnswerHub.

**Siga estos pasos para habilitar la integración de aplicaciones para AnswerHub:**

1. En el panel de navegación izquierdo del Portal de Azure clásico, haga clic en **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-answerhub-tutorial/IC700993.png "Active Directory")
2. En la lista **Directory** , seleccione el directorio cuya integración desee habilitar.
3. Para abrir la vista de aplicaciones, haga clic en **Applications** , en el menú superior de la vista de directorios.
   
   ![Aplicaciones](./media/active-directory-saas-answerhub-tutorial/IC700994.png "Aplicaciones")
4. Haga clic en **Agregar** en la parte inferior de la página.
   
   ![Agregar aplicaciones](./media/active-directory-saas-answerhub-tutorial/IC749321.png "Agregar aplicaciones")
5. En el cuadro de diálogo **¿Qué desea hacer?**, haga clic en **Agregar una aplicación de la galería**.
   
   ![Agregar una aplicación de la galería](./media/active-directory-saas-answerhub-tutorial/IC749322.png "Agregar una aplicación de la galería")
6. En el **cuadro de búsqueda**, escriba **AnswerHub**.
   
   ![Galería de aplicaciones](./media/active-directory-saas-answerhub-tutorial/IC785166.png "Galería de aplicaciones")
7. En el panel de resultados, seleccione **AnswerHub** y, luego, haga clic en **Completar** para agregar la aplicación.
   
   ![AnswerHub](./media/active-directory-saas-answerhub-tutorial/IC785167.png "AnswerHub")

## <a name="configure-single-sign-on"></a>Configurar inicio de sesión único
El objetivo de esta sección es describir cómo habilitar usuarios para que se autentiquen en AnswerHub con su cuenta de Azure AD mediante federación basada en el protocolo SAML.  

Como parte de este procedimiento, es necesario crear un archivo de certificado codificado en base&64;. Si no está familiarizado con este procedimiento, consulte [Conversión de un certificado binario en un archivo de texto](http://youtu.be/PlgrzUZ-Y1o).

**Siga estos pasos para configurar el inicio de sesión único:**

1. En el Portal de Azure clásico, en la página de integración de aplicaciones de **AnswerHub**, haga clic en **Configurar inicio de sesión único** para abrir el cuadro de diálogo **Configurar inicio de sesión único**.
   
   ![Configurar inicio de sesión único](./media/active-directory-saas-answerhub-tutorial/IC785168.png "Configurar inicio de sesión único")
2. En la página **¿Cómo desea que los usuarios inicien sesión en AnswerHub?**, seleccione **Inicio de sesión único de Microsoft Azure AD** y haga clic en **Siguiente**.
   
   ![Configurar inicio de sesión único](./media/active-directory-saas-answerhub-tutorial/IC785169.png "Configurar inicio de sesión único")
3. En la página **Configurar dirección URL de la aplicación**, en el cuadro de texto **URL de inicio de sesión en AnswerHub**, escriba su dirección URL con el siguiente patrón "*https://company.answerhub.com*" y haga clic en **Siguiente**.
   
   ![Configurar dirección URL de la aplicación](./media/active-directory-saas-answerhub-tutorial/IC785170.png "Configurar dirección URL de la aplicación")
4. En la página **Configurar inicio de sesión único en AnswerHub**, para descargar el certificado, haga clic en **Descargar certificado** y guarde el archivo de certificado en el equipo.
   
   ![Configurar inicio de sesión único](./media/active-directory-saas-answerhub-tutorial/IC785171.png "Configurar inicio de sesión único")
5. En otra ventana del explorador web, inicie sesión en el sitio de la compañía de AnswerHub como administrador.
   
   >[!NOTE]
   >Si necesita ayuda para configurar AnswerHub, póngase en contacto con el [equipo de soporte técnico de AnswerHub](mailto:success@answerhub.com.).
   > 
6. Vaya a **Administración**.
7. Haga clic en la pestaña **Usuario y grupo** .
8. En el panel de navegación izquierdo, en la sección **Social Settings** (Configuración social), haga clic en **SAML Setup** (Configuración de SAML).
9. Haga clic en la pestaña **Configuración de IDP** .
10. En la pestaña **Configuración de IDP** , lleve a cabo estos pasos:

  ![Configuración de SAML](./media/active-directory-saas-answerhub-tutorial/IC785172.png "Configuración de SAML")  
  1. En el Portal de Azure clásico, en la página de diálogo **Configurar inicio de sesión único en AnswerHub**, copie el valor de **Dirección URL de inicio de sesión remoto** y péguelo en el cuadro de texto **IDP Login URL** (Dirección URL de inicio de sesión de IDP).
  2. En el Portal de Azure clásico, en la página de diálogo **Configurar inicio de sesión único en AnswerHub**, copie el valor de **Dirección URL de cierre de sesión remoto** y péguelo en el cuadro de texto **IDP Logout URL** (Dirección URL de cierre de sesión de IDP).
  3. En el Portal de Azure clásico, en la página de diálogo **Configurar inicio de sesión único en AnswerHub**, copie el valor de **Formato de identificador de nombre** y péguelo en el cuadro de texto **IDP Name Identifier Format** (Formato de identificador de nombre de IDP).
  4. Haga clic en **Claves y certificados**.
11. En la pestaña Claves y certificados, realice los pasos siguientes:
    
  ![Claves y certificados](./media/active-directory-saas-answerhub-tutorial/IC785173.png "Claves y certificados")  
  1. Cree un archivo **codificado en base&64;** a partir del certificado descargado.
  
    >[!TIP]
    >Para más información, vea [Conversión de un certificado binario en un archivo de texto](http://youtu.be/PlgrzUZ-Y1o). 
    > 
  2. Abra el certificado codificado en base&64; en el Bloc de notas, copie su contenido en el portapapeles y luego péguelo en el cuadro de texto **Clave pública de IDP (formato x509)** .
  3. Haga clic en **Guardar**.
12. En la pestaña **IDP Config** (Configuración de IDP), haga clic en **Save** (Guardar).
13. En el Portal de Azure clásico, seleccione la confirmación de configuración de inicio de sesión único y haga clic en **Completar** para cerrar el cuadro de diálogo **Configurar inicio de sesión único**.
    
  ![Configurar inicio de sesión único](./media/active-directory-saas-answerhub-tutorial/IC785174.png "Configurar inicio de sesión único")

## <a name="configure-user-provisioning"></a>Configurar aprovisionamiento de usuarios
Para permitir que los usuarios de Azure AD inicien sesión en AnswerHub, tienen que aprovisionarse en AnswerHub.  

* En el caso de AnswerHub, el aprovisionamiento es una tarea manual.

**Siga estos pasos para configurar el aprovisionamiento de usuario:**

1. Inicie sesión en el sitio de la compañía de **AnswerHub** como administrador.
2. Vaya a **Administración**.
3. Haga clic en la pestaña **Users & Groups** (Usuarios y grupos).
4. En el panel de navegación izquierdo, en la sección **Manage Users** (Administrar usuarios), haga clic en **Create or import users** (Crear o importar usuarios).
   
   ![Usuarios y grupos](./media/active-directory-saas-answerhub-tutorial/IC785175.png "Usuarios y grupos")
5. Escriba la **dirección de correo electrónico**, el **nombre de usuario** y la **contraseña** de una cuenta de Azure Active Directory válida que desee aprovisionar en los cuadros de texto relacionados y haga clic en **Save** (Guardar).

>[!NOTE]
>Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de AnswerHub ofrecida por AnswerHub para aprovisionar cuentas de usuario de AAD.
> 

## <a name="assign-users"></a>Asignar usuarios
Para probar la configuración, debe conceder acceso a los usuarios de Azure AD a los que quiere permitir el uso de su aplicación.

**Para asignar usuarios a AnswerHub, lleve a cabo los siguientes pasos:**

1. En el Portal de Azure clásico, cree una cuenta de prueba.
2. En la página de integración de aplicaciones de **AnswerHub**, haga clic en **Asignar usuarios**.
   
   ![Asignar usuarios](./media/active-directory-saas-answerhub-tutorial/IC785176.png "Asignar usuarios")
3. Seleccione su usuario de prueba, haga clic en **Asignar** y en **Sí** para confirmar la asignación.
   
   ![Sí](./media/active-directory-saas-answerhub-tutorial/IC767830.png "Sí")

Si desea probar la configuración de inicio de sesión único, abra el Panel de acceso. Para obtener más información sobre el Panel de acceso, vea [Introducción al Panel de acceso](active-directory-saas-access-panel-introduction.md).


