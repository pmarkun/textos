# Diagnóstico de Transparência no BNDES
## Documento de Analíse elaborado pela Esfera - Hacks Políticos

## Objetivo

Analisar o [portal de transparência do BDNES](http://www.bndes.gov.br/SiteBNDES/bndes/bndes_pt/Institucional/BNDES_Transparente/) e sua adequação aos príncipios dos dados abertos e a legislação brasileira.

## Estrutura

Lançada em 2007 a seção BDNES Transparente oferece as seguintes informações:

* Aplicação dos Recursos Financeiros
* Boletins de Desempenho
* Consulta às Operações do BNDES
* Contas Públicas
* Demonstrativos Financeiros
* Estatísticas Operacionais
* Recursos de Fundos
* Licitações/Editais
* Ouvidoria
* Privatização
* Processos de Contas Anuais
* Pró-Equidade de Gênero
* Convenção da OCDE

Mais recentemente, resultado da Lei de Acesso à Informação uma [seção específica](http://www.bndes.gov.br/SiteBNDES/bndes/bndes_pt/Institucional/acesso_a_informacao/index.html) foi implementada:

* Institucional
** Informações organizacionais do BNDES, compreendendo suas funções, competências, estrutura organizacional, relação de autoridades (quem é quem), agenda do Presidente, horários de atendimento e legislação da entidade.
* Ações e programas
** Informações pertinentes aos programas, ações, projetos e atividades implementadas pelo BNDES.
* Auditorias
** Informações referentes ao resultado de inspeções e auditorias realizadas no BNDES.
* Convênios
** Informações sobre os repasses e transferências de recursos financeiros efetuados pelo BNDES.
* Acesso à Informação - Despesas
** Informações sobre a execução orçamentária e financeira detalhada do BNDES.
* Licitações e contratos
** Informações sobre licitações e contratos de compras realizados pelo BNDES.
* Empregados
** Informações sobre concursos públicos de provimento de cargos no BNDES.
* Perguntas frequentes
** Sobre o BNDES, sobre as ações no âmbito de sua competência e sobre o Serviço de Informação ao Cidadão (SIC).
* Sobre a Lei de Acesso à Informação
** Informações como os temas tratados na lei, os procedimentos para solicitação de acesso através da Transparência Passiva e mecanismos recursais, estatísticas de acesso, entre outros dados.
* Serviço de Informação ao Cidadão - SIC
** Informações pertinentes ao seu funcionamento, localização e dados de contato no âmbito do BNDES.


## Dados Abertos

Ao analisar as políticas de transparência do BNDES esse documento se pauta também pelos *8 príncipios de Dados Abertos* desenhado originalmente por um grupo de trabalhoo em uma conferência em Sebastopool, California em 2007 e incluido no Guia de Dados Governamentais Abertos, elaborado pela CGU e a Transparência Hacker; na INDA - Infraestrutura Nacional de Dados Abertos e nos príncipios da OGP.

1. *Completos.* Todos os dados públicos são disponibilizados. Dados são informações eletronicamente gravadas, incluindo, mas não se limitando a, documentos, bancos de dados, transcrições e gravações audiovisuais. Dados públicos são dados que não estão sujeitos a limitações válidas de privacidade, segurança ou controle de acesso, reguladas por estatutos.

2. *Primários.* Os dados são publicados na forma coletada na fonte, com a mais fina granularidade possível, e não de forma agregada ou transformada.

3. *Atuais.* Os dados são disponibilizados o quão rapidamente seja necessário para preservar o seu valor.

4. *Acessíveis.* Os dados são disponibilizados para o público mais amplo possível e para os propósitos mais variados possíveis.

5. *Processáveis por máquina.* Os dados são razoavelmente estruturados para possibilitar o seu processamento automatizado.

6. *Acesso não discriminatório.* Os dados estão disponíveis a todos, sem que seja necessária identificação ou registro.

7. *Formatos não proprietários.* Os dados estão disponíveis em um formato sorbe o qual nenhum ente tenha controle exclusivo.

8. *Livres de licenças.* Os dados não estão sujeitos a regulações de direitos autorais, marcas, patentes ou segredo industrial. Restrições razoáveis de privacidade, segurança e controle de acesso podem ser permitidas na forma regulada por estatutos.

## Analíse das Operações do BNDES

Não é objetivo desse documento analisar individualmente cada conjunto de informação disponibilizado pelo portal e sim fazer uma leitura mais abrangente sobre as políticas e a forma de disponibilização desses dados.

Ainda assim procurarmos analisar mais detalhadamente alguns conjutos de informação.

O link [Consulta as Operações do BDNES](http://www.bndes.gov.br/SiteBNDES/bndes/bndes_pt/Institucional/BNDES_Transparente/Consulta_as_operacoes_do_BNDES/index.html) nos traz as diferentes operações de financiamento do BNDES divididos por tipos:

* Consulta às operações diretas com empresas
* Consulta às operações indiretas com empresas
* Consulta às operações com micro, pequenas e médias
empresas
* Consulta às operações com Estados/Municípios

Faz ainda menção sobre o fato de apenas informações não sujeitas ao sigilo bancário estarem disponibilizadas.

Ainda que isso seja fato - existe na regulamentação da LAI uma excessão para proteger informações de sigilo bancário - acreditamos ser importante detalhar melhor quais são essas informações protegidas por sigilo bancário de maneira clara e direta.

Dentro dos 3 primerios links - operações diversas relacionadas com empresas - encontramos uma mesma estrutura, as operações são separadas por tipo (Agropecuária e Inclusão Social, Comércio Exterior, Infraestrutura, Insumos Básicos, etc) e ano (2008-2012).

Para os anos mais recentes é disponibilizado uma planilha de Excel (formato XLS) e para os anos iniciais um documento PDF.

Não existe nenhuma informação sobre o que esses tipos significam, de onde vem essa classificação e quais os critérios.

	Uma das boas práticas recomendadas quando da liberação de dados é oferecer ampla documentação sobre o que esta sendo disponibilizado. Ainda que títulos como 'Agroepecuária e Inclusão Digital' pareçam auto-explicativos, quando falamos de transparência e abertura nunca se pode prever onde e em que contexto essa informação vai circular portanto disponibilizar uma documentação de apoio sobre como funcionam as operações de financiamento e seus fluxos é altamente recomendavel.

As diferentes planilhas XLS disponiblizadas não seguem um padrão único, dificultando o cruzamento de informações.

Além disso esse metodo de disponibilização esta em desacordo com vários príncipios dos dados abertos.

1. *Completos.* 
É sempre complicado analisar o quão completo é um determinado conjunto de dados, mas as diferenças entre as planilhas deixa claro que algumas informações - por exemplo o munícipio de atuação estão falantando.

2. *Primários.* (OK)
Na maioria das planilhas as operações estão devidamente separadas por projeto e não totalizadas por Cliente ou outro campo.

3. *Atuais.*
As operações mais atuais disponíveis são referentes ao segundo trimestre de 2012. Faltam portanto as informações referentes ao terceiro e quarto trimestre desse ano.

4. *Acessíveis.*
A falta de documentação atrapalha a compreensão dos arquivos.
Sugerimos incluir um documento anexo que explique os processos dos financiamentos e detalhe os campos das tabelas.

5. *Processáveis por máquina.* 
Os formatos escolhidos para publicação - XLS e PDF - são ambos formatos de díficil leitura por máquina.
O XLS embora seja um formato razoavelmente simples de converter ainda exige um esforço grande do cidadão para operar - especialmente pelo uso de campos desnecessários no cabeçalho do arquivo.
Sugerimos publicar as informações usando o padrão CSV - mantendo compatibilidade com o sistema atual mas migrando para um formato não proprietário e legível por máquina.
Outra opção, melhor detalhada abaixo é a criação de um webservice/api que forneça essas informações de maneira automatizada.

6. *Acesso não discriminatório.* (OK)
Não é necessário qualquer tipo de registro para acessar as informações.

7. *Formatos não proprietários.*
Os formatos escolhidos para publicação - XLS e PDF - são ambos formatos proprietários. Sugerimos publicar as informações usando o padrão CSV - mantendo compatibilidade com o sistema atual mas migrando para um formato não proprietário e legível por máquina.

8. *Livres de licenças.*
Nenhuma licença esta especificada. Existe um © no pé da página o que implicaria que essa informação - caso não fosse pública - estaria protegida e com todos os direitos reservados.

**Sugerimos a explicitação de que os dados são de domínio público e portanto podem ser utilizados por quaisquer pessoas para qualquer fim (inclusive comerciais e derivativos) sem a necessidade de autorização específica.**


## webservice/API

Uma API (Application Programing Interface) é um recurso utilizado por sites e portais para disponibilizar seus dados de maneira estruturada para que se possa construir analíses e aplicações em cima delas.
É a forma como sites como Twitter, Facebook e Foursquare disponibilizam seus dados para que sejam construidos centenas de aplicativos que melhoram ou fornecem meios alternativos de visualizar aquelas informações.

É também a maneira utilizada por diferentes governos e bancos.

* [SICAF](http://api.comprasnet.gov.br/sicaf/doc/)
* [WorldBank](http://data.worldbank.org/developers/api-overview)


O governo federal através da [INDA - Infraestrutura Nacional de Dados Abertos](http://wiki.gtinda.ibge.gov.br/) vem criando uma série de normatizações e APIs para diferentes bancos de dados.

**Recomendamos que o BNDES crie uma API para o acesso automatizado aos seus dados - começando pelos dados de financiamento.**

Essa API deve retornar dados em formatos abertos e estruturados como JSON, XML e HTML (para visualização).

Opcionalmente pode-se estudar também a implementação de um modelo semântico usando a tecnologia do RDF.


## eSIC - Sistema de Atendimento ao Cidadão

A página do eSIC do BDNES encaminha o usuário para o sistema do Governo Federal, produzido pela CGU - Controladoria Geral da União.

Ainda que o uso desse sistema seja satisfatório, a impossibilidade de acompanhar os pedidos e respostas feitas por outras pessoas diminui a transparência e aumenta a carga de retrabalho dos funcionários do BNDES que eventualmente respondem a mesma questão repetidas vezes.

**Recomendamos a utilização de um canal adicional de comunicação que deixe público o registro de pedidos e respostas anteriores (excluindo-se os dados pessoais dos requerentes).**

Ainda assim o grande problema enfrentado pelos atuais sistemas de acesso à informação acontece menos no plano técnico do sistema e mais na falta de capacitação e compreensão dos servidores públicos sobre o que o propósito e a importância da Lei de Acesso à informação.

É essencial que exista um processo de diálogo e discussão com todos os servidores envolvidos no procesos de gerar, processar, armazenar e distribuir informações em especial aqueles que se encontram na ponta do processo, dialogando diretamente com o cidadão.

Algumas das questões-chave que são geralmente negligênciada pelos servidores públicos por desconhecimento e/ou falta de cultura incluem:

**Formatos errados**

É preciso prestar atenção não apenas em qual informação foi solicitada, mas também em que formato foi feita essa solicitação - usos diferentes necessitam de formatos diferentes e normalmente o formato disponibilizado através do portal é insuficiente para usos mais avançados (ver sessão sobre princípios dos dados abertos).

**Dados insuficientes**

É preciso prestar atenção no gráu de detalhamento e pormenores do pedido, via de regra o servidor deve assumir que a informação já disponível não foi suficiente por algum motivo - e ainda que seja sempre importante e útil apontar os links do portal onde a informação específica se encontra (e vale prestar atenção sobre o grau de dificuldade que o próprio servidor teve ao buscar essa informação), tentar estar atento a tudo aquilo que consta no pedido e que por uma razão ou outra não se encontra no site.

**Avaliação do motivo**

O servidor não deve julgar ou avaliar a importância daquele pedido ou requerente. Nenhum pedido deve ser tratado de forma mais importante que outro - por ter vindo de um jornalista ou pesquisador, por exemplo.
A lei é clara que o direito à informação vale para todos e independe de motivo específico.

**Recomendamos desenvolver um proceso interno especifico para lidar com casos de respostas insuficientes e/ou em formatos errados. É preciso um trabalho constante no processo de coleta, armazenamento e disponibilização dos dados para que tenhamos um acesso à informação e uma transparência plenos.**

## Recomendações adicionais

Após análisar a sessão [BNDES Transparente](http://www.bndes.gov.br/SiteBNDES/bndes/bndes_pt/Institucional/BNDES_Transparente/) do site do BNDES, avaliamos que embora tenha existido uma série de melhorias na oferta de dados nos últimos anos, esse trabalho foi feito de maneira inconsistênte e a qualidade dos dados divulgados é bastante variável.

**Recomendamos a criação de uma política de Dados Abertos, um documento que norteie tecnica e filosoficamente as ações de transparência do portal, atualizando-as para a nova maneira de pensar o acesso à informação na sociedade da informação incorporando nesse documento os princípios de dados abertos e a dimensão do uso de dados e informações por diferentes grupos da sociedade (cidadãos, desenvolvedores, especialistas e governo). Esse documento também deve servir como norte para a equipe jurídica do BNDES, cujo trabalho principal deve passar de -encontrar motivos para barrar a transparência- para -encontrar caminhos para permitir a livre publicação e circulação de conhecimento-**

Um problema recorrente enfrentado pelos órgãos públicos na tentativa de modernizar seus processos de transparência é que os modelos tradicionais de desenvolvimento de software utilizados pelos orgãos públicos não dão conta de inovar e acabam gerando soluções incompátiveis com as atuais necessidades da rede.

**Recomendamos a criação ou promoção de uma equipe interna responsável pela implementação tecnica e cultural dessa nova política de transparência. Essa equipe deve ter autônomia tecnica para desenvolver ou contratar o desenvolvimento de tecnologias para auxiliar no processo de disponibilização de informação. Esta equipe deve ser pensada como uma solução de curto-médio prazo com o horizonte de que no longo prazo as práticas culturais e tecnologicas envolvidas sejam difundidas por todo o BNDES.**

Existe uma outra dimensão no processo de transparência pública que é largamente ignorado por orgãos públicos e pouco ou nada se viu nesse sentido na análisa feita ao portal BNDES Transparente. Além de garantir acesso e disponibilidade as informações faz parte de um projeto de transparência de sucesso a qualificação do uso desses dados. Promover atividades e campanhas que aproximem a sociedade cívil dos dados, colocando-os em uma posição de protagonismo e permitindo troca de informação de maneira mais informal entre o servidor público e o cidadão aumenta a eficiência e cria novas possibilidades de inovação para o cidadão e para o poder público.

**Recomendamos que o documento sobre a política de transparência do BNDES inclua diretrizes sobre a realização de eventos e campanhas para a promoção e discussão dos dados disponíveis no portal. Esses eventos podem tomar diferentes formatos, dependendo do público alvo e conjunto de dados específicos, mas algumas formas já testadas podem ser utilizadas como:**

* Transparência Hackdays, eventos mão na massa que reunem desenvolvedores e outros atores da sociedade cívil para desenvolver soluções em cima dos dados apresentados;
* Papo com Dados, mesas de discussão reunindo servidores públicos e organizações sociais para discutir conceitualmente e levantar pontos importantes sobre os dados disponibilizados;
* Concursos de Aplicativos, concursos com premiação que convidem os desenvolvedores e movimentos sociais a apresentarem soluções inovadoras utilizando determinados conjuntos de dados.