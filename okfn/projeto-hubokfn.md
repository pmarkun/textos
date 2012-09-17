# Hub OKFN Brasil
## Rede de implementação de inteligência livre para transparência

## O que é?
Um hub de empresas, coletivos e desenvolvedores independentes para implementação de soluções, capacitação e difusão de software livre para transparência e acesso à informação.

## Como funciona?
A OKFN funciona trazendo peso institucional, certificando e fomentando parcerias e treinamento no hub.
Cada organização ou individuo vinculado ao hub passa a ser um nó replicador de conhecimento sobre os softwares e processos de desenvolvimento adotados.

A ideia central não é criar uma cooperativa de empresas que possam implementar determinados software - mas que isso o foco do projeto esta em criar redes locais que consigam manter e desenvolver o software, incluindo ai uma capacitação em modelos de desenvolvimento livre para que esse código flua de volta para o repositório central.

## Soluções selecionadas
Para a primeira fase do projeto vamos focar em algumas poucas soluções para áreas que já tem uma demanda mapeada e cuja solução disponível já esteja em uso por comunidades internacionais.

### Alaveteli
O alaveteli é um software livre desenvolvido pela MySociety para implementar sistemas de acesso à informação. Originalmente desenvolvido para ser usado pela sociedade cívil, a MySociety recentemente começou a testa-lo como solução viável para pequenos munícipios em Londres que não tenham sistema próprio.

No Brasil a CGU desenvolveu um sistema baseado no sistema do governo mexicano que começa a ser copiado/implementado por outros orgãos e munícipios menores. Na nossa leitura o grande problema do sistema existente é que ele não da transparência para os pedidos - no Alaveteli todos os pedidos são públicos e isso diminui o retrabalho dos orgãos e amplia a transparência possível.

Todo o Alaveteli é baseado em troca de emails, fazendo com que seja razoavelmente simples sua implementação - sem requerer grandes mudanças nos fluxos de informação do orgão.

É necessário desenvolver melhor o painel administrativo para dar conta das necessidades do poder público de acompanhar e gerar relatórios sobre os pedidos.

**Linguagem:** Ruby
**Documentação:** Boa
**Comunidade:** Boa
**Necessidade de customização:** Média

### OpenSpending

O OpenSpending é uma plataforma da OKFN para armazenar e exibir informações de gastos públicos de uma maneira geral.

A Lei de Transparência obriga todos os munícipios acima de 10.000 habitantes a terem um portal de transparência orçamentaria ativa - criando uma demanda enorme por soluções de baixo custo para exibir essas informações.

Boa parte dos portais das pequenas prefeituras e câmaras são desenvolvidos por empresas da região que trabalham em um modelo de 'assinatura', juntando 10-20 prefeituras e com um valor mensal relativamente baixo (2000~8000 reais) para a hospedagem e manutenção do portal.

Além do lock-in tecnologico que devemos combater por príncipio, boa parte dos portais são pouco funcionais e com zero preocupação com o usuário - além de óbviamente não oferecerem os dados em formato legível por maquina.

Uma adaptação do OpenSpending, formatado para o padrão orçamentário brasileiro, pode ser uma solução interessante para esse problema - oferecendo tanto os dados brutos em um formato amigável para desenvolvedores locais, quanto uma série de visualizações consolidadas baseadas em HTML5 para o cidadão que estiver flanando no site.

O sucesso dessa implementação depende de estabelecer uma rede de instâncias implementadas para estabelecer um ciclo virtuoso de trocas onde a visualização/leitura desenvolvida no munícipio A possa ser rapidamente aproveitada pelo munícipio B.

A ideia de SaaS e contrato mensal de hospedagem pode inclusive ser eventualmente utilizado, pensando em pequenos arranjos locais de munícipios que não tenham bala/necessidade de ter um sistema para chamar de seu.

É importante nesse caso o processo de treinamento e transferência de know-how abordar especificamente processos de migração para não voltarmos ao mau e velho lock-in tecnologico.

Vale notar que o OpenSpending é basicamente uma camada de abstração para visualizar e representar esses dados - ainda precisariamos desenvolver os importers ou conectores para o sistema de gestão orçamentária em uso nas prefeituras. Um módulo de conexão com o eCidades pode ser um bom começo.

**Linguagem:** Python
**Documentação:** Média
**Comunidade:** Média
**Necessidade de customização:** Alta

### CKAN + DataHub

O combo CKAN + DataHub foi criado pela OKFN e da conta da criação de catalogos de dados abertos + repositório para esses dados.

Embora não exista lei que force os munícipios a criarem um catalogo de dados próprio - a força crescente dos dados abertos faz com que isso seja uma possibilidade.

De todos os softwares listados esse é certamente o que conta com mais implementação e experiência acumulada no Brasil. Tanto o Executivo Federal quanto o Senado já trabalham com instâncias do CKAN e existem alguns outros estados que estão em processo de implementação.

Desenvolver expertise para implementar e customizar o CKAN pode então garantir uma base de trabalho sólido e ao mesmo tempo disseminar boas práticas de gestão e armazenamento de informação pública.

**Linguagem:** Python
**Documentação:** Boa
**Comunidade:** Boa
**Necessidade de customização:** Baixa


## Passo a passo
### Passo 1 - preparação
* Escrever projeto base
* Definir os softwares
* Convidar empresas e desenvolvedores para o processo de bootstraping

### Passo 2 - bootstraping
* Rodadas de formação nos softwares selecionados
* Implementação piloto dos softwares selecionados
* Escrever material de apoio e venda

### Passo 3 - treinamento/implementação
* Convidar os desenvolvedores internos e/ou da região
* Treinamento e transferência de conhecimento das metodologias de desenvolvimento e cultura livre
* Treinamento e transferência de conhecimento do processo de desenvolvimento do software
* Implementação do software

### Passo 4 - manutenção/contribuição
* Manutenção inicial do software
* Incentivo ao retorno das melhorias do código para o repositório central
