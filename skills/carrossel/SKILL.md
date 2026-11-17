---
name: carrossel
description: Cria carrosséis e posts estáticos com a identidade da marca (Instagram, X, LinkedIn), do roteiro ao PNG em alta, com galeria, slider, comparação de versões e legenda. Use quando o usuário pedir carrossel, post, capa, slide para redes sociais, ou quiser montar a identidade visual da marca para posts.
---

# Carrossel para Codex

Workflow para produzir carrosséis com cara de marca, não de template. A qualidade vem de quatro coisas: **roteiro antes do visual**, **a galeria como referência**, **assets próprios** e **checagem visual de cada render**. Use o fluxo abaixo, respeitando o escopo, as preferências e as aprovações já dadas pelo usuário.

Caminhos: `<skill>` é a pasta deste arquivo. Rode os scripts da raiz do projeto do usuário; `--dir` é relativo a ela. O projeto do usuário fica em `carrosseis/` na raiz do repositório dele (ou outro caminho passado com `--dir`).

## Uso no Codex

Invocação explícita: `$carrossel`. Também se aplica a pedidos de carrosséis e posts para redes sociais.

- Resolva `<skill>` como o diretório que contém este `SKILL.md`; use caminhos absolutos entre aspas nos comandos. Não dependa do diretório atual para localizar scripts.
- Use o executor de terminal disponível para os scripts Node e o visualizador de imagens disponível para conferir os PNGs. Para a galeria, use o navegador disponível; se não houver, entregue o caminho e declare a limitação de validação visual.
- Para criar ou editar assets, use a ferramenta de geração de imagens disponível na sessão, conforme [image-gen](references/image-gen.md). Não presuma que ela salva automaticamente no projeto.
- Aproveite identidade, briefing e aprovações já fornecidos. Pergunte apenas pelo que falta. Quando precisar aguardar uma decisão de roteiro ou identidade, mostre primeiro uma proposta concreta e explique qual etapa depende dela; uma solicitação explícita de produção completa autoriza continuar com escolhas justificadas.
- A identidade da marca e as instruções do usuário prevalecem sobre as preferências visuais padrão destas referências. Para post único, use um slide; 4 a 7 é uma sugestão para carrosséis, não um limite obrigatório.

## 0. Primeira vez no projeto

1. `node <skill>/scripts/checar-setup.mjs`. Se faltar algo, instale as dependências locais da skill com `npm ci --prefix "<skill>"` e o navegador com `node "<skill>/node_modules/playwright/cli.js" install chromium`, dentro das permissões da sessão; rode a checagem novamente. Não altere as dependências do aplicativo do usuário. Se houver impedimento, informe o erro e o comando pendente.
2. Em uma pasta nova e vazia, se `carrosseis/marca/marca.json` não existe: `node <skill>/scripts/novo-projeto.mjs` e faça o onboarding.
   - Se a pasta já contém material, preserve os arquivos e complete só os ausentes em vez de executar o inicializador sobre ela.
   - Descubra pelo contexto se a marca já tem identidade (logo, cores, posts); pergunte somente se isso não estiver claro.
   - Já tem: siga `references/onboarding-marca-existente.md`.
   - Do zero: siga `references/onboarding-marca-nova.md`.
3. O onboarding termina com `marca/marca.json`, `marca/manual.md`, `GOSTO.md` iniciado e, se possível, uma série de assets (`references/image-gen.md`).

## 1. Antes de qualquer peça: ler

Sempre, nesta ordem: `carrosseis/GOSTO.md`, `carrosseis/marca/manual.md`, `carrosseis/carrosseis.json` (o que já existe e foi aprovado) e os `manifest.json` em `carrosseis/assets/`.

Ao sugerir temas, confira a galeria para não repetir o que já foi feito.

## 2. Roteiro primeiro

Siga `references/roteiro.md`. Proponha um bloco por slide, normalmente de 4 a 7 slides, com 2 ou 3 opções de capa. Antes do visual, obtenha aprovação do roteiro quando ela ainda for necessária; não repita essa etapa se o usuário já aprovou ou pediu execução completa.

## 3. Visual

Siga `references/visual.md`. Resumo:
- Parta das peças aprovadas na galeria, não de um layout novo a cada vez.
- Use `templates/slide-base.html` como esqueleto. Ele não lê o `marca.json`: troque à mão os tokens do `:root`, o `<link>` da fonte e o nome da marca pelos valores de `marca.json`.
- Um foco por slide. Assets grandes, integrados à composição. Prints reais quando falar de produto.
- Se nenhum asset da biblioteca servir, gere o asset com a ferramenta de imagens disponível conforme [image-gen](references/image-gen.md). Se a ferramenta não estiver disponível, entregue o prompt e avance nas partes independentes; não invente arquivos ou caminhos de saída.
- Nunca numere slides ("01 / 05"): a plataforma já mostra.

## 4. Render e checagem

1. Salve cada slide em `carrosseis/<id>/v<N>/slide-NN.html` com fontes e imagens resolvendo localmente.
2. Da raiz do projeto: `node <skill>/scripts/render.mjs <id>/v<N> --dir carrosseis` gera `slide-NN.png` (1080×1350) e `slide-NN-2x.png` (2160×2700).
3. **Abra e olhe cada PNG** antes de entregar: texto cortado, sobreposição, texto colado na borda, contraste, asset pequeno demais. Corrija e renderize de novo. Nunca entregue sem ter olhado.

## 5. Entrega

Siga `references/entrega.md`: registrar em `carrosseis.json` (com legenda), gerar o slider (`build-viewer.mjs`) e a galeria (`build-gallery.mjs`), e mostrar o resultado ao usuário.

## 6. Feedback vira gosto

Toda aprovação, recusa ou ajuste entra no `GOSTO.md` **na mesma rodada** (formato no próprio arquivo). Ao ajustar uma peça, mude **só o que foi pedido**. Revisão de um tema vira nova versão (`v2`, `v3`), nunca sobrescreve a anterior.

## Regras fixas

- Não publicar, agendar ou postar nada. A skill entrega arquivos.
- Não inventar interface de produto: use print real ou o markup do próprio projeto.
- Dados de exemplo são identificados como exemplo.
- Português do usuário na copy; sem jargão que o público da marca não usa.

## Origem

Adaptada de [guilhermealvestech/carrossel-skill](https://github.com/guilhermealvestech/carrossel-skill), commit `7df72b9adbdc8a1600cdd0132720878959e3fe2b`. Scripts, templates e referências preservados com ajustes de integração ao Codex. Licença MIT em [LICENSE](LICENSE).
