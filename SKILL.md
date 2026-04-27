---
name: funnelx-quiz
description: >
  Skill completa para criar, editar e otimizar funis de quiz de conversão na plataforma FunnelX usando as tools MCP.
  Use SEMPRE que o usuário pedir para criar um funil, quiz, template, adicionar etapas, editar componentes, corrigir oferta,
  ajustar tema, trocar tipo de componente, ou qualquer operação na FunnelX — mesmo pedidos simples como "cria um funil de X"
  ou "ajusta a etapa de oferta". Cobre workflow completo: criação, tema, montagem em blocos, regras de componentes,
  padrões de copy, checklist de entrega e operações de edição.
---

# FunnelX Quiz — Skill Completa de Criação de Funis

## Instalação via skills.sh

Esta skill segue o padrão público do ecossistema `skills.sh`:
- arquivo principal `SKILL.md` na raiz do repositório;
- frontmatter YAML com `name` e `description`;
- materiais auxiliares dentro de `references/`;
- instruções auto-contidas, sem depender de arquivos externos privados.

Para instalar em agentes compatíveis com o Skills CLI:

```bash
npx skills add https://github.com/Theus-Gui-Developer/funnelx-quiz-skill --skill funnelx-quiz
```

Se o agente/CLI aceitar shorthand de GitHub, também pode funcionar:

```bash
npx skills add Theus-Gui-Developer/funnelx-quiz-skill
```

Depois de instalada, use a skill quando o usuário pedir criação, edição, auditoria ou otimização de quizzes/funis na FunnelX usando as tools MCP da plataforma.

## Convenções para Manutenção

- Mantenha o `name` como `funnelx-quiz` para preservar compatibilidade com instalações existentes.
- Atualize a `description` sempre que ampliar ou restringir o escopo de uso da skill.
- Guarde documentação longa em `references/` e deixe no `SKILL.md` apenas o workflow operacional e regras críticas.
- Não inclua segredos, tokens, URLs privadas ou dados de clientes nos arquivos da skill.
- Prefira exemplos compactos e schemas mínimos. Payloads grandes devem ser divididos em blocos, como o workflow de `append_quiz_steps`.

## Antes de qualquer coisa: consultar tools de contexto

Antes de criar ou reestruturar um funil, chame estas tools para ter contexto atualizado:
- `get_funnel_creation_skill` — playbook estratégico oficial da plataforma
- `list_supported_step_kinds` — kinds com componentes aceitos e obrigatórios
- `list_supported_component_types` — catálogo oficial com schemas e notas
- `get_component_schema("<tipo>")` — schema completo antes de usar um componente novo

---

## Workflow Obrigatório (sempre nesta ordem)

```
1. create_blank_funnel            → cria o funil, obtém slug e id
2. update_funnel_global_presets   → aplica tema exclusivo do nicho
3. append_quiz_steps (bloco 1)    → intro + Q1–Q3
4. append_quiz_steps (bloco 2)    → Q4–Q6 + content (transição) + capture
5. append_quiz_steps (bloco 3)    → result (diagnóstico) + content (depoimentos) + result (oferta)
6. update_funnel_metadata         → marca conversionStepIds com o id da última etapa
7. get_quiz_blueprint             → valida 0 warnings
```

**Nunca enviar todas as etapas em um único append.** Sempre 3 blocos de no máximo 4 etapas cada.
Use `replace_quiz_structure` apenas para estruturas curtas e controladas.

---

## Arquitetura Estratégica das 4 Fases

A plataforma define 4 fases para funis de alta conversão:

| Fase | Objetivo | Kinds |
|------|----------|-------|
| **Quebra-gelo** | Conexão, ritmo, primeiro clique fácil | `intro`, `question` |
| **Consciência do problema** | Lead admite dor, obstáculos, frustrações | `question`, `content` |
| **Desenho da solução** | Coletar preferências, justificar solução sob medida | `question`, `content`, `capture` |
| **Diagnóstico e oferta** | Entregar conclusão, apresentar oferta como consequência lógica | `result`, `offer` |

**Princípio central:** o quiz deve parecer personalizado, diagnóstico e construído para o lead específico. A oferta final deve soar como continuação natural da conversa, usando o que o lead revelou no quiz.

---

## Estrutura de Flow Padrão (12 etapas)

| Pos | Kind | Objetivo |
|-----|------|----------|
| 1 | `intro` | Promessa específica + benefícios + CTA de entrada |
| 2 | `question` | Perfil inicial / faixa etária (quebra-gelo fácil) |
| 3 | `question` | Problema principal (autodiagnóstico da dor) |
| 4 | `question` | Sintomas / sinais (multiselect) |
| 5 | `question` | Fatores de rotina (multiselect) |
| 6 | `question` | Tentativas anteriores |
| 7 | `question` | Impacto emocional / motivação |
| 8 | `content` | Transição educativa + imagem + argumentos |
| 9 | `capture` | Form nome+email após valor percebido |
| 10 | `result` | Diagnóstico + level + progressArgument |
| 11 | `content` | Prova social (testimonial carousel) |
| 12 | `result` | **OFERTA ÚNICA DE CONVERSÃO** ← conversionStepId |

> A etapa de conversão (pos 12) é `result`, não `offer`. Não existe etapa `offer` separada no padrão.

---

## Tema Global

Cada nicho tem identidade visual própria. Leia `references/themes.md` para temas completos.

```js
{
  primaryColor, secondaryColor, accentColor,
  backgroundColor, textColor, fontFamily, borderRadius,
  buttonStyle: { backgroundColor, textColor, borderRadius, fontSize, fontWeight, padding },
  headerStyle: { backgroundColor, progressColor, textColor },
  optionStyle: { backgroundColor, borderColor, borderRadius, padding,
                 selectedBackgroundColor, selectedBorderColor, selectedTextColor }
}
```

**Regra visual:** masculino → `borderRadius: "10px"` | feminino → `borderRadius: "50px"` (pill)

---

## Regras Críticas de Componentes

Leia `references/components.md` para o catálogo completo com schemas e exemplos de todos os 24 componentes.

### useTheme — tabela de decisão

| `true` | `false` |
|--------|---------|
| `text`, `button`, `form` | `progressArgument`, `faq`, `testimonial` |
| `level`, `countdown`, `notification` | `codeBlock`, `pricingCard` |
| `argument` SEM backgroundColor | `argument` COM backgroundColor customizado |

**Nunca use `style=color:#xxx` inline onde o componente deveria herdar do tema.**

### Componente certo para cada situação

| Situação | ✅ Correto | ❌ Errado |
|---|---|---|
| Bloco com cor de fundo | `argument` com `backgroundColor` + `borderColor` | `text` com `<div style='background:...'>` |
| Preço / oferta | `pricingCard` com `useTheme: true` | `text` com HTML de gradiente |
| Layout complexo / cards | `codeBlock` com html limpo + `customCSS` separado | `text` com HTML inline pesado |
| Score / diagnóstico visual | `level` percentage 22–35% | — |
| Barras de indicadores | `progressArgument` | — |
| Gráfico de queda/evolução | `cartesianChart` | — |
| Antes/depois visual | `comparison` | — |
| Toast de urgência | `notification` variant warning | — |

### Regras específicas dos componentes mais usados

**`argument` como card colorido:**
- Lista ❌ → `backgroundColor: "#fef2f2"`, `borderColor: "#fca5a5"`, `useTheme: false`
- Lista ✅ → `backgroundColor: "#f0f9f0"`, `borderColor: "#86efac"`, `useTheme: false`
- Urgência amarela → `backgroundColor: "#fefce8"`, `borderColor: "#eab308"`, `useTheme: false`
- Card simples de destaque → 1 item com `mediaType: none`

**`codeBlock`** — HTML com classes, CSS isolado em `customCSS`:
```json
{
  "html": "<div class='cards'><div class='card'><p class='titulo'>Módulo 1</p></div></div>",
  "customCSS": ".cards{display:flex;flex-direction:column;gap:12px}.card{border:1px solid #d4cfc4;border-radius:12px;padding:14px;background:#fff}.titulo{margin:0;font-weight:700;font-size:15px}",
  "customJS": "", "minHeight": "auto", "useTheme": false
}
```

**`countdown`** — usar `{{timer}}` no content:
```json
{ "content": "<p style='text-align:center;font-size:28px;font-weight:900;'>{{timer}}</p>", "duration": 900, "format": "mm:ss", "actionType": "none", "placementMode": "normal" }
```

**`carousel`** — quando `ctaActionType: url`, `ctaUrl` é obrigatório em cada slide. Se não quiser CTA por slide, usar `codeBlock`.

**`options`** multiselect → `autoAdvance: false` + adicionar `button` na etapa.

---

## Etapa de Oferta — 17 componentes em sequência

```
1.  text          useTheme:true   → headline de exclusividade
2.  countdown     useTheme:true   → timer 15min com {{timer}}
3.  notification  warning         → toast de urgência
4.  button        url             → 1º CTA acima do fold
5.  text          useTheme:true   → headline "Por que X falha"
6.  image                         → imagem contextual (Unsplash)
7.  argument      ❌ fundo vermelho → por que o método antigo falha
8.  argument      ✅ fundo verde   → por que o protocolo funciona
9.  text          useTheme:true   → headline "O que você recebe"
10. codeBlock                     → cards dos módulos com valor unitário
11. pricingCard   useTheme:true   → preço âncora (originalPrice riscado)
12. button        url             → 2º CTA principal
13. text          useTheme:true   → micro-copy de segurança (🔒 · 📱 · ✅)
14. testimonial                   → carrossel com starColor = accentColor
15. argument      ⚠️ fundo amarelo → gatilho de urgência "por que agir hoje"
16. faq                           → 3 objeções específicas do nicho
17. button        url             → 3º CTA final
```

**Todos os buttons na oferta: `actionType: url` — NUNCA `nextStep`.**

---

## Copy e Tom

- **Sem travessão** ( — ) nos textos. Substituir por vírgula, ponto ou reescrita.
- Copy agressiva mas sem vergonha. Foco em devolver controle, não culpar o lead.
- Diagnóstico com nome dramático e específico: "Colapso Hormonal Progressivo", "Queda Multifatorial Ativa".
- `level` com percentage baixo (22–35%) para urgência visual máxima.
- Depoimentos com dados mensuráveis: kg, cm, dias até resultado.
- CTAs emocionais: "QUERO MEU CABELO DE VOLTA" em vez de "Comprar agora".
- Imagem em pelo menos 1 etapa (intro ou transição) via Unsplash.
- Oferta conecta diagnóstico à solução: "seu perfil foi analisado e X é o método indicado para Y".

**Princípios de conversão da plataforma:**
- Promessa específica com benefício claro desde a intro
- Microcompromissos progressivos — perguntas fáceis primeiro
- Personalização percebida — oferta como consequência das respostas do lead
- Uma intenção por etapa — sem CTAs concorrentes na mesma tela
- Fechamento orientado à decisão — CTA claro + prova + risco reduzido + urgência honesta

---

## Checklist de Entrega

- [ ] `create_blank_funnel` com slug descritivo kebab-case
- [ ] `update_funnel_global_presets` com tema exclusivo do nicho
- [ ] 3 blocos de `append_quiz_steps` sem erro (máx 4 etapas por bloco)
- [ ] Última etapa é `result` com a oferta completa (17 componentes)
- [ ] Todos os buttons da oferta com `actionType: url`
- [ ] `update_funnel_metadata` com `conversionStepIds` = [id da última etapa]
- [ ] `get_quiz_blueprint` retorna 0 warnings
- [ ] Sem cores hardcoded onde deveria usar `useTheme: true`
- [ ] Sem `text` com `<div style='background:...'>` — usar `argument` com `backgroundColor`
- [ ] `codeBlock` com CSS em `customCSS`, não inline no html
- [ ] `pricingCard` para preço — nunca `text` com HTML de gradiente

---

## Operações de Edição

```
Inspecionar:         get_quiz_blueprint
Trocar tipo de componente:
  1. create_quiz_component    → criar o novo componente correto
  2. reorder_quiz_components  → posicionar na ordem certa
  3. delete_quiz_component    → deletar o antigo
  ⚠️ Nunca deletar o único componente restante de uma etapa
Atualizar settings:  update_component_settings (patch parcial)
Mover etapa:         update_quiz_step com position
Reordenar etapas:    reorder_quiz_steps com lista completa de stepIds
Deletar etapa:       delete_quiz_step (obter stepId via blueprint)
```

`type` de componente não pode ser mudado via `update_component_settings` — sempre criar → reordenar → deletar.

---

## Anti-patterns (da plataforma + sessão)

- Não começar com perguntas difíceis antes de contextualizar a promessa
- Não pedir captura muito cedo, sem valor percebido
- Não usar `replace_quiz_structure` gigante quando `append_quiz_steps` resolve
- Não criar etapas com muitos componentes concorrendo por atenção
- Não usar `text` com HTML de cor de fundo — sempre `argument` com `backgroundColor`
- Não misturar CSS no campo `html` do `codeBlock`
- Não usar `text` para bloco de preço — sempre `pricingCard`
- Não encerrar com diagnóstico genérico que poderia servir para qualquer pessoa

---

## Referências

- `references/components.md` — catálogo dos 24 componentes com schemas e exemplos completos
- `references/themes.md` — temas prontos por nicho com JSON completo para `update_funnel_global_presets`
