---
title: Registro de Contêiner do Azure - funções e permissões
description: Use o controle de acesso baseado nas função do Azure (RBAC) e o gerenciamento de identidades e acesso (IAM) para fornecer permissões refinadas a recursos em um registro de contêiner do Azure.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 12/17/2018
ms.author: danlep
ms.openlocfilehash: 5dabab02758b4ade0f668adddf7644a654e1804b
ms.sourcegitcommit: 7862449050a220133e5316f0030a259b1c6e3004
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/22/2018
ms.locfileid: "53755781"
---
# <a name="azure-container-registry-roles-and-permissions"></a>Funções e permissões do Registro de Contêiner do Azure

O serviço de Registro de Contêiner do Azure dá suporte a um conjunto de funções do Azure que fornecem níveis diferentes de permissões para um registro de contêiner do Azure. Use o [controle de acesso baseado em função do Azure](../role-based-access-control/index.yml) (RBAC) para atribuir permissões específicas a usuários ou entidades que precisam interagir com um registro de serviço.

| Função/permissão       | [Acessar o Resource Manager](#access-resource-manage)| [Criar/excluir registro](#create/delete-registry) | [Enviar uma imagem por push](#push-image) | [Pull de imagem](#pull-image) | [Alterar políticas](#change-polices) |   [Imagens de entrada](#sign-images)  |
| ---------| --------- | --------- | --------- | --------- | --------- | --------- |
| Proprietário | X | X | X | X | X |  |  
| Colaborador | X | X | X | X | X |  |  
| Leitor | X |  |  | X |  |  | 
| AcrPush |  |  | X | X |  |  |  
| AcrPull |  |  |  | X |  |  |  
| AcrImageSigner |  |  |  |  |  | X |

## <a name="differentiate-users-and-services"></a>Diferenciar usuários e serviços

Qualquer permissão de tempo é aplicada, uma prática recomendada é fornecer o conjunto mais limitado de permissões para uma pessoa ou serviço realizar uma tarefa. Os conjuntos de permissões a seguir representam um conjunto de recursos que podem ser usados por seres humanos e serviços sem periféricos.

### <a name="cicd-solutions"></a>Soluções CI/CD

Ao automatizar `docker build` comandos de soluções CI/CD, você precisa de `docker push` recursos. Para esses cenários de serviço sem periféricos, sugerimos atribuir a função **AcrPush**. Essa função, ao contrário da função mais ampla **Colaborador**, impede que a conta execute utras operações de registro ou acesse o Azure Resource Manager.

### <a name="container-host-nodes"></a>Nós de host do contêiner

Da mesma forma, nós que executam seus contêineres precisam da função **AcrPull**, mas não devem exibir os recursos de **Leitor**.

### <a name="visual-studio-code-docker-extension"></a>Extensão do Visual Studio Code Docker

Para ferramentas como o Visual Studio Code [extensão Docker](https://code.visualstudio.com/docs/azure/docker), é necessário acesso de provedor de recursos adicionais para listar os registros de contêiner do Azure disponíveis. Nesse caso, forneça aos usuários acesso para a função de **Leitor** ou **Colaborador**. Essas funções permitem `docker pull`, `docker push`, `az acr list`, `az acr build`e outros recursos. 

## <a name="access-resource-manager"></a>Acessar o Resource Manager

O Acesso ao Azure Resource Manager é necessário para o portal do Azure e [CLI do Azure](/cli/azure/). Por exemplo, para obter uma lista de registros usando o `az acr list` comando, você precisa dessa permissão definida. 

## <a name="createdelete-registry"></a>Criar/excluir registro

A capacidade de criar e excluir registros de contêiner do Azure.

## <a name="push-image"></a>Enviar uma imagem por push

A capacidade de `docker push` uma imagem, ou enviar outro artefato com suporte, para um registro. Requer [autenticação](container-registry-authentication.md) com o registro usando a identidade autorizada. 

## <a name="pull-image"></a>Pull de imagem

A capacidade de `docker pull` imagem de não quarentena ou pull de outro artefato com suporte, de um registro. Requer [autenticação](container-registry-authentication.md) com o registro usando a identidade autorizada.

## <a name="change-policies"></a>Alterar políticas

A capacidade de configurar políticas em um registro. Políticas incluem a limpeza de imagem, habilitando a quarentena e assinatura de imagem.

## <a name="sign-images"></a>Imagens de entrada

A capacidade de imagens de entrada normalmente é atribuída a um processo automatizado, que usaria uma entidade de serviço. Normalmente, essa permissão é combinada com [imagem por push](#push-image) para permitir o envio de uma imagem confiável para um registro. Para obter detalhes, consulte [Confiança de conteúdo no Registro de Contêiner do Azure](container-registry-content-trust.md).

## <a name="next-steps"></a>Próximas etapas

* Saiba mais sobre como atribuir funções RBAC para uma identidade do Azure usando o [portal do Azure](../role-based-access-control/role-assignments-portal.md), [CLI do Azure](../role-based-access-control/role-assignments-cli.md), ou outras ferramentas do Azure.

* Saiba mais sobre [opções de autenticação](container-registry-authentication.md) para Registro de Contêiner do Azure.
