# Challenge 
```js
var express = require('express')
var app = express()

var bodyParser = require('body-parser')
app.use(bodyParser.urlencoded({}));

var path    = require("path");
var moment = require('moment');
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";

MongoClient.connect(url, function(err, db) {
    if (err) throw err;
    dbo = db.db("test_db");
    var collection_name = "users";
    var password_column = "password_"+Math.random().toString(36).slice(2)
    var password = "XXXXXXXXXXXXXXXXXXXXXX";
    // flag is flag{password}
    var myobj = { "username": "admin", "last_access": moment().format('YYYY-MM-DD HH:mm:ss Z')};
    myobj[password_column] = password;
    dbo.collection(collection_name).remove({});
    dbo.collection(collection_name).update(
        { name: myobj.name },
        myobj,
        { upsert: true }
    );

    app.get('/', function (req, res) {
        res.sendFile(path.join(__dirname,'index.html'));
    })
    app.post('/check', function (req, res) {
        var check_function = 'if(this.username == #username# && #username# == "admin" && hex_md5(#password#) == this.'+password_column+'){\nreturn 1;\n}else{\nreturn 0;}';

        for(var k in req.body){
            var valid = ['#','(',')'].every((x)=>{return req.body[k].indexOf(x) == -1});
            if(!valid) res.send('Nope');
            check_function = check_function.replace(
                new RegExp('#'+k+'#','gm')
                ,JSON.stringify(req.body[k]))
        }
        var query = {"$where" : check_function};
        var newvalue = {$set : {last_access: moment().format('YYYY-MM-DD HH:mm:ss Z')}}
        dbo.collection(collection_name).updateOne(query,newvalue,function (e,r){
            if(e) throw e;
            res.send('ok');
            // ... implementing, plz dont release this.
        });
    })
    app.listen(8081)

});
```


# Refference
+ [0ctf 2018 Loginme]
