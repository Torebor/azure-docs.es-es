---
title: 'Azure Active Directory B2C: notas del desarrollador acerca del uso de directivas personalizadas|Microsoft Docs'
description: Notas para los desarrolladores que configuran y mantienen B2C con directivas personalizadas
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 05/05/2017
ms.author: joroja
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: 72447a234c5889f36b10b828d28775fc5995c4d4
ms.contentlocale: es-es
ms.lasthandoff: 05/09/2017


---
# <a name="release-notes-for-azure-active-directory-b2c-custom-policy-public-preview"></a>Notas de la versión preliminar de la directiva personalizada de Azure Active Directory B2C
El conjunto de características de la **directiva personalizada** está disponible para su evaluación en versión preliminar pública para todos los clientes de Azure Active Directory B2C (Azure AD B2C).  Este conjunto de características está destinado a desarrolladores de identidades avanzados que crean las soluciones de identidad más complejas.  

En la actualidad, este conjunto de características requiere que el desarrollador configure directamente el marco de experiencia de identidad (IEF) directamente a través de la edición de archivos XML.  Este método de configuración es eficaz y complejo.  Los desarrolladores de identidades avanzados que usen el IEF deben planear invertir un tiempo en aprenderlo, para lo que deben completar los tutoriales y leer los distintos documentos de referencia. 

## <a name="features-included-in-this-public-preview"></a>Características que se incluyen en la versión preliminar pública
Las nuevas características introducidas en la versión preliminar pública permiten a los desarrolladores realizar las siguientes tareas:
1. Crear y cargar recorridos de usuarios de autenticación personalizada mediante directivas personalizadas
* Describir los recorridos de usuarios paso a paso como intercambios entre proveedores de notificaciones
* Definir la creación de ramas condicional en recorridos de usuarios
2. Integrar servicios con la API de REST habilitada en los recorridos de usuarios de autenticación personalizada
3. Agregar federación con proveedores de identidades compatibles con el estándar de OpenIDConnect
4. Agregar federación con proveedores de identidades que cumplen el protocolo SAML 2.0



## <a name="terms-of-the-public-preview"></a>Términos de la versión preliminar pública
1. Se aconseja usar las nuevas características solo con fines de evaluación
2. Las nuevas características no están pensadas para que se usen en entornos de producción
3. Los Acuerdos de Nivel de Servicio no se aplican a las nuevas características. Las solicitudes de soporte técnico pueden enviarse a través de los canales de soporte técnico habituales.
5. No se puede garantizar ninguna fecha de disponibilidad general.
6. Microsoft se reserva el derecho, a su entera discreción y por cualquier motivo, de marcar y rechazar o restringir aquellos escenarios y recorridos de usuarios que superen el ámbito de la carta de producto de Azure AD B2C para actuar como plataforma de administración de acceso e identidad de clientes (CIAM)

## <a name="responsibilities-for-custom-policy-feature-set-developers"></a>Responsabilidades de los desarrolladores de conjunto de características de directivas personalizadas
La configuración manual de directivas concede un acceso de nivel inferior a la plataforma subyacente de Azure AD B2C y da como resultado la creación de un marco de confianza único y totalmente personalizable.  Las permutaciones casi infinitas de proveedores de identidades personalizados, las relaciones de confianza, las integraciones con servicios externos y los flujos de trabajo paso a paso exigen más a los desarrolladores avanzados que los consumen.
Para aprovechar completamente la versión preliminar pública, se recomienda que los desarrolladores consuman el conjunto de características de directivas personalizadas cumplan lo siguiente:
* Conocer el lenguaje de configuración de Identity Experience Engine (IEE) y la administración de claves/secretos.
* Tomar posesión de escenarios e integraciones personalizadas.
* Realizar pruebas metódicas de los escenarios.
* Seguir los procedimientos recomendados de desarrollo de software y almacenamiento provisional con un mínimo de un desarrollo o una prueba, y un entorno de producción.
* Mantenerse informado acerca de los nuevos desarrollos de los proveedores de identidades y los servicios con los que se integran; por ejemplo, realizar un seguimiento de los cambios de los secretos, de los cambios programados o sin programar en el servicio, etc. (configurar una supervisión activa y supervisar la capacidad de respuesta de sus entornos de producción).
* Mantener actualizados los mensajes de correo electrónico de contacto y responder a los correos mensajes de correo electrónico del equipo del sitio activo de Microsoft
* Realizar las acciones pertinentes cuando se lo indique el equipo del sitio activo de Microsoft. 


>[!NOTE]
>Estas características pueden llegar a incluirse en las directivas integradas de Azure AD, lo que hace que todos los desarrolladores puedan acceder más fácilmente a ellas.

## <a name="next-steps"></a>Pasos siguientes
[Introducción a las directivas personalizadas](active-directory-b2c-get-started-custom.md).

