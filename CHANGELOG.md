# Changelog

## 1.0.0 — 2026-11-16

Primeira versão, extraída do `mpanel/mpanel.css`.

- Tokens em mapas Sass, emitidos como variáveis CSS `--ms-*`, com tema claro, escuro e automático (pelo sistema operacional).
- Reboot com todos os elementos HTML nativos.
- Layout: `container` e casca de app (`app`, header, sidebar recolhível, off-canvas no mobile).
- 26 componentes: botão, card, formulário, check/radio/switch, tag/badge/dot, chip, abas, nav, breadcrumb, tabela, lista, paginação, progresso, alerta, avatar, divisor, acordeão, terminal, spinner/skeleton/vazio, menu, modal, drawer, toast, tooltip, prose/link.
- Gerador de utilidades configurável, com variantes responsivas desktop-first (`lg:`, `md:`, `sm:`, `xl:`).
- Configuração por `@use … with (…)`: prefixo de classes, sobrescrita de tokens e remoção de grupos de utilidades.
- Bundles: completo, core e utilities, cada um em versão expandida e minificada.
