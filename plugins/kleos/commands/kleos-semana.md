---
description: Planeja a semana de posts no Kleos — lê o plano, estima e só gera com o seu sim
argument-hint: "[projeto] [posts por dia] [agente]"
---

Planeje a próxima semana de posts no Kleos com o método da skill `kleos`.
Pedido da pessoa (pode vir vazio): $ARGUMENTS

1. `list_projects`: resolva o projeto pelo nome ou slug do pedido. Se houver
   mais de um e o pedido não disser qual, pergunte. Nunca chute id.
2. `list_agents` do projeto e `get_agent` de cada um que vai postar: mostre a
   cadência atual (posts por dia, horários, dias da semana).
3. `get_usage`: diga quantos posts ainda cabem no plano (`remaining` da
   alavanca `posts`) e quando renova.
4. `plan_week` com `days: 7`, os agentes e `posts_per_day` do pedido (ou a
   cadência atual) e `dry_run: true`. Mostre a estimativa: quantos posts, por
   agente, e o que sobra no plano.
5. Pare e pergunte: "Posso gerar?". Só com o sim, chame `plan_week` de novo
   com `confirm: true`. Não ligue `activate` sem a pessoa pedir produção
   contínua.
6. Termine com o link (`url`) do calendário e diga que os posts chegam em
   `pronto` para revisão — nada é publicado sem aprovação, a não ser nas
   contas em modo automático.

Se alguma ferramenta responder `code: "limite"`, `camada_desligada` ou
`escopo_faltando`, não repita: explique o que falta e mande o link de planos
(`planos_url`) ou de Configurações.
