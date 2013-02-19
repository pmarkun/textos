# Monitor Legislativo

Sistema que monitora e alerta sobre mudanças de curso no processo legislativo e na tramitação de matérias.

## Arquitetura

O Monitor Legislativo funciona também como um espelho dos repositórios oficiais de informação. Toda a informação que for coletada através das fontes oficiais deve ficar armazenada no banco do aplicativo.

	Crawler -> Banco -> APIs -> Visualizações

Os crawlers devem separar o processo de raspagem e coleta do processamento dos dados quando possível. Devem armazenar uma cópia bruta dos dados antes do processamento (sejam eles XML, HTML ou o que for). E devem criar um arquivo cabeçalho com uma descrição do conteúdo raspado, campos, data da raspagem e url base.

## Dados

O sistema trabalha e monitora duas entidades básicas:

	Projetos de Lei - Parlamentares


## Projeto de Lei
* Nome
* Apelido
(...)

## Parlamentar

A ideia aqui é trabalhar com a 'pessoa política', a 'figura pública' em toda sua extensão. Idealmente o sistema seria capaz de dar todo o histórico político de uma determinada pessoa em um determinado espaço de tempo. 

* Onde esta?
* De onde veio?
* O que fazia?
* Quem eram seus aliados e oponentes?
* Quais seus interesses?

Isso pode ser visualizado de várias formas mas, idealmente, precisamos de um sistema que permita visualizar esse histórico de ações como uma 'timeline'.

	{Paulo Teixeira} {deputado federal} do {Partido dos Trabalhadores} {pediu vistas} ao {PL ????} no dia {xx/xx/xxxx}.

* Nome -> Batismo (não único) String -> "LUIZ PAULO TEIXEIRA FERREIRA"
* Apelidos -> Lista (não único) Lista -> ["PAULO TEIXEIRA"]
* Cargos -> Lista [{ "cargo" : "Deputado Federal", "data_inicio" : Data, "data_fim" : Data, "partido" : "PT", "estado" : "SP" },]
