# Roadmap — whatsapp-go-api-postman

**Referencia:** [MASTER_BLUEPRINT.md](../MASTER_BLUEPRINT.md)
**Ultima atualizacao:** 2026-03-30

---

## Estado Atual: Scaffold Vazio

- Collection criada (schema v2.1.0) — vazia
- Environment configurado com 10 variaveis (6 originais + 4 sysadmin)
- Governance completa (5 rules, 1 workflow, 1 skill)

---

## TASK 0 (OBRIGATORIA): State Checkpoint

**Antes de iniciar QUALQUER milestone ou task, o agente DEVE:**

1. Ler `.agents/STATE_CHECKPOINT.json` deste repositorio
2. Comparar `last_checkpoint.commit_hash` com `git log --oneline -1`
3. Se hash diferente → executar `git log --oneline <old>..HEAD` e `git diff --stat <old>..HEAD`
4. Classificar mudancas como COMPATIVEL / RELEVANTE / CONFLITANTE
5. Se CONFLITANTE → reportar ao usuario antes de prosseguir
6. Se dirty working tree (`git status --short` nao vazio) → reportar ao usuario
7. Ao concluir cada task → atualizar `STATE_CHECKPOINT.json` com novo commit hash
8. **Especifico Postman:** Alem do Git, verificar se a collection JSON foi alterada manualmente (comparar numero de folders/requests com `known_state.collection_folders`)

**Detalhes completos:** Ver [MASTER_BLUEPRINT.md — Task 0](../MASTER_BLUEPRINT.md#task-0-obrigatoria-state-checkpoint--verificacao-de-estado-pre-execucao)

**Arquivo de controle:** `.agents/STATE_CHECKPOINT.json`

### TASK 0.5 (OBRIGATORIA): Refinamento Pre-Milestone

**Apos o checkpoint e ANTES de executar qualquer task, o agente DEVE:**

1. Ler o estado atual da collection JSON (folders, requests existentes)
2. Ler os endpoints que a API ja expoe (verificar router.go dos modulos correspondentes)
3. Decompor cada task em sub-tasks atomicas (1 folder ou 1-2 requests por sub-task)
4. Sequenciar: collection-level scripts → folder → requests → test scripts → RBAC negative tests
5. Apresentar o plano ao usuario antes de comecar a codar
6. Salvar o plano em `.agents/REFINEMENT_M<N>.md`

**Cada sub-task = 1 commit. Cada request DEVE ter test scripts (status, body, content-type).**

**Detalhes completos:** Ver [MASTER_BLUEPRINT.md — Secao 0.5](../MASTER_BLUEPRINT.md#05-refinamento-obrigatorio-pre-milestone-atomizacao-de-sub-tasks)

---

## Tarefas Pendentes (por Milestone)

### MILESTONE 1: Auth (4 requests)

| # | Task | Descricao | Status |
|---|------|-----------|--------|
| 1.1 | Folder Auth | [POST] Login, [POST] Refresh Token, [POST] Logout, [GET] Me | PENDENTE |
| 1.2 | Pre-request Scripts | Auto-login global + Idempotency-Key injection | PENDENTE |
| 1.3 | RBAC Tests | Testes positivos + negativos (403) para cada endpoint protegido | PENDENTE |

### MILESTONE 1.5: Sysadmin Auth (1 request)

**Referencia:** [SYSADMIN_BLUEPRINT.md](../SYSADMIN_BLUEPRINT.md)

| # | Task | Descricao | Status |
|---|------|-----------|--------|
| 1.5.1 | Env vars sysadmin | Adicionar sysadmin_email, sysadmin_password, sysadmin_access_token, sysadmin_refresh_token | PENDENTE |
| 1.5.2 | Login Sysadmin request | [POST] Login Sysadmin na pasta [Concierge] Backoffice | PENDENTE |

### MILESTONE 2: Tenants + Users (8 requests)

| # | Task | Descricao | Status |
|---|------|-----------|--------|
| 2.1 | Folder Tenants | [POST] Create (sysadmin), [GET] List, [GET] Detail, [PUT] Update | PENDENTE |
| 2.2 | Folder Users | [GET] List, [POST] Create, [PUT] Update, [DELETE] Deactivate | PENDENTE |

### MILESTONE 2.5: Concierge Backoffice (4 requests)

**Referencia:** [SYSADMIN_BLUEPRINT.md](../SYSADMIN_BLUEPRINT.md)

| # | Task | Descricao | Status |
|---|------|-----------|--------|
| 2.5.1 | Pasta [Concierge] Backoffice | Criar pasta com pre-request de auto-login sysadmin | PENDENTE |
| 2.5.2 | Create Tenant request | [POST] Create Tenant + test script salva last_created_tenant_id | PENDENTE |
| 2.5.3 | Create Manager request | [POST] Create Manager (gerente) com tenant_id dinamico | PENDENTE |
| 2.5.4 | Dashboard Backoffice | [GET] /v1/backoffice/dashboard — metricas globais (sysadmin only) | PENDENTE |

### MILESTONE 3: WhatsApp (7 requests)

| # | Task | Descricao | Status |
|---|------|-----------|--------|
| 3.1 | Folder WABA Numbers | [GET] List, [POST] Create, [GET] Detail, [PUT] Update, [DELETE] Deactivate | PENDENTE |
| 3.2 | Folder Webhook | [GET] Verification (Meta challenge), [POST] Receive Event (HMAC) | PENDENTE |

### MILESTONE 4: CRM (20 requests)

| # | Task | Descricao | Status |
|---|------|-----------|--------|
| 4.1 | Folder Contacts | [GET] List, [POST] Create, [GET] Detail, [PUT] Update, [DELETE] Deactivate | PENDENTE |
| 4.2 | Folder Groups | [GET] List, [POST] Create, [GET] Detail, [PUT] Update, [DELETE] Delete, [POST] Add Member, [DELETE] Remove Member | PENDENTE |
| 4.3 | Folder Portfolios | [GET] List, [POST] Create, [GET] Detail, [PUT] Update, [DELETE] Delete, [GET] List Contacts, [POST] Add Contact, [DELETE] Remove Contact | PENDENTE |

### MILESTONE 5: Chat (15 requests)

| # | Task | Descricao | Status |
|---|------|-----------|--------|
| 5.1 | Folder Conversations | [GET] List, [POST] Create, [GET] Detail, [PUT] Update Status, [POST] Assign | PENDENTE |
| 5.2 | Folder Messages | [GET] List, [POST] Send Text, [POST] Send Template, [POST] Send Media | PENDENTE |
| 5.3 | Folder Locks | [POST] Claim Lock, [DELETE] Release Lock, [GET] Get Active Lock | PENDENTE |
| 5.4 | Folder Notes | [GET] List, [POST] Add Note | PENDENTE |
| 5.5 | Folder Start Conversation | [POST] Start Outbound (com template) | PENDENTE |

### MILESTONE 6: WebSocket (documentacao)

| # | Task | Descricao | Status |
|---|------|-----------|--------|
| 6.1 | Folder WebSocket | Documentacao de referencia (nao executavel): eventos, payloads, handshake via GET /v1/ws?token={jwt} | PENDENTE |

### MILESTONE 7: Automation (15 requests)

| # | Task | Descricao | Status |
|---|------|-----------|--------|
| 7.1 | Folder Quick Replies | [GET] List, [POST] Create, [GET] Detail, [PUT] Update, [DELETE] Remove | PENDENTE |
| 7.2 | Folder SLA Configs | [GET] List, [POST] Create, [GET] Detail, [PUT] Update, [DELETE] Remove | PENDENTE |
| 7.3 | Folder Routing Rules | [GET] List, [POST] Create, [GET] Detail, [PUT] Update, [DELETE] Remove | PENDENTE |

### MILESTONE 9: Dashboard (2 requests)

| # | Task | Descricao | Status |
|---|------|-----------|--------|
| 9.1 | Folder Dashboard | [GET] Tenant Metrics (/v1/dashboard/metrics?period=7d) | PENDENTE |
| 9.2 | Backoffice Dashboard | [GET] Global Metrics (/v1/backoffice/dashboard) — sysadmin only | PENDENTE |

### MILESTONE 10: CI

| # | Task | Descricao | Status |
|---|------|-----------|--------|
| 10.1 | Newman CI | GitHub Action para rodar collection contra API | PENDENTE |

---

## Mapa Completo: Collection vs Rotas da API (73 endpoints)

```
WhatsApp GO API/
├── [Concierge] Backoffice/                          [sysadmin only]
│   ├── [POST] Login Sysadmin          → POST /v1/iam/login
│   ├── [POST] Create Tenant           → POST /v1/tenants
│   ├── [POST] Create Manager          → POST /v1/iam/users
│   ├── [GET]  List All Tenants        → GET  /v1/tenants
│   ├── [GET]  Get Tenant              → GET  /v1/tenants/{id}
│   ├── [PUT]  Update Tenant           → PUT  /v1/tenants/{id}
│   └── [GET]  Backoffice Dashboard    → GET  /v1/backoffice/dashboard
│
├── Auth/                                            [publico / auth]
│   ├── [POST] Login                   → POST /v1/iam/login
│   ├── [POST] Refresh Token           → POST /v1/iam/refresh
│   ├── [POST] Logout                  → POST /v1/iam/logout
│   └── [GET]  Me                      → GET  /v1/iam/me
│
├── Users/                                           [gerente+]
│   ├── [GET]    List Users            → GET    /v1/iam/users
│   ├── [POST]   Create User           → POST   /v1/iam/users
│   ├── [PUT]    Update User           → PUT    /v1/iam/users/{id}
│   └── [DELETE] Deactivate User       → DELETE /v1/iam/users/{id}
│
├── WhatsApp/
│   ├── WABA Numbers/                                [gerente+]
│   │   ├── [GET]    List Numbers      → GET    /v1/waba
│   │   ├── [POST]   Create Number     → POST   /v1/waba
│   │   ├── [GET]    Get Number        → GET    /v1/waba/{id}
│   │   ├── [PUT]    Update Number     → PUT    /v1/waba/{id}
│   │   └── [DELETE] Deactivate Number → DELETE /v1/waba/{id}
│   └── Webhook/                                     [publico — Meta callback]
│       ├── [GET]  Verify Endpoint     → GET  /v1/webhook/{id}
│       └── [POST] Receive Event       → POST /v1/webhook/{id}
│
├── CRM/
│   ├── Contacts/                                    [auth]
│   │   ├── [GET]    List Contacts     → GET    /v1/crm/contacts
│   │   ├── [POST]   Create Contact    → POST   /v1/crm/contacts
│   │   ├── [GET]    Get Contact       → GET    /v1/crm/contacts/{id}
│   │   ├── [PUT]    Update Contact    → PUT    /v1/crm/contacts/{id}
│   │   └── [DELETE] Deactivate Contact→ DELETE /v1/crm/contacts/{id}
│   ├── Groups/                                      [gerente+]
│   │   ├── [GET]    List Groups       → GET    /v1/crm/groups
│   │   ├── [POST]   Create Group      → POST   /v1/crm/groups
│   │   ├── [GET]    Get Group         → GET    /v1/crm/groups/{id}
│   │   ├── [PUT]    Update Group      → PUT    /v1/crm/groups/{id}
│   │   ├── [DELETE] Delete Group      → DELETE /v1/crm/groups/{id}
│   │   ├── [POST]   Add Member        → POST   /v1/crm/groups/{id}/members
│   │   └── [DELETE] Remove Member     → DELETE /v1/crm/groups/{id}/members/{user_id}
│   └── Portfolios/                                  [gerente+]
│       ├── [GET]    List Portfolios   → GET    /v1/crm/portfolios
│       ├── [POST]   Create Portfolio  → POST   /v1/crm/portfolios
│       ├── [GET]    Get Portfolio     → GET    /v1/crm/portfolios/{id}
│       ├── [PUT]    Update Portfolio  → PUT    /v1/crm/portfolios/{id}
│       ├── [DELETE] Delete Portfolio  → DELETE /v1/crm/portfolios/{id}
│       ├── [GET]    List Contacts     → GET    /v1/crm/portfolios/{id}/contacts
│       ├── [POST]   Add Contact       → POST   /v1/crm/portfolios/{id}/contacts
│       └── [DELETE] Remove Contact    → DELETE /v1/crm/portfolios/{id}/contacts/{contact_id}
│
├── Chat/
│   ├── Conversations/                               [auth + RBAC]
│   │   ├── [GET]  List Conversations  → GET  /v1/chat/conversations
│   │   ├── [POST] Create Conversation → POST /v1/chat/conversations
│   │   ├── [GET]  Get Conversation    → GET  /v1/chat/conversations/{id}
│   │   ├── [PUT]  Update Status       → PUT  /v1/chat/conversations/{id}/status
│   │   └── [POST] Assign             → POST /v1/chat/conversations/{id}/assign
│   ├── Messages/                                    [auth + lock required n1/n2]
│   │   ├── [GET]  List Messages       → GET  /v1/chat/conversations/{id}/messages
│   │   ├── [POST] Send Text           → POST /v1/chat/conversations/{id}/messages
│   │   ├── [POST] Send Template       → POST /v1/chat/conversations/{id}/messages/template
│   │   └── [POST] Send Media          → POST /v1/chat/conversations/{id}/messages/media
│   ├── Locks/                                       [auth]
│   │   ├── [POST]   Claim Lock        → POST   /v1/chat/conversations/{id}/lock
│   │   ├── [DELETE] Release Lock      → DELETE /v1/chat/conversations/{id}/lock
│   │   └── [GET]    Get Active Lock   → GET    /v1/chat/conversations/{id}/lock
│   ├── Notes/                                       [auth]
│   │   ├── [GET]  List Notes          → GET  /v1/chat/conversations/{id}/notes
│   │   └── [POST] Add Note            → POST /v1/chat/conversations/{id}/notes
│   └── Start/                                       [gerente+ ou n1]
│       └── [POST] Start Outbound      → POST /v1/chat/conversations/start
│
├── Automation/
│   ├── Quick Replies/                               [auth — RBAC: gerente=global, n1/n2=pessoal]
│   │   ├── [GET]    List              → GET    /v1/automation/quick-replies
│   │   ├── [POST]   Create            → POST   /v1/automation/quick-replies
│   │   ├── [GET]    Get               → GET    /v1/automation/quick-replies/{id}
│   │   ├── [PUT]    Update            → PUT    /v1/automation/quick-replies/{id}
│   │   └── [DELETE] Remove            → DELETE /v1/automation/quick-replies/{id}
│   ├── SLA Configs/                                 [gerente+]
│   │   ├── [GET]    List              → GET    /v1/automation/sla-configs
│   │   ├── [POST]   Create            → POST   /v1/automation/sla-configs
│   │   ├── [GET]    Get               → GET    /v1/automation/sla-configs/{id}
│   │   ├── [PUT]    Update            → PUT    /v1/automation/sla-configs/{id}
│   │   └── [DELETE] Remove            → DELETE /v1/automation/sla-configs/{id}
│   └── Routing Rules/                               [gerente+]
│       ├── [GET]    List              → GET    /v1/automation/routing-rules
│       ├── [POST]   Create            → POST   /v1/automation/routing-rules
│       ├── [GET]    Get               → GET    /v1/automation/routing-rules/{id}
│       ├── [PUT]    Update            → PUT    /v1/automation/routing-rules/{id}
│       └── [DELETE] Remove            → DELETE /v1/automation/routing-rules/{id}
│
├── Dashboard/                                       [gerente+]
│   └── [GET] Tenant Metrics           → GET /v1/dashboard/metrics?period=7d|30d
│
└── WebSocket/                                       [DOC only — nao executavel]
    └── [DOC] Protocol Reference       → GET /v1/ws?token={jwt} (upgrade)
```

**Total: 73 requests mapeados + 1 doc WebSocket = cobertura completa da API**
