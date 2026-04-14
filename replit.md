# MozLink - Portal de Oportunidades de Moçambique

## Overview

MozLink é um portal automático que recolhe e publica oportunidades de emprego, concursos públicos e bolsas de estudo focado em Moçambique. O sistema funciona automaticamente, actualizando diariamente e enviando notificações para o canal do WhatsApp.

## Stack

- **Monorepo tool**: pnpm workspaces
- **Node.js version**: 24
- **Package manager**: pnpm
- **TypeScript version**: 5.9
- **Frontend**: React + Vite (artifacts/mozlink)
- **API framework**: Express 5 (artifacts/api-server)
- **Database**: PostgreSQL + Drizzle ORM
- **Validation**: Zod (zod/v4), drizzle-zod
- **API codegen**: Orval (from OpenAPI spec)
- **Build**: esbuild (CJS bundle)

## Features

- **Homepage** (`/`) — Hero com estatísticas, categorias, oportunidades em destaque e CTA WhatsApp
- **Listagem** (`/oportunidades`) — Pesquisa, filtros por categoria, paginação
- **Detalhe** (`/oportunidades/:slug`) — Informação completa: instituição, localização, requisitos, prazo, como candidatar
- **Admin** (`/admin`) — Painel de gestão: scraper control, estatísticas, lista de publicações
- **Scraper automático** — Recolhe oportunidades de múltiplas fontes e publica automaticamente
- **WhatsApp** — Integração com canal https://whatsapp.com/channel/0029Vb8Jbpr17EmuUtFUmu3v
- **AdSense** — Slots de anúncios preparados (leaderboard, rectangle, in-content)
- **SEO** — Títulos e descrições otimizados por categoria em português

## Database Schema

- `opportunities` — Oportunidades com categoria, instituição, localização, requisitos, prazo, slug
- `scraper_logs` — Histórico de execuções do scraper

## Categories

- `emprego` — Vagas de emprego
- `concurso_publico` — Concursos públicos
- `bolsa_estudo` — Bolsas de estudo

## Key Commands

- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- `pnpm --filter @workspace/api-server run dev` — run API server locally
- `pnpm --filter @workspace/mozlink run dev` — run frontend locally

## WhatsApp Channel

https://whatsapp.com/channel/0029Vb8Jbpr17EmuUtFUmu3v

## Adsterra Integration

Os slots de anúncios estão em `artifacts/mozlink/src/components/AdSlot.tsx`.

Para activar os anúncios Adsterra:
1. Crie a sua conta em https://publishers.adsterra.com
2. Crie zonas de anúncio para cada tamanho: 728×90, 300×250, 320×100
3. Copie o Zone ID de cada zona
4. Passe o `zoneId` como prop no componente `<AdSlot>` nas páginas Home, Oportunidades e OportunidadeDetalhe

Exemplo:
```tsx
<AdSlot size="leaderboard" zoneId="abc123def456" />
<AdSlot size="rectangle" zoneId="xyz789ghi012" />
<AdSlot size="banner" zoneId="mno345pqr678" />
```

## API Endpoints

- `GET /api/opportunities` — Listar com filtros (category, search, page, limit, featured)
- `POST /api/opportunities` — Criar oportunidade manualmente
- `GET /api/opportunities/recent` — Mais recentes
- `GET /api/opportunities/featured` — Em destaque
- `GET /api/opportunities/:id` — Detalhe
- `POST /api/opportunities/:id/whatsapp` — Enviar notificação WhatsApp
- `POST /api/scraper/run` — Executar scraper manualmente
- `GET /api/scraper/status` — Estado do scraper
- `GET /api/stats/summary` — Resumo de estatísticas
- `GET /api/stats/by-category` — Contagens por categoria
