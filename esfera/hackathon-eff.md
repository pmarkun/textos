# TOSBack Hackathon: How to Make a Rule Contribution

*This is a re-posting of a guide by TOSBack developer Jimm Stout*

TOSBack is an open-source project that aims to assist users around the world by tracking the changes to Terms of Service (TOS) and other policies on the web, but we need some help to bring it back to life! We are hosting a [hackathon][1] at Campus Party Brazil later this week to give the project a healthy revamp. The project uses Rails and we'd love people to contribute code. But if you aren't a Rails developer, you can still contribute by submitting rules and letting us know which policies are important to you. This is a developers' guide for submitting new policies for TOSBack to crawl. If you want to get started as quickly as possible, you can scroll down to the "Putting it all together" section below.

The code is hosted on Github. Here is the most up to date version: https://github.com/JimmStout/ToSBack3

## What you will need

* A browser (Chrome, Firefox, Safari)  
* A text editor to like TextEdit/TextMate (Mac), Notepad (Windows), or Emacs (all platforms) to modify the XML files.  
* Make sure you have these installed in order to test your rules:
** [Git][2] and a [Github][3] account.
** Ruby 1.9.3 - the version is important
* The proper gems:

      gem install nokogiri mechanize sanitize

Note that you may need to install dependencies before installing these gems.
* (Optional) If you use Firefox, the [Firebug][4] extension adds functionality to the browser's developer tools.

**Take a look at TOSBack's XML structure**

The app scans a set of XML files that define attributes for sites and policies. Then, it uses those attributes to find the policy, look for changes, and store new versions. Here's an example from the current [rules][5]:

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

**Curious about the content? (The elements and attributes)**

* Each site has its own XML file ("500px.com.xml") in the rules directory, and a "sitename" element.  
* name: The name attribute shouldn't include "http://" and is just used to organize the policies.  
* Nested beneath the "sitename" element should be one or more "docname" elements  
* name: Make sure your docname's name is present and doesn't have any [strange characters][6].  
* Nested beneath that is the "url" element and its attributes:  
* name: Encode your ampersands and use the \*full\* URL! This is the location that TOSBack's scraper will visit to find the policy and if the site is owned by another company, it may not match the "sitename". "http://fullyQualifiedDomainName.com/includes?request=true&lang=EN"  
* [xpath][7]: Use single quotes inside the brackets, and check the section below.  
* lang: Find the *two character* [language code][8].

**Still confused about the XPath?**

XPath defines where the policy is nested on the page, and allows us to strip away unrelated content from the policy (ads, related articles, and etc.) In this example, the policy at http://www.500px.com/terms has an XPath of "//div[@id='terms']". Here's a snippet from their source to give you an idea:

	<div id="terms" class="col d1 rounded shadow filled">
	  <div class="intro">
	    ...
	  </div>
	  <div class="left-legal">
	    ...
	  </div>
	</div>       

Since the policy exists only in elements nested below the div tag with the id of "terms", we can extract it with XPath and ignore the headers, footers, and etc.

**Putting it all together**

1. Clone the current git repo to your local machine:

> git clone https://github.com/JimmStout/ToSBack3.git

2. Identify the website for which you want to add a rule. Search through the "rules" directory to make sure the website is not already present.  
3. Visit the site and find its terms of service and privacy policies. The footer of the site is a great place to find the link, but you may have to really dig!  
4. Finding the XPath is a complicated subject, and if you aren't familiar with the syntax, it might be pretty confusing at first. Take a moment on [W3Schools][7] and look at the XPath section above.  
5. Save your new XML file and add it to your project: git add example.co.uk.xml

6. Switch to the rubycode directory and test your new rule by passing it  
as an argument to tosback.rb:

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

7. Make sure you run "git pull" often to ensure your code is up to date.

8. Commit often, descriptively, and in the [present tense][9]:

> git commit -m "Add new rule for example.co.uk"

9. Pull once more and merge if needed, and recommit. Then push to a github repository to which you have access, and submit a pull request to the master repo with your additions.  
10. Add some more rules!

**If you need some help...**

It may seem very difficult if you're just starting out, but if your policy requires a tricky XPath attribute or if you just need help remembering which git command to use next, get on our IRC channel #tosback on irc.oftc.net and we'll be happy to help you!

 [1]: https://www.eff.org/deeplinks/2013/01/campus-party-brazil-hackathon-liberty-enhancing-tech-project-tosback
 [2]: https://help.github.com/articles/set-up-git
 [3]: https://github.com/
 [4]: https://www.getfirebug.com/
 [5]: https://github.com/JimmStout/tosback2/tree/master/rules
 [6]: http://jimmstout.com/2012/troubleshooting-tosback-file-handling/
 [7]: http://www.w3schools.com/xpath/xpath_syntax.asp
 [8]: http://en.wikipedia.org/wiki/List_of_ISO_639-1_codes
 [9]: http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html  