# **artTemplate**

A state-of-the-*art template* engine in JavaScript.

* **Clean, concise code!** *< 3kb non-gzipped; < 2kb gzipped; no dependencies.*
* **Lightning fast!** *Up to 20-30 times faster than "Mustache" and alikes.*
* **Relevant features built-in!** *Runtime debugging, `include` statements for template components, pre-compiled templates and more.* 
* **State-of-the-art technology!** *Supports server-side environments, such as Node.JS (+ Express), module loaders like RequireJS and any modern web browser.* 

<hr>

## Table of Contents

* Features / Unique Selling Points (USPs)
* Downloads
* Quick Start
 * Write Templates
 * Render Templates
* Template Syntax
 * Concise Syntax
 * Native Syntax
* Pre-compiled Templates
* Methods
* NodeJS
 * Installation
 * Usage
 * Configuration
 * NodeJS + Express
 * Interface Changes
* Upgrade Notice
* Changelog
* License

<hr>

## Features / USPs

* Performance - code executes 20-30 times faster compared to [*Mustache*](http://mustache.github.io/), [*tmpl*](https://blueimp.github.io/JavaScript-Templates/) and alikes ([performance benchmark tests](https://aui.github.com/artTemplate/test/test-speed.html)).
* Security - all input is automatically being escaped and compiled code is being executed within a sandbox envoirement on the server only. (Don't worry about user input!)
* NodeJS + Express support.
* Support for runtime debugging, you can pinpoint where the statement is unusual template ([tech slides](http://aui.github.io/artTemplate/demo/debug.html)).
* Support for `include` statements (!). :)
* Support for pre-compiled templates (can be converted into a very streamlined template js file).
* Can be achieved by the browser to load the template path (details).
* Template concise statement, without prefix reference data, there are concise version of the native syntax versions available.
* Supported by all modern mobile and desktop browsers.

<hr>

## Downloads

There are two files, one for the concise and another one for the native template syntax.

* [latest template.js](https://raw.github.com/eyecatchup/artTemplate/master/dist/template.js) *(Concise Engine, 2.7kb)* 
* [latest template-native.js](https://raw.github.com/eyecatchup/artTemplate/master/dist/template-native.js) *(Native Engine, 2.3kb)*

<hr>

## Quick Start

### Write Templates

Use a `type="text/html"` on `script` tags to make it a template. For example:

```html	
	<script id="test" type="text/html">
	<h1>{{title}}</h1>
	<ul>
	    {{each list as value i}}
	        <li>Index {{i + 1}} ：{{value}}</li>
	    {{/each}}
	</ul>
	</script>
```

### Render Templates

```html		
	var data = {
		title: 'tag',
		list: ['literary', 'blog', 'Photography', 'movie', 'folk', 'travel', 'Guitar']
	};
	var html = template('test', data);
	document.getElementById('content').innerHTML = html;
```

[Demo](http://eyecatchup.github.com/artTemplate/demo/basic.html)

<hr>

## Template Syntax

artTemplate supports two types of template syntax styles.

###	Concise Syntax

Recommended. The syntax is simple and practical to write, and also easier to read for the most.

```html	
	{{if admin}}
		{{include 'admin_content'}}
		
		{{each list}}
			<div>{{$index}}. {{$value.user}}</div>
		{{/each}}
	{{/if}}
```
[Read concise syntax docs (Link).](https://github.com/aui/artTemplate/wiki/syntax:simple)

###	Native Syntax

```html		
	<%if (admin){%>
		<%include('admin_content')%>
	
		<%for (var i=0;i<list.length;i++) {%>
			<div><%=i%>. <%=list[i].user%></div>
		<%}%>
	<%}%>
```
[Read native syntax docs (Link).](https://github.com/aui/artTemplate/wiki/syntax:native)

<hr>

## Pre-compiled Templates

Can break through the browser restrictions, so the front template with the same back-end template Sync "file" Load capacity:

First, according to the template file and directory organization `template('tpl/home/main', data)`.
Second, the introduction of sub-template template support `{{include '../public/header'}}`.

Thanks to pre-compiled templates,

* templates can be converted into a very streamlined js file (independent of the engine)
* templates can be loaded using asynchronous interfaces
* modules support a variety of output formats: AMD, CMD, CommonJS
* GruntJS plugins can be build off of them with ease
* front-end templates/components can be shared with the (NodeJS) back-end
* templates can be compressed automatically while packaging
* Pre-compilation tools: [TmodJS](https://github.com/aui/tmodjs/)

<hr>

## Methods

### `template(elemId, templateData)`

According elemId rendering template. Internal according document.getElementById(elemId) find template.

If the `data` parameter is left undefined, return data is the result of the render function.

### `template.compile(templateObj, templateOpts)`

Returns a rendering function. For an example, see the [this demo](http://aui.github.com/artTemplate/demo/compile.html).

### `template.render(templateObj, templateOpts)`

Returns the rendering results.

### `template.helper(fnName, cbName)`

Add a secondary method. For an example, see the [time format demo](http://aui.github.com/artTemplate/demo/helper.html).

### `template.config(field, value)`

Set template engine configuration option (overwrites defaults).

||Field	||Type	||Default Value	||Description||
|openTag	|String	|'{{'	|Logical syntax opening tag.|
|closeTag	|String	|"}}"	|Logical syntax closing tag.|
|escape	|Boolean	|true	|Whether to encode HTML output characters.|
|cache	|Boolean	|true	|Whether to open the cache (depending on options of the filename field).|
|compress	|Boolean	|false	|Whether to remove extra whitespaces in HTML.|

<hr>

## NodeJS

### Installation

	`npm install art-template`

### Usage
```js
	var template = require('art-template'); 
	
	var data = {
		list: ["aui", "test"]
	}; 
	var html = template(__dirname + '/index/main', data); 
```	
### Configuration

NodeJS version adds the following default configuration:

||Field	||Type	||Default Value	||Description||
|base	|String	|''	|Specified template directory
|extname	|String	|'.html'	|Specify the template extension
|encoding	|String	|'utf-8'	|Specify template coding

Configuring base template specified template directory path can be shortened, and can be avoided include statements leapfrog access any path caused safety problems, such as:

	template.config('base', __dirname); var html = template('index/main', data) 

### NodeJS + Express

 var template = require('art-template'); template.config('base', ''); template.config('extname', '.html'); app.engine('.html', template.__express); app.set('view engine', 'html'); //app.set('views', __dirname + '/views'); 
Run demo:

 node demo/node-template-express.js 
If you are using js native syntax as a template syntax, use require('art-template/node/template-native.js')
Upgrade Reference

To adapt NodeJS express, artTemplate v3.0.0 interface to adjust.

### Interface Changes

Concise syntax used by default
template.render() method is no longer the first parameter is id, but the template string
Use the new configuration interface template.config() and field names have modified
template.compile() method does not support the id parameter
helper methods are no longer mandatory text output, depending on whether the coding template statement
template.helpers of $string , $ $escape , $ $each have migrated to template.utils in
template() method does not support passing directly compiled template

## Upgrade Notice

If you want to continue using js native language syntax as a template, use the template-native.js
Find items template.render Replace template
Use template.config(name, value) to replace the previous configuration
template() method directly into the template instead template.compile() (v2 early version)

## Changelog

### v3.0.3

* Solved template.helper() method incoming data is converted to a string of issue #96
* Solved {{value || value2}} is recognized as a pipeline problem statement #105 https://github.com/aui/tmodjs/issues/48

### v3.0.2

* Pipeline syntax must solve the problem of using space-separated

### v3.0.1

* Adaptation express 3.x and 4.x, repair path BUG

### v3.0.0

* Add NodeJS-exclusive version. Also, add support for: 
  1.) relative paths, 2.) path templates and 3.) templates include statements. 
* Adapt/merge express framework goodies.
* ?Built- print statement supports multiple parameters passed
* Add support for a global cache configuration.
* Concise syntax supports pipeline style helper call - for example `{{time | dateFormat:'yyyy MM dd hh:mm:ss'}}`.
* Due to changes on the interface, please read the [upgrade reference](https://github.com/aui/artTemplate).

artTemplate precompiled tool TmodJS updated

### v2.0.4

May generate a syntax error fixes low Andrews browser version compiled (because this version of the browser js engines exist BUG)

### v2.0.3

Helper methods to optimize performance
NodeJS users can access artTemplate through npm: $ npm install art-template -g
Does not recommend the use of the escape output statement <%=#value%> (compatible with versions prior to v2.0.3 of <%==value%> ), but you can use the simple version of the syntax {{#value}}
Provide simple version syntax merged version dist / template-simple.js

### v2.0.2

Optimized custom syntax extensions, reduce the volume
[Important] in order to maximize compatible third-party libraries, custom syntax extensions modify the default delimiter {{ and }} .
Repair merge tool BUG # 25
Discloses the internal cache, you can template.cache access to the compiled function
Helper methods disclosed cache, you can template.helpers access to
Optimized debugging information
v2.0.1

Repair template variables static analysis BUG

### v2.0 release

Compiler tool renamed atc, become artTemplate subprojects separate maintenance: https://github.com/cdc-im/atc

### v2.0 beta5

Repair compiler tools may duplicate dependency problems. Thankswarmhug
Repair precompiled include internal implementation context inconsistencies may arise. Thankswarmhug
Compilation tools support the use of drag and drop files compiled separately

### v2.0 beta4

Repair compilation tools in the compressed HTML template may lead to an unexpected truncation problem. Thankswarmhug
Perfect compilation tools include support for support, you can support between different directories nested template
Repair compiler tool did not properly handle self-assisted method definition syntax plugin

### v2.0 beta1

Non-String, Number data type is not output, while the Function type evaluation output.
The default output for html escape, the original output can use <%==value%> (Note: v2.0.3 is recommended to use <%=#value%> ), you can turn off the default escape function template.defaults.escape = false .
Increasing the batch tool support is compiled into the template does not rely on a template engine js file that can be loaded asynchronously via RequireJS, SeaJS modules loader.

## License

Released under the MIT, BSD, and GPL Licenses

[All demo examples](http://aui.github.io/artTemplate/demo/index.html) | [engine principle (in chinese)](http://cdc.tencent.com/?p=5723)

© tencent.com