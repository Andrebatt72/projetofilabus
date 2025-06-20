
# 📘 Documentação – Execução de Scripts via Azure Service Bus

## 🧩 Visão Geral
Execução assíncrona de comandos SQL armazenados no banco **DBFILA**, orquestrada via **Azure Service Bus** e processada por **Azure Functions**.

---

## ✅ Componentes da Solução

### 🗄️ 1. Banco de Dados – `DBFILA`
- Tabelas: `fila_analisecompras`, `fila_compras`, etc.
- Colunas padrão: `script`, `status`, `INICIO`, `TERMINO`, `mensagem`
- View unificadora: `vw_FILA`

---

### 📬 2. Azure Service Bus
- **Fila principal:** `fila-analisecompras`
- **Função:** Canal de mensagens com scripts SQL a serem executados

---

### ⚙️ 3. Azure Function – HTTP Trigger
- **Endpoint:** `POST /api/send-message`
- **Função:** Recebe JSON com `"script"` e envia para a fila

---

### 🔄 4. Azure Function – Service Bus Trigger
- **Escuta:** `fila-analisecompras`
- **Função:** Executa script no banco e atualiza status para `'E'`

---

### 🧪 5. Stored Procedure – `EnviarScriptParaFila`
- **Uso:** Executada via SSMS
- **Função:** Envia script para a Azure Function HTTP usando `sp_invoke_external_rest_endpoint`

---

## 🚀 Fluxo de Execução

1. Um script SQL é inserido na tabela `fila_analisecompras` com `status = 'P'`
2. A procedure `EnviarScriptParaFila` é chamada com o conteúdo do script
3. A Azure Function HTTP envia a mensagem para o Service Bus
4. A Azure Function com gatilho Service Bus consome a mensagem
5. O script é executado no banco e o status é atualizado para `'E'`

---

## 🔐 Segurança
- Conexão com Service Bus via **connection string segura**
- Azure Function protegida com **chave de função** ou **identidade gerenciada**
- Recomendado: uso de **Azure Key Vault** para segredos

---

## 📈 Monitoramento
- **Application Insights** habilitado nas Azure Functions
- Logs de execução e falhas disponíveis no portal do Azure
