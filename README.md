# 🎫 Tickets Mock API

API de dados mockados para o **teste técnico de Front-End**.

Disponibiliza 20 chamados (tickets) realistas com histórico, prioridades e status variados — incluindo alguns edge cases propositais (campos nulos, dados inconsistentes) para avaliar robustez do código do candidato.

---

## ⚡ Setup em 3 passos

### 1. Fork / clone este repositório

```bash
git clone https://github.com/SEU_USUARIO/tickets-mock-api.git
cd tickets-mock-api
```

### 2. Ativar GitHub Pages

1. Acesse **Settings → Pages** no repositório
2. Em **Source**, selecione `main` branch e pasta `/` (root)
3. Clique em **Save**
4. Aguarde ~2 minutos para o deploy

A API estará disponível em:

```
https://SEU_USUARIO.github.io/tickets-mock-api/api/tickets.json
```

### 3. Enviar o link ao candidato

Compartilhe o arquivo `API.md` com o candidato. Ele contém todos os endpoints disponíveis e exemplos de uso.

---

## 📁 Estrutura do repositório

```
tickets-mock-api/
├── db.json                    # Base de dados completa (json-server)
├── API.md                     # Documentação para o candidato
├── README.md                  # Este arquivo (uso interno)
└── api/
    ├── tickets.json           # GET /api/tickets.json  → lista resumida
    └── tickets/
        ├── TKT-001.json       # GET /api/tickets/TKT-001.json
        ├── TKT-002.json
        └── ...                # TKT-001 até TKT-020
```

---

## 🔀 Duas formas de usar

### Opção A — GitHub Pages (estática, sem config)

Endpoints somente leitura via HTTPS. Ideal para candidatos que querem focar no front-end sem configurar back-end.

| Endpoint | Retorno |
|---|---|
| `GET /api/tickets.json` | Lista com 20 tickets (sem history) |
| `GET /api/tickets/TKT-001.json` | Ticket completo com history |

### Opção B — json-server local (CRUD completo)

Permite GET, POST, PUT, PATCH e DELETE. Candidatos mais avançados podem usá-lo para implementar alteração de status com persistência real.

```bash
# Instalar json-server (requer Node.js)
npm install -g json-server

# Clonar o repo e rodar
git clone https://github.com/SEU_USUARIO/tickets-mock-api.git
cd tickets-mock-api
json-server --watch db.json --port 3001
```

Endpoints gerados automaticamente:

```
GET    /tickets
GET    /tickets/:id
POST   /tickets
PUT    /tickets/:id
PATCH  /tickets/:id
DELETE /tickets/:id
```

---

## 🐛 Edge cases propositais

Os dados contêm situações que testam a robustez do código:

| Ticket | Edge case |
|---|---|
| TKT-015 | `user` é `null` |
| TKT-004, TKT-007, TKT-010, TKT-012, TKT-015, TKT-016, TKT-018 | `assignee` é `null` |
| TKT-001 a TKT-020 | Mix de todos os status e prioridades |
| TKT-015 | history registrado como `"user": "sistema"` (não humano) |

Candidatos que não tratam valores nulos terão erros visíveis na interface — isso é proposital e faz parte da avaliação.

---

## 🔄 Resetar dados (json-server)

Se um candidato alterar os dados via json-server, basta re-clonar o repositório ou restaurar `db.json` via Git:

```bash
git checkout db.json
```

---

## 📌 Observações

- Os dados são fictícios e qualquer semelhança com pessoas ou empresas reais é coincidência.
- O GitHub Pages pode levar até 5 minutos para propagar atualizações.
- CORS já é habilitado por padrão no GitHub Pages e no json-server.
