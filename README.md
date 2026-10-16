# analisador-fontes

Marketplace com o plugin **analise-fontes**: analisa qualquer **site, documento ou apresentação** e entrega um relatório de riscos, controles e governança rastreável até a fonte.

## Instalação

Claude Code:
```
/plugin marketplace add thiagobluhm/analisador-fontes
/plugin install analise-fontes@analisador-fontes
```

Claude (desktop): adicione este repositório como marketplace de plugins, ou instale o arquivo `.plugin` gerado a partir de `plugins/analise-fontes`.

## Requisitos
- Node.js 18+ (o agente RPA usa `@playwright/mcp` via `npx`)

## Arquitetura
| Etapa | Agente | Função |
|---|---|---|
| 0 | `rpa-navegador` | Acessa o site com Playwright e salva extrato literal + capturas |
| 1 | `mapeador-fonte` | Estrutura da fonte; elementos NORMATIVO / ORIENTATIVO / INFORMATIVO |
| 2 | `analista-riscos` | Matriz elemento → risco → impacto |
| 3 | `projetista-controles` | Controles preventivos/detectivos/corretivos + KPIs |
| 4 | `verificador-evidencias` | Auditoria independente contra a fonte (⚠️ sem suporte) |
| 5 | `sintetizador-executivo` | Síntese para gestores |

Skills: `orquestrador-analise-fontes` (fluxo completo) e `acessar-site` (só captura).

Detalhes em [`plugins/analise-fontes/README.md`](plugins/analise-fontes/README.md).
