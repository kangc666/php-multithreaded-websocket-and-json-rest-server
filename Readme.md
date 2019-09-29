The aim of this project is to ease the life of a developer by providing a simple architecture in which he can write code for JSON Rest based API (object oriented way) and serve it through either a URL or a multi-threaded web socket server. So you have the option to write API code once and then serve/use in multiple ways. The web-socket server in this project is purely built in PHP 7.2 using STREAM SOCKET, PTHREADS version 3.16 or higher. 

Please note that the Multithreading with PTHREADS v3.16 and PHP 7.2 is very very stable unlike the older versions of PHP and Pthreads.

Following is the simplest example for an API class

PHP Code:
Create a class named Mathematics by inheriting it from ServiceBase

class Mathematics extends ServiceBase {

  public function add($num1,$num2) {
      return $num1 + $num2;
  }
 
}

Thats all, what we did in above code is that we created the code to server it over JSON REST API or WebSockets.

1) Calling add method from URL:

http://localhost/myproject/service?a=Mathematics:add&p=[2,5]

Url in this format shall instentiate Matematics class using Reflection and call the method "add" with parameters. Whatever the add returns, send it in response. 

2) Calling "add" method through web-sockets

First of all run batch file "ServiceOnWebSocketThreaded.bat" this will run a PHP file in CLI mode. This is our multi-threaded websocket server.

Create an HTML file on your server or edit existing and add the following javascript code in it to call the api through web socket message.

        var ws = new WebSocket("ws://127.0.0.1:8088");
        ws.onopen = function () {
            console.log('connected');
            
            // Call the API by ending a message
            var cmd = {a:"Mathematics:add",p:[20,30]};
            ws.send(JSON.stringify(cmd));
        };

        ws.onerror = function (e) {
            console.log(e.data);
        };

        ws.onmessage = function (e) {
            // Result shall be received here
            alert(e.data);
        };

        ws.onclose = function (e) { };


Over a successfull call the API shall send a response like this:
{"status":true,"data":"50"}

Over a faulty call the API shall send a response like this:
{"status":false,"msg":"Invalid api/method information"}

The response shall always be a JSON encoded string where status=true represents there wasn't any error and you can fetch results from data attribute. So first thing is that you must decode (JSON.parse) the message received in "e.data" in "onmessage" event.











