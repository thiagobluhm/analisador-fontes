---
name: acessar-site
description: Acessa o site que o usuário indicar com o agente RPA (navegador Playwright), percorre as subpáginas no escopo pedido e entrega o extrato fiel com metadados e capturas de tela, sem rodar a análise de riscos. Use quando o usuário disser "acessa esse site", "entra nesse link e pega o conteúdo", "captura essa página", "extrai o texto desse site", "tira print dessa página" ou quiser só o conteúdo de uma URL antes de decidir analisar.
---

# Acessar site com o RPA

1. Pegue a URL. Se faltar, peça. Se o usuário não disser o escopo, use: só a página indicada + subpáginas do mesmo domínio diretamente ligadas ao tema, até 10.
2. Crie a pasta `captura-<slug-do-dominio>/` (na pasta conectada do usuário, se houver; senão no diretório de trabalho).
3. Despache `analise-fontes:rpa-navegador` em missão de captura com: `URL`, `ID da fonte = F1`, `ESCOPO`, `PASTA DE SAÍDA = captura-<slug>/`.
4. Trate o retorno:
   - `requer_login` → pergunte se o usuário quer entrar manualmente na janela do navegador; se sim, despache de novo avisando que ele fará o login. Nunca peça senha no chat.
   - `sem_navegador` → diga que o servidor Playwright do plugin não está ativo (precisa do app desktop e do Node.js instalado) e que, sem ele, a página não é acessada.
   - `parcial` → diga o que ficou de fora e por quê.
5. Entregue o `extrato.md` (e as capturas, se pedidas) e diga em uma linha: quantas páginas, o que ficou inacessível, anexos encontrados.
6. Ofereça em uma linha rodar a análise completa (skill `orquestrador-analise-fontes`) sobre esse extrato — a captura já feita é reaproveitada como `fonte/F1/`.

Não resuma nem interprete o conteúdo nesta skill, a menos que o usuário peça.
