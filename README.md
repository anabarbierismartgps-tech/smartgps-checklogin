# SmartGPS · Consulta (checklogin / checkimei)

Tela simples com duas consultas:

- **Login** — em qual servidor/cluster um login está e **quem é o admin (conta acima)** dele (`__external/checklogin`).
- **IMEI** — em qual(is) servidor(es) um IMEI está **cadastrado** (`__external/checkimei`).

É um app **estático** (um único `index.html`, sem build e sem backend). Os endpoints têm CORS liberado e não exigem token, então roda direto no navegador.

## Como usar

Abra o `index.html` no navegador — ou sirva a pasta:

```bash
python3 -m http.server 8777
# depois acesse http://localhost:8777
```

Escolha o modo (**Login** ou **IMEI**), digite o termo e pressione **Enter** (ou clique em **Consultar**).
Também dá para consultar por link direto: `index.html?login=demosp` ou `index.html?imei=862476054300065`.

## API

### Login

```
GET https://api-sp.smartgps.com.br/__external/checklogin/{login}
```

Apesar do nome `api-sp`, esse endpoint funciona como gateway e localiza o login em qualquer servidor.

#### Formatos de resposta

**Login encontrado, conta principal (sem admin acima):**
```json
{ "SP": { "has_admin_above": false, "admin": null } }
```

**Login encontrado, com admin acima:**
```json
{ "AL": { "has_admin_above": true, "admin": { "client_name": "GOL PLUS ...", "email": "financeiro@..." } } }
```

**Login não encontrado:**
```json
{ "server": null, "error": "User not found" }
```

A chave de topo é o **código do servidor** (`SP`, `AL`, `MA`, `SC`, …).

### IMEI

```
GET https://api-sp.smartgps.com.br/__external/checkimei/{imei}
```

#### Formatos de resposta

**IMEI não cadastrado:**
```json
{ "existingServers": [] }
```

**IMEI cadastrado:** `existingServers` traz a lista de servidores. O renderizador
é defensivo — aceita tanto uma lista de siglas (`["SP","AL"]`) quanto de objetos
(`[{ "server": "SP" }, ...]`), extraindo a sigla de `server`/`code`/`name`/`cluster`.

## Configuração

No topo do `<script>` em `index.html`:

- `API_LOGIN_BASE` / `API_IMEI_BASE` — URLs base dos endpoints.
- `MODES` — configuração de cada modo (placeholder, subtítulo, etc.).

## Estrutura

```
index.html   → app completo (HTML + CSS + JS inline)
README.md
```
