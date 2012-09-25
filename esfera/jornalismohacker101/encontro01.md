# Jornalismo Hacker 101 - Encontro 1

## Baixando os dados
Para esse exercício vamos utilizar a primeira parcial de doações de campanha do TSE - disponíveis [aqui](http://www.tse.jus.br/eleicoes/repositorio-de-dados-eleitorais)

Vamos começar criando uma espaço de trabalho:

	$ mkdir encontro1
	$ cd encontro1

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

Para visualizar se o arquivo esta correto podemos usar o comando ``head`` para ler as primeiras linhas do arquivo ``DespesasCandidatos.csv``

	$ cd 2012
	$ head -n 5 DespesasCandidatos.csv
	"Data e hora";"Sequencial Candidato";"UF";"N�mero UE";"Munic�pio";"Sigla  Partido";"N�mero candidato";"Cargo";"Nome candidato";"CPF do candidato";"Tipo do documento";"N�mero do documento";"CPF/CNPJ do fornecedor";"Nome do fornecedor";"Data da despesa";"Valor despesa";"Tipo despesa";"Descri�ao da despesa"
	"23/08/201215:28:44";"100000000122";"MA";"09210";"S�O LU�S";"PSTU";"16789";"Vereador";"LUIZ CARLOS NOLETO CHAVES";"23843616353";"Nota Fiscal";"001307";"06331045000125";"CORINGRA-COROAT� IND�STRIA GR�FICA LTDA";"27/07/2012";"300";"Publicidade por materiais impressos";"MATERIAIS GRAFICOS"
	"23/08/201215:28:44";"100000000199";"MA";"08419";"MORROS";"PV";"43";"Prefeito";"SIDRACK SANTOS FEITOSA";"45011990320";"Nota Fiscal";"1168";"07153251000155";"L O SIMOES BARBOSA - ME";"27/07/2012";"1650";"Combust�veis e lubrificantes";"IMPORTA O DEBITO EM 600 LTS DE DIESEL NO VALOR DE R$ 2,10/LITRO, E 150 LTS DE GASOLINA NO VALOR DE 2,60/LITRO"
	"23/08/201215:28:44";"100000000210";"MA";"08419";"MORROS";"PRB";"10222";"Vereador";"MARIA DO ESPIRITO SANTO SILVA RODRIGUES";"49428730378";"Nota Fiscal";"0804";"10780247000121";"COMERCIO VAREJISTA DE COMBUSTIVEIS PARA VEICULOS AUTOMOTORES";"27/07/2012";"308,4";"Combust�veis e lubrificantes";"DESPESA COM COMBUSTIVEL"
	"23/08/201215:28:44";"100000000210";"MA";"08419";"MORROS";"PRB";"10222";"Vereador";"MARIA DO ESPIRITO SANTO SILVA RODRIGUES";"49428730378";"Recibo";"SN";"84276606349";"SUED RICHARLLYS NASCIMENTO";"20/07/2012";"200";"Produ��o de jingles, vinhetas e slogans";"DESPESA COM PRUDU�AO DE JINGLES"

Possívelmente (mas vai depender de algumas configurações do seu computador) o arquivo vai aparecer como acima. Com alguns caracteres estranhos no lugar dos acentos.

Um outro detalhe é que os valores de despesa usam virgula e não ponto para separar as casas decimais.

## Corrigindo o arquivo

Esse é um problema recorrente quando estamos trabalhando com tabelas e arquivos de fontes diversas. A maioria dos sistemas e programas que vamos utilizar no curso estão preparados para entender o padrão [utf-8](http://pt.wikipedia.org/wiki/UTF-8). Aqui no Brasil por muito tempo utilizou o padrão [iso-8859-1](http://pt.wikipedia.org/wiki/ISO_8859-1) e vários sites e sistemas (como o do TSE) continuam utilizando esse formato.

A convenção de usar a virgula como separador de decimal é também um problema recorrente e vale sempre dar uma investigada antes de mexer nos arquivos.

Se alguém quiser entender melhor sobre encodings tem uma série de textos sobre o assunto na rede. Eu recomendo [esse aqui](http://kunststube.net/encoding/). Mas o processo de 'olhar o arquivo e ver se esta engraçado' funciona também.

Para corrigir o encoding, podemos usar uma ferramenta do próprio csvkit. Mas antes disso é melhor a gente substituir as virgulas por pontos no campo do valor. Para isso vamos utilizar o ``sed``. (Acaba sendo mais fácil corrigir isso no OpenOffice ou no Recline, mas eu não quis sair da linha de comando nessa aula.)

O sed é o 'steam editor', um editor de linha de comando que pode fazer um monte de coisas legais.

	$ cat DespesasCandidatos.csv | sed "s/\([0-9]\),\([0-9]\)/\1.\2/" > DespesasCandidatos-tmp.csv

Basicamente o que fizemos aqui foi usar o comando ``s`` do sed para substituir a expressão regular \([0-9])\),\([0-9]\) por \1.\2 e colar o resultado no arquivo DespesasCandidatos-tmp.csv

Esse é um jeito meio gambiarra de fazer isso. Mas deu pro gasto. Be lazy.

O ``in2csv`` é um conversor de arquivos para o formato csv. Com ele você consegue converter outros formatos (XLS, GeoJSON, tsv, etc) para o padrão CSV mas ele também faz um belo trabalho de padronizar o próprio CSV.

	$ in2csv -e iso-8859-1 DespesasCandidatos-tmp.csv > DespesasCandidatos-clean.csv
	$ head -n 5 DespesasCandidatos-clean.csv
	Data e hora,Sequencial Candidato,UF,Número UE,Município,Sigla  Partido,Número candidato,Cargo,Nome candidato,CPF do candidato,Tipo do documento,Número do documento,CPF/CNPJ do fornecedor,Nome do fornecedor,Data da despesa,Valor despesa,Tipo despesa,Descriçao da despesa
	23/08/201215:28:44,100000000122,MA,09210,SÃO LUÍS,PSTU,16789,Vereador,LUIZ CARLOS NOLETO CHAVES,23843616353,Nota Fiscal,001307,06331045000125,CORINGRA-COROATÁ INDÚSTRIA GRÁFICA LTDA,2012-07-27,300.0,Publicidade por materiais impressos,MATERIAIS GRAFICOS
	23/08/201215:28:44,100000000199,MA,08419,MORROS,PV,43,Prefeito,SIDRACK SANTOS FEITOSA,45011990320,Nota Fiscal,1168,07153251000155,L O SIMOES BARBOSA - ME,2012-07-27,1650.0,Combustíveis e lubrificantes,"IMPORTA O DEBITO EM 600 LTS DE DIESEL NO VALOR DE R$ 2.10/LITRO, E 150 LTS DE GASOLINA NO VALOR DE 2,60/LITRO"
	23/08/201215:28:44,100000000199,MA,08419,MORROS,PV,43,Prefeito,SIDRACK SANTOS FEITOSA,45011990320,Recibo,01,48273384349,MARIA LUCIA XAVEIR PEREIRA,2012-07-24,2000.0,Locação/cessão de bens imóveis,PAGAMENTO DO ALUGUEL DO COMITE DE CAMPANHA
	23/08/201215:28:44,100000000210,MA,08419,MORROS,PRB,10222,Vereador,MARIA DO ESPIRITO SANTO SILVA RODRIGUES,49428730378,Nota Fiscal,0804,10780247000121,COMERCIO VAREJISTA DE COMBUSTIVEIS PARA VEICULOS AUTOMOTORES,2012-07-27,308.4,Combustíveis e lubrificantes,DESPESA COM COMBUSTIVEL

Agora sim. De quebra ele também trocou os separadores (de ``;`` por ``,``) e removeu as aspas duplas.

## Redirecionamento de saída

Importante notar que ao invés de sobreescrever o arquivo eu criei um arquivo novo chamado ``DespesasCandidatos-clean.csv``.

Para isso eu usei um 'output redirector'. O caractere ``>`` faz com que a saída do programa seja direcionado para um arquivo - sobreescrevendo-o. Se eu tivesse usado ``>>`` a saída do programa seria adicionado ao final do arquivo ao invés de sobreescrever.
 
(Tente rodar o comando acima sem o ``>`` para ver o que acontece =)

## Investigando o dataset com o csvcut

Agora a gente começa de fato a brincar com as ferramentas do csvkit. A primeira coisa a fazer é entender quais as colunas disponíveis no dataset. Para isso podemos usar o ``csvcut``.

	$ csvcut -n DespesasCandidatos-clean.csv
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

A opção ``-n`` simplesmente faz o sistema imprimir a primeira linha do arquivo.

Isso nos mostra quais os campos, mas não mostra que tipo de informação eles contém. Vamos usar o csvcut de novo para visualizar apenas colunas específicas:

	$ csvcut -c 14,16 DespesasCandidatos-clean.csv | head -n 5
	Nome do fornecedor,Valor despesa
	CORINGRA-COROATÁ INDÚSTRIA GRÁFICA LTDA,300.0
	L O SIMOES BARBOSA - ME,1650.0
	MARIA LUCIA XAVEIR PEREIRA,2000.0
	COMERCIO VAREJISTA DE COMBUSTIVEIS PARA VEICULOS 	AUTOMOTORES,308.4

A opção -c permite escolher as colunas que queremos utilizar. Usamos um pipe ``|`` para passar esse arquivo pelo comando ``head`` para que só sejam mostrados os 5 primeiros resultados.

## Pipe

Os pipes ``|`` são um conceito chave no terminal e para usar o csvkit. Com ele a gente consegue fazer uma cadeia de operações em um único comando - economizando etapas e fazendo o processo ser mais eficiente.

Se quizessemos extrair um arquivo apenas Nome, CNPJ e Valor das 500 primeiras linhas de doações poderiamos combinar o pipe com o redirecionador de saída ``>``:

	$ csvcut -c 13,14,16 DespesasCandidatos-clean.csv | head -n 500 > 500despesas.csv

## Estatísticas com csvstat

Outra ferramenta disponível no csvkit é o ``csvstat`` que faz as vezes do comando ``summary`` da linguagem de programação para estatísticas R e mostra uma série de informações interessantes sobre os nossos dados.

	$ csvcut -c 14,16 DespesasCandidatos-clean.csv | csvstat -d "," -y 1024
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
		<type 'float'>
		Nulls: False
		Min: -10000.0
		Max: 1740000.0
		Sum: 191607160.73
		Mean: 1169.34883088
		Median: 290.0
		Standard Deviation: 8544.13328164
		Unique values: 18517
		5 most frequent values:
			300.0:	6245
			100.0:	5826
			200.0:	5053
			50.0:	4432
			150.0:	3606
	Row count: 163858

O csvstats deu algumas informações interessantes pra nossa exploração - a empresa que mais recebeu pagamentos (em quantidade e não na soma do valor) foi a Ediouro, e das 163858 doações - existem apenas 79329 empresas/pessoas prestando serviços.

O csvstats tenta interpretar também o tipo de informação disponível, por algum motivo se eu rodo o comando apenas ele não só se confunde com relação ao separador (por isso tive que explicitamente usar a opção ``-d`` para selecionar a virgula como delimitador como ele também não entendeu que o campo de Valor era um valor numérico, então usei a opção ``-y`` para pedir que ele tentasse descobrir quais os tipos dos campos a partir apenas dos primeiros 1024 bytes. Com isso ele deu uma série de consolidações estatísticas.

A maior despesa foi no valor de 1.74mi! A despesa média de 290 reais. E a despesa mais frequente de 300 reais...

## Procurando colunas com csvgrep

O ``csvgrep`` é uma ferramenta do kit que permite a gente fazer perguntas diretamente pros dados - ele busca dentro do csv por parametros específicos.

Vamos buscar os 10 primeiros pagamentos feito para a EDIOURO então para entender melhor o que isso significa:

	$ csvcut -c 14,16,18 DespesasCandidatos-clean.csv | csvgrep -c 1 -m "EDIOURO GRAFICA E EDITORA LTDA" | head -n 10
	EDIOURO GRAFICA E EDITORA LTDA,7431.25,FOLHETOS
	EDIOURO GRAFICA E EDITORA LTDA,3445.0,FOLHETOS
	EDIOURO GRAFICA E EDITORA LTDA,3445.0,FOLHETOS
	EDIOURO GRAFICA E EDITORA LTDA,3445.0,FOLHETOS
	EDIOURO GRAFICA E EDITORA LTDA,3445.0,FOLHETOS
	EDIOURO GRAFICA E EDITORA LTDA,1722.5,FOLHETOS
	EDIOURO GRAFICA E EDITORA LTDA,2756.0,FOLHETOS
	EDIOURO GRAFICA E EDITORA LTDA,4134.0,FOLHETOS
	EDIOURO GRAFICA E EDITORA LTDA,2067.0,FOLHETOS

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

## csvlook

Para finalizar e mostrar o ``csvlook`` que mostra a tabela no terminal de uma maneira que seja fácil de ler:

	$ cat DespesasCandidatos-clean.csv | csvgrep -c 18 -r "JINGLE" | csvsort -c 16 -r -d "," | head -n 6 | csvcut -c 9,6,5,3,16 | csvlook
	|-----------------------------------+----------------+------------------+----+----------------|
	|  Nome candidato                   | Sigla  Partido | Município        | UF | Valor despesa  |
	|-----------------------------------+----------------+------------------+----+----------------|
	|  RENATO REZENDE ROCHA FILHO       | PSDB           | PILAR            | AL | 15000.0        |
	|  OTÁVIO MARCELO MATOS DE OLIVEIRA | PP             | MATA DE SÃO JOÃO | BA | 12000.0        |
	|  CARLOS MAGNO DE MOURA SOARES     | PC do B        | CONTAGEM         | MG | 10000.0        |
	|  MARCELO ESSVEIN                  | PDT            | TRIUNFO          | RS | 6000.0         |
	|  FELIX MONTEIRO LENGRUBER         | PMDB           | MACUCO           | RJ | 5000.0         |
	|-----------------------------------+----------------+------------------+----+----------------|

Uma lista com os 5 jingles mais caros.

Se você conseguir entender o comando acima, já esta bem afiado no csvkit ;)

Como isso aqui é basicamente uma adaptação do [tutorial gringo](http://csvkit.readthedocs.org/en/latest/index.html) e ele ainda não esta completo, eu também ainda não vou completar esse aqui.

Mas a lógica é mais ou menos essa. Então acho que podemos escrever colaborativamente exemplos dos outros comandos disponíveis. Principalmente o ``csvstack`` [documentação](http://csvkit.readthedocs.org/en/latest/scripts/csvstack.html) e o ``csvjoin`` [documentação](http://csvkit.readthedocs.org/en/latest/scripts/csvjoin.html)

Que permitem agrupar CSVs - csvstack (por exemplo, a mesma tabela de vários anos) ou combinar tabelas a partir de campos específicos - csvjoin (por exemplo, juntar diferentes informações sobre munícipios a partir do código IBGE).

A documentação completa do csvkit esta [aqui](http://csvkit.readthedocs.org/).
