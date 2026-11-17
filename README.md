# Carrossel para Codex

Skill para criar carrosséis e posts estáticos com a identidade da sua marca: roteiro, slides HTML editáveis, PNGs em alta resolução, legenda e galeria com comparação de versões.


## O que entrega

- Roteiro slide a slide, opções de capa e legenda.
- Identidade visual baseada nos materiais da marca e nas peças aprovadas.
- Slides HTML editáveis e PNGs em 1080 × 1350 e 2160 × 2700 no formato padrão.
- Galeria local, navegação entre slides, comparação de versões e download em ZIP.
- Registro de feedback em `GOSTO.md` para orientar as próximas peças.

A skill produz arquivos locais. Não publica nem agenda posts nas redes sociais.

## Instalação

Requisitos: Git, Node.js 22 ou superior e npm. O Playwright precisa do Chromium para renderizar os slides; `sharp` processa as imagens.

Clone este repositório:

```bash
git clone https://github.com/lzAmaral/carrossel-codex-skill.git
cd carrossel-codex-skill
```

Copie a skill para a pasta de skills do Codex. O comando abaixo interrompe a instalação se já houver uma pasta `carrossel`, para preservar sua instalação existente:

```bash
(
  set -eu
  skill_dir="${CODEX_HOME:-$HOME/.codex}/skills/carrossel"
  if [ -e "$skill_dir" ]; then
    echo "Já existe uma skill em $skill_dir. Compare as versões antes de atualizar."
    exit 1
  fi
  mkdir -p "$(dirname "$skill_dir")"
  cp -R skills/carrossel "$skill_dir"
  npm ci --prefix "$skill_dir"
  node "$skill_dir/node_modules/playwright/cli.js" install chromium
  node "$skill_dir/scripts/checar-setup.mjs"
)
```

Abra uma nova sessão do Codex para carregar a skill instalada.

## Como usar

No projeto em que deseja criar o material:

```text
$carrossel crie um carrossel de 5 slides sobre [tema] para [marca].
O público é [público] e o objetivo é [objetivo].
Use a identidade visual existente no projeto.
```

Também é possível pedir apenas o roteiro:

```text
$carrossel proponha três roteiros para apresentar meu produto.
Nesta etapa quero apenas o texto dos slides e as legendas.
```

O fluxo aproveita briefing, identidade e aprovações já fornecidos. Quando uma decisão de roteiro ou identidade ainda estiver pendente, apresenta uma proposta concreta para o usuário escolher. Um pedido explícito de produção completa permite continuar com escolhas justificadas.

Os arquivos são organizados em `carrosseis/` no projeto do usuário, ou em outro diretório informado:

```text
carrosseis/
├── marca/
├── assets/
├── GOSTO.md
├── carrosseis.json
├── index.html
└── tema/
    ├── v1/
    └── v2/
```

## Geração de imagens

Quando a sessão oferece uma ferramenta de geração ou edição de imagens, a skill pode usá-la para produzir assets da marca. Texto e composição final dos slides continuam em HTML.

A disponibilidade da ferramenta depende do ambiente. Sem ela, a skill prepara prompts e informa a limitação. Um arquivo só é registrado no projeto depois que sua gravação é confirmada.

## Adaptações para Codex

- Invocação por `$carrossel` e metadados em `agents/openai.yaml`.
- Uso do executor de terminal, visualizador de imagens e navegador disponíveis na sessão.
- Geração de assets dentro da sessão quando a ferramenta estiver disponível.
- Respeito ao briefing, às aprovações existentes e ao formato solicitado, incluindo posts de um slide.
- Dependências instaladas na pasta da skill, sem alterar o aplicativo do usuário.
- Preservação de pastas de trabalho existentes durante o início de um projeto.
- Substituição do nome da marca de origem também em títulos escritos em maiúsculas.

## Validação realizada

A versão adaptada passou pelo validador de estrutura de skills do Codex e por um teste local com duas versões de dois slides. Foram conferidos oito PNGs com dimensões 1x/2x, legenda, navegação, comparação lado a lado, download ZIP e geração da galeria, sem erros JavaScript no navegador. A comparação e a galeria mobile também foram inspecionadas visualmente.

A geração de imagens por IA e a publicação em redes sociais não fazem parte desse teste. O teste usou dados sintéticos e não acompanha este repositório.

## Estrutura

```text
skills/carrossel/
├── SKILL.md
├── agents/openai.yaml
├── references/
├── scripts/
├── templates/
├── package.json
├── package-lock.json
└── LICENSE
```

## Créditos e licença

Base: [guilhermealvestech/carrossel-skill](https://github.com/guilhermealvestech/carrossel-skill), commit `7df72b9adbdc8a1600cdd0132720878959e3fe2b`.

Copyright original: Guilherme Fortuna, 2026. Adaptação para Codex publicada por [lzAmaral](https://github.com/lzAmaral).

Distribuído sob a [licença MIT](LICENSE), com o aviso de copyright original preservado.
