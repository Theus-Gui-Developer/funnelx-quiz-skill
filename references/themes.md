# Temas por Nicho - FunnelX Quiz

## Regra Principal

O visual premium do quiz nasce em `theme` e `settings`, não em headers improvisados dentro de componentes.

- Use `theme.header` para logo, emoji/texto do topo, progresso, botão voltar e cores do header.
- Nunca crie header paralelo com `text`, `image`, `layoutContainer` ou `codeBlock` no topo das etapas.
- Use `headerOverride` só como exceção pontual de uma etapa, nunca como padrão do funil.
- Use `replaceTheme: false` e `replaceSettings: false` quando enviar patch parcial. Use `true` apenas se enviar o objeto completo.
- Quando habilitar áudio, chame `list_audio_assets` antes e use uma URL real do usuário.

## Estrutura Real do Tema

```js
update_funnel_global_presets({
  funnel: "<slug>",
  replaceTheme: false,
  replaceSettings: false,
  theme: {
    colors: {
      primary: "#1e2d1e",
      primaryText: "#ffffff",
      background: "#f2f0ed",
      text: "#1a1a14",
      border: "#ccc8bf"
    },
    typography: {
      fontFamily: "Inter, system-ui, sans-serif",
      headingFont: "Inter, system-ui, sans-serif",
      fontSize: { xs: "12px", sm: "14px", base: "16px", lg: "18px", xl: "20px", "2xl": "24px", "3xl": "30px" },
      lineHeight: { tight: "1.2", normal: "1.5", relaxed: "1.75" },
      fontWeight: { normal: "400", medium: "500", semibold: "700", bold: "900" }
    },
    layout: {
      borderRadius: "12px",
      shadowLevel: 2,
      containerMaxWidth: "448px",
      contentMaxWidth: "448px",
      pagePaddingX: "16px",
      contentGap: "22px"
    },
    header: {
      enabled: true,
      showProgress: true,
      progressBarStyle: "default",
      showBackButton: true,
      contentType: "text",
      content: "DIAGNOSTICO CAPILAR",
      fontSize: "12px",
      height: "auto",
      marginBottom: "0px",
      backgroundColor: "#1e2d1e",
      textColor: "#ffffff",
      progressColor: "#c0932a",
      backButtonColor: "#ffffff"
    },
    background: {
      enabled: false,
      imageUrl: "",
      imageFit: "cover",
      imagePosition: "center",
      imageRepeat: "no-repeat",
      applyToHeader: true,
      overlayEnabled: true,
      overlayColor: "#111827",
      overlayOpacity: 0.45
    },
    form: {
      fieldHeight: "52px",
      fieldPadding: "14px 16px",
      fieldFontSize: "16px",
      fieldBorderRadius: "12px",
      fieldSpacing: "14px",
      labelSpacing: "8px",
      fieldBorderColor: "#cfc8bc",
      fieldFocusBorderColor: "#c0932a",
      fieldBackgroundColor: "#ffffff",
      fieldTextColor: "#1a1a14",
      fieldPlaceholderColor: "#8a8174",
      labelColor: "#1a1a14",
      labelFontSize: "14px",
      labelFontWeight: "700"
    }
  },
  settings: {
    pageWidth: "448px",
    showBackButton: true,
    showProgress: true,
    progressBarStyle: "default",
    gamification: {
      enabled: true,
      persistSession: true,
      score: {
        enabled: true,
        scoreKey: "scoreDiagnostico",
        label: "Analise desbloqueada",
        prefix: "",
        suffix: "%",
        decimals: 0,
        initialValue: 0,
        position: "aboveHeader"
      },
      backgroundMusic: {
        enabled: false,
        audioUrl: "",
        volume: 0.35,
        loop: true,
        autoPlay: true,
        showControl: false,
        label: "Trilha do diagnostico"
      },
      interactionEffects: {
        enabled: true,
        rules: [
          { id: "button-click", label: "Clique em botao", trigger: "buttonClick", enabled: true, vibration: { enabled: true, preset: "light" }, sound: { enabled: false, audioUrl: "", volume: 0.45 }, visual: { enabled: false, effect: "successPulse", color: "#22e06f" } },
          { id: "option-select", label: "Selecao de opcao", trigger: "optionSelect", enabled: true, vibration: { enabled: true, preset: "light" }, sound: { enabled: false, audioUrl: "", volume: 0.45 }, visual: { enabled: true, effect: "successPulse", color: "#c0932a" } },
          { id: "form-error", label: "Erro de validacao", trigger: "formValidationError", enabled: true, vibration: { enabled: true, preset: "impact" }, sound: { enabled: false, audioUrl: "", volume: 0.45 }, visual: { enabled: true, effect: "shake", color: "#ef4444" } },
          { id: "flow-complete", label: "Conclusao do quiz", trigger: "flowComplete", enabled: true, vibration: { enabled: true, preset: "success" }, sound: { enabled: false, audioUrl: "", volume: 0.45 }, visual: { enabled: true, effect: "successPulse", color: "#22c55e" } }
        ]
      }
    }
  }
})
```

## Header Oficial

Use este padrão em todos os funis novos:

```json
{
  "header": {
    "enabled": true,
    "showProgress": true,
    "progressBarStyle": "default",
    "showBackButton": true,
    "contentType": "text",
    "content": "DIAGNOSTICO PERSONALIZADO",
    "fontSize": "12px",
    "height": "auto",
    "marginBottom": "0px",
    "backgroundColor": "#1e2d1e",
    "textColor": "#ffffff",
    "progressColor": "#c0932a",
    "backButtonColor": "#ffffff"
  }
}
```

Variações permitidas:

- `contentType: "emoji"` para quizzes leves, como beleza, pets ou lifestyle.
- `contentType: "image"` quando o usuário fornecer logo real. Configure `imageWidth`, `imageHeight`, `imageObjectFit` e `imageBorderRadius`.
- `progressBarStyle: "full-top"` para experiências mais imersivas, mas não duplique progresso com componente visual.

## Presets de Nicho

Use os valores abaixo dentro da estrutura real acima.

### Calvície Masculina Premium

```json
{
  "colors": { "primary": "#1e2d1e", "primaryText": "#ffffff", "background": "#f2f0ed", "text": "#1a1a14", "border": "#ccc8bf" },
  "header": { "content": "DIAGNOSTICO CAPILAR", "backgroundColor": "#1e2d1e", "textColor": "#ffffff", "progressColor": "#c0932a", "backButtonColor": "#ffffff" },
  "layout": { "borderRadius": "12px", "shadowLevel": 2, "contentGap": "22px" },
  "form": { "fieldBorderRadius": "12px", "fieldFocusBorderColor": "#c0932a" }
}
```

Direção visual: clínica masculina, premium, com verde escuro, dourado seco e fundo mineral. Evite cards neon ou gradientes genéricos.

### Disfunção Erétil / Libido Masculina

```json
{
  "colors": { "primary": "#1a2e4a", "primaryText": "#ffffff", "background": "#f5f3ef", "text": "#1c1a18", "border": "#d4cfc8" },
  "header": { "content": "CHECKUP MASCULINO", "backgroundColor": "#1a2e4a", "textColor": "#ffffff", "progressColor": "#c8a84b", "backButtonColor": "#ffffff" },
  "layout": { "borderRadius": "10px", "shadowLevel": 2 },
  "form": { "fieldBorderRadius": "10px", "fieldFocusBorderColor": "#c8a84b" }
}
```

Direção visual: discreta, adulta, sem clichês agressivos. Use linguagem de performance, energia e confiança.

### Perda de Peso Feminina 35+

```json
{
  "colors": { "primary": "#8b1a4a", "primaryText": "#ffffff", "background": "#fff8f9", "text": "#2d1b20", "border": "#f0c0d0" },
  "header": { "content": "ANALISE METABOLICA", "backgroundColor": "#8b1a4a", "textColor": "#ffffff", "progressColor": "#f48fb1", "backButtonColor": "#ffffff" },
  "layout": { "borderRadius": "22px", "shadowLevel": 2 },
  "form": { "fieldBorderRadius": "18px", "fieldFocusBorderColor": "#c2185b" }
}
```

Direção visual: acolhedora e sofisticada. Use rosa profundo, fundo claro e cartões suaves, sem estética infantil.

### Queda de Cabelo Feminina

```json
{
  "colors": { "primary": "#6b2d3e", "primaryText": "#ffffff", "background": "#fdf9f7", "text": "#2a1a1f", "border": "#e8c5bc" },
  "header": { "content": "MAPA CAPILAR", "backgroundColor": "#6b2d3e", "textColor": "#ffffff", "progressColor": "#c97d6e", "backButtonColor": "#ffffff" },
  "layout": { "borderRadius": "20px", "shadowLevel": 2 },
  "form": { "fieldBorderRadius": "16px", "fieldFocusBorderColor": "#c97d6e" }
}
```

Direção visual: dermocosmética, íntima e elegante. Evite alertas agressivos demais no início; deixe urgência para diagnóstico/oferta.

## Checklist de Tema

- [ ] `theme.header` configurado antes de criar etapas.
- [ ] Nenhuma etapa tem bloco visual fingindo header.
- [ ] `text` usa `useTheme: true` para headlines e copy normal.
- [ ] `argument` com fundo customizado usa `useTheme: false`.
- [ ] `pricingCard`, `faq`, `testimonial`, `progressArgument` e `codeBlock` só recebem cores customizadas quando isso melhora a hierarquia.
- [ ] `settings.gamification` configurado globalmente, sem componentes legados de score/música em funis novos.
