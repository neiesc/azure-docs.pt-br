---
title: 'Python: operações de sistema de arquivos no Azure Data Lake Storage Gen1 | Microsoft Docs'
description: Saiba como usar o SDK do Python para trabalhar com o sistema de arquivos do Data Lake Storage Gen1.
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: 33abaf7488579a501dc7e2d0b63645726b86c28b
ms.sourcegitcommit: 794bfae2ae34263772d1f214a5a62ac29dcec3d2
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44390715"
---
# <a name="filesystem-operations-on-azure-data-lake-storage-gen1-using-python"></a>Operações de sistema de arquivos no Azure Data Lake Storage Gen1 usando Python
> [!div class="op_single_selector"]
> * [SDK .NET](data-lake-store-data-operations-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [API REST](data-lake-store-data-operations-rest-api.md)
> * [Python](data-lake-store-data-operations-python.md)
>
> 

Neste artigo, você aprenderá a usar o SDK do Python para executar operações de sistema de arquivos no Azure Data Lake Storage Gen1. Para obter instruções sobre como executar operações de gerenciamento de conta no Data Lake Storage Gen1 usando o Python, veja [Operações de gerenciamento de conta no Data Lake Storage Gen1 usando o Python](data-lake-store-get-started-python.md).

## <a name="prerequisites"></a>Pré-requisitos

* **Python**. Você pode baixar o Python [aqui](https://www.python.org/downloads/). Este artigo usa o Python 3.6.2.

* **Uma assinatura do Azure**. Consulte [Obter a avaliação gratuita do Azure](https://azure.microsoft.com/pricing/free-trial/).

* **Conta do Azure Data Lake Storage Gen1**. Siga as instruções em [Introdução ao Azure Data Lake Storage Gen1 usando o portal do Azure](data-lake-store-get-started-portal.md).

## <a name="install-the-modules"></a>Instalar os módulos

Para trabalhar com o Data Lake Storage Gen1 usando o Python, você precisa instalar três módulos.

* O módulo `azure-mgmt-resource`, que inclui módulos do Azure para o Active Directory etc.
* O módulo `azure-mgmt-datalake-store`, que inclui as operações de gerenciamento de conta do Azure Data Lake Storage Gen1. Para obter mais informações sobre este módulo, confira a [referência do módulo azure-datalake-store](https://docs.microsoft.com/python/api/azure.mgmt.datalake.store?view=azure-python).
* O módulo `azure-datalake-store`, que inclui as operações do sistema de arquivos do Azure Data Lake Storage Gen1. Para obter mais informações sobre este módulo, confira a [referência do módulo de sistema de arquivos azure-datalake-store](http://azure-datalake-store.readthedocs.io/en/latest/).

Use os comandos a seguir para instalar os módulos.

```
pip install azure-mgmt-resource
pip install azure-mgmt-datalake-store
pip install azure-datalake-store
```

## <a name="create-a-new-python-application"></a>Criar um novo aplicativo Python

1. Use o IDE de sua escolha para criar um novo aplicativo Python, por exemplo, **mysample.py**.

2. Adicione as linhas a seguir para importar os módulos necessários

    ```
    ## Use this only for Azure AD service-to-service authentication
    from azure.common.credentials import ServicePrincipalCredentials

    ## Use this only for Azure AD end-user authentication
    from azure.common.credentials import UserPassCredentials

    ## Use this only for Azure AD multi-factor authentication
    from msrestazure.azure_active_directory import AADTokenCredentials

    ## Required for Azure Data Lake Storage Gen1 account management
    from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
    from azure.mgmt.datalake.store.models import DataLakeStoreAccount

    ## Required for Azure Data Lake Storage Gen1 filesystem management
    from azure.datalake.store import core, lib, multithread

    # Common Azure imports
    from azure.mgmt.resource.resources import ResourceManagementClient
    from azure.mgmt.resource.resources.models import ResourceGroup

    ## Use these as needed for your application
    import logging, getpass, pprint, uuid, time
    ```

3. Salve as alterações a mysample.py.

## <a name="authentication"></a>Autenticação

Nesta seção, falamos sobre as diferentes maneiras de autenticação com o Azure AD. As opções disponíveis são:

* Para autenticação do usuário final para seu aplicativo, confira [Autenticação de usuário final com o Data Lake Storage Gen1 usando o Python](data-lake-store-end-user-authenticate-python.md).
* Para autenticação de serviço a serviço do aplicativo, confira [Autenticação de serviço a serviço com o Data Lake Storage Gen1 usando o Python](data-lake-store-service-to-service-authenticate-python.md).

## <a name="create-filesystem-client"></a>Criar cliente do sistema de arquivos

O snippet de código a seguir primeiro cria o cliente de conta do Data Lake Storage Gen1. Ele usa o objeto de cliente para criar uma conta do Data Lake Storage Gen1. Finalmente, o trecho de código cria um objeto de cliente do sistema de arquivos.

    ## Declare variables
    subscriptionId = 'FILL-IN-HERE'
    adlsAccountName = 'FILL-IN-HERE'

    ## Create a filesystem client object
    adlsFileSystemClient = core.AzureDLFileSystem(adlCreds, store_name=adlsAccountName)

## <a name="create-a-directory"></a>Criar um diretório

    ## Create a directory
    adlsFileSystemClient.mkdir('/mysampledirectory')

## <a name="upload-a-file"></a>Carregar um arquivo


    ## Upload a file
    multithread.ADLUploader(adlsFileSystemClient, lpath='C:\\data\\mysamplefile.txt', rpath='/mysampledirectory/mysamplefile.txt', nthreads=64, overwrite=True, buffersize=4194304, blocksize=4194304)


## <a name="download-a-file"></a>Baixar um arquivo

    ## Download a file
    multithread.ADLDownloader(adlsFileSystemClient, lpath='C:\\data\\mysamplefile.txt.out', rpath='/mysampledirectory/mysamplefile.txt', nthreads=64, overwrite=True, buffersize=4194304, blocksize=4194304)

## <a name="delete-a-directory"></a>Excluir um diretório

    ## Delete a directory
    adlsFileSystemClient.rm('/mysampledirectory', recursive=True)

## <a name="next-steps"></a>Próximas etapas
* [Operações de gerenciamento de conta no Data Lake Storage Gen1 usando Python](data-lake-store-get-started-python.md).

## <a name="see-also"></a>Consulte também

* [Referência de Python do Azure Data Lake Storage Gen1 (sistema de arquivos)](http://azure-datalake-store.readthedocs.io/en/latest)
* [Abrir aplicativos de Big Data de software livre compatíveis com o Azure Data Lake Storage Gen1](data-lake-store-compatible-oss-other-applications.md)
