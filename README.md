# Dashboard de vendas da hamburgueria

Dashboard estático para explorar as vendas demonstrativas fornecidas com a atividade. A página abre diretamente no navegador e também pode ser publicada no Render como Static Site.

## Abrir e usar

1. Abra `public/index.html` no navegador. A demonstração já aparece sem precisar carregar um CSV pela rede.
2. Use os campos de período, categoria e canal para filtrar os indicadores, gráficos e tabela. Esta base não possui uma coluna de região; por isso, o terceiro filtro e o gráfico correspondente usam **canal** (Delivery ou Balcão).
3. Use a busca da tabela para localizar registros. O botão de exportação baixa as linhas que correspondem aos filtros e à busca.
4. Para analisar sua própria base, clique em **Importar CSV** e selecione um arquivo no seu dispositivo. A leitura acontece localmente no navegador; o arquivo não é enviado a um servidor.

O arquivo `public/dados-exemplo.csv` é a mesma base usada na demonstração inicial. O arquivo original `vendas_hamburgueria.csv`, na raiz, foi preservado.

## Colunas e validação

A base fornecida contém: `id_venda`, `data`, `dia_semana`, `periodo`, `produto`, `categoria`, `quantidade`, `preco_unitario`, `forma_pagamento`, `canal`, `tempo_espera_min` e `avaliacao`. Cada linha é uma venda de um produto. As datas estão em `AAAA-MM-DD`, e os valores usam ponto decimal, sem símbolo monetário.

Para importar outra base, inclua os campos `id_venda`, `data`, `categoria`, `quantidade` e `preco_unitario`. O campo `produto` é opcional. Para comparar regiões, use uma coluna `regiao` (também são reconhecidos `region`, `estado` e `uf`). Se ela não existir, o dashboard usa `canal`, quando disponível. Caso não haja nenhuma dessas colunas, a comparação por região/canal fica indisponível.

Linhas sem ID, data, categoria, quantidade ou preço, com data inválida ou com quantidade/preço não numérico ou negativo são descartadas e contabilizadas na mensagem após a importação. Zero é um valor válido e não é confundido com dado ausente. Campos não usados nos indicadores, como `avaliacao`, podem ficar vazios sem descartar a venda. IDs repetidos permanecem na tabela, pois cada linha pode ser uma venda de um produto; para o indicador de vendas, cada ID conta uma vez.

## Indicadores

- **Faturamento por linha:** `quantidade × preco_unitario`.
- **Faturamento total:** soma do faturamento das linhas após os filtros.
- **Quantidade de vendas:** número de `id_venda` distintos nas linhas filtradas.
- **Unidades vendidas:** soma das quantidades filtradas.
- **Ticket médio:** faturamento total ÷ quantidade de vendas distintas. Se não houver vendas, o painel mostra R$ 0,00.

O gráfico mensal acompanha a evolução do faturamento; os gráficos de categoria, canal/região e período do dia comparam receitas. A página também mostra vendas por forma de pagamento e por dia da semana, além da avaliação média por categoria quando essas colunas estão disponíveis. Notas ausentes ou fora da faixa de 0 a 5 são ignoradas apenas na média. Os filtros de período, categoria e canal/região atualizam os indicadores, gráficos e tabela.

A interface usa uma paleta retrô noturna, com fundo preto, cartões escuros e vários tons de verde. Gráficos opcionais ficam ocultos quando o CSV importado não possui as respectivas colunas.

## Bibliotecas

O dashboard usa HTML, CSS e JavaScript puro, com Chart.js 4.5.1 e Papa Parse 5.7.0 carregados por CDN com versões fixadas. É necessário ter acesso à internet para carregar essas bibliotecas. A amostra já incorporada permite abrir a demonstração sem `fetch`, mas os gráficos dependem do Chart.js.

## Publicar no Render pelo GitHub

Envie o projeto para um repositório GitHub e, no Render, crie um novo **Static Site** conectado a esse repositório. Configure:

- **Tipo de serviço:** Static Site
- **Branch:** a branch que contém o projeto (por exemplo, `main`)
- **Root Directory:** vazio
- **Build Command:** `echo "Site pronto"`
- **Publish Directory:** `public`

Salve e aguarde a publicação. O Render fornecerá o endereço público do site. Cada atualização enviada para a branch configurada dispara uma nova publicação.
