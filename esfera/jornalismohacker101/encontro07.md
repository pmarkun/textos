# Jornalismo Hacker 101 - Encontro 7

Como encontrar as receitas de um Candidato.

## Materia necessário

Para fazer o exercício abaixo você vai precisar:
* Não ter medo do `Terminal`
* [python](http://www.python.org/getit/)
* [csvkit](http://csvkit.readthedocs.org/en/latest/)

## Instalando o csvkit no Ubuntu (e semelhantes)

O Python já vem por padrão. Para instalar o csvkit no Ubuntu basta fazer o seguinte:

	$ sudo apt-get install python-pip python-dev build-essential 
	$ sudo pip install csvkit
	
O `pip` é um gerenciador de pacotes e módulos para o python. Uma espécie de 'apt-get' do python.

## Baixando os dados
Para esse exercício vamos utilizar a prestação final de doações de campanha do TSE - disponíveis [aqui](http://www.tse.jus.br/eleicoes/repositorio-de-dados-eleitorais)

Vamos começar criando uma espaço de trabalho:

	$ mkdir encontro7
	$ cd encontro7

Agora vamos baixar os arquivos:

	$ wget http://agencia.tse.jus.br/estatistica/sead/odsele/prestacao_contas/prestacao_contas_2010.zip

Dezipando o arquivo:

	$ unzip prestacao_final_2010.zip

Por enquanto vamos trabalhar apenas com as receitas dos candidatos.
O arquivo de 2010 contem pastas dividas por Estado. Para facilitar vamos trabalhar direto com o arquivo de SP.

## Investigando o dataset com o csvcut

Agora a gente começa de fato a brincar com as ferramentas do csvkit.
A primeira coisa a fazer é entender quais as colunas disponíveis no dataset. Para isso podemos usar o ``csvcut``. 

	$ tail -n+6 candidato/SP/ReceitasCandidatos.txt | csvcut -d ";" -n
	1: Data e hora
	2: UF
	3: Sigla Partido
	4: Número candidato
	5: Cargo
	6: Nome candidato
	7: CPF do candidato
	8: Entrega em conjunto?
	9: Número Recibo Eleitoral
	10: Número do documento
	11: CPF/CNPJ do doador
	12: Nome do doador
	13: Data da receita
	14: Valor receita
	15: Tipo receita
	16: Fonte recurso
	17: Espécie recurso
	18: Descrição da receita

  

O primeiro comando `tail` é usado para pular as 6 primeiras linhas do arquivo. Isso porque o arquivo do TSE esta com algum problema e essas linhas são uma mensagem de erro.
O segundo comando depois da `|` é o próprio `csvcut`, o `-d` o separador das colunas (no caso `;`) e o -n pede para o csvcut mostrar os nomes e número das colunas.

## Cortando a tabela com o csvgrep

	$ tail -n+6 candidato/SP/ReceitasCandidatos.txt | csvgrep -d ";" -c 6 -m "MARCO ANTONIO FELICIANO" > doacoes-feliciano.csv
	
Novamente usamos o `tail` para pular a mensagem de erro.
O parametro `-d` já foi explicado. O `-c` escolhe a coluna que vamos usar para filtrar e o `-m` é o texto que vamos tentar encontrar nas linhas.
Por fim, o `>` é um redirecionador de saída e diz que o programa deve salvar os resultados do comando no arquivo `doacoes-feliciano.csv`. Se omitirmos esse parametro, o programa vai simplesmente imprimir as linhas na tela.

Feito isso, basta abrir o arquivo `doacoes-feliciano.csv` no seu aplicativo de planilhas favorito.

**Atenção**

Boa parte das doações de campanha são roteadas através do Fundo Partidário - assim fica difícil saber com precisão quais grupos de interesse e empresas estão apoiando de fato determinado candidato.
