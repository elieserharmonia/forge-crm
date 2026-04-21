# FORECAST-CRM — Guia de Deploy com Supabase + Google OAuth
# ═══════════════════════════════════════════════════════════

## VISÃO GERAL
Após seguir este guia, o FORECAST-CRM terá:
✅ Login com Google (OAuth seguro)
✅ 3 perfis: Vendedor / Engenheiro / Gestor
✅ Dados no Supabase (sincronizados entre usuários)
✅ Deploy automático no Vercel

---

## PASSO 1 — SUPABASE: Criar o banco de dados

1. Acesse https://supabase.com/dashboard
2. Abra o projeto `forecast-crm`
3. Vá em **SQL Editor** → **New query**
4. Cole TODO o conteúdo do arquivo `supabase-setup.sql`
5. Clique em **Run** (▶)
6. Confirme: deve aparecer "Success. No rows returned"

---

## PASSO 2 — SUPABASE: Ativar Google OAuth

### 2a. Google Cloud Console
1. Acesse https://console.cloud.google.com
2. Crie um projeto novo (ex: "forecast-crm")
3. Vá em **APIs & Services → OAuth consent screen**
   - User Type: External
   - App name: FORECAST-CRM
   - User support email: elieser.sinuelo@gmail.com
   - Developer contact: elieser.sinuelo@gmail.com
   - Clique em SAVE
4. Vá em **APIs & Services → Credentials**
5. Clique em **+ CREATE CREDENTIALS → OAuth client ID**
   - Application type: **Web application**
   - Name: forecast-crm
   - Authorized redirect URIs — adicione EXATAMENTE:
     `https://SEU_PROJECT_ID.supabase.co/auth/v1/callback`
     (substitua SEU_PROJECT_ID pelo ID do seu projeto Supabase)
6. Clique em **CREATE**
7. Copie o **Client ID** e **Client Secret**

### 2b. Supabase Auth
1. No Supabase, vá em **Authentication → Providers**
2. Clique em **Google**
3. Toggle **Enable**
4. Cole o **Client ID** e **Client Secret** do passo anterior
5. Clique em **Save**

---

## PASSO 3 — SUPABASE: Obter as chaves da API

1. No Supabase, vá em **Settings → API**
2. Copie:
   - **Project URL**: `https://xxxxxxxx.supabase.co`
   - **anon/public key**: `eyJhbGc...` (chave longa)

---

## PASSO 4 — GITHUB: Atualizar o repositório

O projeto agora é um app Vite (não mais HTML estático).
Você precisa substituir o repositório atual.

### Opção A — Via terminal (recomendado):
```bash
cd forecast-crm          # pasta com os arquivos novos
git init
git remote add origin https://github.com/elieserharmonia/forge-crm.git
git add .
git commit -m "feat: migrar para Vite + Supabase auth"
git push --force origin main
```

### Opção B — Via GitHub.com:
1. Acesse https://github.com/elieserharmonia/forge-crm
2. Delete todos os arquivos existentes
3. Faça upload dos novos arquivos da pasta `forecast-crm`

---

## PASSO 5 — VERCEL: Reconfigurar o projeto

1. Acesse https://vercel.com/dashboard
2. Abra o projeto `forge-crm`
3. Vá em **Settings → Environment Variables**
4. Adicione as duas variáveis:
   ```
   VITE_SUPABASE_URL = https://xxxxxxxx.supabase.co
   VITE_SUPABASE_ANON_KEY = eyJhbGc...
   ```
5. Vá em **Settings → Build & Development Settings**
   - Framework Preset: **Vite**
   - Build Command: `npm run build`
   - Output Directory: `dist`
6. Vá em **Deployments** → clique em **Redeploy** no último deploy

---

## PASSO 6 — SUPABASE: Adicionar URL de callback do Vercel

1. No Supabase, vá em **Authentication → URL Configuration**
2. Em **Site URL**, coloque: `https://forge-crm-tawny.vercel.app`
3. Em **Redirect URLs**, adicione: `https://forge-crm-tawny.vercel.app/**`
4. Salve

---

## PASSO 7 — PRIMEIRO LOGIN de cada usuário

Cada pessoa acessa https://forge-crm-tawny.vercel.app e clica em
"Entrar com Google" com seu e-mail autorizado.

O sistema cria automaticamente o perfil correto:
- eliesermusicoccb@gmail.com → **Vendedor** (Elieser Fernandes)
- elmorallesfernandes@gmail.com → **Engenheiro** (Washington)
- elieser.sinuelo@gmail.com → **Gestor** (Rafael Gomez)

---

## RESUMO DE PERMISSÕES

| Ação | Vendedor | Engenheiro | Gestor |
|------|----------|------------|--------|
| Ver pipeline | Só as suas | Todas | Todas |
| Criar oportunidade | ✅ | ❌ | ✅ |
| Editar oportunidade | Só as suas | ❌ | ✅ |
| Excluir oportunidade | ❌ | ❌ | ✅ |
| Ver clientes | ✅ | ✅ | ✅ |
| Editar clientes | ❌ | ❌ | ✅ |
| Ver projetos | ✅ | ✅ | ✅ |
| Editar projetos | ❌ | ✅ | ✅ |
| Budget/configurações | ❌ | ❌ | ✅ |
| API Key (IA) | ❌ | ❌ | ✅ (senha) |

---

## SUPORTE

Em caso de dúvida em qualquer passo, entre em contato.
