# Cartografias Colaborativas
## 10, 11 e 12 de Dezembro de 2012

## Abertura

blábláblá institucional - todos amam mapeamento e ele é muito importante para o crescimento do país.

## Apresentação SNIIC

O Minc tem um monte de dados. O SNIIC vai ser uma plataforma para centralizar essas informações e criar uma plataforma para a participação dos cidadãos na construção de políticas públicas.

O Hacklab é quem esta desenvolvendo.

*Pressupostos (relevantes):*
* Livre participação do cidadão
* Dados Abertos
* Aplicações e serviços com base em APIs abertas
* Informações georreferenciadas (oi?¡)
* Escalabilidade
* Manutenção flexível das tabelas
** 6 em 6 meses?
** Orientado a documentos? É necessário ter árvores e tabelas?

*Abrangência:*
Onde estão os acervos?

*Aparte*
CPF E CNPJ? Modelo CGI.br / Registro.br

Casa da Cultura Digital
Transparência Hacker
VJ Pixel
Circos
Movimentos de Bikes

No nível da rua, não é o CNPJ/CPF que conta.
Problema de política pública é não-reconhecer o 'informal'.



## Protocolos e Plataformas

Falando aqui da camada 'técnica'. Mas entendendo que as escolhas tecnologicas são escolhas polítcas.

Georeferenciamento e os mapas trabalham naturalmente em camadas.
Google Maps -> Pontos de Interesse

Chutando: Dos sites que vão ser apresentados todos compartilham mais de 80% das mesmas funcionalidades.
Os outros 20% é que são o 'diferencial'.

Não da pra parar de reinventar a roda. Mas da pra gente aprender conjuntamente...
* Usar GeoJSON e/ou KML (GeoRSS?) como formato de intercâmbio.
** Toda camada disponibilizada pode ser exportada/acessada separadamente, idealmente via API.
** Precisamos encontrar um (ou mais) conjuntos de metadados básicos importantes para o nosso contexto. 
* Ter um repositório livre de SHAPEs básicos.
** Brasil
** Estados
** Municipios
** Malhas tematicas
*** População
*** Eleição
*** Educação
*** Cultura
* Ter um servidor de mapas dentro do governo/minc/sniic.
** Disponibiliza as camadas básicas, garante autonomia e cria um espaço de interação governo<->sociedade
** Esse servidor pode/deve ser replicado fora do governo
* Fomentar o uso de códigos reutilizaveis, coordenar diferentes equipes de desenvolvimento.
** Questionário técnico - tentar encontrar pontos de contao entre as diferentes plataformas
** Tentar encontrar os problemas comuns e as soluções encontradas.
* Fazer a ponte com a INDE (Infraestrutura Nacional de Dados Espaciais)

Tem um grupo bom aqui de pessoas que usam, pensam e fazem mapas. Creio que valia aproveitar esse encontro para já começar a articular algumas dessas questões em conjunto com minc e outros.