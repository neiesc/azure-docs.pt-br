---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: iot-hub
author: robinsh
ms.service: iot-hub
ms.topic: include
ms.date: 11/06/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 4a4ec51430242cc3e3d6a0b801f2b4be7858ab50
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2018
ms.locfileid: "51516061"
---
<!-- put the ## header in the file that includes this file -->

Nesta seção, você cria uma identidade de dispositivo no registro de identidade no hub IoT. Um dispositivo não pode se conectar ao Hub IoT, a menos que ele tenha uma entrada no Registro de identidade. Para obter mais informações, consulte a seção "Registro de identidade" do [guia do desenvolvedor do Hub IoT ](../articles/iot-hub/iot-hub-devguide-identity-registry.md) 

1. No menu de navegação do Hub IoT, abra **Dispositivos IoT** e, em seguida, clique em **Adicionar** para registrar um novo dispositivo no hub IoT.

    ![Criar identidade do dispositivo no portal](./media/iot-hub-include-create-device/create-identity-portal.png)

1. Forneça um nome para o novo dispositivo, como **myDeviceId** e clique em **Salvar**. Essa ação cria uma nova identidade do dispositivo para o Hub IoT.

   ![Adicionar um novo dispositivo](./media/iot-hub-include-create-device/create-a-device.png)

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]


1. Após a criação do dispositivo, abra o dispositivo na lista do painel **Dispositivos IoT**. Copie a **Cadeia de conexão – chave primária** para uso futuro.

    ![Cadeia de conexão de dispositivo](./media/iot-hub-include-create-device/device-details.png)

> [!NOTE]
> O Registro de identidade do Hub IoT armazena apenas as identidades de dispositivo para habilitar o acesso seguro ao Hub IoT. Ele armazena as IDs e as chaves do dispositivo a usar como credenciais de segurança e um sinalizador habilitado/desabilitado que você poderá usar para desabilitar o acesso de um dispositivo individual. Se seu aplicativo precisar armazenar outros metadados específicos do dispositivo, ele deverá usar um repositório específico do aplicativo. Para saber mais, confira [Guia de Desenvolvedor do Hub IoT](../articles/iot-hub/iot-hub-devguide-identity-registry.md).
