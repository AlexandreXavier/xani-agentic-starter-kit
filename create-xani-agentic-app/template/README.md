# Xani Agentic Starter Kit

Um starter kit orientado para produção que permite criar aplicações web com inteligência artificial através de um fluxo de desenvolvimento agêntico. Inclui uma aplicação Next.js funcional, autenticação, PostgreSQL, Drizzle ORM, integração com o AI SDK, componentes shadcn/ui e instruções de projeto que ajudam os agentes de programação a planear, dividir, implementar, rever e verificar alterações.

O objetivo é simples: instalar o starter, descrever o produto que pretende construir e deixar o seu agente de programação ajudar a transformar o boilerplate na sua POC, MVP ou ferramenta interna real.

## O Que Recebe

- **Next.js 16 e React 19** com o App Router
- **TypeScript** e uma configuração de projeto rigorosa
- **Better Auth** com autenticação por email/palavra-passe ativada por predefinição
- **PostgreSQL e Drizzle ORM** para esquema e migrações
- **AI SDK e OpenRouter** para chat e funcionalidades de IA
- **shadcn/ui, Tailwind CSS e ícones Lucide** como base da interface
- **Armazenamento de ficheiros local ou Vercel Blob** através de uma única abstração de armazenamento
- **Instruções para agentes** através de `AGENTS.md` e `CLAUDE.md`
- **Skills para agentes** para especificações, implementação, revisões, análises de segurança, trabalho de interface e publicação

## Início Rápido

Crie uma nova aplicação com a CLI:

```bash
npx create-xani-agentic-app@latest my-app
cd my-app
```

Ou crie a aplicação no diretório atual:

```bash
npx create-xani-agentic-app@latest .
```

De seguida, configure e execute a aplicação:

```bash
cp env.example .env
docker compose up -d
pnpm db:migrate
pnpm dev
```

Abra [http://localhost:3000](http://localhost:3000).

A CLI copia os ficheiros do starter, instala as dependências com o gestor de pacotes que selecionou e prepara o ficheiro de ambiente. Se utilizar o `npm`, substitua os comandos `pnpm` acima por `npm run`.

## Configuração Guiada com o Claude Code (Opcional)

Se utilizar o Claude Code, pode instalar a skill `create-xani-agentic-app` e deixar o Claude conduzi-lo por toda a configuração — estratégia de pastas, gestor de pacotes, PostgreSQL (Docker / Neon / Vercel / próprio), configuração do `.env`, migrações, integrações opcionais (OpenRouter, Vercel Blob, Polar, email), verificação de build e validação do servidor de desenvolvimento — terminando num `http://localhost:3000` verificado.

Instale a skill:

```bash
npx skills add AlexandreXavier/xani-agentic-starter-kit@create-xani-agentic-app --agent claude-code
```

A flag `--agent claude-code` é obrigatória. Sem ela, o instalador coloca a skill em mais de 37 pastas de adaptadores de IDE na raiz do projeto.

Depois de instalada, peça algo ao Claude como:

```text
Scaffold a new Xani Agentic Starter Kit project here.
```

O Claude executará a skill de ponta a ponta e fará apenas as poucas decisões que realmente precisa de tomar.

## Pré-requisitos

- Node.js 18 ou mais recente
- Git
- PostgreSQL, através do ficheiro Docker Compose incluído ou de um fornecedor alojado
- Um gestor de pacotes: `pnpm`, `npm` ou `yarn`
- Opcional: uma chave de API do OpenRouter para as funcionalidades de chat com IA
- Opcional: uma conta Vercel para implementação, Postgres alojado e armazenamento Blob

## Variáveis de Ambiente

Comece a partir de `env.example` e atualize os valores para o seu ambiente:

```env
# Database
POSTGRES_URL=postgresql://dev_user:dev_password@localhost:5432/postgres_dev

# Authentication - Better Auth
BETTER_AUTH_SECRET=your-random-secret

# AI Integration via OpenRouter
OPENROUTER_API_KEY=
OPENROUTER_MODEL="openai/gpt-5-mini"

# Optional - for vector search only
OPENAI_EMBEDDING_MODEL="text-embedding-3-large"

# App URL
NEXT_PUBLIC_APP_URL="http://localhost:3000"

# File storage
BLOB_READ_WRITE_TOKEN=

# Polar payment processing
POLAR_WEBHOOK_SECRET=polar_
POLAR_ACCESS_TOKEN=polar_
```

Para desenvolvimento local, o URL de base de dados predefinido funciona com o `docker-compose.yml` incluído. Para produção, utilize o URL de base de dados do seu fornecedor de alojamento.

Gere um `BETTER_AUTH_SECRET` forte antes de implementar. O starter inclui apenas um valor de desenvolvimento para que possa começar rapidamente.

## Autenticação Predefinida

O starter passa agora a utilizar por predefinição a **autenticação por email e palavra-passe** através do Better Auth. Isto mantém a primeira configuração simples e ajuda-o a começar a construir POCs e MVPs sem criar credenciais OAuth à partida.

A configuração de autenticação atual inclui:

- registo de utilizadores
- início de sessão por email/palavra-passe
- rotas protegidas
- fluxo de reposição de palavra-passe
- fluxo de verificação de email

Em desenvolvimento, as ligações de verificação e de reposição de palavra-passe são registadas no terminal em vez de serem enviadas através de um fornecedor de email. Quando estiver pronto para produção, peça ao seu agente de programação para ligar um serviço de email e atualizar os callbacks de email do Better Auth.

### Adicionar o Google OAuth

O Google OAuth já não é a predefinição, mas voltar a adicioná-lo é simples. Peça ao seu agente de programação:

```text
Add Google OAuth to this Better Auth setup. Keep email/password login enabled, add the Google provider, update the auth UI, and document the required Google environment variables.
```

O seu agente deverá atualizar a configuração do Better Auth, adicionar as variáveis `GOOGLE_CLIENT_ID` e `GOOGLE_CLIENT_SECRET` necessárias e ajustar a interface de início de sessão.

## Construir com um Agente

Este starter foi concebido para ser utilizado com agentes de programação. O projeto gerado inclui instruções que dizem aos agentes como planear, fazer perguntas, dividir o trabalho, utilizar sub-agentes quando útil, seguir o sistema de design e verificar as alterações.

- `AGENTS.md` é o ficheiro de instruções principal para o Codex, o Cursor e outras ferramentas compatíveis com agentes.
- `CLAUDE.md` encaminha os utilizadores do Claude para a mesma orientação de projeto.
- `.agents/skills/` e `.claude/skills/` incluem fluxos de trabalho opcionais para tarefas mais especializadas.
- `DESIGN.md` define o sistema de design da interface que os agentes devem seguir.

O fluxo de trabalho predefinido não requer comandos de barra (slash commands) nem um ficheiro de especificação separado.

### Fluxo de Trabalho Predefinido Recomendado

1. Instale o starter e abra o projeto no ambiente do seu agente de programação.
2. Coloque a sua ferramenta de agente em modo de planeamento.
3. Descreva a aplicação que pretende construir em linguagem simples.
4. Deixe o agente fazer perguntas de esclarecimento e dar forma a um plano claro.
5. Confirme o plano assim que o objetivo, o âmbito, as restrições e os critérios de sucesso estiverem claros.
6. Coloque a sua ferramenta de agente em modo de edição.
7. Peça ao agente para implementar o plano aprovado.
8. O agente principal deve dividir o trabalho em fluxos paralelos, silos ou blocos de funcionalidades que caibam no contexto.
9. O agente deve utilizar sub-agentes para implementar esses blocos em paralelo, quando útil, e depois coordenar os resultados.
10. O agente deve executar verificações de qualidade como lint, typecheck e build.
11. Reveja o resultado no navegador e itere.

Não necessita de um comando especial para este fluxo de trabalho predefinido. As instruções do projeto já dizem ao agente como planear, dividir o trabalho de implementação, utilizar sub-agentes e verificar o resultado.

## Prompt Inicial

Utilize isto como primeira mensagem para o seu agente de programação após instalar o starter:

```text
I am using the Xani Agentic Starter Kit. Treat the existing app as boilerplate that should be replaced by the product I describe.

Use the project instructions in AGENTS.md or CLAUDE.md. During planning, ask clarifying questions before making assumptions. During implementation, split the work into small chunks, use sub-agents where useful, follow DESIGN.md for UI, preserve the existing tech stack unless there is a good reason to change it, and run lint, typecheck, and build before finishing.

What I want to build:
[Describe your app here]
```

Por exemplo:

```text
What I want to build:
A lightweight CRM for solo consultants. It should let users manage clients, track deals, write notes, set follow-up reminders, and view a simple dashboard of open opportunities.
```

## Quando Utilizar Especificações

Para a maioria das POCs e MVPs, o fluxo de trabalho normal do agente é suficiente. Utilize uma especificação quando a funcionalidade for grande, de longa duração, arriscada ou tiver de ser dividida por várias sessões de implementação.

O starter inclui duas skills para esse fluxo de trabalho:

- `create-spec`: transforma uma conversa de planeamento em `specs/{feature}/` com requisitos, ficheiros de tarefas, ondas de dependências e notas de ações manuais.
- `implement-feature`: lê uma pasta de especificação e coordena a implementação onda a onda com pontos de controlo de revisão.

Utilize este fluxo de trabalho quando:

- a funcionalidade abrange muitos ficheiros ou módulos
- vários agentes devem trabalhar em paralelo
- a implementação pode demorar mais do que uma sessão
- precisa de acompanhamento de progresso retomável
- pretende um registo de implementação escrito antes de começar a programar

Exemplo de pedido ao agente:

```text
Create a spec for the billing and subscriptions feature we just planned. Break it into parallel implementation waves and include any manual setup steps.
```

Depois:

```text
Implement the billing and subscriptions spec from specs/billing-subscriptions.
```

## Estrutura do Projeto

```text
src/
├── app/
│   ├── (auth)/
│   │   ├── forgot-password/
│   │   ├── login/
│   │   ├── register/
│   │   └── reset-password/
│   ├── api/
│   │   ├── auth/
│   │   ├── chat/
│   │   └── diagnostics/
│   ├── chat/
│   ├── dashboard/
│   ├── profile/
│   ├── layout.tsx
│   └── page.tsx
├── components/
│   ├── auth/
│   ├── ui/
│   ├── site-footer.tsx
│   └── site-header.tsx
├── hooks/
└── lib/
    ├── auth.ts
    ├── auth-client.ts
    ├── db.ts
    ├── env.ts
    ├── schema.ts
    ├── session.ts
    ├── storage.ts
    └── utils.ts
```

Ficheiros importantes na raiz:

- `AGENTS.md`: regras de comportamento dos agentes de programação
- `CLAUDE.md`: ponto de entrada do Claude para a mesma orientação
- `DESIGN.md`: sistema de design da interface e orientação sobre componentes
- `drizzle.config.ts`: configuração das migrações do Drizzle
- `docker-compose.yml`: serviço PostgreSQL local
- `env.example`: modelo das variáveis de ambiente
- `components.json`: configuração do shadcn/ui

## Scripts Disponíveis

```bash
pnpm dev           # Start the development server with Turbopack
pnpm build         # Run migrations, then build for production
pnpm build:ci      # Build without running migrations
pnpm start         # Start the production server
pnpm lint          # Run ESLint
pnpm typecheck     # Run TypeScript without emitting files
pnpm check         # Run lint and typecheck
pnpm format        # Format the repository
pnpm format:check  # Check formatting
pnpm setup         # Run the setup script
pnpm db:generate   # Generate Drizzle migrations
pnpm db:migrate    # Run Drizzle migrations
pnpm db:studio     # Open Drizzle Studio
```

O repositório também contém scripts auxiliares de push/reset do Drizzle para experimentação local. Para alterações de esquema que pretenda manter, prefira:

```bash
pnpm db:generate
pnpm db:migrate
```

Não utilize o schema push como substituto das migrações em trabalho de projeto real.

## Fluxo de Trabalho da Base de Dados

Para desenvolvimento local:

```bash
docker compose up -d
pnpm db:migrate
```

Quando a sua aplicação necessitar de alterações de esquema, peça ao seu agente para atualizar `src/lib/schema.ts`, gerar uma migração e executá-la:

```bash
pnpm db:generate
pnpm db:migrate
```

Se implementar na Vercel ou noutro ambiente alojado, defina `POSTGRES_URL` nesse ambiente antes de executar as migrações ou compilar a aplicação.

## Funcionalidades de IA

O starter utiliza o Vercel AI SDK com o OpenRouter. Defina estas variáveis para ativar o chat com IA:

```env
OPENROUTER_API_KEY=sk-or-v1-your-key
OPENROUTER_MODEL="openai/gpt-5-mini"
```

O OpenRouter permite-lhe trocar de modelo sem alterar o código da aplicação. Atualize `OPENROUTER_MODEL` quando quiser experimentar um modelo diferente.

## Armazenamento de Ficheiros

O starter inclui uma abstração de armazenamento que pode utilizar armazenamento local em desenvolvimento ou o Vercel Blob em produção.

Para desenvolvimento local, deixe `BLOB_READ_WRITE_TOKEN` vazio. Os ficheiros são guardados em `public/uploads/`.

Para o Vercel Blob:

1. Crie um Blob store na Vercel.
2. Copie o `BLOB_READ_WRITE_TOKEN`.
3. Adicione-o às variáveis de ambiente de produção.

A aplicação escolhe o backend de armazenamento consoante o `BLOB_READ_WRITE_TOKEN` esteja ou não configurado.

## Implementação

A Vercel é o destino de implementação recomendado.

```bash
npm install -g vercel
vercel --prod
```

Defina as variáveis de ambiente de produção necessárias:

- `POSTGRES_URL`
- `BETTER_AUTH_SECRET`
- `NEXT_PUBLIC_APP_URL`
- `OPENROUTER_API_KEY`, se utilizar funcionalidades de IA
- `OPENROUTER_MODEL`, se utilizar funcionalidades de IA
- `BLOB_READ_WRITE_TOKEN`, se utilizar o Vercel Blob
- `POLAR_WEBHOOK_SECRET` e `POLAR_ACCESS_TOKEN`, se utilizar pagamentos Polar

O script `pnpm build` predefinido executa as migrações da base de dados antes do `next build`. Se a sua CI ou alojamento não deve executar migrações durante o build, utilize `pnpm build:ci` e execute as migrações como um passo de implementação separado.

## Resolução de Problemas

### A aplicação não consegue ligar-se ao Postgres

Confirme que o Docker está a correr e inicie a base de dados:

```bash
docker compose up -d
```

Depois verifique que o `POSTGRES_URL` no `.env` corresponde à string de ligação da base de dados.

### Os emails de reposição ou verificação de autenticação não chegam

Em desenvolvimento, as ligações são registadas no terminal. Isto é intencional. Ligue um fornecedor de email antes de utilizar a reposição de palavra-passe ou a verificação em produção.

### O chat com IA não funciona

Defina `OPENROUTER_API_KEY` e reinicie o servidor de desenvolvimento. Confirme também que `OPENROUTER_MODEL` é um modelo disponível na sua conta OpenRouter.

### O meu agente está a preservar demasiado boilerplate

Diga ao agente diretamente que a interface do starter é andaime (scaffolding) e deve ser substituída:

```text
Replace the starter UI with the actual product UI. Do not keep setup checklists, placeholder navigation, demo content, or boilerplate copy unless I explicitly ask for it.
```

### Preciso de início de sessão com a Google

Peça ao seu agente para adicionar o Google OAuth através do Better Auth, mantendo o email/palavra-passe ativado. Vai precisar de credenciais OAuth da Google e de URLs de callback de produção.

## Contribuir

1. Faça fork deste repositório.
2. Crie um branch de funcionalidade.
3. Faça as suas alterações.
4. Execute as verificações relevantes.
5. Abra um pull request.

## Licença

Este projeto está licenciado sob a Licença MIT.

## Precisa de Ajuda?

- Consulte os issues do repositório: [github.com/AlexandreXavier/xani-agentic-starter-kit/issues](https://github.com/AlexandreXavier/xani-agentic-starter-kit/issues)
- Reveja `AGENTS.md`, `CLAUDE.md` e `DESIGN.md`
- Abra um novo issue com os passos exatos de configuração, o output do erro e os detalhes do ambiente
