# 📡 API — Documentação

> Este documento faz parte do teste técnico de Front-End.
> Utilize os endpoints abaixo para consumir os dados da aplicação.

---

## Base URL

```
https://ThiagoLifters.github.io/api_test
```

> **Nota:** substitua pela URL real do seu repositório GitHub Pages.

---

## Endpoints disponíveis

### `GET /api/events.json`

Retorna a lista completa de eventos.

```js
const response = await fetch('https://ThiagoLifters.github.io/api_test/api/events.json')
const { data, total } = await response.json()
```

**Resposta:**
```json
{
  "total": 5,
  "data": [
    {
      "id": "EVT-001",
      "name": "Tech Summit 2025",
      "date": "2025-05-15T09:00:00-03:00",
      "location": "Centro de Convenções, São Paulo – SP",
      "status": "active",
      "description": "...",
      "expected_count": 12,
      "checkin_count": 11,
      "error_count": 1,
      "entry_rate": 0.92
    }
  ]
}
```

---

### `GET /api/events/{id}.json`

Retorna os dados completos de um evento, incluindo participantes e histórico de check-ins.

```js
const response = await fetch('https://ThiagoLifters.github.io/api_test/api/events/EVT-001.json')
const event = await response.json()
```

**Resposta:**
```json
{
  "id": "EVT-001",
  "name": "Tech Summit 2025",
  "date": "2025-05-15T09:00:00-03:00",
  "location": "Centro de Convenções, São Paulo – SP",
  "status": "active",
  "description": "...",
  "expected_count": 12,
  "checkin_count": 11,
  "error_count": 1,
  "entry_rate": 0.92,
  "participants": [
    {
      "id": "EVT-001-P001",
      "event_id": "EVT-001",
      "name": "Lucas Silva",
      "type": "vip",
      "status": "inside",
      "checkin_count": 4
    }
  ],
  "checkins": [
    {
      "id": "EVT-001-CK001",
      "event_id": "EVT-001",
      "participant_id": "EVT-001-P001",
      "timestamp": "2025-05-15T12:00:00.000Z",
      "success": true,
      "action": "entry",
      "error_reason": null
    }
  ]
}
```

---

## Tipos de dados

### Status do evento (`status`)

| Valor | Descrição |
|---|---|
| `"active"` | Ativo — aceita check-ins |
| `"closed"` | Encerrado — não aceita novos check-ins |
| `"cancelled"` | Cancelado — sem check-ins |

### Tipo do participante (`type`)

| Valor | Regra de negócio |
|---|---|
| `"vip"` | Pode entrar e sair múltiplas vezes |
| `"normal"` | Apenas um check-in permitido |

### Status do participante (`status`)

| Valor | Descrição |
|---|---|
| `"inside"` | Dentro do evento |
| `"outside"` | Fora do evento |

### Ação de check-in (`action`)

| Valor | Descrição |
|---|---|
| `"entry"` | Entrada |
| `"exit"` | Saída (apenas VIP) |

### Motivo de erro (`error_reason`)

| Valor | Descrição |
|---|---|
| `null` | Sem erro (sucesso) |
| `"already_checked_in"` | Participante normal tentou entrar novamente |
| `"event_closed"` | Tentativa em evento encerrado |

---

## IDs disponíveis

| ID | Nome | Status |
|---|---|---|
| `EVT-001` | Tech Summit 2025 | `active` |
| `EVT-002` | Design Week Rio | `closed` |
| `EVT-003` | Startup Pitch Night | `active` |
| `EVT-004` | Festival de Música Indie | `cancelled` |
| `EVT-005` | DevConf Brasil | `active` |

---

## ⚠️ Observações

- A API é **somente leitura** (GET). Gerencie o estado de check-ins localmente no front-end.
- `error_reason: null` nos registros de sucesso — trate esse campo como nullable.
- EVT-002 (`closed`) contém tentativas com `error_reason: "event_closed"` para teste.
- EVT-004 (`cancelled`) não possui check-ins nem participantes dentro.
- Participantes VIP têm `checkin_count > 1` no histórico.
- Não há autenticação necessária.

---

## Alternativa: json-server local (CRUD completo)

```bash
git clone https://github.com/ThiagoLifters/api_test.git
cd api_test
npm install -g json-server
json-server --watch db.json --port 3001
```

Endpoints disponíveis com json-server:

| Método | Endpoint | Descrição |
|---|---|---|
| `GET` | `/events` | Lista eventos |
| `GET` | `/events/:id` | Detalhe do evento |
| `GET` | `/participants?event_id=EVT-001` | Participantes do evento |
| `GET` | `/checkins?event_id=EVT-001` | Check-ins do evento |
| `POST` | `/checkins` | Registrar check-in |
| `PATCH` | `/participants/:id` | Atualizar status do participante |
| `PATCH` | `/events/:id` | Atualizar métricas do evento |

**Exemplo — registrar check-in:**
```js
// 1. Registrar no histórico
await fetch('http://localhost:3001/checkins', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    event_id: 'EVT-001',
    participant_id: 'EVT-001-P004',
    timestamp: new Date().toISOString(),
    success: true,
    action: 'entry',
    error_reason: null,
  })
})

// 2. Atualizar status do participante
await fetch('http://localhost:3001/participants/EVT-001-P004', {
  method: 'PATCH',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ status: 'inside', checkin_count: 1 })
})
```

> Resetar dados: `git checkout db.json`
