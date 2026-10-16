# analise-fontes

Analisa qualquer **site, documento ou apresentação** e entrega um relatório de riscos, controles e governança com rastreabilidade até a fonte (`EL → RSK → CTL → KPI → evidência`).

## Skills
| Skill | Para quê |
|---|---|
| `orquestrador-analise-fontes` | Fluxo completo: captura → mapa → riscos → controles → verificação → síntese executiva |
| `acessar-site` | Só acessar uma URL com o RPA e devolver o extrato fiel + capturas de tela |

## Agentes
| Etapa | Agente | Origem |
|---|---|---|
| 0 | `rpa-navegador` | novo — único com navegador (Playwright) |
| 1 | `mapeador-fonte` | AGENTE_PARTE_1 |
| 2 | `analista-riscos` | AGENTE_PARTE_2 |
| 3 | `projetista-controles` | AGENTE_PARTE_3 |
| 4 | `verificador-evidencias` | AGENTE_PARTE_4 (auditoria independente) |
| 5 | `sintetizador-executivo` | AGENTE_PARTE_5 |

## Playwright (RPA)
O servidor `playwright` (`@playwright/mcp`) roda no computador do usuário via `npx`, com perfil isolado (cada sessão começa sem cookies nem logins salvos).

Requisitos: app desktop do Claude + **Node.js 18+**. Na primeira execução o `npx` baixa o pacote; se faltar navegador, o agente chama `browser_install`.

Limites de segurança do RPA: somente leitura — não envia formulários, não compra, não aceita termos, não contorna CAPTCHA/paywall/bloqueio. Login: o usuário entra manualmente na janela; senhas nunca passam pelo chat.

## Saída
Uma pasta `analise-<fonte>/` com `00-premissas.md`, `fonte/F1/extrato.md`, `E1…E5` e `relatorio-final.md`.
