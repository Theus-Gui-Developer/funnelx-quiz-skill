# Temas por Nicho — FunnelX Quiz

## Padrão de Configuração

```js
update_funnel_global_presets({
  funnel: "<slug>",
  replaceSettings: true,
  replaceTheme: true,
  settings: {
    pageWidth: "600px",
    progressBarStyle: "default",
    showBackButton: true,
    showProgress: true
  },
  theme: { /* ver abaixo por nicho */ }
})
```

---

## Nichos Masculinos

### Disfunção Erétil / Calvície
```json
{
  "primaryColor": "#1a2e4a",
  "secondaryColor": "#2c4a6e",
  "accentColor": "#c8a84b",
  "backgroundColor": "#f5f3ef",
  "textColor": "#1c1a18",
  "fontFamily": "Inter, sans-serif",
  "borderRadius": "12px",
  "buttonStyle": {
    "backgroundColor": "#1a2e4a",
    "textColor": "#ffffff",
    "borderRadius": "10px",
    "fontSize": "17px",
    "fontWeight": "700",
    "padding": "16px 32px"
  },
  "headerStyle": {
    "backgroundColor": "#1a2e4a",
    "progressColor": "#c8a84b",
    "textColor": "#ffffff"
  },
  "optionStyle": {
    "backgroundColor": "#ffffff",
    "borderColor": "#d4cfc8",
    "borderRadius": "10px",
    "padding": "14px 18px",
    "selectedBackgroundColor": "#1a2e4a",
    "selectedBorderColor": "#1a2e4a",
    "selectedTextColor": "#ffffff"
  }
}
```

### Libido Baixa Masculina
```json
{
  "primaryColor": "#1c2e1c",
  "secondaryColor": "#2d4a2d",
  "accentColor": "#b8860b",
  "backgroundColor": "#f4f1eb",
  "textColor": "#1a1a12",
  "borderRadius": "12px",
  "buttonStyle": {
    "backgroundColor": "#1c2e1c",
    "textColor": "#ffffff",
    "borderRadius": "10px",
    "fontSize": "17px",
    "fontWeight": "700",
    "padding": "16px 32px"
  },
  "headerStyle": {
    "backgroundColor": "#1c2e1c",
    "progressColor": "#b8860b",
    "textColor": "#ffffff"
  },
  "optionStyle": {
    "backgroundColor": "#ffffff",
    "borderColor": "#ccc9b8",
    "borderRadius": "10px",
    "padding": "14px 18px",
    "selectedBackgroundColor": "#1c2e1c",
    "selectedBorderColor": "#1c2e1c",
    "selectedTextColor": "#ffffff"
  }
}
```

### Calvície Masculina
```json
{
  "primaryColor": "#1e2d1e",
  "secondaryColor": "#2e4a2e",
  "accentColor": "#c0932a",
  "backgroundColor": "#f2f0ed",
  "textColor": "#1a1a14",
  "borderRadius": "12px",
  "buttonStyle": {
    "backgroundColor": "#1e2d1e",
    "textColor": "#ffffff",
    "borderRadius": "10px",
    "fontSize": "17px",
    "fontWeight": "700",
    "padding": "16px 32px"
  },
  "headerStyle": {
    "backgroundColor": "#1e2d1e",
    "progressColor": "#c0932a",
    "textColor": "#ffffff"
  },
  "optionStyle": {
    "backgroundColor": "#ffffff",
    "borderColor": "#ccc8bf",
    "borderRadius": "10px",
    "padding": "14px 18px",
    "selectedBackgroundColor": "#1e2d1e",
    "selectedBorderColor": "#1e2d1e",
    "selectedTextColor": "#ffffff"
  }
}
```

---

## Nichos Femininos

### Perda de Peso Feminina 35+
```json
{
  "primaryColor": "#8b1a4a",
  "secondaryColor": "#c2185b",
  "accentColor": "#f48fb1",
  "backgroundColor": "#fff8f9",
  "textColor": "#2d1b20",
  "borderRadius": "14px",
  "buttonStyle": {
    "backgroundColor": "#8b1a4a",
    "textColor": "#ffffff",
    "borderRadius": "50px",
    "fontSize": "17px",
    "fontWeight": "700",
    "padding": "16px 32px"
  },
  "headerStyle": {
    "backgroundColor": "#8b1a4a",
    "progressColor": "#f48fb1",
    "textColor": "#ffffff"
  },
  "optionStyle": {
    "backgroundColor": "#ffffff",
    "borderColor": "#f0c0d0",
    "borderRadius": "12px",
    "padding": "14px 18px",
    "selectedBackgroundColor": "#8b1a4a",
    "selectedBorderColor": "#8b1a4a",
    "selectedTextColor": "#ffffff"
  }
}
```

### Queda de Cabelo Feminina
```json
{
  "primaryColor": "#6b2d3e",
  "secondaryColor": "#9e4a5a",
  "accentColor": "#c97d6e",
  "backgroundColor": "#fdf9f7",
  "textColor": "#2a1a1f",
  "borderRadius": "14px",
  "buttonStyle": {
    "backgroundColor": "#6b2d3e",
    "textColor": "#ffffff",
    "borderRadius": "50px",
    "fontSize": "17px",
    "fontWeight": "700",
    "padding": "16px 32px"
  },
  "headerStyle": {
    "backgroundColor": "#6b2d3e",
    "progressColor": "#c97d6e",
    "textColor": "#ffffff"
  },
  "optionStyle": {
    "backgroundColor": "#ffffff",
    "borderColor": "#e8c5bc",
    "borderRadius": "12px",
    "padding": "14px 18px",
    "selectedBackgroundColor": "#6b2d3e",
    "selectedBorderColor": "#6b2d3e",
    "selectedTextColor": "#ffffff"
  }
}
```

---

## Regras de Tema

1. **Masculino:** `borderRadius: "10px"` nos botões (quadrado/arredondado)
2. **Feminino:** `borderRadius: "50px"` nos botões (pill shape)
3. **Cores no tema, não no componente:** nunca use `style=color:#xxx` onde deve herdar do tema
4. `useTheme: true` em: text, button, argument (sem backgroundColor customizado), form, level, countdown, notification
5. `useTheme: false` em: progressArgument, pricingCard, faq, testimonial, codeBlock, argument (com backgroundColor customizado)
