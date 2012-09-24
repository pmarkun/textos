# Jornalismo Hacker 101 - Encontro 03
## Mapas, Shapefiles e Tilemill

## Instalação
Para esse encontro você precisa ter alguns softwares livres instalados:

### LibreOffice
Ou algum outro aplicativo que trabalhe com planilhas. Especificamente é legal que o aplicativo suporte o formato ``dbf`` que é o arquivo que mantém as tabelas de atributos dos ``shapefiles`` (o formato de arquivo para mapas que vamos trabalhar).

O dbf não é nada mais do que um ``csv`` com algumas caracteristicas especiais no cabeçalho.

Baixe o LibreOffice [aqui](http://www.libreoffice.org/download/). Não tem muito mistério.

Em teoria podemos fazer tudo que vamos fazer com o LibreOffice usando o csvkit. Se você quiser treinar mais, fique a vontade.

### QuantumGIS

[GIS](http://en.wikipedia.org/wiki/GIS) significa Geographic Information System e é a sigla utilizada para falar de mapas e georeferenciamento de uma maneira geral.

Existe um monte de aplicativos que trabalham com GIS e várias coisas legais e mais avançadas que da para fazer com eles. Vamos usar o QuantumGIS porque ele tem uma comunidade legal, é software livre e serve para o que precisamos.

Na página do QGIS você tem [instruções para download](http://hub.qgis.org/projects/quantum-gis/wiki/Download) e instalação tanto no Linux, quanto no MacOS e no Windows.

Como de costume, recomendo fortemente que você instale o Ubuntu - mesmo que seja em um pendrive com liveboot para acompanhar a oficina - mudar para software livre é legal e ai você vai se acostumando.

Do contrario, a gente bate cabeça juntos.

### GDAL
O [Geospatial Data Abstraction Layer](http://trac.osgeo.org/gdal/wiki/DownloadingGdalBinaries) é na verdade uma série de bibliotecas e aplicativos de linha de comando para trabalhar e manipular com arquivos geoespaciais.

O QGIS usa ele internamente para uma série de coisas, mas como vamos (ou podemos usar) algumas das suas ferramentas em separado - resolvi colocar ele aqui também.

### Tilemill

Por fim, o Tilemill, que é um aplicativo desenvolvido pela DevelopmentSeed para desenhar e publicar mapas.

O foco do Tilemill é produzir mapas que sejam extremamente leves mesmo trabalhando com grandes quantidades de informação - o jeito dele fazer isso é renderizar todo o mapa de antemão, da mesma maneira que faz o Google Maps em pequenos quadradinhos (tiles).

Por isso essa não é a ferramenta ideal para trabalhar com marcadores dinâmicos - por exemplo, um site onde os usuários adicionam relatos ou ocorrências policiais - ao invés disso ela foi desenhada para trabalhar com informações estáticas - por exemplo, o perfil de todas as escolas dentro dos dados da Prova Brasil, o indice de criminalidade por munícipio desde 94, etc - ainda que faça isso de maneira dinâmica.

Vale lembrar que apesar disso da para combinar os mapas desenhados no Tilemill com um monte de outras ferramentas, então da pra resolver os marcadores dinâmicos em outra camada.

Para baixar e instalar o Tilemill basta acessar o (site deles](http://mapbox.com/tilemill/). 

A versão para Linux esta empacotada para Ubuntu e outras distros vão ter que bater um pouco de cabeça.

O site deles tem também um [crash course](http://mapbox.com/tilemill/docs/crashcourse/introduction/) bem tranquilo para quem quiser aprender o básico da ferramenta.

### Mapbox

Esse não é um app, mas um serviço da DevelopmentSeed para hospedar os mapas que você desenvolver online. A conta free é bastante limitada, mas da pra exercitar e fazer algumas brincadeiras.

O servidor deles também é livre, então se você quiser fazer um projeto com o Tilemill mas não usar o Mapbox, é mole também.

Vale fazer uma conta [aqui](https://tiles.mapbox.com/signup/free)

## Coletando os dados

A ideia desse exercício é criar um mapa e visualizar uma série de dados sobre ele.

O primeiro passo é a gente encontrar o nosso mapa 'base' - um mapa limpo da região que a gente quer visualizar, com o grau de detalhamento que a gente quer.

Alguns exemplos:
* Mapa do Brasil - separado por munícipios
* Mapa do Brasil - separado por estados.
* Mapa do Estado de São Paulo - separado por munícipios.
* Mapa da cidade de São Paulo - separado por subprefeituras.
* Mapa da cidade de São Paulo - separado por bairros.

Encontrar essas informações em uma formato que esteja georeferenciado, não é assim tão fácil. Não adianta pra gente simplesmente baixar uma imagem qualquer da internet... o Tilemill e o QGIS não vão saber o que fazer com isso.

Existem vários formatos possíveis para arquivos georeferenciados - a gente vai trabalhar com os dois mais comuns e fáceis de mexer aqui.

### Shape Files
Os [ESRI Shapefiles](http://en.wikipedia.org/wiki/Shapefile) são arquivos bastante comuns e faceis de trabalhar.

Um shapefile é na verdade um conjunto de vários arquivos:

* .shp — shape format; que são os próprios poligonos, pontos e linhas no mapa (por exemplo, as bordas de um estado ou ruas de uma cidade)
* .shx — shape index format; é um indice posicional que diz em que lugar esta uma forma em relação as outras. É criado para o sistema navegar mais rápido pelo mapa.
* .dbf — attribute format; guarda uma série de atributos sobre cada forma (por exemplo, o nome e a população dos estados) em um formato dBase IV

Existem outros arquivos opcionais, mas esses são os básicos e mais importantes.

### GeoJSON

[GeoJSON](http://en.wikipedia.org/wiki/GeoJSON) é um formato baseado em JSON para lidar com informações georeferenciadas.

Ele vai trabalhar com as mesmas informações que os SHPs mas com um formato mais fácil de trabalhar com Javascript e portanto mais fácil de manipular nos aplicativos web.

### KML

Outro formato bastante comum na web é o [KML](http://en.wikipedia.org/wiki/KML) que foi desenvolvido pelo Google na época do Google Earth. Ele é uma extensão do XML o que faz ele menos legível e mais difícil de trabalhar do que o GeoJSON. Mas tem um monte de gente que trabalha com ele... então paciência.

