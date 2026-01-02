# Jogo do Casal

Jogo de tabuleiro digital para casal, feito em HTML/CSS/JS puro. Roda 100% no navegador, sem dependencias externas, e salva progresso no `localStorage`.

## Overview do site
- Experiencia em 3 telas (wizard): nomes + sexo -> nivel -> jogo.
- Tabuleiro estilo Monopoly (perimetro 7x7) com 24 casas e preview simples por casa.
- Dado fisico: o jogador rola fora do app e toca no numero correspondente.
- Desafio aparece no centro do tabuleiro, escolhido por sexo/alvo, e efeitos podem alterar a rodada.
- Salvamento automatico (retomar jogo ao abrir o site).

## Como jogar (usuario final)
1. Preencha os nomes e selecione o sexo de cada jogador.
2. Escolha o nivel (moderado, intenso, explicito).
3. Role um dado fisico e toque no numero 1-6 correspondente.
4. Leia o desafio no centro do tabuleiro e execute.
5. O jogo alterna automaticamente a vez apos o clique no dado.

## Estrutura do projeto
- `index.html`: HTML, CSS e JS em um unico arquivo.
- `README.md`: este guia de funcionamento e desenvolvimento.

## Arquitetura no `index.html`
Organizacao em modulos simples:
- `state`: estado global (nomes, nivel, posicoes, vez, efeitos, desafios).
- `Storage`: salvar/carregar no `localStorage`.
- `UI`: DOM, render do tabuleiro, botao do dado, modal.
- `Game`: regras (movimento, efeitos, vitoria, alternancia).

Fluxo principal:
1. `init()` registra eventos e verifica save.
2. `Game.gerarDesafiosCasas()` prepara o preview das casas e os pools embaralhados.
3. `Game.moverPeao()` move passo a passo e chama `processarCasa`.
4. `Game.processarCasa()` sorteia o desafio por alvo/sexo e aplica efeito.
5. `Game.proximoJogador()` alterna a vez logo apos a jogada.

## Tabuleiro e casas
- O tabuleiro e um grid 7x7.
- Apenas o perimetro e usado (24 casas).
- `UI.calcularPosicoesMopolyBoard()` define a ordem do percurso.
- Cada casa recebe tipo fixo e o desafio e sorteado ao cair:
  - `inicio`, `final`, `acao`, `bonus`, `penalidade`, `escolha`, `repeticao`, `surpresa`.

## Desafios e niveis
Os desafios ficam no objeto `jogo` no final do `index.html`:
- `jogo.moderado`, `jogo.intenso`, `jogo.explicito`.
- Cada nivel possui:
  - `tabuleiro`: sequencia de tipos de casa (24 itens).
  - `desafios`: listas de textos por tipo e alvo.

Para editar desafios:
1. Abra `index.html`.
2. Procure por `const jogo = {`.
3. Atualize os textos nas listas de `desafios`.

Regras importantes:
- `inicio` e `final` usam textos fixos.
- `surpresa` usa efeitos extras (ver `efeitosSurpresa`).
- Seleciona desafios com 60% especificos e 40% genericos, com fallback para `generico`.
- Se uma lista acabar, o jogo recicla os desafios daquele tipo/alvo.

Placeholders:
- `{jogador}` e `{parceiro}` inserem os nomes.
- `{ele_ela_jogador}` e `{ele_ela_parceiro}` inserem "ele/ela" conforme o sexo.

## Efeitos surpresa
Definidos em `const efeitosSurpresa`:
- `mover`: avanca/volta casas.
- `trocar`: troca posicao entre jogadores.
- `perder`: perde a vez.
- `extra`: joga novamente (nao alterna).

## Salvamento
- Chave do `localStorage`: `jogoCasal_save`.
- Salva: nomes, sexo, nivel, posicoes, vez, flags de perder a vez, preview das casas e pools.
- Botao "Continuar" aparece se houver save.
- Saves antigos sem sexo sao ignorados.

## Customizacao visual
Todo o CSS esta no topo do `index.html`.
Pontos principais:
- Tema por nivel: `.tema-moderado`, `.tema-intenso`, `.tema-explicito`.
- Variaveis CSS em `:root` controlam cores e espacamento.
- Tabuleiro responsivo com grid e `aspect-ratio`.

## Guia para novos desenvolvimentos
Sugestoes comuns e onde mexer:
- Ajustar layout do tabuleiro: `UI.renderizarTabuleiro()` e CSS `.tabuleiro-monopoly`.
- Alterar regras de movimento: `Game.moverPeao()` e `Game.aplicarEfeito()`.
- Mudar textos fixos: `Game.sortearDesafio()` para `inicio`/`final`.
- Adicionar novo tipo de casa:
  - CSS (cor e estilo), `iconesCasa`, `jogo.*.tabuleiro` e `jogo.*.desafios`.
  - Atualizar `UI.mostrarDesafio()` se precisar de estilo dedicado.
  - Ajustar `Game.processarCasa()` se houver regra especial.
- Ajustar salvamento: `Storage.salvar()` e `Storage.carregar()`.
 - Fluxo da vez e status no centro: `UI.atualizarInfoJogo()` e texto do bloco do centro.

## Desenvolvimento local
Basta abrir o `index.html` no navegador.
