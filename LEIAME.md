# PEA! — Ervilhas contra o Caos — Protótipo Web (v0.4)

## Novidades da v0.4 (a partir do seu feedback)

- **Painel lateral novo**: som, indicador de turno e quem é Ervilha/Caos saíram da barra do topo e agora ficam numa coluna fixa à esquerda — libera espaço horizontal e não atrapalha mais a visão do tabuleiro. O histórico de jogadas (log) também foi movido para lá.
- **Tabuleiro e cartas bem maiores**: aumentei o tamanho de todas as cartas (mão, Linha de Evolução, pilhas) e espacei melhor os elementos horizontalmente para aproveitar a tela.
- **Novas artes aplicadas**: a imagem da clareira espiral (que você mandou) está no tabuleiro, e a imagem da floresta enfrentando o exército roxo está no menu inicial e na tela de escolha de lado.
- **Música de verdade**: a trilha que você enviou (`PEA_Ervilhas_Contra_o_Caos_Trilha_Game.wav`, comprimida para MP3 para não pesar o arquivo) toca no menu inicial, já ligada por padrão — dá pra desativar no botão de som a qualquer momento. Dentro da partida, uma trilha ambiente (sintetizada) continua tocando.
- **🐛 Bug corrigido — vitória sem cadeia completa**: agora só é possível invocar um nível se **todos** os níveis anteriores estiverem ocupados (não só o imediatamente anterior), e a vitória só é validada se a Linha de Evolução estiver completa do Nível 0 ao 3, sem nenhum buraco. Se um efeito do adversário remover uma carta no meio do caminho, fica bloqueado invocar ou vencer até você repor aquele nível.
- **🐛 Bug corrigido — Substituir não ativava o efeito de Invocar**: agora, ao substituir uma carta por outra do mesmo nível, o Efeito de Invocação da nova carta é sempre ativado (a substituição conta como colocar a carta em campo).



## Novidades da v0.3 (a partir do seu feedback)

- **Escolha de lado movida para uma tela própria**: ao clicar em JOGAR, agora abre uma tela dedicada de "Escolha seu lado" (com fundo temático e cartão para Ervilha / Sortear / Caos), em vez de um popup pequeno em cima do menu.
- **Cartas maiores e mão em leque**: as cartas da mão agora se abrem em leque (como um baralho na mão de verdade), e todas as cartas (mão, Evolução, Descarte) aumentaram de tamanho.
- **Efeitos visuais ao invocar/descartar/ativar Fenômeno**: agora há um "flash" colorido na tela e um pulso de brilho na carta/pilha correspondente, junto com um som (se a música estiver ligada).
- **Fundo do tabuleiro mais evidente**: a arte da caixa (Ervilhas à esquerda, Caos à direita) agora aparece com mais presença atrás do tabuleiro, mantendo a legibilidade das cartas.
- **Correção de bug de clique**: no leque de cartas, passar o mouse sobre uma carta parcialmente coberta por outra agora sempre a traz corretamente para frente (antes podia ficar presa atrás da vizinha).
- Polimento geral: botões com gradientes mais ricos, popups com borda dupla e sombra mais profunda, leque com "respiro" ao passar o mouse.



## Novidades da v0.2 (a partir do seu feedback)

- **Música e efeitos sonoros**: tudo sintetizado no navegador via Web Audio API (sem arquivos externos, então não pesa no arquivo). Botão 🔈/🔊 no canto superior direito do menu e do tabuleiro — o áudio só começa após você clicar nele (exigência dos navegadores).
- **Identidade visual do jogo aplicada**: fundo do menu e do tabuleiro agora usam a arte da caixa (recortada e adaptada), e o menu ganhou uma fileira de cartas em destaque.
- **Popups de efeito bem maiores e mais legíveis**: clique em qualquer carta — na mão, na Linha de Evolução (sua ou da IA), no topo do Descarte ou num Fenômeno ativo — e uma ficha grande abre mostrando a arte, o texto de Invocar/Descartar (ou Efeito) e o flavor text.
- **Corrigido: apenas 1 Fenômeno ativo por vez.** Antes, vários podiam se acumular ao mesmo tempo, o que gerava efeitos conflitantes e confusão (provavelmente a causa do que você notou sobre invocações travando). Agora, ativar um novo Fenômeno manda o anterior para o Descarte automaticamente.
- **IA não descarta mais por engano sua carta de Nível 3 objetivo**, a menos que seja literalmente a única carta na mão dela. Antes, o critério de "pior carta" não dava peso suficiente a isso.
- Sobre "não consigo invocar Nível 3": isso não era um bug de lógica (testei diretamente e o motor permite invocar Nível 3 assim que você tem uma carta de Nível 2 em campo, a carta de Nível 3 em mão, e ainda não usou sua Invocação no turno) — o gargalo real era o acúmulo de Fenômenos conflitantes, já corrigido acima. Lembrete: por regra, você só pode Invocar **1 vez por turno** (a carta Fenômeno "Dias de Sol" é a exceção que libera 2) — subir de Nível 0 até Nível 3 leva no mínimo 3–4 turnos.



## O que é este entregável

- **`pea_prototipo.html`** — o jogo em si. Um único arquivo HTML autocontido (cartas, CSS e motor de jogo embutidos). Basta abrir no navegador — não precisa de servidor nem internet.
- **`pea_cards_dados.json`** — a base de dados estruturada das 72 cartas (48 personagens + 12 Fenômenos + 12 Ativação Rápida/Item), extraída manualmente da leitura de todas as imagens do ZIP fornecido.

## Como as 72 cartas foram lidas

Todas as imagens do ZIP foram abertas e lidas individualmente (nome, subtítulo, facção, nível, texto de Invocação, texto de Descarte, texto de Fenômeno/Efeito e flavor text). Nenhum efeito foi inventado — tudo vem literalmente do texto das cartas. O arquivo `pea_cards_dados.json` é a representação estruturada resultante, no formato:

```json
{
  "id": "loop",
  "tipo": "personagem",
  "nome": "Loop",
  "subtitulo": "Ervilha Curiosa",
  "faccao": "ervilha",
  "nivel": 0,
  "imagem": "pea_loop_nivel_0_logo_official_63x88mm_300dpi.png",
  "invocar": { "texto": "...", "acoes": [ { "a": "look_top", "qty": 3, "mode": "keep_order" } ] },
  "descartar": { "texto": "...", "acoes": [ { "a": "draw", "qty": 1, "target": "self" } ] },
  "flavor": "..."
}
```

Cada efeito em texto foi convertido em uma sequência de **ações executáveis** (`acoes`), interpretadas por um motor genérico (`runActions` em `game.js`) — exatamente o modelo pedido no prompt original (`AÇÃO → COMPRAR → QUANTIDADE: 1`, etc.).

## Decisões de design tomadas (a seu pedido, "use seu melhor julgamento")

1. **Vexa** (invocação sem destino declarado) → destino assumido: Descarte.
2. Remover a carta de **Nível 0** só é possível quando ela é o topo daquela posição; a Linha de Evolução não desmorona quando isso acontece — o slot fica vazio e pode ser reocupado depois.
3. **1 Invocação por turno é o padrão** (confirmado pela existência da carta "Dias de Sol", que dá uma segunda).
4. O descarte-custo do Fenômeno **"Período de Seca"** não ativa o Efeito de Descarte — só o descarte obrigatório de fim de turno ativa efeitos, por padrão.
5. **Revelação** e **Curioso** (mesmo efeito, arte diferente) foram tratadas como duas cartas normais e distintas no baralho.
6. Confirmado: as 12 cartas de Ativação Rápida/Item são exclusivas da Ervilha nesta coleção — arquitetura pronta para receber equivalentes de Caos no futuro sem refatoração.
7. **Kron** mantém o autoalvo em seu Efeito de Descarte (atipico, mas assumido intencional/balanceamento).

## Arquitetura (conforme pedido)

Embora entregue como um único HTML por conveniência de compartilhamento, o código internamente está separado em camadas (você pode pedir os arquivos individuais também: `game.js`, `ui.js`, `main.js`, `style.css`, `cards.json`):

- **Cartas** → `cards.json` (dados puros, sem lógica).
- **Regras/Motor de efeitos** → `game.js`: estado do jogo, ações de turno (Comprar/Invocar/Substituir/Ativar Fenômeno/Ativar Item), pilha de Evolução com empilhamento por substituição, descarte obrigatório com ativação condicional do Efeito de Descarte, verificação de vitória, e o **interpretador genérico de ações** (`runOneAction`) que executa qualquer efeito descrito no JSON.
- **IA** → heurísticas simples em `game.js` (prioriza invocar a facção-objetivo, usa itens/fenômenos disponíveis, descarta a carta de menor valor).
- **Interface** → `ui.js` (renderização do tabuleiro, mãos, pilhas, Linha de Evolução com cartas empilhadas visualmente) + `main.js` (menu, "Como Jogar", galeria de cartas, seleção de lado).

## Testado automaticamente

O protótipo foi testado com um navegador headless (Playwright) simulando múltiplas partidas completas — incluindo compra, invocação, substituição, descarte obrigatório com efeitos, esgotamento e reembaralhamento do baralho, e **vitória detectada corretamente** ao atingir Nível 3 da facção-objetivo. Nenhum erro de console/JS apareceu nos testes.

## Simplificações conscientes desta primeira versão (para deixar claro o que ainda não é 100% do jogo físico)

- **Cartas de "Resposta"** (8 das 12 Ativação Rápida) são reativas por natureza (ex: "quando o adversário for invocar, cancele"). Neste protótipo, o motor já tem os **pontos de interrupção** corretos (antes de uma invocação, antes de remover uma carta da Evolução, antes de resolver o descarte obrigatório, antes do adversário comprar), e oferece a carta de resposta automaticamente quando ela está na mão de quem pode reagir — mas a IA decide usá-la com uma heurística simples (75% de chance), em vez de uma análise estratégica completa.
- **Limites por turno** (1 Invocação, 1 Substituição, 1 Fenômeno, 1 Item, exceto quando um Fenômeno like "Dias de Sol" altera isso) foram definidos como padrão razoável, já que o texto de regras original não especifica quantidade explicitamente para todas as ações.
- **"Desvio"** (redirecionar o alvo de um efeito para "outro alvo válido") foi implementado de forma simplificada como um cancelamento, já que a escolha de um "alvo alternativo válido" exigiria um sistema de re-alvo genérico mais complexo — sinalizando aqui para uma iteração futura.
- Efeitos de "olhar e reorganizar" cartas do topo do baralho são interativos para o jogador humano (você realmente escolhe a ordem) e automáticos/heurísticos para a IA.

## Próximos passos sugeridos

- Refinar a IA (hoje é funcional, mas não estratégica) — dificuldade ajustável.
- Sistema de re-alvo completo para "Desvio".
- Modo "Escolher lado" com mais controles de partida (melhor-de-3, etc.).
- Sons e animações mais ricas (hoje as transições são simples).
- Extrair os arquivos `game.js`/`ui.js`/`main.js`/`style.css` separadamente se for evoluir para um projeto multi-arquivo (o motor já foi escrito modularmente pensando nisso).
