
# 📘 Documentação Atualizada – Execução de Scripts via Azure Service Bus

## 🧩 Visão Geral
Este projeto permite a execução de comandos SQL armazenados em tabelas específicas no banco **DBFILA**, utilizando o **Azure Service Bus** como canal de orquestração e **Azure Functions** para processamento assíncrono.

---

## ✅ Componentes da Solução

### 1. **Banco de Dados: DBFILA**
- Contém tabelas como `fila_analisecompras`, `fila_compras`, etc.
- Cada tabela possui colunas como `script`, `status`, `INICIO`, `TERMINO`, `mensagem`, entre outras.
- A view `vw_FILA` unifica os dados de todas as filas.

### 2. **Azure Service Bus**
- Fila principal: `fila-analisecompras`
- Recebe mensagens com scripts SQL a serem executados

### 3. **Azure Function – HTTP Trigger**
- Endpoint: `POST /api/send-message`
- Recebe um JSON com o campo `script`
- Envia a mensagem para a fila `fila-analisecompras`

### 4. **Azure Function – Service Bus Trigger**
- Escuta a fila `fila-analisecompras`
- Executa o script no banco `DBFILA`
- Atualiza os campos `INICIO`, `TERMINO` e `status = 'E'`

### 5. **Stored Procedure no Azure SQL**
- Nome: `EnviarScriptParaFila`
- Utiliza `sp_invoke_external_rest_endpoint` para chamar a Azure Function HTTP
- Permite envio de scripts diretamente via SSMS

---

## 🚀 Fluxo de Execução

1. Um script SQL é inserido na tabela `fila_analisecompras` com `status = 'P'`
2. A procedure `EnviarScriptParaFila` é chamada com o conteúdo do script
3. A Azure Function HTTP envia a mensagem para o Service Bus
4. A Azure Function com gatilho Service Bus consome a mensagem
5. O script é executado no banco e o status é atualizado para `'E'`

---

## 🔐 Segurança
- A comunicação com o Service Bus é feita via connection string segura
- A Azure Function pode ser protegida com chave de função ou identidade gerenciada
- Recomenda-se uso de **Azure Key Vault** para armazenar segredos

---

## 📈 Monitoramento
- Application Insights habilitado nas Azure Functions
- Logs de execução e falhas disponíveis no portal do Azure
