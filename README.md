# mantis-style

> **Licença proprietária, todos os direitos reservados.** O código é público só para consulta. Usar, copiar ou modificar exige autorização por escrito de mantis-dev-br. Veja [LICENSE](LICENSE).

Design system Mantis em SCSS, no estilo Bootstrap/Tailwind: tokens, tema claro/escuro, reboot de todos os elementos HTML, componentes prontos e utilidades. Nasceu do `mpanel/mpanel.css` e é compatível com as classes dele.

Vitrine com todos os componentes: abra `docs/index.html` no navegador (depois de `npm run build`).

## Uso rápido

### Só CSS (sem Sass)

```html
<html lang="pt-BR" data-theme="light">
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
<link rel="stylesheet" href="mantis-style/dist/mantis-style.min.css">
```

### Com Sass (recomendado)

Instale como dependência (projetos autorizados):

```bash
npm install github:mantis-dev-br/mantis-style          # direto do GitHub
npm install github:mantis-dev-br/mantis-style#v1.0.0   # versão fixa (tag)
npm install ../caminho/para/mantis-style             # pasta local
```

Importe tudo pelo arquivo principal:

```scss
@use "pkg:mantis-style";                        // Sass ≥ 1.71 com --pkg-importer=node
// ou
@use "../node_modules/mantis-style/scss/mantis-style";
```

O Vite já resolve `pkg:` quando configurado com `css.preprocessorOptions.scss.importers: [new NodePackageImporter()]`; senão, use o caminho relativo.

## Arquivos de import

| Arquivo | O que carrega |
|---|---|
| `scss/mantis-style.scss` | **Tudo**: tokens, reboot, layout, componentes e utilidades |
| `scss/mantis-style-core.scss` | Só tokens, temas, reboot e animações (sem classes) |
| `scss/mantis-style-utilities.scss` | Core + utilidades (estilo Tailwind, sem componentes) |
| `scss/tools` | Só variáveis, funções e mixins. **Não gera CSS** |

Import parcial (só o que o projeto usa):

```scss
@use "pkg:mantis-style/core";
@use "pkg:mantis-style/scss/components/button";
@use "pkg:mantis-style/scss/components/form";
```

## Configuração

Todas as variáveis são `!default`. Configure na importação:

```scss
@use "pkg:mantis-style" as ms with (
  $prefix: "ms-",                                   // .ms-btn, .ms-card… (evita conflito com Bootstrap)
  $theme-light-overrides: ("promote": #0a7cff),    // troca só alguns tokens
  $theme-dark-overrides: ("promote": #4da3ff),
  $enable-auto-dark-mode: false,                    // escuro só com data-theme="dark"
  $utilities-disabled: ("opacity", "cursor"),       // remove grupos de utilidades
  $spacers: (…),                                    // escala de espaçamento própria
);
```

Principais opções (veja `scss/settings/`):

| Variável | Padrão | Para quê |
|---|---|---|
| `$prefix` | `""` | Prefixo das classes |
| `$var-prefix` | `"ms-"` | Prefixo das variáveis CSS e animações |
| `$enable-dark-mode` | `true` | Gera o tema escuro |
| `$enable-auto-dark-mode` | `true` | Sem `data-theme`, segue o sistema operacional |
| `$enable-responsive-utilities` | `true` | Gera `md:`, `lg:`… |
| `$enable-important-utilities` | `false` | `!important` em todas as utilidades |
| `$theme-light` / `$theme-dark` | — | Mapas completos dos tokens |
| `$type-scale`, `$spacers`, `$radii`, `$shadows`, `$breakpoints` | — | Escalas |
| `$utilities-extra` | `()` | Grupos de utilidades novos |

### Usando tokens e mixins no seu SCSS

```scss
@use "pkg:mantis-style/scss/tools" as ms;

.meu-card {
  padding: ms.space(mini);                // 1rem
  color: ms.v(content-title);             // var(--ms-content-title)
  border-radius: ms.radius(md);           // var(--ms-radius-md)
  box-shadow: ms.shadow(level2);
  @include ms.type(heading-sm);           // peso, tamanho e altura de linha
  @include ms.media-down(md) { padding: ms.space(nano); }
  @include ms.dark { border-color: ms.v(layout-border); }
  &:focus-visible { @include ms.focus-outline; }
}
```

Funções: `v()`, `space()`, `radius()`, `shadow()`, `z()`, `control-height()`, `bp-min()`, `bp-max()`, `kf()`.
Mixins: `type`, `media-down`, `media-up`, `media-between`, `dark`, `focus-ring`, `focus-outline`, `transition`, `truncate`, `line-clamp`, `visually-hidden`, `disabled-state`, `css-vars`.

## Tema

```html
<html data-theme="light">   <!-- ou "dark"; sem atributo segue o sistema -->
```

Também funciona em um trecho da página: `<section data-theme="dark">`.

Os tokens viram variáveis CSS e podem ser usados direto no CSS: `var(--ms-content-title)`, `var(--ms-layout-surface)`, `var(--ms-promote)`…

| Grupo | Tokens |
|---|---|
| Texto | `content-title`, `content-base`, `content-placeholder`, `content-primary`, `content-brand`, `content-on-color` |
| Superfícies | `layout-body`, `layout-surface`, `layout-spotlight`, `layout-brand`, `layout-border`, `layout-overlay` |
| Interativos | `interactive-primary(-hover, -fg)`, `interactive-secondary(-hover)`, `interactive-destructive(-hover, -fg)` |
| Status | `positive`, `warning`, `negative`, `info`, `neutral` (cada um com `-fg` e `-bg`) |
| Marca | `promote`, `promote-alt`, `promote-fg` |
| Outros | `highlight-focus`, `ring-shadow`, `selection`, `terminal-*`, `font-sans`, `font-mono`, `radius-*`, `shadow-*` |

## Escalas

- **Espaçamento:** none 0 · quark 4 · nano 8 · micro 12 · mini 16 · xxxs 20 · xxs 24 · xs 32 · sm 40 · md 48 · lg 64 · xl 80 (px)
- **Tipografia (`.ts-*`):** `heading-{xl,lg,md,sm,xs,xxs,xxxs}`, `subtitle-{md…xxxs}`, `paragraph-{sm…xxxs}`, `caption-xxxs`
- **Raio:** none, xs, sm, md, lg, pill, circular
- **Sombra:** level1 a level4
- **Breakpoints:** sm 640 · md 768 · lg 1024 · xl 1280

## Responsivo (desktop-first)

Igual ao mpanel: o prefixo vale **abaixo** da largura.

```html
<div class="grid grid-cols-4 lg:grid-cols-2 md:grid-cols-1">   <!-- 4 → 2 (<1024) → 1 (<768) -->
<input class="md:hidden">                                         <!-- some abaixo de 768px -->
<main class="p-xs md:p-mini">
```

Têm variante responsiva: tipografia, alinhamento de texto, espaçamentos, display, flex, grid, largura, posição e ordem.

## Componentes

| Componente | Classes |
|---|---|
| Ícone | `icon` (`-xs/-sm/-lg/-xl/-filled`), `icon-shape` (`-sm/-lg/-xl/-square/-positive…/-promote`) |
| Botão | `btn` + `-primary/-secondary/-outline/-ghost/-danger/-promote`, `-sm/-lg`, `-icon`, `-block`, `-pill`; `btn-link`, `btn-group` |
| Card | `card` (`-muted/-bordered/-elevated/-flush/-link`), `card-header/-title/-subtitle/-footer` |
| Formulário | `form`, `field`, `label` (`data-required`), `hint` (`-negative…`), `input`, `select`, `textarea`, `range`, `input-sm`, `input-group` (+ `input-group-end`) |
| Escolha | `check`, `radio`, `switch` (`-lg`), `check-label` |
| Rótulos | `tag` (`-positive/-warning/-negative/-info/-highlight/-outline/-sm/-lg`), `badge`, `dot` |
| Filtros e abas | `chip`, `chip-group`, `tabs` + `tab`, `tabs-line`, `tab-panel` |
| Navegação | `nav`, `nav-heading`, `nav-item`, `nav-label`, `breadcrumb`, `crumb`, `pagination`, `page-btn`, `pagination-info` |
| Dados | `table` (`-compact/-striped/-bordered/-static`), `table-wrap`, `list`, `list-row`, `list-divided`, `avatar`, `avatar-group` |
| Feedback | `alert` (`-positive…/-promote`), `progress` (`is-warning…`, `-sm/-lg/-indeterminate`), `toast`/`toasts`, `spinner`, `skeleton`, `empty` |
| Sobreposições | `overlay`, `modal` (`-sm/-lg/-xl/-full`, também `<dialog>`), `drawer` (`-left`), `dropdown` + `menu`, `menu-item`, `menu-sep`, `menu-label`, `data-tooltip` |
| Conteúdo | `prose`, `link`, `divider`, `divider-label`, `accordion`, `terminal`, `kbd` |
| Layout | `container` (`-sm…-xl`), `app` + `app-header/app-nav/app-main` (`data-collapsed`, `data-nav-open`) |

Estados seguem ARIA: `aria-selected="true"` (aba), `aria-pressed="true"` (chip), `aria-current="page"` (nav, paginação), `aria-invalid="true"` (campo), `:disabled`/`aria-disabled`. As classes `.is-active` e `.is-disabled` também funcionam.

## Reboot

Todos os elementos nativos já saem estilizados: títulos `h1–h6`, `p`, `a`, `strong`, `small`, `mark`, `code`, `kbd`, `pre`, `blockquote`, `hr`, `abbr`, `del/ins`, `sub/sup`, `img/video/svg`, `table/th`, `fieldset/legend`, `details/summary`, `dialog`, `::selection`, foco por teclado e `prefers-reduced-motion`.

Listas (`ul`, `ol`) ficam **sem** marcador e margens, para servir de menu. Em texto corrido, envolva o conteúdo com `.prose`.

## Migrando do mpanel.css

Todas as classes do `mpanel.css` existem no pacote. Mudanças:

- Variáveis CSS ganharam prefixo: `--content-title` → `--ms-content-title` (ou configure `$var-prefix: ""`).
- O CSS que ficava dentro do `index.html` virou pacote: `.app` (casca), `.term`, `.crumb`, `.cols-2-1/.cols-1-2/.cols-3-2`.
- `md:p-mini`, que o `index.html` usava sem existir, agora existe.
- Animações têm prefixo: `ms-fade`, `ms-pop`…

## Scripts

```bash
npm install
npm run build    # gera dist/*.css e dist/*.min.css
npm run watch    # recompila ao salvar
```

## Estrutura

```
scss/
├── mantis-style.scss            ← import principal (tudo)
├── mantis-style-core.scss
├── mantis-style-utilities.scss
├── settings/    config, cores/temas, tipografia, espaçamento, breakpoints, efeitos, layout
├── tools/       funções e mixins
├── base/        variáveis CSS, reboot, animações
├── layout/      container, casca de app
├── components/  um arquivo por componente
└── utilities/   gerador (_api) e utilidades manuais (_helpers)
```

## Licença

Copyright (c) 2026 mantis-dev-br. Todos os direitos reservados.

Este repositório é público apenas para consulta. Nenhuma licença de uso é concedida: usar, copiar, modificar ou distribuir qualquer parte do código exige autorização prévia e por escrito do titular. Pedidos de autorização: abra uma issue. Texto completo em [LICENSE](LICENSE).
