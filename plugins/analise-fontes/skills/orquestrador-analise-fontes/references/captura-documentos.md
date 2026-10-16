# Captura de documentos e apresentações

Gere `fonte/F<n>/extrato.md` no mesmo formato do extrato de site, para que os agentes leiam tudo igual.

```
# F1 — <título do arquivo ou da capa>
Arquivo: <nome> | Tipo: PDF/DOCX/PPTX | Páginas ou slides: N | Lido em: <data>

## [F1:p.1]            (ou [F1:slide 1], ou [F1:§1 Título])
<texto literal>
```

## PDF
1. `pdftotext -layout arquivo.pdf -` página a página (`-f N -l N`) ou `pdfplumber` em Python; um bloco `## [F1:p.N]` por página.
2. Página sem texto ou com tabela/gráfico importante → renderize (`pdftoppm -r 110 -png -f N -l N`) e olhe a imagem; transcreva o que for legível e marque `(transcrito de imagem)`.
3. PDF escaneado inteiro → OCR (`tesseract`, se disponível) ou leitura visual página a página; avise o usuário que a fidelidade é menor.
4. Metadados: `pdfinfo` (título, autor, datas de criação/modificação).

## DOCX
1. `python-docx` (ou `pandoc -t markdown`) preservando títulos; localizador = `§<número> <título da seção>`. Se o documento tiver numeração de páginas relevante, informe que o localizador é por seção.
2. Tabelas em Markdown; comentários e controle de alterações listados à parte, marcados como tal.

## PPTX
1. `python-pptx`: para cada slide, título, todo texto das formas (na ordem de leitura), tabelas e **notas do apresentador** em `### [F1:slide N/notas]`.
2. Slides cujo conteúdo está em gráfico/imagem → renderize (LibreOffice `soffice --headless --convert-to pdf` + `pdftoppm`) e transcreva o essencial, marcado `(transcrito de imagem)`.
3. PDF de slides: trate como PDF, mas use `slide N` como localizador.

## TXT / MD
Leia direto; localizador = título de seção ou nº de linha.

## Regras
- Texto literal: não resumir, não traduzir, não corrigir.
- Declarar o que não pôde ser lido (página corrompida, imagem ilegível, senha).
- Nunca editar o arquivo original do usuário.
