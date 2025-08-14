# Documento de Análise de Processo – Apontamento Estoque Soja



## Fluxo TO BE

1. **Disponibilizar Planilha**  
   - Área responsável publica no SharePoint a planilha “Internalização Soja DDMMAAAAHH”.
   - Planilha pode ser disponibilizada várias vezes ao dia.

2. **Captura de Dados**  
   - Robô acessa pasta “PENDENTE APONTAMENTO” e lê subpastas (“Entrada-Saída” e “Saída”).
   - Identifica e processa registros de acordo com status de “Nº Documento” (MB31/MB1A).

3. **Registrar Entrada – MB31 (SAP)**  
   - Acessa transação MB31.
   - Lança ordens, centros, depósitos, materiais, quantidades e lotes.
   - Captura número de documento gerado.

4. **Estornar Lançamento – MB31 (quando aplicável)**  
   - Utiliza transação MBST para exclusão de lançamentos inválidos.

5. **Registrar Saída – MB1A (SAP)**  
   - Acessa transação MB1A.
   - Insere dados de ordens, materiais, quantidades e lotes.
   - Captura número de documento gerado.

6. **Estornar Lançamento – MB1A (quando aplicável)**  
   - Utiliza transação MBST para exclusão de lançamentos inválidos.

7. **Atualizar Planilha**  
   - Preenche colunas “Material doc. (MB31)” e “Material doc. (MB1A)”.
   - Registros com erro são movidos para pasta “EXCEÇÃO”.

8. **Unificar Dados**  
   - Consolida dados processados no dia em “Internalização DDMMAAA_Unificada”.

9. **Atualizar Características dos Lotes/Peso – BATCH**  
   - Atualiza e valida dados no arquivo “BATCH.xlsx”.
   - Gera arquivo “BATCH SOJA Modelo 5.txt” para inclusão no SAP.

10. **Atualizar Características dos Lotes/Peso – SCAT**  
    - Atualiza e valida dados no arquivo “SCAT.xlsx”.
    - Gera arquivo “SCAT ZBRSE030 1.txt” para inclusão no SAP.

11. **Incluir Arquivos no SAP**  
    - Inclui arquivos gerados nas transações.

12. **Gerar Relatórios**  
    - Relatório Sintético: tempo, quantidade de lotes processados.
    - Relatório Analítico: dados detalhados das entradas/saídas e atualizações.

13. **Enviar Notificações**  
    - E-mails automáticos de início e fim de processamento.
    - Avisos sobre exceções e resultados finais.

---

## 2. Melhorias Implementadas no Processo

- **Automação RPA** para eliminar digitação manual no SAP.
- **Processamento múltiplo diário**, sempre que nova planilha é publicada.
- **Tratamento automático de erros** com logs e separação de registros problemáticos.
- **Atualização automática de características dos lotes**, integrando arquivos BATCH e SCAT no SAP.
- **Relatórios automáticos** (sintético e analítico) para acompanhamento da operação.
- **Integração direta com SharePoint** para leitura/escrita de arquivos.

---

## 3. Tratativas em Caso de Exceções

- **Erros de aplicação ou negócios não previstos**:  
  Envio de e-mail ao responsável com print da tela de erro.
- **Falha de inclusão no SAP**:  
  Registro movido para pasta “EXCEÇÃO”.
- **Dados ausentes ou inválidos** nas planilhas BATCH/SCAT:  
  Arquivos salvos em pasta “EXCEÇÃO” e excluídos do fluxo de inclusão no SAP.
- **Mensagens de alerta ou erro**:  
  Captura de logs detalhados, sem interromper processamento de outros lotes.
- **Registros estornados**:  
  Não atualizados na planilha principal e tratados em execução futura.

---

## 4. KPI – Indicadores de Desempenho

- **Tempo médio de processamento por planilha** (`Total horas`).
- **Quantidade de lotes processados** por execução (`QTD_LOTES_PROCESSADOS`).
- **Taxa de sucesso dos lançamentos** (% registros incluídos com sucesso no SAP).
- **Taxa de retrabalho** (% registros movidos para pasta “EXCEÇÃO”).
- **Tempo de atualização das características** (BATCH/SCAT).
- **Disponibilidade do robô** (% de execuções realizadas sem falhas técnicas).

---

## 5. Observações ou Destaques

- A robustez do processo depende fortemente do **padrão das planilhas** recebidas; qualquer alteração exige comunicação prévia.
- A **integração com SAP** utiliza transações críticas (MB31, MB1A, MBST, YPPUPLBATCH, SCAT), devendo respeitar perfis de acesso e segregação de funções.
- O **controle de exceções** é bem definido, garantindo rastreabilidade.
- O processo está preparado para **alta frequência de execução** no mesmo dia.
- A manutenção das pastas no SharePoint e nomes dos arquivos é responsabilidade da área solicitante.

---

