---
title: "Configuración de la autenticación con Amazon Web Services | Microsoft Docs"
description: "En este artículo se describe cómo crear y validar una credencial de AWS para los Runbooks en Automatización de Azure y administrar los recursos de AWS."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
keywords: "autenticación AWS, configurar aws"
ms.assetid: b6dde4bb-26ac-4876-9aa9-e586bed30d6b
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/14/2017
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: 81e5e5d56a7e6149409e11aca2e5fdf28d6a7134
ms.lasthandoff: 04/15/2017


---
# <a name="authenticate-runbooks-with-amazon-web-services"></a>Autenticación de Runbooks con Amazon Web Services
La automatización de las tareas comunes con recursos de Amazon Web Services (AWS) se puede realizar con los Runbooks de Automatización de Azure.  Puede automatizar muchas tareas en AWS mediante Runbooks de Automatización exactamente igual que con recursos de Azure.  Todo lo que se necesita son dos cosas:

* Una suscripción a AWS y un conjunto de credenciales.  En concreto, la clave de acceso y la clave secreta de AWS.  Para más información, consulte el artículo [Using AWS Credentials](http://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html)(Uso de credenciales de AWS).
* Una suscripción a Azure y una cuenta de Automatización.  Para más información sobre cómo configurar una cuenta de Azure Automation, consulte el artículo [Configuración de una cuenta de ejecución de Azure](automation-sec-configure-azure-runas-account.md).  

Para realizar la autenticación con AWS, debe especificar un conjunto de credenciales de AWS para autenticar los Runbooks que se ejecutan desde Automatización de Azure. Si ya tiene creada una cuenta de Automatización y quiere usarla para autenticarse con AWS, puede seguir los pasos que se describen en la sección siguiente.  Si desea dedicar una cuenta a runbooks dirigidos a recursos de AWS, primero debe crear una nueva [cuenta de Automation](automation-offering-get-started.md) (omita la opción para crear una entidad de servicio) y, luego, siga los pasos que se indican a continuación.

## <a name="configure-automation-account"></a>Configuración de la cuenta de Automatización
Para que Azure Automation se comunique con AWS, primero deberá recuperar las credenciales de AWS y almacenarlas como recursos en Azure Automation.  Realice los siguientes pasos que se describen en el documento de AWS [Administración de las claves de acceso para la cuenta de AWS](http://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html) y copie el **identificador de clave de acceso** y la **clave de acceso secreta** (también puede descargar el archivo de clave para almacenarlo en un lugar seguro).

Después de crear y copiar las claves de seguridad de AWS, debe crear un recurso de credencial con una cuenta de Azure Automation para almacenarlas de forma segura y hacer referencia a ellas con sus runbooks.  Siga los pasos descritos en la sección**Creación de un nuevo recurso de credencial** del artículo [Recursos de credenciales en Azure Automation](automation-credentials.md#to-create-a-new-credential-asset-with-the-azure-portal) y escriba la información siguiente:

1. En el cuadro **Nombre**, escriba **AWScred** o un valor adecuado que siga las normas de nomenclatura.  
2. En el cuadro **Nombre de usuario**, escriba su **identificador de acceso** y su **clave de acceso secreta** en el cuadro **Contraseña** y **Confirmar contraseña**.   

## <a name="next-steps"></a>Pasos siguientes
* Revise el artículo [Automatizar la implementación de una máquina virtual en Amazon Web Services](automation-scenario-aws-deployment.md) para aprender a crear runbooks y automatizar las tareas en AWS.


