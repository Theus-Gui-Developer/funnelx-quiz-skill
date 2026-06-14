# FunnelX Quiz Skill

Skill para criar, editar e otimizar funis de quiz de conversão na plataforma FunnelX usando as tools MCP.

## Instalação

Via Skills CLI:

```bash
npx skills add https://github.com/Theus-Gui-Developer/funnelx-quiz-skill --skill funnelx-quiz
```

Alternativa shorthand, quando suportada pelo agente/CLI:

```bash
npx skills add Theus-Gui-Developer/funnelx-quiz-skill
```

## Estrutura

- `SKILL.md` - instruções principais, frontmatter e workflow obrigatório.
- `references/components.md` - catálogo de 26 componentes FunnelX, schemas e exemplos.
- `references/themes.md` - temas prontos por nicho e regras de uso.

## Quando Usar

Use esta skill quando o usuário pedir para:

- criar um funil ou quiz na FunnelX;
- adicionar, remover ou reorganizar etapas;
- editar componentes e settings;
- montar funis de forma granular, criando etapas antes dos componentes finais;
- ajustar tema global;
- configurar `theme.header` global sem criar header paralelo nas etapas;
- configurar gamificação global, placar, música, efeitos por gatilho e gamificação por etapa;
- usar áudios enviados pelo usuário em música, efeitos, toast ou `audioPlayer`;
- melhorar copy, oferta, diagnóstico ou conversão;
- validar estrutura do funil via blueprint e warnings.

## Requisitos

A skill presume que o agente tenha acesso às tools MCP da FunnelX, como:

- `create_blank_funnel`
- `get_funnel_creation_skill`
- `list_supported_step_kinds`
- `list_supported_component_types`
- `get_component_schema`
- `list_audio_assets`
- `append_quiz_steps`
- `update_funnel_global_presets`
- `update_funnel_metadata`
- `get_quiz_blueprint`

## Compatibilidade com skills.sh

Este repositório segue o formato esperado pelo ecossistema `skills.sh`:

- `SKILL.md` na raiz;
- frontmatter YAML com `name` e `description`;
- referências em diretório versionado;
- conteúdo auto-contido e sem segredos.
