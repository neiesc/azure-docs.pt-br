---
title: Solução de monitoramento remoto adicionar dispositivo de borda - Azure | Microsoft Docs
description: Este artigo descreve como adicionar um dispositivo IoT Edge a um acelerador de solução de monitoramento remoto
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/09/2018
ms.topic: conceptual
ms.openlocfilehash: 67bfde828287d9892ad404f3d950dbe373503a56
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/16/2018
ms.locfileid: "51826964"
---
# <a name="add-an-iot-edge-device-to-your-remote-monitoring-solution-accelerator"></a>Adicionar um dispositivo IoT Edge ao seu acelerador de solução de monitoramento remoto

Para adicionar um dispositivo [ IoT Edge ](../iot-edge/about-iot-edge.md) ao seu acelerador de solução, conclua as duas etapas a seguir:

1. Adicione o dispositivo Edge na página **Dispositivos** na interface da Web da web do acelerador de solução do Remote Monitoring.
1. Instale o tempo de execução IoT Edge no seu dispositivo Edge.

## <a name="add-the-iot-edge-device"></a>Adicionar o dispositivo do IoT Edge

Para adicionar um dispositivo do IoT Edge ao acelerador de solução de Monitoramento Remoto, navegue até a página **Dispositivos** na interface do usuário Web e clique em **+ Novo dispositivo**.

No painel **Novo dispositivo**, escolha **dispositivo IoT Edge**. Deixe os valores padrão para as outras configurações. Em seguida, clique em **Aplicar**:

![Adicionar dispositivo do IoT Edge](media/iot-accelerators-remote-monitoring-add-edge-device/addedgedevice.png)

### <a name="alternative-ways-to-add-an-iot-edge-device"></a>Maneiras alternativas para adicionar um dispositivo IoT Edge

Também é possível registrar um dispositivo IoT Edge diretamente com a instância do IoT Hub no acelerador de solução. Você precisa saber o nome do hub IoT no acelerador de solução antes de seguir qualquer um desses guias:

- [Registre um novo dispositivo do Azure IoT Edge no portal do Azure](../iot-edge/how-to-register-device-portal.md)
- [Registrar um novo dispositivo IoT Edge do Azure com CLI do Azure](../iot-edge/how-to-register-device-cli.md)
- [Registrar um novo dispositivo IoT Edge do Azure do Visual Studio Code](../iot-edge/how-to-register-device-vscode.md)

Quando você registra um dispositivo diretamente com o hub IoT no acelerador de solução de monitoramento remoto, ele é listado na página **Dispositivos** na interface do usuário da web.

## <a name="install-the-iot-edge-runtime"></a>Instalar o tempo de execução do Azure IoT Edge

Antes de implantar módulos no seu dispositivo Edge, você deve instalar o tempo de execução do IoT Edge no dispositivo físico. Os guias de instruções a seguir mostram como instalar o tempo de execução em plataformas de dispositivos comuns:

- [Instalar o tempo de execução do Azure IoT Edge no Linux (x64)](../iot-edge/how-to-install-iot-edge-linux.md)
- [Instalar o tempo de execução do Azure IoT Edge no Linux (ARM32v7/armhf)](../iot-edge/how-to-install-iot-edge-linux-arm.md)
- [Instalar o tempo de execução do Azure IoT Edge no Windows para usar com contêineres do Windows](../iot-edge/how-to-install-iot-edge-windows-with-windows.md)
- [Instalar o tempo de execução do Azure IoT Edge no Windows para usar com contêineres do Linux](../iot-edge/how-to-install-iot-edge-windows-with-linux.md)
- [Instalar o tempo de execução do IoT Edge no Windows IoT Core](../iot-edge/how-to-install-iot-core.md)

## <a name="next-steps"></a>Próximas etapas

Agora que você preparou seu dispositivo IoT Edge, a próxima etapa é implantar módulos nele. Consulte [importar um pacote do IoT Edge para o solution accelerator de monitoramento remoto](iot-accelerators-remote-monitoring-import-edge-package.md)
