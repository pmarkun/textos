# Jornalismo Hacker 101 - Encontro 1

## Baixando os dados
Para esse exercício vamos utilizar a primeira parcial de doações de campanha do TSE - disponíveis [aqui](http://www.tse.jus.br/eleicoes/repositorio-de-dados-eleitorais)

Vamos começar criando uma espaço de trabalho:

`$ mkdir encontro1
`$ cd encontro1

Agora vamos baixar os arquivos:

	$ wget http://agencia.tse.jus.br/estatistica/sead/odsele/prestacao_contas/primeira_parcial_2012.zip

Dezipando o arquivo:

	$ unzip primeira_parcial_2012.zip

Isso vai criar uma série de arquivos na pasta 2012:

	creating: 2012/
	 inflating: 2012/DespesasCandidatos.csv  
	 inflating: 2012/DespesasComites.csv  
	 inflating: 2012/DespesasPartidos.csv  
	 inflating: 2012/ReceitasCandidatos.csv  
	 inflating: 2012/ReceitasComites.csv  
	 inflating: 2012/ReceitasPartidos.csv  

Por enquanto vamos trabalhar apenas com as despesas dos candidatos - boa parte das operações de receita e despesa são viabilizadas através dos comites e dos partidos e fica bem mais difícil de rastrear.

Para visualizar se o arquivo esta correto podemos usar o comando ``head`` para ler as primeiras duas linhas do arquivo ``DespesasCandidatos.csv``

	$ cd 2012
	$ head -n 2 DespesasCandidatos.csv
	"Data e hora";"Sequencial Candidato";"UF";"N�mero UE";"Munic�pio";"Sigla  Partido";"N�mero candidato";"Cargo";"Nome candidato";"CPF do candidato";"Tipo do documento";"N�mero do documento";"CPF/CNPJ do fornecedor";"Nome do fornecedor";"Data da despesa";"Valor despesa";"Tipo despesa";"Descri�ao da despesa"
	"23/08/201215:28:44";"100000000122";"MA";"09210";"S�O LU�S";"PSTU";"16789";"Vereador";"LUIZ CARLOS NOLETO CHAVES";"23843616353";"Nota Fiscal";"001307";"06331045000125";"CORINGRA-COROAT� IND�STRIA GR�FICA LTDA";"27/07/2012";"300";"Publicidade por materiais impressos";"MATERIAIS GRAFICOS"

Possívelmente (mas vai depender de algumas configurações do seu computador) o arquivo vai aparecer como acima. Com alguns caracteres estranhos no lugar dos acentos.

## Corrigindo os acentos

Esse é um problema recorrente quando estamos trabalhando com tabelas e arquivos de fontes diversas. A maioria dos sistemas e programas que vamos utilizar no curso estão preparados para entender o padrão [utf-8](http://pt.wikipedia.org/wiki/UTF-8). Aqui no Brasil por muito tempo se utilizou o padrão [iso-8859-1](http://pt.wikipedia.org/wiki/ISO_8859-1) e vários sites e sistemas (como o do TSE) continuam utilizando esse formato.

Se alguém quiser entender melhor sobre encodings tem uma série de textos sobre o assunto na rede. Eu recomendo [esse aqui](http://kunststube.net/encoding/). Mas o processo de 'olhar o arquivo e ver se esta engraçado' funciona também.

Para corrigir isso, podemos usar uma ferramenta do próprio csvkit. O ``in2csv`` é um conversor de arquivos para o formato csv. Com ele você consegue converter outros formatos (XLS, GeoJSON, tsv, etc) para o padrão CSV mas ele também faz um belo trabalho de padronizar o próprio CSV.

	$ in2csv -e iso-8859-1 DespesasCandidatos.csv > DespesasCandidatos-utf8.csv
	$ head -n 2 DespesasCandidatos-utf8.csv
	Data e hora,Sequencial Candidato,UF,Número UE,Município,Sigla  Partido,Número candidato,Cargo,Nome candidato,CPF do candidato,Tipo do documento,Número do documento,CPF/CNPJ do fornecedor,Nome do fornecedor,Data da despesa,Valor despesa,Tipo despesa,Descriçao da despesa
	23/08/201215:28:44,100000000122,MA,09210,SÃO LUÍS,PSTU,16789,Vereador,LUIZ CARLOS NOLETO CHAVES,23843616353,Nota Fiscal,001307,06331045000125,CORINGRA-COROATÁ INDÚSTRIA GRÁFICA LTDA,2012-07-27,300,Publicidade por materiais impressos,MATERIAIS GRAFICOS

Agora sim. De quebra ele também trocou os separadores (de ; por ,) e removeu as aspas duplas.

## Redirecionamento de saída

Importante notar que ao invés de sobreescrever o arquivo eu criei um arquivo novo chamado ``DespesasCandidatos-utf8.csv``.

Para isso eu usei um 'output redirector'. O caracter ``>`` faz com que a saída do programa seja direcionado para um arquivo - sobreescrevendo-o. Se eu tivesse usado ``>>`` a saída do programa seria adicionado ao final do arquivo ao invés de sobreescrever.
 
(Tente rodar o comando acima sem o > para ver o que acontece =)

## Investigando o dataset com o csvcut

Agora a gente começa de fato a brincar com as ferramentas do csvkit. A primeira coisa a fazer é entender quais as colunas disponíveis no dataset. Para isso podemos usar o ``csvcut``.

	$ csvcut -n DespesasCandidatos-utf8.csv
	  1: Data e hora
	  2: Sequencial Candidato
	  3: UF
	  4: Número UE
	  5: Município
	  6: Sigla  Partido
	  7: Número candidato
	  8: Cargo
	  9: Nome candidato
	 10: CPF do candidato
	 11: Tipo do documento
	 12: Número do documento
	 13: CPF/CNPJ do fornecedor
	 14: Nome do fornecedor
	 15: Data da despesa
	 16: Valor despesa
	 17: Tipo despesa
	 18: Descriçao da despesa

A opção -n simplesmente faz o sistema imprimir a primeira linha do arquivo.

Isso nos mostra quais os campos, mas não mostra que tipo de informação eles contém. Vamos usar o csvcut de novo para visualizar apenas colunas específicas:

	$ csvcut -c 14,16 DespesasCandidatos-utf8.csv | head -n 5
	Nome do fornecedor,Valor despesa
	CORINGRA-COROATÁ INDÚSTRIA GRÁFICA LTDA,300
	L O SIMOES BARBOSA - ME,1650
	MARIA LUCIA XAVEIR PEREIRA,2000
	COMERCIO VAREJISTA DE COMBUSTIVEIS PARA VEICULOS 	AUTOMOTORES,3084

A opção -c permite escolher as colunas que queremos utilizar. Usamos um pipe | para passar esse arquivo pelo comando ``head`` para que só sejam mostrados os 5 primeiros resultados.

## Pipe

Os pipes ``|`` são um conceito chave no terminal e para usar o csvkit. Com ele a gente consegue fazer uma cadeia de operações em um único comando - economizando etapas e fazendo o processo ser mais eficiente.

Se quizessemos extrair um arquivo apenas Nome, CNPJ e Valor das 500 primeiras linhas de doações poderiamos combinar o pipe com o redirecionador de saída ``>``:

	$ csvcut -c 13,14,16 DespesasCandidatos-utf8.csv | head -n 500 > 500despesas.csv

## Estatísticas com csvstat


