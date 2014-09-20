# rabbitmq dock

## test

Testing it using another container. How about a little nodejs spice?



```
// use `docker` or `sudo docker`
$ docker run -d --name rabbit rockybars/rabbitmq
$ docker run -i -t --link rabbit:mq nodesource/node:wheezy /bin/bash
$ docker logs rabbit // peek the user:password for amqp uri

// ... inside the nodesource/node wheezy container
# mkdir test
# npm init
# npm install rabbit.js --save
```

Create following `test.js` file

```js
var port = process.env.MQ_PORT_5672_TCP_ADDR;
var addr = process.env.MQ_PORT_5672_TCP_PORT; 
var user = 'admin:password' // peek it (while you're on the host) by using `$ docker logs rabbit`
var amqp = 'amqp://' user + '@' + addr + ':' + port;

var ctx = require('rabbit.js').createContext(amqp);
ctx.on('ready', function(){
  console.log ('ready');
});
ctx.on('error', console.warn);
```

Run it

```
# node test.js
# ready
```
And you're done!

## license

MIT

