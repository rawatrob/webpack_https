<h1>Enable https/ssl in Webpack-dev-server  with Reactjs OR Node OR Angular Project</h1>

By default webpack-dev-server will generate a self-signed, 2048 bit, sha256 SSL Certificate, which is used to enable https. The certificate will be located in the ssl directory after the server is started. which is good for 30 day.. And re-generates that certificate several times per day, and each time the certificate needs to be re-accepted in the browser

We highly recommend creating and managing your own certificates using below command:
<pre>
openssl genrsa -out private.key 4096

openssl req -new -sha256 -out private.csr -key private.key

openssl x509 -req -days 3650 -in private.csr -signkey private.key -out private.crt -extensions req_ext

openssl x509 -in private.crt -out private.pem -outform PEM
</pre>
and you can try below command 
<pre>
npm run dev -- --open --https --cert private.pem --key private.key
</pre>

Note : the npm run dev command is depand on your package.json, if you have setup add "dev" in you package.json script object

and For webpack.config.js

https Should be true 
port: 443, // defult ssl port
<pre>
devServer: {
  https: true,
  host: '0.0.0.0',
  	port: 443,
  	inline:true,
     
    historyApiFallback: true,
    publicPath: "/",
    contentBase: "./dist",
    disableHostCheck: true
  }
  </pre>

Certificate configure devServer.https option in webpack.config.js:
<pre>
devServer: {
    https: {
        key: fs.readFileSync('/path/to/private.pem'),
        cert: fs.readFileSync('C:/Users/User/private.pem'),
        ca: fs.readFileSync('C:/Users/User/AppData/Local/mkcert/private.pem')
    }
}
</pre>
if you doesn't want to add in webpack.config.js then you have to add these certificate in package.json file
<pre>
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack --mode production",
    "start": "webpack-dev-server  --open --https --cert /path/to/private.crt --key /path/to/private.key"
  },
</pre>
  and then run npm start comamnd on terminal or you can also use
  pm2 start npm -- start

 For tutorial person Have create a self sign certificate but if you want to live your website with https then  please use public ssl certificate 











