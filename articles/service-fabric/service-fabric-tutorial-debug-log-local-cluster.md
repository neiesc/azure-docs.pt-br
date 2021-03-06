---
title: Depurar um aplicativo Java em um cluster local do Azure Service Fabric | Microsoft Docs
description: Neste tutorial, saiba como depurar e obter logs de um aplicativo Java do Service Fabric em execução em um cluster local.
services: service-fabric
documentationcenter: java
author: suhuruli
manager: mfussell
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: java
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/26/2018
ms.author: suhuruli
ms.custom: mvc
ms.openlocfilehash: c1a8b18062f61be9eb020beefd3ad741c41b55f8
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38652695"
---
# <a name="tutorial-debug-a-java-application-deployed-on-a-local-service-fabric-cluster"></a>Tutorial: Depurar um aplicativo Java implantado em um cluster local do Service Fabric

Este tutorial é a parte dois de uma série. Você aprenderá como anexar um depurador remoto usando o Eclipse para o aplicativo do Service Fabric. Além disso, você aprenderá a redirecionar os logs dos aplicativos em execução para um local conveniente para o desenvolvedor.

Na segunda parte da série, você aprenderá como:
> [!div class="checklist"]
> * Depurar o aplicativo Java usando o Eclipse
> * Redirecionar logs para um local configurável

Nesta série de tutoriais, você aprenderá a:
> [!div class="checklist"]
> * [Compilar um aplicativo Java do Reliable Services no Service Fabric](service-fabric-tutorial-create-java-app.md)
> * Implantar e depurar o aplicativo em um cluster local
> * [Implantar o aplicativo em um cluster do Azure](service-fabric-tutorial-java-deploy-azure.md)
> * [Configurar monitoramento e diagnóstico para o aplicativo](service-fabric-tutorial-java-elk.md)
> * [Configurar CI/CD](service-fabric-tutorial-java-jenkins.md)

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar este tutorial:

* Configurar seu ambiente de desenvolvimento no [Mac](service-fabric-get-started-mac.md) ou no [Linux](service-fabric-get-started-linux.md). Siga as instruções para instalar o plug-in do Eclipse, o Gradle, o SDK do Service Fabric e a CLI do Service Fabric (sfctl).

## <a name="download-the-voting-sample-application"></a>Baixar o aplicativo de exemplo Votação

Se você não tiver criado o aplicativo de exemplo Votação na [parte um esta série de tutoriais](service-fabric-tutorial-create-java-app.md), poderá baixá-lo. Em uma janela de comando, execute o comando a seguir para clonar o repositório de aplicativos de exemplo no computador local.

```bash
git clone https://github.com/Azure-Samples/service-fabric-java-quickstart
```

[Compile e implante](service-fabric-tutorial-create-java-app.md#deploy-application-to-local-cluster) o aplicativo em um cluster de desenvolvimento local.

## <a name="debug-java-application-using-eclipse"></a>Depurar o aplicativo Java usando o Eclipse

1. Abra o IDE do Eclipse em seu computador e clique em **Arquivo -> Importar...**

2. Na janela pop-up, selecione a opção **Geral -> Projetos Existentes no Espaço de trabalho** e pressione Avançar.

3. Na janela Importar Projetos, escolha a opção **Selecionar diretório raiz** e escolha o diretório **Votação**. Se você seguiu a primeira série de tutoriais, o diretório **Votação** estará no diretório **Eclipse-espaço de trabalho**.

4. Atualize entryPoint.sh do serviço que você quer depurar para que ele inicie o processo java com parâmetros de depuração remota. Para este tutorial, o front-end sem estado é usado: *Voting/VotingApplication/VotingWebPkg/Code/entryPoint.sh*. Neste exemplo, a porta 8001 foi definida para depuração.

    ```bash
    java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=n -Djava.library.path=$LD_LIBRARY_PATH -jar VotingWeb.jar
    ```

5. Atualize o Manifesto do Aplicativo, definindo a contagem de instâncias ou a contagem de réplicas para o serviço que está sendo depurado como um. Essa configuração evita conflitos para a porta que é usada para depuração. Por exemplo, para serviços sem estado, defina ``InstanceCount="1"`` e, para serviços com estado, defina o destino e o tamanho mínimo do conjunto de réplicas para 1, como se segue: ``TargetReplicaSetSize="1" MinReplicaSetSize="1"``.

6. No IDE do Eclipse, escolha **Executar -> Configurações de Depuração -> Aplicativo Java Remoto**, pressione o botão **Novo** e defina as propriedades da seguinte maneira e clique em **Aplicar**.

    ```
    Name: Voting
    Project: Voting
    Connection Type: Standard
    Host: localhost
    Port: 8001
    ```

7. Coloque um ponto de interrupção na linha 109 do arquivo *Voting/VotingWeb/src/statelessservice/HttpCommunicationListener.java*.

8. No Package Explorer, clique com botão direito do mouse no projeto **Votação** e clique em **Service Fabric -> Publicar Aplicativo...**

9. Na janela **Publicar Aplicativo**, selecione **Local.json** na lista suspensa e clique em **Publicar**.

10. No IDE do Eclipse, selecione **Executar -> Configurações de Depuração -> Aplicativo Java Remoto**, clique na configuração de **Votação** que você criou e clique em **Depurar**.

11. Acesse seu navegador da Web e acesse **localhost:8080** para atingir o ponto de interrupção, e insira a **Perspectiva de depuração** no Eclipse.

## <a name="redirect-application-logs-to-custom-location"></a>Redirecionar os logs de aplicativo para o local personalizado

As etapas a seguir explicam como redirecionar os logs de aplicativo do local padrão */var/log/syslog* para um local personalizado.

1. Atualmente, os aplicativos em execução em clusters do Linux do Service Fabric dão suporte à seleção de um único arquivo de log. Como resultado, os logs sempre vão para */tmp/mysfapp0.0.log*. Crie um arquivo chamado logging.properties no seguinte local *Voting/VotingApplication/VotingWebPkg/Code/logging.properties* e adicione o seguinte conteúdo.

    ```
    handlers = java.util.logging.FileHandler

    java.util.logging.FileHandler.level = ALL
    java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter

    # This value specifies your custom location. You will have to ensure this path has read and write access by the process running the SF Application
    java.util.logging.FileHandler.pattern = /tmp/mysfapp0.0.log
    ```

2. Adicione o seguinte parâmetro no *Voting/VotingApplication/VotingWebPkg/Code/entryPoint.sh* para o comando de execução de Java:

    ```bash
    -Djava.util.logging.config.file=logging.properties
    ```

    O exemplo a seguir mostra um exemplo de execução:

    ```bash
    java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=n -Djava.library.path=$LD_LIBRARY_PATH -Djava.util.logging.config.file=logging.properties -jar VotingWeb.jar
    ```

Neste estágio, você aprendeu a depurar e acessar seus logs de aplicativo durante o desenvolvimento de seus aplicativos Java do Service Fabric.

## <a name="next-steps"></a>Próximas etapas

Nesta parte do tutorial, você aprendeu a:

> [!div class="checklist"]
> * Depurar o aplicativo Java usando o Eclipse
> * Redirecionar logs para um local configurável

Prosseguir para o próximo tutorial:
> [!div class="nextstepaction"]
> [Implantar aplicativo no Azure](service-fabric-tutorial-java-deploy-azure.md)