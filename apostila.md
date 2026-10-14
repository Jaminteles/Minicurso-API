# Apostila de exercícios: Introdução a APIs financeiras

Semana da Tecnologia · Jamínteles e Pedro Cesar

## Como usar esta apostila

Ao final da aula, você terá um painel com a Selic, o dólar, o IPCA e quatro ações da B3, feito com HTML e JavaScript puro em um único arquivo: `painel.html`.

Os exercícios seguem a ordem dos slides. Faça cada um quando o instrutor pedir.

### Pré-requisitos

- Um navegador atualizado (Chrome, Edge ou Firefox)
- Um editor de código (VS Code recomendado; o Bloco de Notas também serve)
- O arquivo `painel.html`, entregue no início da aula

### Como testar

1. Salve o arquivo no editor.
2. Abra o arquivo no navegador com `F5`.
3. Se um card ficar parado em `Carregando...`, pressione `F12`, abra a aba `Console` e leia o erro em vermelho.

Dica: o gabarito está ao final da apostila. Tente fazer cada exercício sozinho antes de consultar a resposta.

---

# Parte 1: o que é uma API

## Exercício 1: sua primeira API

Abra este endereço no navegador:

https://api.bcb.gov.br/dados/serie/bcdata.sgs.432/dados/ultimos/1?formato=json

### Perguntas

- A resposta é uma lista ou um objeto? Como você sabe?
- Quais são os nomes dos campos?
- Qual é o valor da Selic e de que data ele é?
- Troque `432` por `1` no endereço e pressione `Enter`. O que apareceu agora?

---

# Parte 2: a Selic e o dólar na página

## Exercício 2: a Selic na página

Abra o `painel.html` no editor e no navegador. Dentro da tag `script`, no fim do arquivo, escreva junto com o instrutor:

```js
async function mostrarSelic() {
  const url = 'https://api.bcb.gov.br/dados/serie/bcdata.sgs.432/dados/ultimos/1?formato=json';
  const resposta = await fetch(url);
  const dados = await resposta.json();
  const selic = dados[0]; // o primeiro item da lista
  document.getElementById('selic').innerHTML =
    'Selic: <b>' + selic.valor + '% ao ano</b> <small>(' + selic.data + ')</small>';
}

mostrarSelic();
```

Salve o arquivo e aperte `F5`.

Resultado esperado: o texto `Carregando...` do card da Selic deve virar um valor real.

## Exercício 3: o dólar sozinho

Copie a função `mostrarSelic()` inteira e mude o nome da cópia para `mostrarDolar()`.

Ajustes que você precisa fazer:

- Troque o código `432` por `1`
- Troque o id `'selic'` por `'dolar'`
- Troque o texto `"Selic: "` por `"Dólar: R$ "`
- Remova o texto `"% ao ano"`
- Chame `mostrarDolar();` no final do script, ao lado de `mostrarSelic();`

Dica: se o dólar aparecer no lugar da Selic, você esqueceu de trocar o id.

---

# Parte 3: ações da bolsa

## Exercício 4: onde está o preço?

As únicas quatro ações que funcionam sem cadastro são:

- PETR4
- VALE3
- MGLU3
- ITUB4

Abra este endereço no navegador:

https://brapi.dev/api/v2/stocks/quote?symbols=PETR4,VALE3,MGLU3,ITUB4

### Perguntas

- Qual é o nome da lista que guarda as ações?
- Escreva o caminho até o preço da PETR4, campo por campo.
- Ache o campo com a variação do dia. Como ele se chama? Você vai usar esse nome no exercício 7.

## Exercício 5: um card para cada ação

Escreva, junto com o instrutor, o código abaixo, logo após a função do dólar:

```js
async function mostrarAcoes() {
  const url = 'https://brapi.dev/api/v2/stocks/quote?symbols=PETR4,VALE3,MGLU3,ITUB4';
  const resposta = await fetch(url);
  const dados = await resposta.json();

  let html = '';
  for (const acao of dados.results) { // repete para cada ação
    const preco = acao.data.regularMarketPrice;
    html += '<div class="card">' + acao.symbol + ': <b>R$ ' + preco.toFixed(2) + '</b></div>';
  }
  document.getElementById('acoes').innerHTML = html;
}

mostrarAcoes();
```

Salve e aperte `F5`.

Resultado esperado: devem aparecer quatro cards, um para cada ação.

---

# Parte 4: com cara de painel

## Exercício 6: o IPCA

O IPCA é a inflação oficial. Ele é mais um card do Banco Central, igual ao do dólar.

### Tarefa

- Copie a função `mostrarDolar()`
- Mude o nome para `mostrarIpca()`
- Troque o código da série para `433`
- Troque o id para `'ipca'`
- Mude o texto para `"IPCA do mês"` com o símbolo `%`
- Não esqueça de chamar `mostrarIpca();` no final do script

Observação: o IPCA sai uma vez por mês, semanas depois do fim do mês. Se a data estiver de um ou dois meses atrás, isso está correto.

## Exercício 7: subiu ou caiu?

Dentro do `for` da função `mostrarAcoes()`, pegue a variação do dia e use um `if` para mostrar a ação em verde se ela subiu e em vermelho se caiu.

Ponto de partida:

```js
const variacao = acao.data.regularMarketChangePercent;
let cor = 'red';
if (variacao >= 0) {
  cor = 'green';
}
```

Em seguida:

- Coloque a variação no card dentro de um `span` com `style="color: ..."`
- Use a variável `cor`
- Mostre o número com `toFixed(2)` e com `%`

## Exercício 8: a fonte

No fim da página, dentro da tag `footer`, escreva de onde vêm os dados:

- Banco Central do Brasil
- brapi.dev

Além disso, avise que os dados são educativos e não são recomendação de investimento.

---

# Para levar

- Mostre a fonte e a data.
- Todo número precisa indicar de onde veio e quando foi coletado.
- Informar não é recomendar. Um painel mostra dados, mas não diz a ninguém o que comprar.

---

# Desafios extras

## Desafio 1: os últimos 5 dias do dólar

Troque `ultimos/1` por `ultimos/5` no endereço do dólar e use um `for` para mostrar os cinco valores, cada um com sua data.

## Desafio 2: o CDI

Encontre no portal de dados abertos do Banco Central o código da série do CDI e faça mais um card.

## Desafio 3: conversor de reais para dólar

Crie um campo em que a pessoa digite um valor em reais e veja quanto isso vale em dólares.

Dica: o valor do dólar vem como texto; converta com `Number()` antes de fazer a conta.

## Desafio 4: publique o painel

Coloque o arquivo no GitHub e ative o GitHub Pages, ou envie para a Vercel. Assim, o painel passa a ser um projeto do seu portfólio.

---

# Gabarito

## Respostas curtas

### Exercício 1

É uma lista (colchetes) com um objeto dentro (chaves). Os campos são `data` e `valor`. O valor e a data dependem do dia. Com o código `1`, aparece o dólar.

### Exercício 4

A lista se chama `results`. O caminho é: `results` → item da ação → `data` → `regularMarketPrice`. A variação do dia fica em `regularMarketChangePercent`.

### Exercício 3

```js
async function mostrarDolar() {
  const url = 'https://api.bcb.gov.br/dados/serie/bcdata.sgs.1/dados/ultimos/1?formato=json';
  const resposta = await fetch(url);
  const dados = await resposta.json();
  const dolar = dados[0];
  document.getElementById('dolar').innerHTML =
    'Dólar: <b>R$ ' + dolar.valor + '</b> <small>(' + dolar.data + ')</small>';
}
```

### Exercício 6

```js
async function mostrarIpca() {
  const url = 'https://api.bcb.gov.br/dados/serie/bcdata.sgs.433/dados/ultimos/1?formato=json';
  const resposta = await fetch(url);
  const dados = await resposta.json();
  const ipca = dados[0];
  document.getElementById('ipca').innerHTML =
    'IPCA do mês: <b>' + ipca.valor + '%</b> <small>(' + ipca.data + ')</small>';
}
```

### Exercício 7

```js
async function mostrarAcoes() {
  const url = 'https://brapi.dev/api/v2/stocks/quote?symbols=PETR4,VALE3,MGLU3,ITUB4';
  const resposta = await fetch(url);
  const dados = await resposta.json();

  let html = '';
  for (const acao of dados.results) {
    const preco = acao.data.regularMarketPrice;
    const variacao = acao.data.regularMarketChangePercent;

    let cor = 'red';
    let texto = 'caiu';
    if (variacao >= 0) {
      cor = 'green';
      texto = 'subiu';
    }

    html += '<div class="card">' + acao.symbol + ': <b>R$ ' + preco.toFixed(2) + '</b> '
      + '<span style="color: ' + cor + '">' + variacao.toFixed(2) + '% (' + texto + ')</span></div>';
  }
  document.getElementById('acoes').innerHTML = html;
}
```

### Exercício 8

```html
<footer>
  Fontes: Banco Central do Brasil e brapi.dev. Dados para fins educativos, não são recomendação de investimento.
</footer>
```

### O final do script

```js
mostrarSelic();
mostrarDolar();
mostrarIpca();
mostrarAcoes();
```

---

# Referências

- Banco Central do Brasil, portal de dados abertos: dadosabertos.bcb.gov.br
- brapi, documentação: brapi.dev/docs
- MDN Web Docs, Usando a Fetch API: developer.mozilla.org/pt-BR/docs/Web/API/Fetch_API/Using_Fetch
