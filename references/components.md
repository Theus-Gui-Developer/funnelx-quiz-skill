# Catálogo de Componentes FunnelX (26 componentes)

## Compatibilidade por Kind

Todos os componentes abaixo são compatíveis com: `intro`, `question`, `capture`, `content`, `result`, `offer`
Exceções: `options` só em `question` | `form` só em `capture`

---

## Composição de Quiz Elaborado

Um quiz real da Funilix deve parecer uma experiência guiada, não uma sequência de textos simples. Use o header global do tema, componentes nativos e progressão visual.

| Momento | Componentes recomendados | Função |
|---|---|---|
| Intro premium | `text`, `image` ou `video`, `argument`, `button` | Promessa, autoridade e primeiro clique |
| Pergunta simples | `text`, `options`, `progressArgument` opcional | Microcompromisso e score |
| Pergunta com contexto | `text`, `image`, `options`, `iphoneToast` opcional | Diagnóstico com feedback sensorial |
| Multiselect | `text`, `options` com `multiSelect: true`, `button` | Coletar sinais sem auto-avançar |
| Transição educativa | `text`, `argument`, `comparison`, `cartesianChart`, `button` | Aumentar consciência do problema |
| Captura | `text`, `form`, `button`, `testimonial` ou `faq` | Capturar após valor percebido |
| Diagnóstico | `text`, `level`, `progressArgument`, `gamifiedModal`, `button` | Entregar conclusão e recompensa |
| Oferta | `text`, `countdown`, `notification`, `button`, `argument`, `comparison`, `codeBlock`, `pricingCard`, `testimonial`, `faq` | Converter com prova, valor e urgência |

Regras de composição:

- Header, progresso, botão voltar e logo pertencem a `theme.header`, não aos componentes.
- Use `text` para copy, não para cards, preço, header, grids ou seções com fundo complexo.
- Use `argument`, `comparison`, `progressArgument`, `level`, `cartesianChart`, `pricingCard` e `testimonial` para mostrar que a Funilix faz quizzes elaborados com componentes nativos.
- Use `codeBlock` apenas para layouts realmente customizados, como cards de módulos, stack de bônus ou mockups de app.
- Não coloque CSS inline pesado em `text`; se precisa de CSS, use `codeBlock` com `customCSS`.

---

## 1. text
Bloco de texto rico (TipTap). Apenas texto: sem cor de fundo, sem cards, sem layouts e sem header paralelo.

Papel estratégico: headline, promessa, instrução, prova verbal, CTA contextual.

```json
{ "type": "text", "settings": { "content": "<h2 style='text-align:center;'>Título</h2><p>Parágrafo</p>", "useTheme": true } }
```
**NÃO USE** `<div style='background:...'>` - use `argument` com `backgroundColor`.
**NÃO USE** `text` para simular logo, barra de progresso ou header; use `theme.header`.

---

## 2. button
CTA de navegação ou link externo.

Papel estratégico: mover a decisão para o próximo passo com ação explícita. Escreva o CTA com verbo de ação e benefício — evite rótulos genéricos como "Enviar".

```json
{
  "type": "button",
  "settings": {
    "text": "Quero ver meu resultado →",
    "actionType": "nextStep",  // nextStep | url | specificStep
    "url": "",                  // obrigatório se actionType = url
    "targetStepId": "",         // obrigatório se actionType = specificStep
    "scoreDeltaEnabled": false,
    "scoreDelta": 10,
    "useTheme": true
  }
}
```
Na etapa de oferta: **sempre** `actionType: url`.

---

## 3. image
Imagem simples por URL absoluta.

Papel estratégico: ancorar percepção visual, contexto e desejo. Evite imagem decorativa sem função argumentativa.

```json
{
  "type": "image",
  "settings": {
    "src": "https://images.unsplash.com/photo-XXX?auto=format&fit=crop&w=600&q=80",
    "alt": "Descrição acessível",
    "borderRadius": "12px",
    "objectFit": "cover",
    "useTheme": false
  }
}
```
Não use `image` para repetir logo no topo de todas as etapas. Logo global pertence a `theme.header` com `contentType: "image"`.

---

## 4. options
Respostas do quiz. Obrigatório em etapas `question`.

Papel estratégico: conduzir autodiagnóstico, segmentação e avanço com microcompromissos. As respostas devem refletir dores reais, desejos ou perfis do público.

```json
{
  "type": "options",
  "settings": {
    "items": [
      { "label": "🟡 Opção A", "value": "10", "scoreDeltaEnabled": true },
      { "label": "🔴 Opção B", "value": "0" }
    ],
    "autoAdvance": true,    // false quando multiSelect: true
    "multiSelect": false
  }
}
```
Quando `items[].scoreDeltaEnabled: true`, o runtime usa `value` como número para somar/remover do placar global.

---

## 5. form
Captura de leads. Obrigatório em etapas `capture`. Combine sempre com `button`.

Papel estratégico: capturar dados com baixa fricção quando o valor já foi percebido. Peça apenas o mínimo necessário.

```json
{
  "type": "form",
  "settings": {
    "fields": [
      { "type": "text",  "name": "name",  "label": "Seu nome",        "placeholder": "Como posso te chamar?", "required": true, "variableName": "nome" },
      { "type": "email", "name": "email", "label": "Seu melhor e-mail","placeholder": "voce@email.com",       "required": true, "variableName": "email" },
      { "type": "number", "name": "score", "label": "Pontuação", "required": false, "scoreDeltaEnabled": true }
    ],
    "showLabels": true,
    "useTheme": true
  }
}
```
Campos `number` com `scoreDeltaEnabled: true` somam o valor preenchido ao placar global após validação.

---

## 6. argument
Lista de argumentos com mídia opcional por item. **Também funciona como card com cor de fundo.**

Papel estratégico: empilhar razões de valor, benefícios concretos ou objeções respondidas. Cada item deve reforçar valor ou reduzir objeção.

```json
{
  "type": "argument",
  "settings": {
    "items": [
      { "content": "<p>❌ <strong>Método antigo</strong> não trata a causa</p>", "mediaType": "none" }
    ],
    "layout": "list",
    "padding": "14px",
    "showBorder": true,
    "backgroundColor": "#fef2f2",  // cor do card inteiro
    "borderColor": "#fca5a5",
    "borderRadius": "12px",
    "useTheme": false               // false quando há backgroundColor customizado
  }
}
```

**Padrões de cor:**
- Lista ❌ → `backgroundColor: "#fef2f2"`, `borderColor: "#fca5a5"`
- Lista ✅ → `backgroundColor: "#f0f9f0"`, `borderColor: "#86efac"`
- Urgência/destaque amarelo → `backgroundColor: "#fefce8"`, `borderColor: "#eab308"`
- Card neutro com borda → `showBorder: true`, sem backgroundColor, `useTheme: true`

---

## 7. progressArgument
Argumentos com barra de progresso por item.

Papel estratégico: mostrar progresso, potencial ou gap percebido de forma visual. Ideal para diagnóstico de indicadores hormonais, metabólicos ou capilares.

```json
{
  "type": "progressArgument",
  "settings": {
    "items": [
      { "content": "<p><strong>Testosterona livre</strong><br/>Reduzida</p>", "percentage": 32 },
      { "content": "<p><strong>Potencial de recuperação</strong><br/>Alto</p>", "percentage": 91 }
    ],
    "layout": "2-columns",
    "barSize": 120,
    "barOrientation": "vertical",
    "barDisplay": "percentage",
    "barFillColor": "#c8a84b",
    "barBackgroundColor": "#e8e4dc",
    "barBorderRadius": "14px",
    "padding": "18px",
    "showBorder": true,
    "useTheme": false
  }
}
```

---

## 8. level
Barra de nível/score com indicador.

Papel estratégico: reforçar diagnóstico, score ou progresso percebido. Transforma respostas em percepção visual de situação crítica.

```json
{
  "type": "level",
  "settings": {
    "title": "Nível de vitalidade hormonal",
    "percentage": 28,            // 22–35% para máxima urgência visual
    "indicatorText": "Seu nível atual",
    "labels": "Suprimido,Parcial,Pleno",
    "actionType": "none",
    "stylePreset": "default",
    "useTheme": true
  }
}
```

---

## 9. countdown
Contador regressivo com HTML rico.

Papel estratégico: adicionar urgência visual honesta em momentos de decisão. Funciona melhor próximo à oferta.

```json
{
  "type": "countdown",
  "settings": {
    "content": "<p style='text-align:center;font-weight:700;font-size:14px;'>Oferta reservada por:</p><p style='text-align:center;font-size:28px;font-weight:900;'>{{timer}}</p>",
    "duration": 900,
    "format": "mm:ss",
    "actionType": "none",
    "placementMode": "normal",
    "useTheme": true
  }
}
```
Use `{{timer}}` no content para exibir o valor do timer.

---

## 10. pricingCard
Bloco de preço e oferta. **Componente obrigatório na etapa de oferta.**

Papel estratégico: apresentar a oferta, o valor e o CTA de fechamento. Combine com prova, FAQ ou urgência para reduzir objeção final.

```json
{
  "type": "pricingCard",
  "settings": {
    "items": [
      {
        "title": "Protocolo Calvície Zero",
        "originalPrice": "R$ 535,00",
        "discountPrice": "R$ 97,00",
        "priceDescription": "Acesso imediato e vitalício",
        "isHighlighted": true,
        "redirectUrl": "https://checkout.exemplo.com/produto"
      }
    ],
    "actionType": "redirect",
    "redirectOnClick": true,
    "enableCheckbox": false,
    "useTheme": true
  }
}
```
**Nunca use `text` com HTML de gradiente para preço.**

---

## 11. testimonial
Depoimentos em carrossel ou grid.

Papel estratégico: transferir confiança e reduzir ceticismo com prova social. Prefira depoimentos com transformação específica e mensurável. Posicione próximo à captura ou oferta.

```json
{
  "type": "testimonial",
  "settings": {
    "items": [
      {
        "name": "<p><strong>Ricardo A., 47 anos</strong></p>",
        "role": "SP — resultado em 45 dias",
        "text": "<p>\"Depoimento com resultado específico e mensurável.\"</p>",
        "rating": 5
      }
    ],
    "layout": "carousel",
    "variant": "classic",
    "autoplay": true,
    "autoplayInterval": 5000,
    "loop": true,
    "showRating": true,
    "starColor": "#c8a84b",   // usar accentColor do tema
    "showImage": false,
    "showBorder": true,
    "showDots": true,
    "showArrows": false,
    "useTheme": false
  }
}
```

---

## 12. faq
Accordion de perguntas e respostas.

Papel estratégico: responder objeções reais de compra sem quebrar a fluidez. Use linguagem objetiva e orientada à decisão — não perguntas decorativas.

```json
{
  "type": "faq",
  "settings": {
    "items": [
      {
        "question": "<p><strong>Em quanto tempo vejo resultado?</strong></p>",
        "answer": "<p>Primeiros sinais entre 21 e 45 dias.</p>",
        "isOpenByDefault": false
      }
    ],
    "allowMultipleOpen": true,
    "iconStyle": "chevron",
    "iconPosition": "right",
    "showDivider": true,
    "padding": "14px",
    "useTheme": false
  }
}
```

---

## 13. cartesianChart
Gráfico com variantes cartesian, radar, radial, pie.

Papel estratégico: materializar evolução, deterioração ou potencial percebido de forma visual. Ideal na etapa de transição para mostrar queda de densidade folicular, hormonal, etc.

```json
{
  "type": "cartesianChart",
  "settings": {
    "title": "Densidade folicular ao longo do tempo",
    "points": [
      { "label": "20a", "value": 100 },
      { "label": "30a", "value": 64 },
      { "label": "Agora", "value": 22, "isHighlighted": true, "indicatorText": "Você está aqui" }
    ],
    "chartVariant": "cartesian",
    "stylePreset": "greenToRed",
    "showArea": true,
    "showGrid": true,
    "showLegend": false,
    "animationDuration": 1500,
    "useTheme": true
  }
}
```

---

## 14. comparison
Slider antes/depois com duas imagens.

Papel estratégico: criar contraste entre o caminho antigo e a solução recomendada. Trabalhe dor versus solução, esforço versus resultado. Ótimo para nichos visuais (calvície, queda de cabelo, corpo).

```json
{
  "type": "comparison",
  "settings": {
    "firstImageInputType": "url",
    "firstImageUrl": "https://images.unsplash.com/...",
    "firstImageAlt": "Antes",
    "secondImageInputType": "url",
    "secondImageUrl": "https://images.unsplash.com/...",
    "secondImageAlt": "Depois",
    "initialPosition": 50,
    "aspectRatio": "4/3",
    "borderRadius": "12px",
    "useTheme": false
  }
}
```

---

## 15. notification
Toast/alerta com delay e variantes.

Papel estratégico: destacar aviso, reforço ou prova curta sem trocar a hierarquia da tela. Use para pequenos reforços de urgência ou benefício sem competir com a headline.

```json
{
  "type": "notification",
  "settings": {
    "content": "<p><strong>Atenção:</strong> mensagem de urgência.</p>",
    "variant": "warning",       // warning | info | success | error
    "styleType": "subtle",
    "showIcon": true,
    "showCloseButton": false,
    "delay": 0,
    "duration": 0,              // 0 = permanente
    "useTheme": true
  }
}
```

---

## 16. codeBlock
HTML + CSS + JS customizados. Para layouts que o TipTap não suporta.

Papel estratégico: exibir cards de módulos, grids customizados, checklists técnicos ou qualquer layout estruturado que o `text` não consegue renderizar bem.

Não use `codeBlock` para criar header, progresso ou navegação paralela. Esses elementos pertencem ao tema global.

```json
{
  "type": "codeBlock",
  "settings": {
    "html": "<div class='cards'><div class='card'><p class='titulo'>Módulo 1. Título</p><p class='desc'>Descrição. <strong>Valor: R$ 97</strong></p></div></div>",
    "customCSS": ".cards{display:flex;flex-direction:column;gap:12px}.card{border:1px solid #d4cfc4;border-radius:12px;padding:14px;background:#fff}.card.bonus{background:#fff8f0}.titulo{margin:0;font-weight:700;font-size:15px}.desc{margin:4px 0 0;font-size:13px;color:#6b7280}",
    "customJS": "",
    "minHeight": "auto",
    "useTheme": false
  }
}
```
**Regra:** HTML limpo com classes no campo `html`. CSS isolado em `customCSS`. NUNCA `style=` inline no html.

Para conectar ações do HTML ao fluxo do quiz, use `data-fxf-action-id` e o campo `actions`:
```json
{
  "html": "<button data-fxf-action-id='cta'>Continuar</button>",
  "actions": [{ "id": "cta", "label": "CTA", "actionType": "nextStep" }]
}
```

---

## 17. carousel
Slides com imagem, texto e CTA opcional por slide.

Papel estratégico: organizar provas, cases, antes/depois ou módulos sem sobrecarregar a etapa.

**Atenção:** quando `ctaActionType: url`, `ctaUrl` é obrigatório em cada slide. Se não quiser CTA por slide, use `codeBlock`.

Campos obrigatórios (todos necessários para evitar erro de validação):
`slides, variant, autoplay, autoplayInterval, showArrows, arrowStyle, showDots, dotStyle, loop, dragFree, slidesPerView, slideGap, height, imageSizeMode, imageWidth, imageHeight, textPosition, imageBorderRadius, backgroundColor, borderRadius, arrowColor, arrowBackgroundColor, dotColor, dotActiveColor, titleColor, titleFontSize, titleFontWeight, titleAlignment, descriptionColor, descriptionFontSize, descriptionAlignment, ctaBackgroundColor, ctaTextColor, ctaBorderRadius, ctaPadding, ctaFontSize, ctaFontWeight, ctaAlignment, overlayBackgroundColor, overlayOpacity`

---

## 18. weightSlider / 19. heightSlider
Réguas de peso e altura com variável numérica.

Use `scoreDeltaEnabled: true` quando o valor selecionado deve entrar no placar global ao avançar.

```json
{
  "type": "weightSlider",
  "settings": {
    "value": 80, "min": 40, "max": 200, "step": 1,
    "unit": "kg", "allowUnitToggle": true,
    "showValue": true, "valueSize": "xl",
    "variableName": "peso", "scoreDeltaEnabled": false, "useTheme": true
  }
}
```

---

## 20. timer
Barra de progresso com ação ao completar.

Papel estratégico: loading animado entre etapas (ex: "analisando suas respostas...") ou sustentar escassez e ritmo.

```json
{
  "type": "timer",
  "settings": {
    "duration": 5,
    "title": "Analisando suas respostas...",
    "actionType": "nextStep",
    "variant": "line",
    "stylePreset": "default",
    "useTheme": true
  }
}
```

---

## 21. audioPlayer
Player de áudio com waveform visual.

Antes de preencher `settings.audioUrl`, chame `list_audio_assets` e use uma URL real retornada pela biblioteca do usuário.

```json
{
  "type": "audioPlayer",
  "settings": {
    "audioUrl": "https://cdn.exemplo.com/audio-do-usuario.mp3",
    "senderName": "Dr. Silva",
    "messageText": "Ouça o recado especial para você:",
    "showMessage": true,
    "autoPlay": false,
    "showWaveform": true,
    "variant": "default",
    "useTheme": true
  }
}
```

---

## 22. video
Vídeo por URL, YouTube ou embed.

Papel estratégico: aprofundar mecanismo, prova e emoção quando texto curto não basta.

```json
{
  "type": "video",
  "settings": {
    "videoType": "youtube",       // url | youtube | embed
    "youtubeUrl": "https://youtube.com/watch?v=XXX",
    "aspectRatio": "wide",
    "containerWidth": "full",
    "alignment": "center",
    "controls": true,
    "autoplay": false,
    "useTheme": false
  }
}
```

---

## 23. layoutContainer
Container estrutural em grid/flex para organizar componentes filhos.

```json
{
  "type": "layoutContainer",
  "settings": {
    "defaultConfig": { "mode": "grid", "columns": 2, "gap": "16px" },
    "padding": "0px",
    "useTheme": true
  },
  "children": [
    { "type": "text", "settings": { "content": "<p>Coluna 1</p>" } },
    { "type": "text", "settings": { "content": "<p>Coluna 2</p>" } }
  ]
}
```

---

## 24. spacer
Espaço vertical entre componentes.

```json
{ "type": "spacer", "settings": { "height": 24, "useTheme": true } }
```

---

## 25. gamifiedModal
Modal de conquista, desbloqueio, bônus ou decisão. É componente de etapa, não preset global.

```json
{
  "type": "gamifiedModal",
  "settings": {
    "triggerOnStepEnter": true,
    "content": "<h2 style='text-align:center'><strong>FASE DESBLOQUEADA</strong></h2><p style='text-align:center'>Continue avançando.</p>",
    "mediaType": "emoji",
    "emoji": "🔓",
    "size": "comfortable",
    "alignment": "center",
    "showButton": true,
    "buttonText": "CONTINUAR",
    "buttonVariant": "glow",
    "showCloseButton": true,
    "closeOnBackdrop": false,
    "useTheme": false
  }
}
```

Regras:
- Garanta uma saída clara: botão, botão X ou clique fora.
- Use para conteúdo rico com copy editável; não coloque esse conteúdo em `interactionEffects` global.

---

## 26. iphoneToast
Notificações estilo iPhone para prova social, venda aprovada, alerta curto ou feedback contextual.

Se `soundEnabled: true`, preencha `soundUrl` somente com URL retornada por `list_audio_assets`.

```json
{
  "type": "iphoneToast",
  "settings": {
    "items": [
      {
        "trigger": "stepEnter",
        "title": "Venda aprovada!",
        "description": "Valor:",
        "valueText": "R$ 37,00",
        "timeText": "agora",
        "duration": 3,
        "delay": 0.5,
        "soundEnabled": false,
        "soundUrl": ""
      }
    ],
    "displayMode": "queue",
    "allowSwipeDismiss": true,
    "triggerOnStepEnter": true,
    "useTheme": false
  }
}
```

Triggers:
- `stepEnter`: dispara ao entrar na etapa.
- `buttonClick`: informe `targetModuleId` para reagir a um botão.
- `optionSelect`: informe `targetModuleId` e, se necessário, `targetOptionId`.
