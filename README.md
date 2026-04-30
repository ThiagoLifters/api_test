# 🎪 Events Mock API

API de dados mockados para o **teste técnico de Front-End** — Painel de Gestão de Eventos.

Disponibiliza 5 eventos com participantes, histórico de check-ins e edge cases propositais para avaliar robustez do código e aplicação de regras de negócio.

---

## ⚡ Setup em 3 passos

### 1. Fork / clone

```bash
git clone https://github.com/ThiagoLifters/api_test.git
cd api_test
```

### 2. Ativar GitHub Pages

1. **Settings → Pages**
2. Source: `main` branch, pasta `/` (root)
3. **Save** — aguarde ~2 min

Endpoints disponíveis em:
```
https://ThiagoLifters.github.io/api_test/api/events.json
https://ThiagoLifters.github.io/api_test/api/events/EVT-001.json
```

### 3. Compartilhar o `API.md` com o candidato

---

## 📁 Estrutura

```
api_test/
├── db.json                        # Base completa (json-server)
├── API.md                         # Documentação para o candidato
├── README.md                      # Este arquivo
└── api/
    ├── events.json                # GET lista de eventos
    └── events/
        ├── EVT-001.json           # active  — Tech Summit 2025
        ├── EVT-002.json           # closed  — Design Week Rio
        ├── EVT-003.json           # active  — Startup Pitch Night
        ├── EVT-004.json           # cancelled — Festival de Música Indie
        └── EVT-005.json           # active  — DevConf Brasil
```

---

## 🔀 Opção A — GitHub Pages (estática)

| Endpoint | Retorno |
|---|---|
| `GET /api/events.json` | Lista com métricas agregadas |
| `GET /api/events/EVT-001.json` | Evento completo com participants + checkins |

## 🔀 Opção B — json-server local (CRUD)

```bash
npm install -g json-server
json-server --watch db.json --port 3001
```

---

## 🐛 Edge cases propositais

| Evento | Edge case |
|---|---|
| EVT-002 (`closed`) | Não deve aceitar novos check-ins |
| EVT-004 (`cancelled`) | Sem participantes inside, checkin_count = 0 |
| Qualquer evento | VIPs com `checkin_count > 1` e múltiplos registros no histórico |
| EVT-002 | Registros com `error_reason: "event_closed"` |
| Qualquer evento | Participante normal com `error_reason: "already_checked_in"` |
| Qualquer evento | Participantes com `status: "outside"` apesar de terem feito check-in (saíram) |
| `entry_rate` | Calculado como `checkin_count / expected_count` — pode ser menor que 1 |

---

## 🔄 Resetar dados (json-server)

```bash
git checkout db.json
```

## 📌 Observações

- Os dados são fictícios e qualquer semelhança com pessoas ou empresas reais é coincidência.
- O teste não deve ser compartilhado com terceiros.
- O GitHub Pages pode levar até 5 minutos para propagar atualizações.
- CORS já é habilitado por padrão no GitHub Pages e no json-server.
