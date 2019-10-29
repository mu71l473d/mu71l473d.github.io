---
layout: post
title: Kiwa Workshop
---

Het is nu tijd om te starten. Maar allereerst nog even:

#### [Regels Kiwa Workshop](#regels)
- Je hebt tot 14:45 om de challenges op te lossen. Daarna zal ik nog even wat dingen vertellen.
- Je mag geen DoS/DDoS uitvoeren op de Kiwa Workshop
- Je mag niet instanties van andere teams bekijken, bewerken, aanvallen o.i.d. Ook andere manieren om teams lastig te vallen is niet toegestaan.
- De broncode van bkimminich en mu71l473d zijn tijdens de challenge off-limits, ook het zoeken naar antwoorden is niet toegestaan.
- Het gebruik van een vulnerability scanner is niet de bedoeling. Het is de bedoeling dat je zelf op zoek gaat.
- Je mag niet de database aanpassen zodat je alle challenges hebt opgelost.
- Vragen staat vrij! De spelleider probeert niet te veel weg te geven.
- Het is verder wel toegestaan om je teamlid of de spelleider te vragen naar het antwoord.
- Ook mag je algemene informatie zoeken over security tests en hackmethodes.
- Have fun!



#### [Het doel van de workshop](#doel)
De kiwa workshop bevat verschillende uitdagingen met verschillende moeilijkheidsgraden waarbij het doel is om de onderliggende beveiligingsproblemen te herkennen en er misbruik van te maken om challenges op te lossen. Deze kwetsbaarheden zijn opzettelijk in de applicatie geplant voor precies dat doel, maar op een manier die ook overeenkomt met "real-life" webontwikkeling. De Webshop is namelijk gebaseerd op de OWASP Web top 10 lijst.

De voortgang wordt bijgehouden door de applicatie met behulp van pushmeldingen voor succesvolle exploits en een scorebord voor het voortgangsoverzicht. Het vinden van dit scorebord is een van de (gemakkelijkste) uitdagingen. Het idee achter deze workshop is om gamification-technieken te gebruiken om de teams te motiveren om zoveel mogelijk uitdagingen op te lossen, en het strijden tegen andere teams. 


LET OP: HET VERVOLG GAAT DOOR IN HET ENGELS, EN IS EEN SAMENVATTING VAN PWNING-JUICE-SHOP


### Architecture overview

The OWASP Juice Shop is a pure web application implemented in JavaScript
and TypeScript (which is compiled into regular JavaScript). In the
frontend the popular [Angular](https://angular.io/) framework is used to
create a so-called _Single Page Application_. The user interface layout
is implementing Google's [Material Design](https://material.io/) using
[Angular Material](https://material.angular.io/) components. It uses
[Angular Flex-Layout](https://github.com/angular/flex-layout) to achieve
responsiveness. All icons found in the UI are originating from the
[Font Awesome](https://fontawesome.com) library.

JavaScript is also used in the backend as the exclusive programming
language: An [Express](http://expressjs.com) application hosted in a
[Node.js](https://nodejs.org) server delivers the client-side code to
the browser. It also provides the necessary backend functionality to the
client via a RESTful API. As an underlying database a light-weight
[SQLite](https://www.sqlite.org) was chosen, because of its file-based
nature. This makes the database easy to create from scratch
programmatically without the need for a dedicated server.
[Sequelize](http://docs.sequelizejs.com) and
[finale-rest](https://www.npmjs.com/package/finale-rest) are used as an
abstraction layer from the database. This allows to use dynamically
created API endpoints for simple interactions (i.e. CRUD operations)
with database resources while still allowing to execute custom SQL for
more complex queries.

As an additional data store a [MarsDB](https://github.com/c58/marsdb) is
part of the OWASP Juice Shop. It is a JavaScript derivate of the widely
used [MongoDB](https://www.mongodb.com) NoSQL database and compatible
with most of its query/modify operations.

The push notifications that are shown when a challenge was successfully
hacked, are implemented via
[WebSocket Protocol](https://tools.ietf.org/html/rfc6455). The
application also offers convenient user registration via
[OAuth 2.0](https://oauth.net/2/) so users can sign in with their Google
accounts.

The following diagram shows the high-level communication paths between
the client, server and data layers:

![Architecture overview diagram](img/architecture-diagram.png)

### Browser

When hacking a web application a good internet browser is mandatory. The
emphasis lies on _good_ here, so you do _not_ want to use Internet
Explorer. Other than that it is up to your personal preference. Chrome
and Firefox both work fine from the authors experience.

#### Browser development toolkits

When choosing a browser to work with you want to pick one with good
integrated (or pluggable) developer tooling. Google Chrome and Mozilla
Firefox both come with powerful built-in _DevTools_ which you can open
via the `F12`-key.

When hacking a web application that relies heavily on JavaScript, **it
is essential to your success to monitor the _JavaScript Console_
permanently!** It might leak valuable information to you through error
or debugging logs!

Other useful features of browser DevTools are their network overview as
well as insight into the client-side JavaScript code, cookies and other
local storage being used by the application.

#### Tools for HTTP request tampering

[Tamper Chrome](https://chrome.google.com/webstore/detail/tamper-chrome-extension/hifhgpdkfodlpnlmlnmhchnkepplebkb)
lets you monitor and - more importantly - modify HTTP requests _before_
they are submitted from the browser to the server.

Mozilla Firefox has built-in tampering capabilities and does not need a
plugin. On the _Network_ tab of Firefox's DevTools you have the option
to _Edit and Resend_ every recorded HTTP request.

Tampering is extremely useful when probing for holes in the server-side
validation logic. It can also be helpful when trying to bypass certain
input validation or access restriction mechanisms, that are not properly
checked _on the server_ once more.

An API testing plugin like
[PostMan](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop)
for Chrome allows you to communicate with the RESTful backend of a web
application directly. Skipping the UI can often be useful to circumvent
client-side security mechanisms or simply get certain tasks done faster.
Here you can create requests for all available HTTP verbs (`GET`,
`POST`, `PUT`, `DELETE` etc.) with all kinds of content-types, request
headers etc.

If you feel more at home on the command line, `curl` will do the trick
just as fine as the recommended browser plugins.

#### Scripting tools

A small number of challenges is not realistically solvable manually
unless you are cheating or are incredibly 🍀-lucky.

For these challenges you will require to write some scripts that for
example can submit requests with different parameter values
automatically in a short time. As long as the tool or language of choice
can submit HTTP requests, you should be fine. Use whatever you are most
familiar with.

If you have little experience in programming, best pick a language that
is easy to get into and will give you results without forcing you to
learn a lot of syntax elements or write much _boilerplate code_. Python,
Ruby or JavaScript give you this simplicity and ease-of-use. If you
consider yourself a "command-line hero", Bash or PowerShell will get the
job done for you. Languages like Java, C# or Perl are probably less
suitable for beginners. In the end it depends entirely on your
preferences, but being familiar with at least one programming language
is kind of mandatory if you want to get 100% on the score board.

> In computer programming, boilerplate code or boilerplate refers to
> sections of code that have to be included in many places with little
> or no alteration. It is often used when referring to languages that
> are considered verbose, i.e. the programmer must write a lot of code
> to do minimal jobs.[^1]

### Penetration testing tools

You _can_ solve all challenges just using a browser and the
plugins/tools mentioned above. If you are new to web application hacking
(or penetration testing in general) this is also the _recommended_ set
of tools to start with. In case you have experience with professional
pentesting tools, you are free to use those! And you are _completely
free_ in your choice, so expensive commercial products are just as fine
as open source tools. With this kind of tooling you will have a
competitive advantage for some of the challenges, especially those where
_brute force_ is a viable attack. But there are just as many
multi-staged vulnerabilities in the OWASP Juice Shop where - at the time
of this writing - automated tools would probably not help you at all.

In the following sections you find some recommended pentesting tools in
case you want to try one. Please be aware that the tools are not trivial
to learn - let alone master. Trying to learn about the web application
security basics _and_ hacking tools _at the same time_ is unlikely to
get you very far in either of the two topics.

#### Intercepting proxies

An intercepting proxy is a software that is set up as _man in the
middle_ between your browser and the application you want to attack. It
monitors and analyzes all the HTTP traffic and typically lets you
tamper, replay and fuzz HTTP requests in various ways. These tools come
with lots of attack patterns built in and offer active as well as
passive attacks that can be scripted automatically or while you are
surfing the target application.

The open-source
[OWASP Zed Attack Proxy (ZAP)](https://www.owasp.org/index.php/OWASP_Zed_Attack_Proxy_Project)
is such a software and offers many useful hacking tools for free:

> ZAP is an easy to use integrated penetration testing tool for finding
> vulnerabilities in web applications. It is designed to be used by
> people with a wide range of security experience and as such is ideal
> for developers and functional testers who are new to penetration
> testing. ZAP provides automated scanners as well as a set of tools
> that allow you to find security vulnerabilities manually.[^2]



#### Pentesting Linux distributions

Instead of installing a tool such as ZAP on your computer, why not take
it, add _several hundred_ of other offensive security tools and put them
all into a ready-to-use Linux distribution? Entering
[Kali Linux](https://www.kali.org) and similar toolboxes:

> Kali Linux is a Debian-based Linux distribution aimed at advanced
> Penetration Testing and Security Auditing. Kali contains several
> hundred tools aimed at various information security tasks, such as
> Penetration Testing, Forensics and Reverse Engineering.[^3]

The keyword in the previous quote is _advanced_! More precisely, Kali
Linux is _easily overwhelming_ when beginners try to work with it, as
even the Kali development team states:

> As the distribution’s developers, you might expect us to recommend
> that everyone should be using Kali Linux. The fact of the matter is,
> however, that Kali is a Linux distribution specifically geared towards
> professional penetration testers and security specialists, and given
> its unique nature, it is __NOT__ a recommended distribution if you’re
> unfamiliar with Linux \[...\]. Even for experienced Linux users, Kali
> can pose some challenges.[^4]

Although there exist some more light-weight pentesting distributions,
they basically still present a high hurdle for people new to the IT
security field. If you still feel up to it, give Kali Linux a try!

### Internet

You are free to use Google during your hacking session to find helpful
websites or tools. The Kiwa Workshop is leaking useful information
all over the place if you know where to look, but sometimes you simply
need to extend your research to the Internet in order to gain some
relevant piece of intel to beat a challenge.

### Source code

Juice Shop is supposed to be attacked in a "black box" manner. That
means you cannot look into the source code to search for
vulnerabilities. As the application tracks your successful attacks on
its challenges, the code must contain checks to verify if you succeeded.
These checks would give many solutions away immediately.

The same goes for several other implementation details, where
vulnerabilities were arbitrarily programmed into the application. These
would be obvious when the source code is reviewed.

Finally the end-to-end test suite of Juice Shop was built hack all
challenges automatically, in order to verify they can all be solved.
These tests deliver all the required attacks on a silver plate when
reviewed.

### GitHub repository

While stated earlier that "the Internet" is fine as a helpful resource,
consider the GitHub repositories https://github.com/bkimminich/juice-shop
and https://github.com/mu71l473d/juice-shop as entirely off limits. 
First and foremost because it contains the source code (see above).

Additionally it hosts the issue tracker of the project, which is used
for idea management and task planning as well as bug tracking. You can
of course submit an issue if you run into technical problems that are
not covered by the [Troubleshooting section of the README.md](). You
just should not read issues labelled `challenge` as they might contain
spoilers or solutions.

Of course you are explicitly allowed to view
[the repository's README.md page](https://github.com/bkimminich/juice-shop/blob/master/README.md),
which contains no spoilers but merely covers project introduction, setup
and troubleshooting. Just do not "dig deeper" than that into the
repository files and folders.

### Database table `Challenges`

The challenges (and their progress) live in one database together with
the rest of the application data, namely in the `Challenges` table. Of
course you could "cheat" by simply editing the state of each challenge
from _unsolved_ to _solved_ by setting the corresponding `solved` column
to `1`. You then just have to keep your fingers crossed, that nobody
ever asks you to _demonstrate how_ you actually solved all the 4- and
5-star challenges so quickly.

### Configuration REST API Endpoint

The Juice Shop offers a URL to retrieve configuration information which
is required by the customization feature that allows
redressing the UI and overwriting the product catalog:
<http://localhost:3000/rest/admin/application-configuration>

The returned JSON contains spoilers for all challenges that depend on a
product from the inventory which might be customized. As not all
customization can be prepared on the server side, exposing this REST
endpoint is unavoidable for the customization feature to work properly.

### Score Board HTML/CSS

In the current context it is suffice to say, that you could manipulate
the score board in the web browser to make challenges _appear as
solved_. Please be aware that this "cheat" is even easier (and more
embarrassing) to uncover in a classroom training than the previously
mentioned database manipulation: A simple reload of the score board URL
will let all your local CSS changes vanish in a blink and reveal your
_real_ hacking progress.

    
    
    