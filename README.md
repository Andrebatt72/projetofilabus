# Projeto: Sistema de Execução de Scripts SQL com Azure Service Bus

## ✅ Cenário Atual

### 🔹 Base de Dados
- **Nome**: DBFILA
- **Servidor**: gruposave.database.windows.net
- **Localização**: Brazil South
- **Camada**: Uso Geral (Gen5, 2 vCores)
- **Status**: Online

### 🔹 Estrutura
- Tabelas criadas conforme modelo original
- View unificada criada como `vw_FILA`
- Triggers criadas com prefixo `trg_`
- Índices otimizados com prefixo `idx_`

---

## 🚀 Próximos Passos

### 1. Criar Namespace e Filas no Azure Service Bus
- **Namespace**: `sb-dbfila`
- **Filas**: uma por módulo (ex: `fila-modulo-k`, `fila-modulo-a`, etc.)
- **Configurações recomendadas**:
  - Partitioning: `true`
  - Max delivery count: `10`
  - TTL: `14d`

### 2. Criar Azure Function (Service Bus Trigger)
- Uma Function por fila ou uma genérica com roteamento
- Executa o campo `script` da mensagem no banco `DBFILA`
- Atualiza os campos `INICIO`, `TERMINO` e `status = 'E'`

### 3. Criar Azure Function (HTTP Trigger)
- Para inserir comandos na fila
- Gera e envia mensagens para o Service Bus

### 4. Monitoramento
- Habilitar Application Insights para rastrear execuções
- Criar alertas para falhas ou lentidão

---

## 🧠 Ajuda Necessária
> Solicito ajuda para:
- Criar as filas no Azure Service Bus
- Criar a Azure Function que consome as mensagens
- Criar a Azure Function que envia mensagens
- Garantir segurança com Key Vault e Managed Identity
