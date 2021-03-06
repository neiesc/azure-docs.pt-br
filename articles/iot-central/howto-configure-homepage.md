---
title: Configurar a home page do seu aplicativo Azure IoT Central | Microsoft Docs
description: Como construtor, saiba como configurar a home page do seu aplicativo Azure IoT Central.
author: dominicbetts
ms.author: dobett
ms.date: 07/10/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: a03ac0ef66f4ffdce53d0bd2a35839bbe1615d0b
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54199078"
---
# <a name="configuring-homepage"></a>Configurando a home page

A home page é a página carregada quando os usuários que têm acesso navegam até a URL do aplicativo. Se você tiver selecionado os modelos de aplicativo "Exemplo Contoso" ou "Exemplo Devkits" ao criar seu aplicativo, ele terá Home pages predefinidas. Se, por outro lado, tiver selecionado o modelo de aplicativo "Aplicativo personalizado", sua home page ficará em branco.

Por exemplo, veja a home page dos aplicativos com base no modelo "Exemplo Contoso". Para personalizar a home page para seu aplicativo, primeiro selecione **Editar** no canto superior direito. 

![Home page de aplicativos com base no modelo "Exemplo Contoso"](media/howto-configure-homepage/image1.png)

Selecionar **Editar**, abrirá a biblioteca de dashboard em um painel à esquerda. Há muitos tipos de títulos e primitivos de painel que podem ser adicionados para personalizar sua home page.

![Biblioteca de painéis](media/howto-configure-homepage/image2.png)

Por exemplo, é possível adicionar um bloco **Configurações e Propriedades** para mostrar uma seleção dos valores atuais de configurações e propriedades. Para fazer isso, primeiro selecione um **Modelo de dispositivo**, em seguida, selecione uma **Instância de dispositivo**. Depois disso, dê um título ao título e selecione uma **Configuração** ou uma **Propriedade** para exibir. Nesse caso, selecionamos **Definir temperatura**. Clicar em **Pronto** fará com que essa peça apareça na home page.

![Formulário "Configurar detalhes do dispositivo" com detalhes para configurações e propriedades](media/howto-configure-homepage/image3.png)

Agora, quando o operador exibir a home page, ele poderá ver esse título que exibe as propriedades ou configurações do dispositivo:

![Guia "Painel" com as configurações e propriedades exibidas para o bloco](media/howto-configure-homepage/image4.png)

Familiarize-se com vários outros tipos de blocos na biblioteca para descobrir como é possível personalizar a home page do seu aplicativo ainda mais.

## <a name="next-steps"></a>Próximas etapas

Agora que você aprendeu como configurar a home page do Azure IoT Central, é possível:

> [!div class="nextstepaction"]
> [Saiba como preparar e carregar imagens](howto-prepare-images.md)
