# 📡 API — Documentação

> Este documento faz parte do teste técnico de Front-End.
> Utilize os endpoints abaixo para consumir os dados da aplicação.

---

## Base URL

```
https://ThiagoLifters.github.io/api_test
```

> **Nota:** substitua pela URL real informada pelo avaliador.

---

## Endpoints disponíveis

### `GET /api/tickets.json`

Retorna a lista completa de chamados.

**Exemplo de requisição:**

```js
const response = await fetch('https://ThiagoLifters.github.io/api_test/api/tickets.json')
const { data, total } = await response.json()
```

**Estrutura da resposta:**

```json
{
  "total": 20,
  "data": [
    {
      "id": "TKT-001",
      "title": "Login não funciona após atualização do sistema",
      "user": "mariana.costa@empresa.com",
      "status": "open",
      "priority": "high",
      "created_at": "2025-04-01T08:14:22Z",
      "updated_at": "2025-04-01T09:30:00Z",
      "tags": ["auth", "critical", "regression"],
      "assignee": "joao.silva@empresa.com"
    }
  ]
}
```

---

### `GET /api/tickets/{id}.json`

Retorna os dados completos de um chamado, incluindo o histórico.

**Exemplo de requisição:**

```js
const response = await fetch('https://ThiagoLifters.github.io/api_test/api/tickets/TKT-001.json')
const ticket = await response.json()
```

**Estrutura da resposta:**

```json
{
  "id": "TKT-001",
  "title": "Login não funciona após atualização do sistema",
  "user": "mariana.costa@empresa.com",
  "status": "open",
  "priority": "high",
  "created_at": "2025-04-01T08:14:22Z",
  "updated_at": "2025-04-01T09:30:00Z",
  "description": "Após a atualização realizada em 31/03, vários usuários relataram...",
  "tags": ["auth", "critical", "regression"],
  "assignee": "joao.silva@empresa.com",
  "history": [
    {
      "id": "h1",
      "action": "Chamado aberto",
      "user": "mariana.costa@empresa.com",
      "timestamp": "2025-04-01T08:14:22Z"
    }
  ]
}
```

---

## Tipos de dados

### Status (`status`)

| Valor | Descrição |
|---|---|
| `"open"` | Aberto |
| `"in_progress"` | Em andamento |
| `"resolved"` | Resolvido |

### Prioridade (`priority`)

| Valor | Descrição |
|---|---|
| `"low"` | Baixa |
| `"medium"` | Média |
| `"high"` | Alta |

---

## ⚠️ Observações importantes

- A API é **somente leitura** (GET). Para simular alterações de status, gerencie o estado localmente no front-end.
- Alguns campos podem retornar `null` — seu código deve tratar esses casos.
- Os dados **não persistem** entre sessões — isso é esperado para um ambiente de teste.
- Não há autenticação necessária para acessar os endpoints.

---

## Alternativa: json-server local (CRUD completo)

Se preferir ter uma API com suporte a escrita (PUT/PATCH), você pode rodar um servidor local:

**Pré-requisito:** Node.js instalado.

```bash
# 1. Clone o repositório da API
git clone https://github.com/ThiagoLifters/api_test.git
cd api_test

# 2. Instale o json-server
npm install -g json-server

# 3. Inicie o servidor
json-server --watch db.json --port 3001
```

Com json-server rodando, os endpoints disponíveis são:

| Método | Endpoint | Descrição |
|---|---|---|
| `GET` | `/tickets` | Lista todos os chamados |
| `GET` | `/tickets/:id` | Retorna um chamado pelo ID |
| `PATCH` | `/tickets/:id` | Atualiza campos de um chamado |
| `PUT` | `/tickets/:id` | Substitui um chamado completo |

**Exemplo de atualização de status:**

```js
await fetch('http://localhost:3001/tickets/TKT-001', {
  method: 'PATCH',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ status: 'in_progress' })
})
```

> Dica: caso queira resetar os dados ao estado original, restaure o `db.json` via `git checkout db.json`.

---

## IDs disponíveis

`TKT-001` até `TKT-020`
