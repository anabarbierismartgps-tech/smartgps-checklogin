# SmartGPS · Consulta de Login (checklogin)

Tela simples para consultar **em qual servidor/cluster** um login está e **quem é o admin (conta acima)** dele, usando o endpoint interno `__external/checklogin`.

É um app **estático** (um único `index.html`, sem build e sem backend). O endpoint tem CORS liberado e não exige token, então roda direto no navegador.

## Como usar

Abra o `index.html` no navegador — ou sirva a pasta:

```bash
python3 -m http.server 8777
# depois acesse http://localhost:8777
```

Digite o login e pressione **Enter** (ou clique em **Consultar**).
Também dá para consultar por link direto: `index.html?login=demosp`.

## API

```
GET https://api-sp.smartgps.com.br/__external/checklogin/{login}
```

Apesar do nome `api-sp`, esse endpoint funciona como gateway e localiza o login em qualquer servidor.

### Formatos de resposta

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

## Configuração

No topo do `<script>` em `index.html`:

- `API_BASE` — URL base do endpoint.
- `SERVER_NAMES` — mapeamento de código → nome amigável do servidor (edite à vontade).

## Estrutura

```
index.html   → app completo (HTML + CSS + JS inline)
README.md
```
