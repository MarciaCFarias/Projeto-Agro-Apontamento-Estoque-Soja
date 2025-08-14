# Documento de Requisitos do Processo – Apontamento Estoque Soja

## 1. Regras de Negócio

1. **Gatilho de Execução**
   - O robô inicia automaticamente sempre que a planilha “Internalização Soja DDMMAAAAHH” for disponibilizada na pasta SharePoint especificada.
   - Podem ocorrer múltiplas execuções diárias.

2. **Padrão das Planilhas**
   - Planilhas de entrada devem seguir o layout e nomenclatura definidos.
   - Alterações de campos, fórmulas, nomes de arquivos ou estrutura precisam ser comunicadas previamente.

3. **Fluxo de Processamento**
   - Registros sem “Nº Documento” nas colunas MB31 e MB1A → seguir para “Registrar Entrada – MB31” e depois “Registrar Saída – MB1A”.
   - Registros com “Nº Documento” somente na coluna MB31 → seguir para “Registrar Saída – MB1A”.

4. **Integração com SAP**
   - As transações SAP devem ser executadas com os códigos e parâmetros pré-definidos:
     - MB31 (Tipo Movimento)
     - MB1A (Tipo Movimento)
     - MBST para estornos
     - Para inclusão de características.
   - Uso de perfis de acesso com permissões adequadas.

5. **Tratamento de Erros**
   - Exceções não previstas → envio de e-mail ao responsável com print da tela.
   - Registros com erro são movidos para pasta “EXCEÇÃO” no SharePoint.

6. **Atualização de Características**
   - Arquivos BATCH e SCAT devem ser atualizados e validados antes de inclusão no SAP.
   - Valores ausentes ou inválidos impedem a continuação da etapa.

---

## 2. Validações Necessárias

1. **Antes da Execução**
   - Verificar disponibilidade de acesso ao SharePoint e SAP.
   - Conferir se a planilha de entrada segue o layout padrão.
   - Garantir que as credenciais do robô estão ativas.

2. **Durante o Processamento**
   - Checar se cada campo obrigatório (Invoice, Order, Batch, Quantidades, CD, Depósito, FERT Code, UNBW) está preenchido.
   - Validar respostas do SAP (mensagens de alerta, sucesso ou erro).
   - Garantir que apenas itens válidos sejam lançados, estornados ou atualizados.

3. **Após Processamento**
   - Confirmar que as colunas “Material doc. (MB31)” e “Material doc. (MB1A)” foram atualizadas.
   - Conferir integridade dos arquivos gerados (BATCH SOJA 5.txt e SCAT 1.txt).
   - Validar se relatórios sintético e analítico foram gerados e salvos nas pastas corretas.

---

## 3. Saídas Esperadas

1. **Planilhas**
   - “Internalização DDMMAAA_Unificada” consolidando os registros processados.
   - Planilhas de exceção contendo registros com erro.

2. **Arquivos para SAP**
   - “BATCH SOJA 5.txt” atualizado e validado.
   - “SCAT 1.txt” atualizado e validado.

3. **Relatórios**
   - Relatório Sintético (tempo de execução, quantidade de lotes processados).
   - Relatório Analítico (detalhes de entradas/saídas, características atualizadas e status).

4. **Notificações**
   - E-mails automáticos de início e fim de processamento.
   - Alertas de exceções e status final do processo.

---

## 4. Critérios de Sucesso

1. **Execução**
   - Robô inicia sempre que houver planilha nova, sem falhas técnicas.
   - Todas as etapas do fluxo TO BE concluídas sem intervenção manual.

2. **Integridade de Dados**
   - 100% dos registros válidos processados com sucesso no SAP.
   - Arquivos BATCH e SCAT atualizados e incluídos no SAP sem inconsistências.

3. **Controle de Erros**
   - Todas as exceções registradas com LOG e enviadas para análise.
   - Registros inválidos devidamente movidos para pasta “EXCEÇÃO”.

4. **Entrega de Relatórios**
   - Relatórios sintético e analítico gerados e salvos no diretório correto.
   - E-mails de notificação enviados com informações completas e corretas.

---

