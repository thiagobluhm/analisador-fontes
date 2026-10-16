# Convenções (copiar inteiro em toda missão de agente)

## IDs rastreáveis — nunca renumerar entre etapas
- `F1…` fontes
- `EL-01…` elementos-chave da fonte (E1), cada um com natureza e localizador
- `RSK-01…` riscos (E2), cada um com `elemento: EL-xx`
- `CTL-01…` controles (E3), cada um com `risco: RSK-xx`
- `KPI-01…` indicadores (E3), cada um com `controle: CTL-xx`
- `AF-01…` afirmações auditadas (E4), cada uma com `origem: E<n> / <ID>`

## Localizadores
- Site: `F1:/caminho#ancora`
- Documento: `F1:p.12` ou `F1:§3.2`
- Apresentação: `F1:slide 7` (notas: `F1:slide 7/notas`)

## Rótulo de origem — obrigatório em toda linha
- `[FONTE]` está na fonte analisada (com localizador)
- `[EXTERNA]` fonte externa autorizada (com link)
- `[INFERÊNCIA]` interpretação técnica derivada
- `[RECOMENDAÇÃO DE IMPLEMENTAÇÃO]` proposta da análise, não da fonte
- `[PENDENTE]` falta dado ou evidência — dizer qual

## Natureza do elemento (E1)
- `NORMATIVO` exige/obriga
- `ORIENTATIVO` recomenda/propõe
- `INFORMATIVO` descreve/afirma/promove

## Proibições
- Não tratar recomendação, boa prática ou marketing como obrigação legal. Obrigação legal só com base normativa citada (lei, regulamento, contrato assinado); senão `[PENDENTE]` com a fonte necessária.
- Não inventar seções, datas, versões, autores, metas, probabilidades, gravidades, multas, periodicidades ou tecnologias obrigatórias.
- Não tratar afirmação promocional ou opinativa como fato verificado.
- Ajustar a profundidade ao tamanho da fonte; cobrir NORMATIVO e ORIENTATIVO primeiro.
- Escrever em português do Brasil.
