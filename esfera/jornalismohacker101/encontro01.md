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

Outra ferramenta disponível no csvkit é o ``csvstat`` que faz as vezes do comando ``summary`` da linguagem de programação para estatísticas R e mostra uma série de informações interessantes sobre os nossos dados.

	$ csvcut -c 14, 16 DespesasCandidatos-utf8.csv | csvstat
	  1. Nome
		<type 'unicode'>
		Nulls: True
		Unique values: 19551
		5 most frequent values:
			GRAFICA:	7790
			AUTO:	5133
			POSTO:	4947
			JOSE:	2572
			MARIA:	2184
		Max length: 19851
	  2. do
		<type 'unicode'>
		Nulls: True
		Unique values: 15264
		5 most frequent values:
			DE:	7807
			E:	6924
			POSTO:	5267
			GRAFICA:	3648
			&:	3602
		Max length: 8165
	  3. fornecedor,Valor
		<type 'unicode'>
		Nulls: True
		Unique values: 24444
		5 most frequent values:
			DE:	15501
			E:	7843
			DA:	7244
			DOS:	3462
			EDITORA:	2943
		Max length: 15507
	  4. despesa
		<type 'unicode'>
		Nulls: True
		Unique values: 24204
		5 most frequent values:
			E:	5464
			DE:	4619
			LTDA:	3911
			-:	2933
			EDITORA:	2626
		Max length: 28

Estou escrevendo esse documento transcrevendo a experiência da linha de comando - com todos os seus erros e acertos.
Nesse momento, o que deveria ter aparecido era um sumário das linhas 14 (Nome do fornecedor) e 16 (Valor da despesa) ao invés disso o sistema aparentemente endoidou e desentendeu as linhas!

Fiquei tentando debuggar o problema algum tempo e estou certo de que é um bug na padronização inicial que fizemos com o ``in2csv`` - provavelmente um desentendimento entre o separador e o fato do arquivo do TSE ter todos os campos envolvidos em aspas duplas em alguma das linhas do arquivo. Isso porque se pegarmos apenas as primeiras 50 linhas, tudo fica ok:

	$ csvcut -c 14,16 DespesasCandidatos-utf8.csv | head -n 50 | csvstat
	  1. Nome do fornecedor
		<type 'unicode'>
		Nulls: False
		Unique values: 47
		5 most frequent values:
			BRASIL EDITORA E COMUNICAÇÃO VISUAL LTDA.:	3
			DOMINGOS SOBRINHO SANDES BARROS:	1
			RUDSON DOS SANTOS MACIEL:	1
			GRÁFICA E EDITORA NORTESUL LTDA:	1
			AZEITAO DERIVA. DE PETROLEO LTDA:	1
		Max length: 60
	  2. Valor despesa
		<type 'int'>
		Nulls: False
		Min: 15
		Max: 89614
		Sum: 317055
		Mean: 6470.51020408
		Median: 2000
		Standard Deviation: 14744.8120909
		Unique values: 37
		5 most frequent values:
			10000:	5
			3000:	5
			1500:	2
			200:	2
			300:	2
	Row count: 49

Mas estamos desviando do assunto. Já que o in2csv não funcionou corretamente para esse caso, resolvi de uma forma mais fácil. Abri o OpenOffice, escolhi o encoding Europa Ocidental ``ISO-8859-1`` na tela de abertura, o separador ``;`` e mandei abrir o arquivo DespesasCandidatos.csv.

Depois cliquei em Salvar, escolhi a checkbox 'Editar as configurações do filtro' e selecionei ``UTF-8`` como codificação e ``,`` como separador - chamei o arquivo de DespesasCandidatos-clean.csv

E de volta pra linha de comando:

	$ csvcut -c 14,16 DespesasCandidatos-clean.csv | csvstat
	  1. Nome do fornecedor
		<type 'unicode'>
		Nulls: True
		Unique values: 79329
		5 most frequent values:
			EDIOURO GRAFICA E EDITORA LTDA:	206
			ODILON TADEU PROBST:	151
			AUTO POSTO NOSSA SENHORA APARECIDA LTDA:	150
			MIRIAM RODRIGUES SANTANA:	144
			JARDSON EDSON GUEDES DA SILVA ALMEIDA:	138
		Max length: 70
	  2. Valor despesa
		<type 'int'>
		Nulls: False
		Min: -50063
		Max: 20042609
		Sum: 1345229744
		Mean: 8209.72881397
		Median: 400.0
		Standard Deviation: 106083.376838
		Unique values: 15013
		5 most frequent values:
			300:	6245
			100:	5826
			200:	5053
			50:	4432
			150:	3606
	Row count: 163858

Há, agora funcionou. E o csvstats deu algumas informações interessantes pra nossa exploração - a empresa que mais recebeu pagamentos (em quantidade e não na soma do valor) foi a Ediouro, e das 163858 doações - existem apenas 79329 empresas/pessoas prestando serviços.

O csvstats tenta interpretar também o tipo de informação disponível - ele entendeu que o campo de Valor era um valor numérico e deu uma série de consolidações estatísticas.

A maior despesa foi no valor de 20mi! A despesa média de 400 reais. E a despesa mais frequente de 300 reais...

## Procurando colunas com csvgrep

O ``csvgrep`` é uma ferramenta do kit que permite a gente fazer perguntas diretamente pros dados - ele busca dentro do csv por parametros específicos.

Vamos buscar os 10 primeiros pagamentos feito para a EDIOURO então para entender melhor o que isso significa:

	$ csvcut -c 14,16,18 DespesasCandidatos-clean.csv | csvgrep -c 1 -m "EDIOURO GRAFICA E EDITORA LTDA" | head -n 10
	EDIOURO GRAFICA E EDITORA LTDA,7431.25,FOLHETOS
	EDIOURO GRAFICA E EDITORA LTDA,3445,FOLHETOS
	EDIOURO GRAFICA E EDITORA LTDA,3445,FOLHETOS
	EDIOURO GRAFICA E EDITORA LTDA,3445,FOLHETOS
	EDIOURO GRAFICA E EDITORA LTDA,3445,FOLHETOS
	EDIOURO GRAFICA E EDITORA LTDA,1722.5,FOLHETOS
	EDIOURO GRAFICA E EDITORA LTDA,2756,FOLHETOS
	EDIOURO GRAFICA E EDITORA LTDA,4134,FOLHETOS
	EDIOURO GRAFICA E EDITORA LTDA,2067,FOLHETOS

Folhetos!
O parametro ``-c`` do csvgrep escolhe a coluna em que vamos buscar (lembrando que o retorno do csvcut é um csv em si, então ali é a primeira coluna, pq foi assim que decididimos no primeiro parametro do ``-c`` do csvcut. Meio confuso, mas brinca ae.)

O parametro ``-m`` faz uma busca pelo valor naquela coluna.

Também podemos usar o parametro ``-r``` que permite buscas com expressões regulares... deixando a coisa beeem mais poderosa.

Por exemplo, podemos buscar (e tirar estatísticas) de todos os pagamentos para 'EDITORA':

	$ csvcut -c 14,18 DespesasCandidatos-clean.csv | csvgrep -c 1 -r ".* EDITORA .*" | csvstat
	  1. Nome do fornecedor
		<type 'unicode'>
		Nulls: False
		Unique values: 2542
		5 most frequent values:
			EDIOURO GRAFICA E EDITORA LTDA:	206
			STYLO GRAFICA E EDITORA LTDA:	103
			GRAFICA E EDITORA IARA LTDA:	73
			GRAFICA E EDITORA HOFFMANN LTDA:	70
			BRASPOR GRÁFICA E EDITORA LTDA.:	61
		Max length: 68
	  2. Descriçao da despesa
		<type 'unicode'>
		Nulls: True
		Unique values: 5632
		5 most frequent values:
			FOLHETOS:	188
			SANTINHOS:	134
			5.000 SANTINHOS:	87
			10000 SANTINHOS:	84
			10.000 SANTINHOS:	59
		Max length: 255
	Row count: 8573

Enfim, deu pra pegar a idéia.
Expressões regulares não estão exatamente na ementa desse curso - mas é algo que vale muito a pena aprender. Eu sugiro o maravilhoso [piazinho](http://www.piazinho.com.br/).

## Ordenando com o csvorder

O ``csvorder`` permite a gente reeordenar o csv. Podemos usar isso, por exemplo, para descobrir quais são as dez maiores despesas do PT:

	$ csvcut -c 16,14,6,18 DespesasCandidatos-clean.csv | csvgrep -c 3 -r "PT" | csvsort -r | head -n 11
Valor despesa,Nome do fornecedor,Sigla  Partido,Descriçao da despesa
	1740000.0,ANDASOM COMÉRCIO DE APARELHOS ELETRONICOS LTDA.ME,PT,"LOCAÇÃO DE 100 KOMBI COM CONDUTOR, EQUIP.SONORIZAÇÃO E FORNECIMENTO DE COMBUSTIVEL DURANTE A CAMPANHA ELEITORAL"
	1500000.0,POLIS PROPAGANDA & MARKETING LTDA,PT,"COORDENAÇÃO, CRIAÇÃO E PRODUÇÃO DE MATERIAIS PUBLICITÁRIOS DE PROGRAMAS PARA RÁDIOS E TELEVISÃO"
	960000.0,"ANALITICA, AMARAL & ASSOCIADOS COMUNICAÇÃO LTDA.",PT,PRESTAÇÃO DE SERVIÇOS DE RELACIONAMENTO COM A MIDIA E ASSESSORIA DE IMPRENSA
	580000.0,A.J.M. DE AZEVEDO ELETRONICOS - EPP,PT,100 LOCAÇÃO DE KOMBI SONORIZADA PARA ELEIÇÃO 2012-FERNANDO HADDAD PREFEITO
	500000.0,POLIS PROPAGANDA & MARKETING LTDA,PT,CRIAÇÃO DE HOME PAGE
	500000.0,MARINANGELO E AOKI ADVOGADOS,PT,"PRESTAÇÃO DE SERVIÇOS JURÍDICOS, CONSISTINDO CONSULTORIA E ASSESSORIA JURÍDICA EM DIREITO POLÍTICO E ELEITORAL"
	500000.0,485 PRODUÇÕES,PT,"REFERENTE A PRODUÇÃO DE VIDEO PARA PROPAGANDA ELEITORAL, A 485 PRODUÇÕES"
	435000.0,ANDASOM COMÉRCIO DE APARELHOS ELETRONICOS LTDA.ME,PT,LOCAÇÃO E PRESTAÇÃO DE SERVIÇOS DE SONORIZAÇÃO MÓVEL EM 75 VEÍCULOS
	416000.0,ASSEGURE-ASSESSORIA E CONSULTORIA EM PATRIMONIO EMPRESARIAL LTDA.,PT,PRESTAÇÃO DE SERVIÇOS DE SEGURANÇA PRIVADA DESARMADA AO CANDIDATO E MEMBROS INTEGRANTES DA COORDENÇÃO DA CAMPANHA MAJORITÁRIA
	360000.0,XISCOM PRODUTORA DE AUDIO E VIDEO LTDA,PTB,SERVIÇÃO DE AUDIO E VIDEO PROGRAMA ELEITORAIS ELEIÇÃO 2012

De novo, usamos o csvcut para cortar apenas as colunas 16 (valor), 14 (nome), 6 (partido), e 18 (descrição), pedimos para o grep filtrar a coluna 3 (da nova tabela) por "PT" e depois para o csvsort ordenar em ordem reversa (com a opção ``-r``) se não especificarmos qual coluna, o csvsort vai usar a primeira coluna pro sort. Por fim usamos o head para mostrar apenas as primeiras 10 linhas (11 com o cabeçalho).


