---
title: Auditoria da Instância Gerenciada do Banco de Dados SQL do Azure | Microsoft Docs
description: Saiba como começar com a Auditoria da Instância Gerenciada do Banco de Dados SQL do Azure usando o T-SQL
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
f1_keywords:
- mi.azure.sqlaudit.general.f1
author: vainolo
ms.author: vainolo
ms.reviewer: vanto
manager: craigg
ms.date: 01/12/2019
ms.openlocfilehash: 716c4caa1b28cc40470d366e5fc6901de9462f9a
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/14/2019
ms.locfileid: "54267259"
---
# <a name="get-started-with-azure-sql-database-managed-instance-auditing"></a>Comece com a Auditoria da Instância Gerenciada do Banco de Dados SQL do Azure

A Auditoria da [Instância Gerenciada do Banco de Dados SQL do Azure](sql-database-managed-instance.md) rastreia eventos de banco de dados e os grava em um log de auditoria na conta de armazenamento do Azure. A auditoria também:

- Ajuda você a manter a conformidade regulatória, entender a atividade do banco de dados e obter informações sobre discrepâncias e anomalias que podem indicar preocupações para os negócios ou suspeitas de violações de segurança.
- Permite e facilita a adesão aos padrões de conformidade, embora não garanta a conformidade. Para obter mais informações sobre os programas Azure que oferecem suporte à conformidade com os padrões, consulte o [Azure Trust Center](https://azure.microsoft.com/support/trust-center/compliance/).

## <a name="set-up-auditing-for-your-server-to-azure-storage"></a>Configurar a auditoria para o servidor no Armazenamento do Microsoft Azure 

A seção a seguir descreve a configuração de auditoria na Instância Gerenciada.

1. Vá para o [Portal do Azure](https://portal.azure.com).
2. As etapas a seguir criam um **contêiner** de Armazenamento do Microsoft Azure onde os logs de auditoria são armazenados.

   - Navegue até o Armazenamento do Microsoft Azure onde você deseja armazenar seus logs de auditoria.

     > [!IMPORTANT]
     > Use uma conta de armazenamento na mesma região que o servidor da Instância Gerenciada para evitar leituras/gravações entre regiões.

   - Na página da conta de armazenamento, vá até **Visão geral** e clique em **Blobs**.

     ![Painel de navegação][1]

   - No menu superior, clique em **+ Contêiner** para criar um novo contêiner.

     ![Painel de navegação][2]

   - Forneça um contêiner **Nome**, defina o nível de acesso Público para **Privado** e, em seguida, clique em **OK**.

     ![Painel de navegação][3]

   - Na lista de contêineres, clique no contêiner recém-criado e, em seguida, clique em **Propriedades do contêiner**.

     ![Painel de navegação][4]

   - Copie a URL do contêiner, clicando no ícone de cópia e salve-a (por exemplo, no bloco de notas) para uso futuro. O formato da URL do contêiner deve ser `https://<StorageName>.blob.core.windows.net/<ContainerName>`

     ![Painel de navegação][5]

3. As etapas a seguir geram um **Token SAS** do Armazenamento do Microsoft Azure usado para conceder direitos de acesso de Auditoria de Instância Gerenciada para a conta de armazenamento.

   - Navegue até a Conta de Armazenamento do Microsoft Azure em que você criou o contêiner na etapa anterior.

   - Clique em **Assinatura de acesso compartilhado** no menu Configurações de Armazenamento.

     ![Painel de navegação][6]

   - Configure a SAS da seguinte forma:
     - **Serviços permitidos**: Blob
     - **Data de início**: para evitar problemas relacionados a fuso horário, é recomendável usar a data de ontem.
     - **Data de término**: escolha a data de vencimento desse Token SAS. 

       > [!NOTE]
       > Renove o token após o vencimento para evitar falhas de auditoria.

     - Clique em **Gerar SAS**.

       ![Painel de navegação][7]

   - Depois de clicar em Gerar SAS, o Token SAS aparece na parte inferior. Copie o token clicando no ícone de cópia e salve-o (por exemplo, no bloco de notas) para uso futuro.

     > [!IMPORTANT]
     > Remova o caractere de ponto de interrogação ("?") do início do token.

     ![Painel de navegação][8]

4. Conecte-se a sua Instância Gerenciada por meio do SQL Server Management Studio (SSMS).

5. Execute a seguinte instrução T-SQL para **criar uma nova Credencial** usando a URL do Contêiner e o Token SAS criado nas etapas anteriores:

    ```SQL
    CREATE CREDENTIAL [<container_url>]
    WITH IDENTITY='SHARED ACCESS SIGNATURE',
    SECRET = '<SAS KEY>'
    GO
    ```

6. Execute a seguinte instrução T-SQL para criar um novo Servidor de Auditoria (escolha seu próprio nome de auditoria, use a URL do Contêiner que você criou nas etapas anteriores):

    ```SQL
    CREATE SERVER AUDIT [<your_audit_name>]
    TO URL ( PATH ='<container_url>' [, RETENTION_DAYS =  integer ])
    GO
    ```

    Se não for especificado, `RETENTION_DAYS` padrão é 0 (retenção ilimitada).

    Para informações adicionais:
    - [Diferenças de auditoria entre a Instância Gerenciada, o banco de dados SQL do Azure e o SQL Server](#auditing-differences-between-managed-instance-azure-sql-database-and-sql-server)
    - [CRIAR AUDITORIA DE SERVIDOR](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-transact-sql)
    - [ALTERAR AUDITORIA DE SERVIDOR](https://docs.microsoft.com/sql/t-sql/statements/alter-server-audit-transact-sql)

7. Crie uma especificação de Auditoria de Servidor ou Especificação de Auditoria de Banco de Dados como faria para o SQL Server:
    - [Guia Criar T-SQL de especificação de auditoria de servidor](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-specification-transact-sql)
    - [Guia Criar T-SQL de especificação de auditoria de banco de dados](https://docs.microsoft.com/sql/t-sql/statements/create-database-audit-specification-transact-sql)

8. Habilite a auditoria de servidor criada na etapa 6:

    ```SQL
    ALTER SERVER AUDIT [<your_audit_name>]
    WITH (STATE=ON);
    GO
    ```

## <a name="set-up-auditing-for-your-server-to-event-hub-or-log-analytics"></a>Configurar a auditoria para o servidor no Hub de Eventos ou no Log Analytics

Os logs de auditoria de uma Instância Gerenciada podem ser enviados para os Hubs de Eventos ou para o Log Analytics usando o Azure Monitor. Esta seção descreve como fazer essa configuração:

1. No [Portal do Azure](https://portal.azure.com/), navegue até a Instância Gerenciada de SQL.

2. Clique em **Configurações de diagnóstico**.

3. Clique em **Ativar diagnóstico**. Se o diagnóstico já estiver habilitado, *+Adicionar configuração de diagnóstico* será exibido.

4. Selecione **SQLSecurityAuditEvents** na lista de logs.

5. Selecione um destino para os eventos de auditoria – Hub de Eventos, Log Analytics ou ambos. Configure os parâmetros necessários (por exemplo, workspace do Log Analytics) para cada destino.

6. Clique em **Salvar**.

  ![Painel de navegação][9]

7. Conecte-se à Instância Gerenciada usando o **SQL Server Management Studio (SSMS)** ou qualquer outro cliente com suporte.

8. Execute a seguinte instrução T-SQL para criar uma auditoria de servidor:

    ```SQL
    CREATE SERVER AUDIT [<your_audit_name>] TO EXTERNAL_MONITOR;
    GO
    ```

9. Crie uma especificação de auditoria de servidor ou especificação de auditoria de banco de dados como faria para o SQL Server:

   - [Guia Criar T-SQL de especificação de auditoria de servidor](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-specification-transact-sql)
   - [Guia Criar T-SQL de especificação de auditoria de banco de dados](https://docs.microsoft.com/sql/t-sql/statements/create-database-audit-specification-transact-sql)

10. Habilite a auditoria de servidor criada na etapa 7:
 
    ```SQL
    ALTER SERVER AUDIT [<your_audit_name>] WITH (STATE=ON);
    GO
    ```

## <a name="consume-audit-logs"></a>Consumir logs de auditoria

### <a name="consume-logs-stored-in-azure-storage"></a>Consumir logs armazenados no Armazenamento do Azure

Há vários métodos que podem ser usados para exibir os logs de auditoria de blob.

- Use a função do sistema caractere `sys.fn_get_audit_file` (T-SQL) para retornar os dados do log de auditoria em um formato tabular. Para obter mais informações sobre como usar essa função, consulte a [documentação de sys.fn_get_audit_file](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql).

- Explore os logs de auditoria usando uma ferramenta como o [Gerenciador de Armazenamento do Azure](https://azure.microsoft.com/en-us/features/storage-explorer/). No Armazenamento do Azure, os logs de auditoria de blob são salvos como uma coleção de arquivos de blob em um contêiner chamado sqldbauditlogs. Para obter mais detalhes sobre a hierarquia da pasta de armazenamento, as convenções de nomenclatura e o formato do log, consulte a [Referência de formato do log de auditoria de blob](https://go.microsoft.com/fwlink/?linkid=829599).

- Para obter uma lista completa de métodos de consumo de log de auditoria, consulte o [Introdução à auditoria de banco de dados do SQL](https://docs.microsoft.com/azure/sql-database/sql-database-auditing).

> [!IMPORTANT]
> A exibição dos registros de auditoria do Portal do Azure (painel de ‘registros de auditoria’) atualmente não está disponível para a Instância Gerenciada.

### <a name="consume-logs-stored-in-event-hub"></a>Consumir logs armazenados no Hub de Eventos

Para consumir dados de logs de auditoria do Hub de Eventos, você precisará configurar um fluxo para consumir eventos e gravá-las em um destino. Para obter mais informações, veja a Documentação de Hubs de Eventos do Azure.

### <a name="consume-and-analyze-logs-stored-in-log-analytics"></a>Consumir e analisar logs armazenados no Log Analytics

Se os logs de auditoria forem gravados no Log Analytics, eles estarão disponíveis no workspace do Log Analytics, onde você poderá executar pesquisas avançadas nos dados de auditoria. Como ponto de partida, navegue até o Log Analytics e, na seção *Geral*, clique em *Logs* e insira uma consulta simples, como: `search "SQLSecurityAuditEvents"` para exibir os logs de auditoria.  

O Log Analytics fornece análises operacionais em tempo real usando pesquisa integrada e painéis personalizados para analisar prontamente milhões de registros em todas as suas cargas de trabalho e servidores. Para obter informações úteis adicionais sobre o idioma e os comandos de pesquisa do Log Analytics, consulte [Referência de pesquisa do Log Analytics](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview).

## <a name="auditing-differences-between-managed-instance-azure-sql-database-and-sql-server"></a>Diferenças de auditoria entre a Instância Gerenciada, o banco de dados SQL do Azure e o SQL Server

As principais diferenças entre a Auditoria do SQL na Instância Gerenciada, banco de dados SQL do Azure e SQL Server local são:

- Na Instância Gerenciada, a Auditoria do SQL funciona no nível de servidor e armazena arquivos de log de `.xel` na conta de armazenamento de blobs do Azure.
- No banco de dados SQL do Azure, a Auditoria do SQL funciona no nível do banco de dados.
- No SQL Server local/máquinas virtuais, a Auditoria do SQL funciona no nível do servidor, mas armazena os eventos em logs de eventos do Windows/sistema de arquivos.

A auditoria XEvent na Instância Gerenciada oferece suporte a destinos de armazenamento de blobs do Azure. **Não há suporte** para logs de arquivo e do Windows.

As principais diferenças na sintaxe `CREATE AUDIT` para a auditoria do armazenamento de blobs do Azure são:

- Uma nova sintaxe `TO URL` é fornecida e permite que você especifique a URL do contêiner de armazenamento de blobs do Azure onde arquivos `.xel` são colocados.
- Uma nova sintaxe `TO EXTERNAL MONITOR` é fornecida para permitir destinos no Hub de Eventos e no Log Analytics.
- A sintaxe `TO FILE` **não é compatível** porque a instância gerenciada não pode acessar compartilhamentos de arquivos do Windows.
- A opção de desligamento **não é compatível**.
- **Não há suporte** para `queue_delay` de 0.

## <a name="next-steps"></a>Próximas etapas

- Para obter uma lista completa de métodos de consumo de log de auditoria, consulte o [Introdução à auditoria de banco de dados do SQL](https://docs.microsoft.com/azure/sql-database/sql-database-auditing).
- Para obter mais informações sobre os programas Azure que oferecem suporte à conformidade com os padrões, consulte o [Azure Trust Center](https://azure.microsoft.com/support/trust-center/compliance/).

<!--Image references-->
[1]: ./media/sql-managed-instance-auditing/1_blobs_widget.png
[2]: ./media/sql-managed-instance-auditing/2_create_container_button.png
[3]: ./media/sql-managed-instance-auditing/3_create_container_config.png
[4]: ./media/sql-managed-instance-auditing/4_container_properties_button.png
[5]: ./media/sql-managed-instance-auditing/5_container_copy_name.png
[6]: ./media/sql-managed-instance-auditing/6_storage_settings_menu.png
[7]: ./media/sql-managed-instance-auditing/7_sas_configure.png
[8]: ./media/sql-managed-instance-auditing/8_sas_copy.png
[9]: ./media/sql-managed-instance-auditing/9_mi_configure_diagnostics.png
