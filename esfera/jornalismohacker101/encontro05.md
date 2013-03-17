# Hackahton Prova Brasil/SAEB
## Como acessar os dados com R

Aproveitando que o INEP em conjunto com a fundação Lemann estão organizando um [Hackathon](http://hackathondadoseducacionais.com/) para trabalhar como os dados do Prova Brasil/SAEB. E que estava com bastante dificuldade para bolar um aplicativo entender melhor os dados, resolvi escrever esse mini-tutorial sobre como ler e ver os microdados no R.

## Material Necessário

* [R](http://www.r-project.org)

## Baixando os dados

Baixe e descompacte os arquivos SAEB/2011 e Prova Brasil/2011. Atenção: eles são grandes!

 	wget ftp://ftp.inep.gov.br/microdados/microdados_saeb_2011.zip
	wget ftp://ftp.inep.gov.br/microdados/microdados_prova_brasil_2011.zip
	unzip microdados_saeb_2011.zip
	unzip microdados_prova_brasil_2011.zip

## Sobre os dados

A Prova Brasil e o Saeb são dois exames complementares que compõem o Sistema de Avaliação da Educação Básica.

A grande diferença é que o Saeb é amostral e portanto não tem informações no nível da Escola. Enquanto o Prova Brasil é (segundo eles) 'quase universal'.

O Saeb é também mais abrangente pegando também alunos do 3o ano do ensino médio além dos de 5a a 8a série do fundamental (que também fazem a Prova Brasil).

Ambos aplicam questionários sócio econômicos para Alunos, Professores, Diretores e Escolas com uma série de questões que ajudam a dar o contexto para o resultado das provas.

* [Questionário Professor](http://download.inep.gov.br/educacao_basica/prova_brasil_saeb/questionarios/2013/questionario_professor.pdf)
* [Questionário Diretor](http://download.inep.gov.br/educacao_basica/prova_brasil_saeb/questionarios/2013/questionario_diretor.pdf)
* [Questionário Escola](http://download.inep.gov.br/educacao_basica/prova_brasil_saeb/questionarios/2013/questionario_escola.pdf)

[Quadro comparativo entre Prova Brasil e SAEB](http://portal.inep.gov.br/web/prova-brasil-e-saeb/semelhancas-e-diferencas)

### Prova Brasil

**Tabelas e informações disponíveis**

* Dados/TS_RESPOSTA_ALUNO	Informações das respostas dos alunos **(enorme! ~589mb)**
* Dados/TS_QUEST_* Respostas dos questionários individuais (p/ Escola, Professor, Diretor, Aluno)
* Dados/TS_RESULTADO_ALUNO	Informações da proficiência dos alunos **(enorme! ~764mb)**
* Dados/TS_RESULTADO_ESCOLA	Média da proficiência dos alunos por disciplina segundo Dependência Administrativa e Escola
* Dados/TS_RESULTADO_MUNICIPIO	Média da proficiência dos alunos por disciplina segundo Dependência Administrativa e Município
* Dados/TS_ITEM	Informações das habilidades dos itens da prova e do gabarito
* Dados/TS_PESOS	Pesos da turma e da escola
* Dicionário/Dicionario_Prova_Brasil_2011.ods Dicionário de dados

Em anos anteriores o arquivo era separado em colunas de largura fixa - com arquivos de referência para softwares estatísticos como SPSS e R. Esse ano resolveram facilitar e soltaram um csv separado por `;`.

## Carregando os dados no R

Para o exercício de Exemplo, vamos carregar o Questionário dos Diretores.

**Atenção** arquivos como o TS_RESPOSTA_ALUNO vão exigir um bocado de RAM para serem operados decentemente.

Mude para o diretório dos arquivos do Prova Brasil:

	cd Microdados\ Prova\ Brasil\ 2011
	cd Dados

Abra o R e carregue os dados usando o comando `read.csv()` use o parametro `sep` para definir o separador `;`:

	R
	provabrasil.diretor <- read.csv("TS_QUEST_DIRETOR.csv", sep=";")

Pronto ;)
Agora você já pode utilizar o maravilhoso mundo do R para visualizar os dados.

O Questionário do Diretor é composto de muitas perguntas, o que dificulta um pouco a leitura.

Para começar a explorar o dataset podemos usar o comando `summary()`. Ele vai retornar um sumário de todos os campos, mostrando quantas respostas temos em cada pergunta. Mantenha aberto o dicionário de dados ou não vai fazer muito sentido.

	summary(provabrasil.diretor)

O resultado vai ser algo parecido com isso:

	TX_RESP_Q210 TX_RESP_Q211 TX_RESP_Q212
	.:1177	     .:1238       .:1214      
	*:  38       *:  19       *:  23      
	A:2072       A: 269       A:3182      
	B:1855       B:3616       B: 723 

Buscando essas perguntas no dicionário, vemos que elas são sobre a presença de Ensino Religioso nas Escolas.

Para obter a resposta de uma questão específica você pode utilizar o próprio summary:

	summary(provabrasil.diretor$TX_RESP_Q030)

O resultado vai ser algo parecido com isso:

	.     *     A     B     C     D     E     F     G     H     I
	1622   470  8250   115  3024  2092 10351 22862  4606   737  2093


Mas o ideal é você utilizar a função `aggregate`, que já te retorna uma objeto `list` facilitando a manipulação posterior (os tipos `matrix`, `data.frame`, `list` são parte do charme e da confusão no R)

	aggregate(provabrasil.diretor$TX_RESP_Q030, by=list(Resposta = provabrasil.diretor$TX_RESP_Q030), FUN=length)

O resultado prático é bem parecido com o `summary`. Note que o primeiro parametro é o campo, o segundo `by` é o elemento e o terceiro `FUN` é a função que vamos usar para agrupar (existem outros como `mean` e `mediam` que só fazem sentido com valores numéricos).

Olhando no nosso dicionário de dados:

* A) O modelo encaminhado pela secretaria da educação.
* B) Foi elaborado por mim. 
* C) depois de eu ter elaborado uma proposta do projeto, 
* D) apresentei-a aos professores para sugestões e só depois escrevi a versão final.
* E) os professores elaboraram uma proposta e, com base nela, escrevi a versão final.	uma equipe de professores e eu elaboramos o projeto.	
* F) Professores, pais, outros servidores, estudantes e eu montamos o projeto
* G) foi elaborado de outra maneira.
* H) não sei como foi desenvolvido.
* I) não existe Projeto Pedagógico.
* .) Marcações em Branco
* *) Marcações anuladas

Com isso já da pra ver que a grande maioria dos projetor foi elaborado com 'Professores, pais, outros servidores, estudantes e eu montamos o projeto'. Não é linda a realidade auto-declaratória?

Nós também podemos visualizar essa distribuição gráficamente com o comando `plot`.

	plot(provabrasil.diretor$TX_RESP_Q030)

Agora fica tricky! Vê se você consegue acompanhar. Vamos usar o comando `subset` para extrair um pedaço da tabela:

	subset(provabrasil.diretor, TX_RESP_Q030=="I", select=c("TX_RESP_Q021"))

O primeiro parametro `provabrasil.diretor` é a própria tabela, o segundo `TX_RESP_Q030=="I"` é o nome do campo que queremos filtrar e o valor. No caso, estou filtrando por todos os diretores que foram honestos o suficiente para marcar `I`: **Não ter Projeto Pedagógico** na pergunta `TX_RESP_Q030`.

Por fim o terceiro parametro `select` diz que vamos selecionar apenas algumas colunas - no caso, apenas uma a `TX_RESP_Q021` que no nosso dicionário é **Você assumiu a direção desta escola por**

Para visualizar a distribuição dessas respostas, voltamos ao comando `plot`:

	plot(subset(provabrasil.diretor, TX_RESP_Q030=="I", select=c("TX_RESP_Q021")))

Podemos usar também o `summary()` para obter uma sintese em texto:

	summary(subset(provabrasil.diretor, TX_RESP_Q030=="I", select=c("TX_RESP_Q021")))

E olha só que curioso... a maioria dos diretores que afirmam não ter um projeto pedagógico para escola forma parar lá por...

* A)Seleção.
* B) Eleição apenas.
* C) seleção e eleição.
* D) indicação de técnicos.
* E) indicação de políticos.
* F) outras indicações.
* G) Outra forma.
* .) Marcações em Branco
* *) Marcações anuladas

Dúvido você adivinhar sem fazer o exercício, há!


## Referências

Para saber mais de R e outras coisas.

[Introduction to R Seminar](http://www.ats.ucla.edu/stat/r/seminars/intro.htm)
[Quick-R](http://www.statmethods.net/)
[An Introduction to R](http://cran.r-project.org/doc/manuals/r-release/R-intro.html)
[Curso de R em PT-BR](http://www.leg.ufpr.br/Rpira/Rpira/)