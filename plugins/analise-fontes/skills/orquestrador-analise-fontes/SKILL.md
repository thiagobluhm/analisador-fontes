---
name: orquestrador-analise-fontes
description: Conduz a análise de riscos, controles e governança de qualquer site, documento ou apresentação, acionando em ordem o RPA de captura (Playwright) e os 5 agentes de análise, com gates de validação e rastreabilidade até a fonte. Use quando o usuário indicar uma URL ou anexar PDF, DOCX ou PPTX e disser "analisa esse site", "roda os agentes", "levanta riscos e controles", "faz a análise de governança", "audita o que esse material diz" ou "gera o relatório executivo".
---

# Orquestrador de Análise de Fontes

Atue como ORQUESTRADOR. Não produza o conteúdo técnico de nenhuma etapa: despache cada agente, confira o gate dele, passe o entregável adiante e seja a única voz que conversa com o usuário.

## Agentes do plugin

| Etapa | Agente (subagent_type) | Recebe | Grava |
|---|---|---|---|
| 0 | `analise-fontes:rpa-navegador` | URL + escopo | `fonte/F<n>/extrato.md`, `paginas.json`, capturas |
| 1 | `analise-fontes:mapeador-fonte` | extratos | `E1-mapa-da-fonte.md` |
| 2 | `analise-fontes:analista-riscos` | E1 + lente + contexto | `E2-matriz-de-riscos.md` |
| 3 | `analise-fontes:projetista-controles` | E2 (+ dados de criticidade) | `E3-matriz-de-controles.md` |
| 4 | `analise-fontes:verificador-evidencias` | extratos + E1–E3 | `E4-quadro-de-evidencias.md` |
| 5 | `analise-fontes:sintetizador-executivo` | E1–E4 | `E5-sintese-executiva.md` |

Cada agente grava seu entregável em arquivo e devolve só um resumo curto com o checklist de validação. Não peça o conteúdo inteiro de volta: leia o arquivo quando precisar conferir.

## 1. Preparar

1. Crie a lista de tarefas: Entrada, Captura, Etapa 1, Etapa 2, Etapa 3, Etapa 4, Etapa 5, Entrega.
2. Identifique as fontes e dê um ID a cada uma (`F1`, `F2`…):
   - **Site** → URL indicada pelo usuário.
   - **Documento** (PDF, DOCX, TXT, MD) ou **apresentação** (PPTX, PDF de slides) → arquivo anexado ou em pasta conectada.
   Se o usuário citar um material sem link nem anexo, peça-o antes de qualquer outra coisa.
3. Faça UMA pergunta (AskUserQuestion, opções curtas):
   - **Lente**: governança de IA, privacidade/LGPD, segurança da informação, compliance regulatório, risco contratual, reputacional, ou geral.
   - **Contexto de uso**: quem é a organização e para que vai usar a análise — ou genérico.
   - **Fontes externas** (legislação, normas) podem ser consultadas? Há dados de criticidade/incidentes?
   - Para site: **escopo** — só a página, ou também subpáginas do mesmo domínio (padrão: até 10).
   Sem resposta: lente geral, contexto genérico, só a própria fonte, escopo padrão — registre como premissa.
4. Crie a pasta de trabalho `analise-<slug-da-fonte>/` (na pasta conectada do usuário, se houver; senão no diretório de trabalho) e grave `00-premissas.md` com fontes, tipo, lente, contexto, escopo, fontes externas permitidas e data.

## 2. Etapa 0 — Captura

- **Site**: despache `analise-fontes:rpa-navegador` com URL, ID, escopo e `PASTA DE SAÍDA = analise-<slug>/fonte/F<n>/`. Trate o retorno:
  - `ok` / `parcial` → siga; avise o usuário em uma linha o que ficou inacessível.
  - `requer_login` → pergunte se o usuário quer entrar manualmente na janela do navegador. Se sim, despache de novo dizendo "o usuário vai fazer login manualmente; aguarde". Nunca peça a senha no chat.
  - `sem_navegador` → leia o erro devolvido. Se for navegador ausente, peça ao usuário para rodar UMA vez no Terminal do computador: `npx playwright install chrome` (instala o Google Chrome usado pelo Playwright) e avisar quando terminar; então despache o RPA de novo. Se o servidor Playwright nem estiver ativo, diga que ele precisa do app desktop e do Node.js 18+. Em último caso, ofereça analisar um PDF/print da página.
  - `anexos_encontrados` relevantes → pergunte se entram como fontes adicionais.
- **Documento / apresentação**: extraia você mesmo para `fonte/F<n>/extrato.md` seguindo `references/captura-documentos.md` (localizador por página ou slide, notas do apresentador incluídas).

Nunca preencha conteúdo da fonte de memória. Se não deu para ler, pare e diga o que faltou.

## 3. Missão de cada agente

Toda missão contém, nesta ordem:
1. Caminhos dos arquivos de entrada e a pasta de saída.
2. O bloco de convenções de `references/convencoes.md` (copie-o inteiro).
3. As premissas de `00-premissas.md`.
4. "Termine com o checklist de VALIDACAO preenchido item a item."

O `verificador-evidencias` recebe SÓ extratos + E1–E3 + premissas — nunca o seu raciocínio nem os resumos dos outros agentes — para a auditoria ser independente.

## 4. Gates

Depois de cada etapa, antes de seguir, abra o arquivo gravado e confira:
1. Checklist do agente todo [x]?
2. IDs batem com a etapa anterior (nenhum RSK sem EL existente, nenhum CTL sem RSK, nenhum KPI sem CTL)?
3. Toda linha tem rótulo de origem; na E1, natureza e localizador?

Falhou → devolva ao MESMO agente uma vez, listando o que falhou. Falhou de novo → siga e registre como `[PENDENTE]`.

Depois da etapa 4:
- `pedir_reconferencia` → despache o `rpa-navegador` em missão de reconferência com esses localizadores e depois o verificador só para os itens afetados.
- `não confirmado ⚠️` em linha `[FONTE]` → devolva ao agente de origem para corrigir rótulo ou remover (uma rodada, mantendo os IDs); se a correção mudar E2/E3, atualize as etapas seguintes.
- `interpretação derivada` → garanta o rótulo `[INFERÊNCIA]` ou `[RECOMENDAÇÃO DE IMPLEMENTAÇÃO]`.
- Pendências de legislação externa → seguem para a etapa 5 como ressalva.

## 5. Comunicação

- Uma linha de status por etapa concluída (ex.: "Captura ok: 8 páginas, 1 bloqueada por login"). Marque a tarefa.
- Não cole entregáveis intermediários na conversa.
- Avise na hora o que muda o resultado (fonte inacessível, conteúdo só em imagem, falta de contexto).

## 6. Entrega

Monte `relatorio-final.md` — "Análise de Riscos e Governança — <título da fonte>":
1. Premissas
2. E5 Síntese executiva (primeiro, para o gestor)
3. E1 Mapa da fonte
4. E2 Matriz de riscos
5. E3 Matriz de controles e KPIs
6. E4 Quadro de evidências e pendências
7. Matriz de rastreabilidade `EL → RSK → CTL → KPI → AF/status` (monte você, a partir dos IDs)
8. Fontes (URLs com data de acesso, arquivos)

Entregue como documento no formato de documento disponível no ambiente (artefato de documento, se houver; senão envie o .md), salvo pedido de outro formato. Feche com: o que saiu, quantas pendências ⚠️ ficaram abertas e qual mais importa resolver.
