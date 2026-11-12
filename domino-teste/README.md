# Dominó para 2 Jogadores

Implementação das regras do Dominó para 2 jogadores em JavaScript, com casos de teste usando o test runner nativo do Node.js (`node:test`).

## Como rodar

Precisa do Node.js 18 ou mais novo. Não precisa instalar nenhuma dependência.

```bash
npm test
```

(ou `node --test domino.test.js`)

## Arquivos

| Arquivo | Conteúdo |
|---|---|
| `domino.js` | Regras do jogo |
| `domino.test.js` | Casos de teste (CT01 a CT28) |

---

## Regras do jogo

### Jogadores

- São **2 jogadores**, com IDs 0 e 1.
- O jogador 0 sempre começa.

### Conjunto de peças

- São **28 peças** com combinações de [0|0] até [6|6].
- Cada par de valores aparece exatamente uma vez.

### Distribuição

- Cada jogador recebe **7 peças**.
- As **14 peças restantes** formam o estoque.

### Jogadas

- Cada jogador joga na sua vez. Jogar fora da vez é proibido.
- Só é possível jogar uma peça que está na própria mão.
- A primeira peça pode ser jogada livremente e define as extremidades da mesa.
- Peças seguintes devem **encaixar em uma das extremidades**: o valor de um dos lados deve coincidir com o número da ponta esquerda ou direita da mesa.
- Se o jogador não tiver peça que encaixe, deve **comprar do estoque** até encontrar uma que sirva.
- Se o estoque estiver vazio e não puder jogar, o jogador **passa a vez**.

### Vitória

- Vence quem **ficar sem peças** primeiro.
- Se o jogo **travar** (ninguém pode jogar e o estoque está vazio), vence quem tiver **menor soma de pontos** na mão.
- Em caso de empate na soma, ninguém vence.

---

## Casos de teste

### Preparação da partida

| Nº | Caso | Resultado esperado |
|---|---|---|
| CT01 | Criar partida | 2 jogadores com IDs 0 e 1 |
| CT02 | Validar conjunto | 28 peças sem repetição |
| CT03 | Embaralhar | Mesmas 28 peças em outra ordem |
| CT04 | Distribuir | 7 peças por jogador e 14 no estoque |
| CT05 | Peça repetida no baralho | Distribuição rejeitada |

### Peças

| Nº | Caso | Resultado esperado |
|---|---|---|
| CT06 | Encaixe verdadeiro | true quando um lado coincide |
| CT07 | Encaixe falso | false quando nenhum lado coincide |
| CT08 | Total de pontos | Soma dos dois lados de todas as peças na mão |

### Jogadas

| Nº | Caso | Resultado esperado |
|---|---|---|
| CT09 | Jogar fora da vez | Jogada rejeitada |
| CT10 | Jogar peça que não está na mão | Jogada rejeitada |
| CT11 | Primeira peça | Define extremidades esquerda e direita |
| CT12 | Encaixe válido na direita | Extremidade direita atualizada |
| CT13 | Encaixe válido na esquerda | Extremidade esquerda atualizada |
| CT14 | Encaixe inválido na direita | Jogada rejeitada |
| CT15 | Encaixe inválido na esquerda | Jogada rejeitada |
| CT16 | Encadeamento de três peças | Extremidades corretas após três jogadas |

### Estoque

| Nº | Caso | Resultado esperado |
|---|---|---|
| CT17 | Comprar do estoque | Peça entregue ao jogador e estoque reduz em 1 |
| CT18 | Estoque vazio | Compra retorna null |
| CT19 | podePjogar em mesa vazia | Sempre true |
| CT20 | Peça que encaixa disponível | podePjogar retorna true |
| CT21 | Nenhuma peça encaixa | podePjogar retorna false |

### Vitória e travamento

| Nº | Caso | Resultado esperado |
|---|---|---|
| CT22 | Passar a vez | Permitido quando não pode jogar e estoque vazio |
| CT23 | Passar com peça disponível | Passagem rejeitada |
| CT24 | Última peça jogada | Partida encerrada e vencedor definido |
| CT25 | Partida encerrada | Novas jogadas rejeitadas |
| CT26 | Jogo travado | Detectado quando ninguém pode jogar e estoque vazio |
| CT27 | Vencedor no travamento | Menor soma vence; empate retorna null |
| CT28 | Partida completa simulada | Jogador 0 vence ao esvaziar a mão |
