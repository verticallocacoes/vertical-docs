# Como publicar o repositório de documentos técnicos

Este pacote é um site pronto (`vertical-docs/`) com um painel de administração embutido
(Decap CMS) para você adicionar e remover documentos sem mexer em código.

## O que você já tem
- Domínio `verticallocacoes.com.br` funcionando, DNS gerenciado em `tomcarvalho.com.br`.
- Site principal já no ar (feito pela agência).

## O que vamos fazer
Publicar este repositório em **Netlify** num subdomínio, por exemplo:
`docs.verticallocacoes.com.br`

Isso não interfere no site principal — é um site separado, só compartilha o domínio.

---

## Passo 1 — Subir o projeto pro GitHub

1. Crie uma conta gratuita em https://github.com (se ainda não tiver).
2. Crie um repositório novo, ex: `vertical-docs`.
3. Suba os arquivos desta pasta (`vertical-docs/`) para esse repositório.
   - Mais fácil: pelo próprio site do GitHub, arraste os arquivos em "Add file → Upload files".

## Passo 2 — Conectar no Netlify

1. Crie uma conta em https://netlify.com (dá pra logar direto com GitHub).
2. Clique em **"Add new site" → "Import an existing project"**.
3. Escolha o repositório `vertical-docs` que você acabou de subir.
4. Configurações de build: pode deixar em branco / "Deploy site" direto — é um site
   estático, não precisa de comando de build.
5. Clique em **Deploy**. Em ~1 minuto o site estará no ar num endereço tipo
   `nome-aleatorio.netlify.app`.

## Passo 3 — Ativar o painel de administração (login)

O painel (`/admin`) usa **Netlify Identity** + **Git Gateway** para autenticação —
assim só quem você convidar consegue editar.

1. No painel do Netlify, vá em **Site settings → Identity → Enable Identity**.
2. Em **Identity → Registration**, deixe como "Invite only" (só quem for convidado
   consegue criar conta).
3. Em **Identity → Services → Git Gateway**, clique em **Enable Git Gateway**.
4. Volte para a aba **Identity** e clique em **Invite users** — coloque seu e-mail
   (igor@vertical-al.com.br). Você receberá um convite por e-mail para criar sua senha.
5. Acesse `https://SEU-SITE.netlify.app/admin/`, faça login, e pronto: você já
   consegue adicionar/editar/remover categorias e documentos por lá.

## Passo 4 — Apontar o subdomínio (docs.verticallocacoes.com.br)

1. No Netlify: **Site settings → Domain management → Add a domain** →
   digite `docs.verticallocacoes.com.br`.
2. O Netlify vai te dar um valor de **CNAME** (algo como `SEU-SITE.netlify.app`).
3. No painel de DNS do seu domínio (gerenciado pela agência em `tomcarvalho.com.br` /
   cPanel), crie um registro:
   - Tipo: `CNAME`
   - Nome/Host: `docs`
   - Valor/Destino: `SEU-SITE.netlify.app`
   - TTL: padrão
4. Se você não tem acesso a esse painel de DNS, peça pra agência (Tom Carvalho)
   criar esse único registro CNAME — é uma mudança de 2 minutos e não afeta o site
   principal.
5. Em algumas horas o certificado SSL é emitido automaticamente pelo Netlify e o
   site fica acessível em `https://docs.verticallocacoes.com.br`.

---

## Como usar o painel no dia a dia

- Acesse `https://docs.verticallocacoes.com.br/admin/`
- Clique em **"Categorias e Documentos"**
- Para adicionar uma categoria nova: clique em **"Add categorias"**, preencha nome
  e slug (ex: "Concretagem" / `concretagem`).
- Para adicionar um documento numa categoria: abra a categoria, clique em
  **"Add documentos"**, dê um título, envie o arquivo (PDF, imagem, etc.) e
  clique em **Publish**.
- Para remover, basta abrir o item e clicar no ícone de lixeira, depois **Publish**.
- Toda alteração publicada atualiza o site em ~1 minuto automaticamente.

## Estrutura do projeto

```
vertical-docs/
├── index.html          → página inicial (lista de categorias)
├── categoria.html       → página de uma categoria (lista de documentos)
├── css/style.css        → estilo visual
├── content/site.json    → dados (categorias + documentos) — editado via /admin
├── uploads/              → onde os arquivos enviados pelo painel ficam salvos
├── admin/
│   ├── index.html        → carrega o painel Decap CMS
│   └── config.yml        → configuração do painel (campos, coleções)
└── netlify.toml          → configuração de build/headers do Netlify
```

## Personalizar visual

- Cores: edite as variáveis no topo de `css/style.css` (`--azul`, `--azul-claro`).
- Logo: pode trocar o texto "📁 Materiais Técnicos" em `index.html` e
  `categoria.html` por uma tag `<img>` com a logo da Vertical.

## Próximos passos possíveis (se quiser evoluir depois)
- Trocar Netlify Identity por um login mais robusto (Auth0, Clerk) se a equipe crescer.
- Adicionar busca por texto entre os documentos.
- Integrar esse repositório como uma seção dentro do site principal, se a agência
  preferir hospedar tudo junto no futuro.
