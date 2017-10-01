# Challenge 
server.js:
```javascript
var http = require('http');
var fs = require('fs');
var url = require('url');
var server = http.createServer(function(req, res) {
    try {
        var path = url.parse(req.url, true).query;
        path = path['path'];
        if (path.indexOf("..") == -1 && path.indexOf("ＮＮ") == -1) {
            var base = "http://localhost:8080/poems/";
            var callback = function(response){
                var str = '';
                response.on('data', function (chunk) {
                    str += chunk;
                });
                response.on('end', function () {
                  res.end(str);
                });
            }
            http.get(base + path, callback).end();
        } else {
            res.writeHead(403);
            res.end("WHOA THATS BANNED!!!!");
        }
    }
    catch (e) {
        res.writeHead(404);
        res.end('Oops');
    }
});
server.listen(9999);
```

back.py:
```python 
#!/usr/bin/python
import SimpleHTTPServer
import SocketServer
PORT = 8080
Handler = SimpleHTTPServer.SimpleHTTPRequestHandler
httpd = SocketServer.TCPServer(("", PORT), Handler)
print "Serving at port", PORT
httpd.serve_forever()
```

serve.sh:
```sh 
#!/usr/bin/env bash
python back.py &
nodejs server.js
```
# Solution 
详细解答请见: [CSAW CTF 2017-Orange v1-writeup](https://chybeta.github.io/2017/09/18/CSAW-CTF-2017-Orange-v1-writeup/)

# Refference 
+ CSAW CTF 2017-Orange v1
+ [l3m0n:CSAW2017_CTF_Web_Writeup](http://www.cnblogs.com/iamstudy/articles/csaw_2017_web_writeup.html)