# TOSBack Hackathon: Como contribuir corretamente

*Esta é uma tradução do guia produzido pelo Jimm Stout desenvolvedor do TOSBack*

TOSBack é um projeto de código aberto que tem como objetivo auxiliar os usuários em todo o mundo, acompanhando as mudanças com os Termos de Serviço (TOS) e de outras políticas na web, mas precisamos de alguma ajuda para faze-lo funcionar! Estamos organizando um [hackathon] [1] na Campus Party Brasil ainda esta semana para trabalhar no projeto. O projeto usa Rails e nós adoraríamos encontrar pessoas que possam contribuir com código. Mas se você não for um desenvolvedor Rails, você ainda pode contribuir enviando termos de uso e apontando quais políticas são importantes para você. Este é um guia para desenvolvedores sobre como enviar novos termos para o TOSBack rastrear. Se você quer começar o mais rápido possível, você pode ir direto para a "Fazendo funcionar" logo abaixo.

Esse código esta hospedado no GitHub. Aqui você encontra a última versão: https://github.com/JimmStout/ToSBack3

## O que você precisa

* Um browser (Chrome, Firefox, Safari)  
* Um editor de texto TextEdit/TextMate (Mac), Notepad (Windows), or Emacs (all platforms) para modificar os arquivos XML.  
* Para testar as configurações certifique-se de ter instalado:
** [Git][2] e uma conta no [Github][3].
** Ruby 1.9.3 - essa versão é importante
* As gems corretas:

      gem install nokogiri mechanize sanitize

Talvez você precise instalar algumas dependencias antes de instalar essas gems.
* (Opcional) Se você usa Firefox, a extensão [Firebug][4] adiciona funcionalidades úteis.

**Compreende a estrutura XML do TOSBack**

O aplicativo lê uma série de arquivos XML que define a configuração dos sites e dos termos de uso. Ele então usa essas configurações para encontrar o termo de uso, procura por mudanças e armazena as novas versões. Aqui um exemplo das [configurações][5] atuais:

	<sitename name="500px.com">
	  <docname name="Terms of Service">
	    <url name="http://500px.com/terms" xpath="//div[@id='terms']" lang="EN">
	      <norecurse name="arbitrary"/>
	    </url>
	  </docname>
	  <docname name="Privacy Policy">
	    <url name="http://www.500px.com/privacy" xpath="//div[@id='terms']" lang="EN">
	      <norecurse name="arbitrary"/>
	    </url>
	  </docname>
	</sitename>        

**Curioso sobre o conteúdo? (Os elementos e atributos)**

* Cada site tem seu próprio aqruivo XML ("500px.com.xml") na paste de configurações (rules), e um elemento "sitename".  
* name: O atributo nome não deve incluir o "http://" e deve ser usado apenas para organizar os termos de uso.  
* Abaixo do elemento "sitename" deve haver um ou mais elementos "docname"
* name: Certifique-se que o "name" do "docname" exista e não tenha nenhum [caracter estranho][6].  
* Abaixo disso esta o elemento "url" e esses atributos:  
* name: Codifique os "e comercial" (&) e use a url \*completa\*! Essa é a localização que o scraper do TOSBack vai visitar para encontrar o termo de uso. E caso o site seja de outra empresa, talvez ele não bata com o "sitename". "http://fullyQualifiedDomainName.com/includes?request=true&lang=EN"  
* [xpath][7]: Use aspas simples dentro dos colchetes e cheque a seção abaixo.  
* lang: Encontre os *dois caracteres* [language code][8].

**Ainda confuso sobre o XPath?**

XPath define onde o termo de uso esta armazenado na pagina, e permite remover conteúdo não relacionados com o termo de uso (anúncios, artigos relacionados, etc.) Nesse exemploe, o termo de uso de http://www.500px.com/terms tem o XPath "//div[@id='terms']". Aqui esta um pedaço do código fonte para te dar uma ideia:

	<div id="terms" class="col d1 rounded shadow filled">
	  <div class="intro">
	    ...
	  </div>
	  <div class="left-legal">
	    ...
	  </div>
	</div>       

Como o termo de uso só existe nos elementos que estão abaixo da div com id "terms", nós podemos extrair ela com um XPath e ignorar cabeçalhos, rodapés, etc.

**Fazendo funcionar**

1. Clone o repositório git na sua maquina:

> git clone https://github.com/JimmStout/ToSBack3.git

2. Encontre o site para o qual você quer adicionar uma configuração. Procure na pasta de configurações (rules) para ver se esse site não esta presente.  
3. Visite o site e encontre o seu termo de serviço e políticas de privacidade. O rodapé do site é um bom local para encontrar esse link, mas talvez você tenha que procurar bem!  
4. Encontrar o XPath é uma assunto complicado, se você não esta acostumado com a sintaxe pode ser bem confuso. De uma olhada no [W3Schools][7] e leia seção sobre XPath acima.  
5. Salve o novo XML e adicione ao seu projeto:

> git add example.co.uk.xml

6. Mude para o diretório rubycode e teste sua nova configuração passando ele como elemento para o tosback.rb:

> cd rubycode  
> rubycode$ ruby tosback.rb ../rules/500px.com.xml
> 
> The following document outlines the terms of use of the  
> 500px website.  
> You can also review our Privacy policy, which outlines our  
> practices towards handling any personal information that you may provide  
> to us.
> 
> Before using any of the 500px services, you are required to  
> read, understand and agree to these terms.  
> You may only create an account after reading and accepting these  
> terms.

7. Lembre-se de dar um "git pull" regularmente para ter certeza de que o código esta atualizado.

8. Comite frequentemente, descreve as mudanças e use [verbos no presente]:

> git commit -m "Adiciona uma configuração para exemplo.com.br"

9. Faça outro pull e dê um merge se necessário e então comite novamente. Faça um push para o repositório no github ao qual você tenha acesso e então submeta um pull request para o repositório master com suas contribuições.  
10. Adicione mais configurações!

**Se você precisar de ajuda...**

Pode ser complicado se você estiver apenas começando, mas se seu termo de uso necessitar de um XPath complicado ou se você precisar de ajudar para lembrar qual comando git deve usar na sequencia, entre no nosso canal de IRC #tosback na rede irc.oftc.net e nós ficaremos felizes em ajudar!

 [1]: https://www.eff.org/deeplinks/2013/01/campus-party-brazil-hackathon-liberty-enhancing-tech-project-tosback
 [2]: https://help.github.com/articles/set-up-git
 [3]: https://github.com/
 [4]: https://www.getfirebug.com/
 [5]: https://github.com/JimmStout/tosback2/tree/master/rules
 [6]: http://jimmstout.com/2012/troubleshooting-tosback-file-handling/
 [7]: http://www.w3schools.com/xpath/xpath_syntax.asp
 [8]: http://en.wikipedia.org/wiki/List_of_ISO_639-1_codes
 [9]: http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html  