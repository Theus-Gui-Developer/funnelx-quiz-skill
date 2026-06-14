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
- Prefira exemplos representativos e schemas enxutos: compactos, mas bons o bastante para mostrar quizzes elaborados da Funilix. Nunca gere um funil inteiro com todas as etapas e componentes finais em um único payload.

## Antes de qualquer coisa: consultar tools de contexto

Antes de criar ou reestruturar um funil, chame estas tools para ter contexto atualizado:
- `get_funnel_creation_skill` — playbook estratégico oficial da plataforma
- `list_supported_step_kinds` — kinds com componentes aceitos e obrigatórios
- `list_supported_component_types` — catálogo oficial com schemas e notas
- `get_component_schema("<tipo>")` — schema completo antes de usar um componente novo
- `list_audio_assets` — biblioteca de áudios reais do usuário para música, sons de interação, toast ou `audioPlayer`

---

## Workflow Obrigatório (sempre nesta ordem)

```
1. create_blank_funnel            → cria o funil, obtém slug e id
2. update_funnel_global_presets   → aplica tema, header, form e gamificação globais
3. list_audio_assets              → consultar antes de usar qualquer audioUrl/soundUrl
4. append_quiz_steps              → criar só a espinha dorsal das etapas em blocos pequenos
5. get_quiz_blueprint             → capturar stepIds reais e validar estrutura de etapas
6. create_quiz_component          → montar componentes da etapa 1
7. update_component_settings      → ajustar settings da etapa 1 quando necessário
8. get_quiz_blueprint             → validar a etapa montada
9. repetir 6–8 etapa por etapa    → até finalizar diagnóstico, captura e oferta
10. update_funnel_metadata        → marcar conversionStepIds com o id da última etapa
11. get_quiz_blueprint            → validação final sem warnings críticos
```

Regra principal: **etapas primeiro, componentes depois**.

- Nunca enviar todas as etapas e todos os componentes finais em um único `append_quiz_steps` ou `replace_quiz_structure`.
- Primeiro crie a estrutura de etapas: intro, perguntas, transição, captura, diagnóstico, prova e oferta.
- Se `append_quiz_steps` exigir `components`, use placeholders mínimos para passar validação: `text`/`button` simples, `options` mínimo em `question`, `form` + `button` mínimo em `capture`.
- Depois use `get_quiz_blueprint` para obter `stepId` de cada etapa.
- Monte uma etapa por vez com `create_quiz_component`, `update_component_settings` e `reorder_quiz_components`.
- Valide com `get_quiz_blueprint` após cada etapa complexa ou após pequenos grupos de componentes.
- Use `replace_quiz_structure` apenas para estruturas curtas e controladas.

Motivo: um único erro em um componente invalida o payload inteiro. A montagem granular evita perder toda a estrutura, reduz retrabalho e economiza tokens.

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

## Tema Global e Header Oficial

Cada nicho precisa de uma identidade visual própria, mas o header/progresso oficial já existe no tema global. Leia `references/themes.md` antes de criar o funil.

Use `update_funnel_global_presets` para configurar:
- `theme.colors` — primária, texto, fundo e borda.
- `theme.typography` — fonte, hierarquia e peso.
- `theme.layout` — largura, raio, sombra e gaps.
- `theme.header` — logo/texto/emoji, progresso, voltar, cores e altura.
- `theme.background` — imagem global e overlay quando fizer sentido.
- `theme.form` — campos, foco, labels e bordas.
- `settings.gamification` — score, música e efeitos.

Regras obrigatórias:

- Header oficial fica em `theme.header`. Não crie header paralelo com `text`, `image`, `codeBlock` ou `layoutContainer`.
- Não recrie barra de progresso, logo, botão voltar ou topo fixo dentro das etapas.
- Use `headerOverride` só como exceção real, por exemplo ocultar progresso em uma etapa especial ou trocar o logo apenas em uma etapa de campanha.
- Se só quer uma headline visual na etapa, use `text` com `useTheme: true`; isso não é header.
- Masculino costuma usar `borderRadius: "10px"` ou `"12px"`; feminino pode usar `"22px"` ou `"50px"` em botões.

---

## Gamificação Global

Use `update_funnel_global_presets` com `settings.gamification` para gamificação do funil inteiro.

Gamificação aqui não é decoração. Use como loop de retenção: microcompromissos fáceis, feedback imediato, micro-recompensas, progressão visível e sensação de conquista. O objetivo é manter atenção, gerar dopamina em momentos-chave e fazer o usuário querer chegar ao diagnóstico/oferta.

```js
settings: {
  gamification: {
    enabled: true,
    persistSession: true,
    score: { enabled: true, scoreKey: "saldo", label: "Saldo desbloqueado", prefix: "R$", position: "aboveHeader" },
    backgroundMusic: { enabled: false, audioUrl: "", volume: 0.45, loop: true, autoPlay: true, showControl: false },
    interactionEffects: { enabled: true, rules: [] }
  }
}
```

Regras críticas:

- Placar novo fica em `settings.gamification.score`; não use `scoreCounter` em funis novos.
- Música nova fica em `settings.gamification.backgroundMusic`; não use `backgroundMusic` como componente em funis novos.
- Antes de preencher qualquer `audioUrl` ou `soundUrl`, chame `list_audio_assets` e use somente URLs retornadas pela biblioteca do usuário.
- Nunca invente URL de áudio. Se não houver áudio disponível, deixe o recurso desligado ou peça ao usuário para enviar um arquivo.
- Vibração, som curto e efeitos visuais ficam em `interactionEffects.rules`.
- Toda regra de `interactionEffects.rules[]` deve ser completa: `enabled`, `trigger`, `vibration`, `sound` e `visual`.
- Se não usar vibração, som ou visual, envie o objeto mesmo assim com `enabled: false`; nunca envie regra parcial só com `trigger`.
- Gatilhos: `buttonClick`, `optionSelect`, `optionAutoAdvance`, `timerComplete`, `pricingClick`, `stepAdvance`, `formValidationError`, `flowComplete`.
- Vibração: `light`, `success`, `impact`. Visual: `screenFlash`, `shake`, `successPulse`.
- `gamifiedModal` e `iphoneToast` são componentes de etapa para mensagens com copy, imagem, CTA ou prova social.
- `gamificationOverride` na etapa ajusta efeitos locais e pode somar score com `score: { enabled: true, value: 10 }`.
- Para embed manual com vibração, o iframe precisa de `allow="clipboard-write; vibrate"`.

Score/deltas:

- `options.items[].scoreDeltaEnabled` usa `value` numérico.
- `button.scoreDeltaEnabled` usa `scoreDelta`.
- `form.fields[].scoreDeltaEnabled` funciona em campos `number`.
- `weightSlider` e `heightSlider` usam `scoreDeltaEnabled`.
- `gamificationOverride.score` soma ao avançar a etapa.

Formato seguro de regra:

```json
{
  "enabled": true,
  "trigger": "buttonClick",
  "vibration": { "enabled": true, "preset": "light" },
  "sound": { "enabled": false, "audioUrl": "", "volume": 0.45 },
  "visual": { "enabled": false, "effect": "successPulse", "color": "#22e06f" }
}
```

---

## Regras Críticas de Componentes

Leia `references/components.md` para o catálogo completo com schemas e exemplos de todos os 26 componentes.

Um quiz Funilix bem elaborado combina tema global forte, componentes nativos e progressão visual. Não monte páginas genéricas com blocos HTML simulando tudo. Use os componentes certos para mostrar diagnóstico, prova, comparação, oferta, urgência e feedback.

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
| Header/logo/progresso | `theme.header` global | `text`, `image` ou `codeBlock` no topo de toda etapa |
| Preço / oferta | `pricingCard` com `useTheme: true` | `text` com HTML de gradiente |
| Layout complexo / cards | `codeBlock` com html limpo + `customCSS` separado | `text` com HTML inline pesado |
| Score / diagnóstico visual | `level` percentage 22–35% | — |
| Barras de indicadores | `progressArgument` | — |
| Gráfico de queda/evolução | `cartesianChart` | — |
| Antes/depois visual | `comparison` | — |
| Toast de urgência | `notification` variant warning | — |
| Conquista/desbloqueio | `gamifiedModal` | preset global com copy fixa |
| Prova social estilo app | `iphoneToast` | `notification` empilhado sem contexto |

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
- Feedback imediato e micro-recompensas para reforçar avanço, escolha e conclusão
- Personalização percebida — oferta como consequência das respostas do lead
- Uma intenção por etapa — sem CTAs concorrentes na mesma tela
- Fechamento orientado à decisão — CTA claro + prova + risco reduzido + urgência honesta

---

## Checklist de Entrega

- [ ] `create_blank_funnel` com slug descritivo kebab-case
- [ ] `update_funnel_global_presets` com tema exclusivo do nicho
- [ ] Header/progresso configurado em `theme.header`, sem header paralelo nas etapas
- [ ] Estrutura de etapas criada antes dos componentes finais
- [ ] `append_quiz_steps` usado só para espinha dorsal ou placeholders mínimos
- [ ] Componentes montados etapa por etapa com `create_quiz_component`/`update_component_settings`
- [ ] `get_quiz_blueprint` usado para validar depois de cada etapa complexa
- [ ] Última etapa é `result` com a oferta completa (17 componentes)
- [ ] Todos os buttons da oferta com `actionType: url`
- [ ] `update_funnel_metadata` com `conversionStepIds` = [id da última etapa]
- [ ] `get_quiz_blueprint` retorna 0 warnings
- [ ] Sem cores hardcoded onde deveria usar `useTheme: true`
- [ ] Sem `text` com `<div style='background:...'>` — usar `argument` com `backgroundColor`
- [ ] `codeBlock` com CSS em `customCSS`, não inline no html
- [ ] `pricingCard` para preço — nunca `text` com HTML de gradiente
- [ ] Gamificação global em `settings.gamification`, não em componentes legados
- [ ] Todo `audioUrl`/`soundUrl` veio de `list_audio_assets` quando áudio estiver habilitado
- [ ] `gamifiedModal`/`iphoneToast` usados como componentes quando houver copy editável

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
- Não criar funil completo com todas as etapas e componentes finais em um único payload
- Não regenerar tudo por causa de erro em um componente; corrija só a etapa/componente afetado
- Não usar `replace_quiz_structure` gigante quando `append_quiz_steps` resolve
- Não criar etapas com muitos componentes concorrendo por atenção
- Não criar header paralelo com `text`, `image`, `layoutContainer` ou `codeBlock`
- Não duplicar barra de progresso ou logo dentro dos componentes
- Não usar `text` com HTML de cor de fundo — sempre `argument` com `backgroundColor`
- Não misturar CSS no campo `html` do `codeBlock`
- Não usar `text` para bloco de preço — sempre `pricingCard`
- Não encerrar com diagnóstico genérico que poderia servir para qualquer pessoa
- Não inventar URLs de áudio para música, efeitos, toast ou `audioPlayer`

---

## Referências

- `references/components.md` — catálogo dos 26 componentes com schemas e exemplos completos
- `references/themes.md` — temas prontos por nicho com JSON completo para `update_funnel_global_presets`
