# DocPad: It's Like Jekyll.

DocPad (like Jekyll) is a static website generator, unlike Jekyll it's written in CoffeeScript+Node.js instead of Ruby, and also allows the template engine complete access to the document model. This means you have unlimited power as a CMS and the simplicity of a notepad.


## Huh?

1. Say you were to create the following website structure:

	> - myWebsite
		- src
			- documents
			- files
			- layouts

1. And you were to create the following files:

	- A layout at `src/layouts/default.html.eco`, which contains
		
		``` html
		<html>
			<head><title><%=@Document.title%></title></head>
			<body>
				<%-@content%>
			</body>
		</html>
		```

	- And another layout at `src/layouts/post.html.eco`, which contains:

		``` html
		---
		layout: default
		---
		<h1><%=@Document.title%></h1>
		<div><%-@content%></div>
		```

	- And a document at `src/documents/posts/hello.html.md`, which contains:

		``` html
		---
		layout: post
		title: Hello World!
		---
		Hello **World!**
		```

1. Then when you generate your website with docpad you will get a html file at `out/posts/hello.html`, which contains:

	``` html
	<html>
		<head><title>Hello World!</title></head>
		<body>
			<h1>Hello World!</h1>
			<div>Hello <strong>World!</strong></div>
		</body>
	</html>
	```

1. And any files that you have in `src/files` will be copied to the `out` directory. E.g. `src/files/styles/style.css` -> `out/styles/style.css`

1. Allowing you to easily generate a website which only changes (and automatically updates) when a document changes (which when you think about it; is the majority of websites)

1. Cool, now what was with the `<%=...%>` and `<%-...%>` parts which were substituted away?

	- This is possible because we parse the documents and layouts through a template rendering engine. The template rendering engine used in this example was [Eco](https://github.com/sstephenson/eco) (hence the `.eco` extensions of the layouts). Templating engines allows you to do some pretty nifty things, in fact we could display all the titles and links of our posts with the following:
		
		``` html
		<% for Document in @Documents: %>
			<% if Document.url.indexOf('/posts') is 0: %>
				<a href="<%= Document.url %>"><%= Document.title %></a><br/>
			<% end %>
		<% end %>
		```

1. Cool that makes sense... now how did `Hello **World!**` in our document get converted into `Hello <strong>World!</strong>`?

	- That was possible as that file was a [Markdown](http://daringfireball.net/projects/markdown/basics) file (hence the `.md` extension it had). Markdown is fantastic for working with text based documents, as it really allows you to focus in on your content instead of the syntax for formatting the document!


## Installing

1. [Install Node.js](https://github.com/balupton/node/wiki/Installing-Node.js)

1. Install dependencies
		
		npm -g install coffee-script

1. Install DocPad

		npm -g install docpad

1. _or... install the cutting edge version_

		git clone git://github.com/balupton/docpad.git
		cd docpad
		git branch -v
		npm install
		git submodule init
		git submodule update
		npm link



## Using

- Firstly, make a directory for your new website and cd into it

		mkdir my-new-website
		cd my-new-website

- To get started, simply run the following - it will run all the other commands at once
	
		docpad run

- To generate a basic website structure in the current working directory if we don't already have one

		docpad scaffold

- To regenerate the rendered website

		docpad generate

- To regenerate the rendered website automatically whenever we make a change to a file

		docpad watch

- To run the docpad server which allows you to access the generated website in a web browser

		docpad server


## Created With

### Core

- [Node.js](http://nodejs.org) - Server Side Javascript
- [Express.js](http://expressjs.com) - The "Server" in Server Side Javascript
- [Query-Engine](https://github.com/balupton/query-engine.npm) - The MongoDB Query-Engine without the Database
- [CoffeeScript](http://jashkenas.github.com/coffee-script) - JavaScript made easy
- [Caterpillar](https://github.com/balupton/caterpillar.npm) - Logging made easy
- [Bal-Util](https://github.com/balupton/bal-util.npm) - Node.js made easy
- [YAML](https://github.com/visionmedia/js-yaml) - Data made easy
- [Commander.js](https://github.com/visionmedia/commander.js) - Console apps made easy
- [Node-Growl](https://github.com/visionmedia/node-growl) - Notifications made easy


### Renderers

#### Markups

- [Markdown](http://daringfireball.net/projects/markdown/basics) to HTML `.html.md|markdown`
- [Eco](https://github.com/sstephenson/eco) to anything `.anything.eco`
- [CoffeeKup](http://coffeekup.org/) to anything `.anything.ck|coffeekup|coffee` and HTML to CoffeeKup `.ck|coffeekup|coffee.html`
- [Jade](http://jade-lang.com/) to anything `.anything.jade` and HTML to Jade `.jade.html`
- [HAML](http://haml-lang.com/) to anything `.anything.haml`

#### Styles

- [Stylus](http://learnboost.github.com/stylus/) to CSS `.css.stylus`
- [CCSS](https://github.com/aeosynth/ccss) to CSS `.css.css|coffee`

#### Scripts

- [CoffeeScript](http://jashkenas.github.com/coffee-script/) to JavaScript `.js.coffee` and JavaScript to CoffeeScript `.coffee.js`


### Extensions

DocPad is awesomely extensible, it's easy to add support for new renderers and even add funky new functionality ontop of docpad! [Check out what others are making](https://github.com/balupton/docpad/wiki/Extensions), or [learn to make your own extensions here.](https://github.com/balupton/docpad/wiki/Extending)



## Learning

[To learn more about DocPad (including using and extending it) visit its wiki here](https://github.com/balupton/docpad/wiki)


## History

- v1.2 September 29, 2011
	- Plugins now conform to a .plugin.coffee naming standard
	- Dependencies now allow for minor patches
	- Stylus Plugin
		- Added [Stylus](http://learnboost.github.com/stylus/) to CSS support
			- Uses [TJ Holowaychuk/LearnBoost's](https://github.com/learnboost) [Stylus](https://github.com/learnboost/stylus)
	- Jade Plugin
		- Added HTML to Jade support
			- Uses [Don Park's](https://github.com/donpark) [Html2Jade](https://github.com/donpark/html2jade)
	- Coffee Plugin
		- Added CoffeeCSS to CSS support
			- Uses [James Campos's](https://github.com/aeosynth) [CCSS](https://github.com/aeosynth/ccss)

- v1.1 September 28, 2011
	- Added [Buildr](http://github.com/balupton/buildr.npm) Plugin so you can now bundle your scripts and styles together :-)
	- The `action` method now supports an optional callback
		- Thanks to [#41](https://github.com/balupton/docpad/pull/41) by [Aaron Powell](https://github.com/aaronpowell)
	- Added a try..catch around the version detection to ensure it never kills docpad if something goes wrong
	- Skeletons have been removed from the repository due to circular references. The chosen skeleton is now pulled during the skeleton action. We also now perform a recursive git submodule init and update, as well as a npm install if necessary.

- v1.0 September 20, 2011
	- [Upgrade guide for v0.x users](https://github.com/balupton/docpad/wiki/Upgrading)
	- The concept of template engines and markup languages have been merged into the concept of renderers
	- Coffee Plugin
		- Added [CoffeeKup](http://coffeekup.org/) to anything and HTML to CoffeeKup support
			- Uses [Maurice Machado's](https://github.com/mauricemach) [CoffeeKup](https://github.com/mauricemach/coffeekup) and [Brandon Bloom's](https://github.com/brandonbloom) [Html2CoffeeKup](https://github.com/brandonbloom/html2coffeekup)
		- Added [CoffeeScript](http://jashkenas.github.com/coffee-script/) to JavaScript and JavaScript to CoffeeScript support
			- Uses [Jeremy Ashkenas's](https://github.com/jashkenas) [CoffeeScript](https://github.com/jashkenas/coffee-script/) and [Rico Sta. Cruz's](https://github.com/rstacruz) [Js2Coffee](https://github.com/rstacruz/js2coffee)
	- Added a [Commander.js](https://github.com/visionmedia/commander.js) based CLI
		- Thanks to [~eldios](https://github.com/eldios)
	- Added support for [Growl](http://growl.info/) notificaitons
	- Added asynchronous version comparison

- v0.10 September 14, 2011
	- Plugin infrastructure
	- Better logging through [Caterpillar](https://github.com/balupton/caterpillar.npm)
	- HAML Plugin
		- Added [HAML](http://haml-lang.com/) to anything support
			- Uses [TJ Holowaychuk's](https://github.com/visionmedia) [HAML](https://github.com/visionmedia/haml.js) support
	- Jade Plugin
		- Added [Jade](http://jade-lang.com/) to anything support
			- Uses [TJ Holowaychuk's](https://github.com/visionmedia) [Jade](https://github.com/visionmedia/jade)

- v0.9 July 6, 2011
	- No longer uses MongoDB/Mongoose! We now use [Query-Engine](https://github.com/balupton/query-engine.npm) which doesn't need any database server :)
	- Watching files now working even better
	- Now supports clean urls :)

- v0.8 May 23, 2011
	- Now supports mutliple skeletons
	- Structure changes

- v0.7 May 20, 2011
	- Now supports multiple docpad instances
	
- v0.6 May 12, 2011
	- Moved to CoffeeScript
	- Removed highlight.js (should be a plugin or client-side feature)

- v0.5 May 9, 2011
	- Pretty big clean

- v0.4 May 9, 2011
	- The CLI is now working as documented

- v0.3 May 7, 2011
	- Got the generation and server going

- v0.2 March 24, 2011
	- Prototyping with [disenchant](https://github.com/disenchant)

- v0.1 March 16, 2011
	- Initial commit with [bergie](https://github.com/bergie)


## License

Licensed under the [MIT License](http://creativecommons.org/licenses/MIT/)
Copyright 2011 [Benjamin Arthur Lupton](http://balupton.com)