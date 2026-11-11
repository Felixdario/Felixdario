<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/banner-dark.svg">
  <img src="assets/banner-light.svg" width="100%" alt="Eduardo Felix, desenvolvedor full-stack júnior em Curitiba e região">
</picture>

# Eduardo Felix

Desenvolvedor full-stack júnior em Curitiba e região. Escrevo APIs em Node.js e TypeScript, interfaces em React e apps em React Native. Nos três projetos em destaque, as garantias principais têm teste.

**English:** [read the English version](#english-version).

## Sobre mim

- Trabalho como Full-Stack Developer Junior na Epays desde agosto de 2025, numa biblioteca de componentes React com Vite usada em produtos internos.
- Antes, trabalhei com back-end no BNE, com React Native na Floox e com React, Node.js e internacionalização na LUGARH.
- Técnico em Informática no IFPR (2022–2025); hoje curso Análise e Desenvolvimento de Sistemas na PUCPR.
- Construí os três projetos em destaque com agentes de IA sob a minha direção, com as decisões registradas por escrito (ADRs e registros de decisão) e testes que provam o resultado.

## Projetos em destaque

### [Pulo](https://github.com/Felixdario/Pulo): encurtador de URLs com analytics

<img src="https://raw.githubusercontent.com/Felixdario/Pulo/main/docs/screenshots/link-5m-1440-claro.png" width="100%" alt="Página de um link no Pulo com 4,4 milhões de cliques humanos: contador ao vivo, barra com humanos, bots e prévias, série de 12 meses e mapa de países">

**O problema:** contar cada clique humano uma vez, com robôs e prévias de WhatsApp à parte, sem deixar o redirecionamento lento.

- Redirecionamento com p95 de 6,42 ms e 10.129 req/s em um processo: com o link em cache, uma ida ao Redis e nenhuma ao PostgreSQL ([medição local](https://github.com/Felixdario/Pulo/blob/main/docs/performance.md#L64)).
- Pipeline de cliques exatamente uma vez com Redis Streams e worker idempotente ([teste com 100.000 eventos](https://github.com/Felixdario/Pulo/blob/main/apps/worker/test/integration/pipeline.test.ts#L141)).
- Com 5 milhões de cliques e cache quente, as consultas por link leem agregados em 0,19 a 0,59 ms, contra 28 a 122 ms do `COUNT` na tabela bruta, medidos numa máquina local de desenvolvimento ([tabela](https://github.com/Felixdario/Pulo/blob/main/README.md#L173-L178), [ADRs](https://github.com/Felixdario/Pulo/tree/main/docs/adr)).

**Stack:** TypeScript, Fastify, Drizzle, PostgreSQL, Redis, React, Vite, Vitest e Playwright.

### [Pontual (repositório Agenda)](https://github.com/Felixdario/Agenda): agendamento que não marca duas vezes o mesmo horário

<img src="https://raw.githubusercontent.com/Felixdario/Agenda/main/docs/screenshots/painel-detalhe.png" width="100%" alt="Painel do Pontual: agenda do dia em colunas por profissional, com o tempo de preparo e limpeza hachurado, e o detalhe de um atendimento">

**O problema:** barbearias, salões e clínicas precisam de reserva online sem conflito de horário, mesmo quando dois clientes confirmam ao mesmo tempo.

- Restrição `EXCLUDE USING gist` no PostgreSQL como última barreira contra sobreposição ([migração](https://github.com/Felixdario/Agenda/blob/main/apps/api/prisma/migrations/20261108194040_exclusao_agendamentos/migration.sql#L5-L12)).
- 20 pedidos simultâneos para o mesmo horário resultam em 1 reserva e 19 recusas por horário indisponível (409 na API), em 10 rodadas ([teste](https://github.com/Felixdario/Agenda/blob/main/apps/api/test/integracao/concorrencia.test.ts#L23-L44)).
- Motor de disponibilidade sem dependência de framework, testado com propriedades (fast-check) ([teste](https://github.com/Felixdario/Agenda/blob/main/apps/api/src/domain/propriedades.test.ts#L170-L193)); 24 regras de negócio rastreadas até os testes ([regras](https://github.com/Felixdario/Agenda/blob/main/docs/regras-de-negocio.md), [decisões](https://github.com/Felixdario/Agenda/blob/main/docs/decisoes.md)).

**Stack:** TypeScript, NestJS, Prisma, PostgreSQL, Next.js, React, Tailwind CSS, Vitest e Playwright.

### [Sobra](https://github.com/Felixdario/Sobra): controle financeiro na web e no celular

<table>
  <tr>
    <td width="62%"><img src="https://raw.githubusercontent.com/Felixdario/Sobra/main/docs/screenshots/web/painel-outubro-claro-desktop.png" alt="Painel web do Sobra: sobra do mês, régua de gastos por categoria, saldo, receitas, despesas e gráficos do mês"></td>
    <td width="38%"><img src="https://raw.githubusercontent.com/Felixdario/Sobra/main/docs/screenshots/mobile/resumo-outubro-claro.png" alt="Resumo do mês no app mobile do Sobra: quanto sobrou, régua do mês e orçamentos"></td>
  </tr>
</table>

**O problema:** lançar o gasto na hora pelo celular e entender o mês no painel web.

- API, web e mobile num monorepo, com um contrato Zod que valida a entrada na API e a resposta no cliente ([arquitetura](https://github.com/Felixdario/Sobra/blob/main/README.md#L146-L148)).
- Refresh token rotativo com detecção de reuso ([teste](https://github.com/Felixdario/Sobra/blob/main/apps/api/src/modules/auth/auth.test.ts#L227)); no web, cookie `httpOnly` com proteção CSRF ([rota](https://github.com/Felixdario/Sobra/blob/main/apps/api/src/modules/auth/routes.ts#L48-L68)).
- Três requisições concorrentes com o token de acesso inválido renovam a sessão uma vez só ([teste](https://github.com/Felixdario/Sobra/blob/main/apps/api/src/client.integration.test.ts#L79)).
- 1087 testes com Vitest, Testing Library, axe-core e Jest ([contagem](https://github.com/Felixdario/Sobra/blob/main/README.md#L310-L320), [decisões](https://github.com/Felixdario/Sobra/blob/main/docs/decisoes.md)).

**Stack:** TypeScript, Fastify, Prisma, SQLite, React, Vite, React Native e Expo.

### Outros projetos

- **SportLink** (meu TCC do técnico no IFPR): [web em Next.js](https://github.com/Felixdario/sportlink-webapp), publicado na [Vercel](https://sportlink-webapp.vercel.app); [app em React Native com Expo](https://github.com/Felixdario/sportlink-mobile); [API em Python com FastAPI](https://github.com/Felixdario/sportlink-api).
- **[Oceanic.ui](https://github.com/Felixdario/Oceanic.ui):** site de design system em Next.js com Radix e Material UI, publicado na [Vercel](https://design-system-six-gamma.vercel.app).
- **[prospecta](https://github.com/Felixdario/prospecta):** ferramenta em Python (Starlette e SQLite) que busca empresas pela API do Google Places e organiza a abordagem comercial.

## Competências

| Área | O que uso | Onde ver |
| - | - | - |
| Back-end | Node.js, TypeScript, Fastify, NestJS, Python (Starlette), REST com OpenAPI, Redis Streams | [Pulo](https://github.com/Felixdario/Pulo), [Pontual](https://github.com/Felixdario/Agenda), [Sobra](https://github.com/Felixdario/Sobra), [prospecta](https://github.com/Felixdario/prospecta/tree/main/prospecta/tests) |
| Front-end | React, Next.js, Vite, TanStack Query, Tailwind CSS, acessibilidade testada com axe | [Pontual](https://github.com/Felixdario/Agenda), [Pulo](https://github.com/Felixdario/Pulo), [Sobra](https://github.com/Felixdario/Sobra), [Oceanic.ui](https://github.com/Felixdario/Oceanic.ui) |
| Mobile | React Native, Expo | [Sobra](https://github.com/Felixdario/Sobra), [SportLink mobile](https://github.com/Felixdario/sportlink-mobile) |
| Banco de dados | PostgreSQL, Prisma, Drizzle, Redis, SQLite | [Pontual](https://github.com/Felixdario/Agenda), [Pulo](https://github.com/Felixdario/Pulo), [Sobra](https://github.com/Felixdario/Sobra) |
| Qualidade | Vitest, Playwright, fast-check, Testing Library, Jest | [Pontual](https://github.com/Felixdario/Agenda), [Pulo](https://github.com/Felixdario/Pulo), [Sobra](https://github.com/Felixdario/Sobra) |
| DevOps | GitHub Actions, Docker Compose, Vercel | Actions: [Pontual](https://github.com/Felixdario/Agenda/blob/main/.github/workflows/ci.yml), [Sobra](https://github.com/Felixdario/Sobra/blob/main/.github/workflows/ci.yml), [Pulo](https://github.com/Felixdario/Pulo/blob/main/.github/workflows/ci.yml) · Compose: [Pontual](https://github.com/Felixdario/Agenda/blob/main/docker-compose.yml), [Pulo](https://github.com/Felixdario/Pulo/blob/main/docker-compose.yml) · Vercel: [SportLink web](https://github.com/Felixdario/sportlink-webapp) |

## Experiência

| Período | Cargo | Empresa | Tecnologias |
| - | - | - | - |
| ago/2025 – hoje | Full-Stack Developer Junior | Epays | React, TypeScript, Vite, Sass |
| mai/2025 – ago/2025 | Front-End Developer | Epays | React, TypeScript |
| fev/2025 – mai/2025 | Front-End Developer | Floox | React Native, TypeScript |
| dez/2024 – fev/2025 | Front-End Developer | LUGARH | React, Node.js, Material UI, i18n |
| set/2024 – dez/2024 | Front-End Developer | Floox | React Native, OCR |
| jan/2024 – out/2024 | Full-Stack Developer | BNE | C#, ASP.NET MVC, Razor, SQL Server, OCR |

**Formação:** Análise e Desenvolvimento de Sistemas na PUCPR (2026–2028, em andamento) e técnico em Informática no IFPR (2022–2025).

**Idiomas:** português (nativo), inglês (profissional pleno) e espanhol (profissional).

## No que estou trabalhando agora

- O quarto projeto do portfólio, depois do Sobra, do Pontual e do Pulo.
- O curso de Análise e Desenvolvimento de Sistemas na PUCPR.

## Contato

- LinkedIn: [linkedin.com/in/felixdariodev](https://www.linkedin.com/in/felixdariodev/)
- E-mail: [eduardojosedario@gmail.com](mailto:eduardojosedario@gmail.com)
- Curitiba e região, Paraná

---

## English version

<details>
<summary><strong>Read in English</strong></summary>

### Eduardo Felix

Junior full-stack developer in the Curitiba area, Brazil. I write APIs in Node.js and TypeScript, interfaces in React and apps in React Native. In the three featured projects, the main guarantees have tests.

### About me

- I have worked as a Full-Stack Developer Junior at Epays since August 2025, on a React component library built with Vite and used across internal products.
- Before that, I worked on the back end at BNE, with React Native at Floox and with React, Node.js and internationalization at LUGARH.
- Technical course in Information Technology at IFPR (2022–2025); now studying Systems Analysis and Development at PUCPR.
- I built the three featured projects with AI agents under my direction, with decisions written down (ADRs and decision logs) and tests that prove the result.

### Featured projects

**[Pulo](https://github.com/Felixdario/Pulo): URL shortener with analytics.** Counts each human click once, with bots and WhatsApp previews kept apart, without slowing the redirect down.

- Redirect p95 of 6.42 ms and 10,129 req/s in one process: with the link cached, one Redis round trip and none to PostgreSQL ([local measurement](https://github.com/Felixdario/Pulo/blob/main/docs/performance.md#L64)).
- Exactly-once click pipeline with Redis Streams and an idempotent worker ([test with 100,000 events](https://github.com/Felixdario/Pulo/blob/main/apps/worker/test/integration/pipeline.test.ts#L141)).
- With 5 million clicks and a warm cache, per-link queries read aggregates in 0.19 to 0.59 ms, against 28 to 122 ms for `COUNT` on the raw table, measured on a local development machine ([table](https://github.com/Felixdario/Pulo/blob/main/README.md#L173-L178), [ADRs](https://github.com/Felixdario/Pulo/tree/main/docs/adr)).
- Stack: TypeScript, Fastify, Drizzle, PostgreSQL, Redis, React, Vite, Vitest and Playwright.

**[Pontual (Agenda repository)](https://github.com/Felixdario/Agenda): booking that never double-books a slot.** Online booking for barbershops, salons and clinics, with no conflicts even when two customers confirm at the same time.

- PostgreSQL `EXCLUDE USING gist` constraint as the last barrier against overlaps ([migration](https://github.com/Felixdario/Agenda/blob/main/apps/api/prisma/migrations/20261108194040_exclusao_agendamentos/migration.sql#L5-L12)).
- 20 simultaneous requests for the same slot end in 1 booking and 19 refusals for an unavailable slot (409 in the API), over 10 rounds ([test](https://github.com/Felixdario/Agenda/blob/main/apps/api/test/integracao/concorrencia.test.ts#L23-L44)).
- Framework-free availability engine with property-based tests (fast-check) ([test](https://github.com/Felixdario/Agenda/blob/main/apps/api/src/domain/propriedades.test.ts#L170-L193)); 24 business rules traced to their tests ([rules](https://github.com/Felixdario/Agenda/blob/main/docs/regras-de-negocio.md), [decisions](https://github.com/Felixdario/Agenda/blob/main/docs/decisoes.md)).
- Stack: TypeScript, NestJS, Prisma, PostgreSQL, Next.js, React, Tailwind CSS, Vitest and Playwright.

**[Sobra](https://github.com/Felixdario/Sobra): personal finance on the web and on the phone.** Log an expense on the phone right away and understand the month on the web dashboard.

- API, web and mobile in one monorepo, with a Zod contract that validates API input and the response on the client ([architecture](https://github.com/Felixdario/Sobra/blob/main/README.md#L146-L148)).
- Rotating refresh tokens with reuse detection ([test](https://github.com/Felixdario/Sobra/blob/main/apps/api/src/modules/auth/auth.test.ts#L227)); on the web, an `httpOnly` cookie with CSRF protection ([route](https://github.com/Felixdario/Sobra/blob/main/apps/api/src/modules/auth/routes.ts#L48-L68)).
- Three concurrent requests with an invalid access token refresh the session only once ([test](https://github.com/Felixdario/Sobra/blob/main/apps/api/src/client.integration.test.ts#L79)).
- 1,087 tests with Vitest, Testing Library, axe-core and Jest ([count](https://github.com/Felixdario/Sobra/blob/main/README.md#L310-L320), [decisions](https://github.com/Felixdario/Sobra/blob/main/docs/decisoes.md)).
- Stack: TypeScript, Fastify, Prisma, SQLite, React, Vite, React Native and Expo.

**Other projects:** SportLink, my final project for the IFPR technical course ([Next.js web app](https://github.com/Felixdario/sportlink-webapp) on [Vercel](https://sportlink-webapp.vercel.app), [React Native app with Expo](https://github.com/Felixdario/sportlink-mobile) and a [Python API with FastAPI](https://github.com/Felixdario/sportlink-api)); [Oceanic.ui](https://github.com/Felixdario/Oceanic.ui), a design system site in Next.js on [Vercel](https://design-system-six-gamma.vercel.app); [prospecta](https://github.com/Felixdario/prospecta), a Python tool that finds local businesses through the Google Places API and organizes sales outreach.

### Skills

| Area | What I use | Where to see it |
| - | - | - |
| Back end | Node.js, TypeScript, Fastify, NestJS, Python (Starlette), REST with OpenAPI, Redis Streams | [Pulo](https://github.com/Felixdario/Pulo), [Pontual](https://github.com/Felixdario/Agenda), [Sobra](https://github.com/Felixdario/Sobra), [prospecta](https://github.com/Felixdario/prospecta/tree/main/prospecta/tests) |
| Front end | React, Next.js, Vite, TanStack Query, Tailwind CSS, accessibility tested with axe | [Pontual](https://github.com/Felixdario/Agenda), [Pulo](https://github.com/Felixdario/Pulo), [Sobra](https://github.com/Felixdario/Sobra), [Oceanic.ui](https://github.com/Felixdario/Oceanic.ui) |
| Mobile | React Native, Expo | [Sobra](https://github.com/Felixdario/Sobra), [SportLink mobile](https://github.com/Felixdario/sportlink-mobile) |
| Databases | PostgreSQL, Prisma, Drizzle, Redis, SQLite | [Pontual](https://github.com/Felixdario/Agenda), [Pulo](https://github.com/Felixdario/Pulo), [Sobra](https://github.com/Felixdario/Sobra) |
| Quality | Vitest, Playwright, fast-check, Testing Library, Jest | [Pontual](https://github.com/Felixdario/Agenda), [Pulo](https://github.com/Felixdario/Pulo), [Sobra](https://github.com/Felixdario/Sobra) |
| DevOps | GitHub Actions, Docker Compose, Vercel | Actions: [Pontual](https://github.com/Felixdario/Agenda/blob/main/.github/workflows/ci.yml), [Sobra](https://github.com/Felixdario/Sobra/blob/main/.github/workflows/ci.yml), [Pulo](https://github.com/Felixdario/Pulo/blob/main/.github/workflows/ci.yml) · Compose: [Pontual](https://github.com/Felixdario/Agenda/blob/main/docker-compose.yml), [Pulo](https://github.com/Felixdario/Pulo/blob/main/docker-compose.yml) · Vercel: [SportLink web](https://github.com/Felixdario/sportlink-webapp) |

### Experience

| Period | Role | Company | Technologies |
| - | - | - | - |
| Aug 2025 – now | Full-Stack Developer Junior | Epays | React, TypeScript, Vite, Sass |
| May 2025 – Aug 2025 | Front-End Developer | Epays | React, TypeScript |
| Feb 2025 – May 2025 | Front-End Developer | Floox | React Native, TypeScript |
| Dec 2024 – Feb 2025 | Front-End Developer | LUGARH | React, Node.js, Material UI, i18n |
| Sep 2024 – Dec 2024 | Front-End Developer | Floox | React Native, OCR |
| Jan 2024 – Oct 2024 | Full-Stack Developer | BNE | C#, ASP.NET MVC, Razor, SQL Server, OCR |

**Education:** Systems Analysis and Development at PUCPR (2026–2028, in progress) and a technical course in Information Technology at IFPR (2022–2025).

**Languages:** Portuguese (native), English (full professional proficiency) and Spanish (professional working proficiency).

### What I am working on

- The fourth portfolio project, after Sobra, Pontual and Pulo.
- My Systems Analysis and Development degree at PUCPR.

### Contact

[LinkedIn](https://www.linkedin.com/in/felixdariodev/) · [eduardojosedario@gmail.com](mailto:eduardojosedario@gmail.com) · Curitiba area, Paraná, Brazil

</details>
