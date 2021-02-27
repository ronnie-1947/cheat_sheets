# NodeJs_CheatSheet

## Table Of Contents
- [NVM (Node Version Manager)](#nvm-node-version-manager-)
    + [NVM Installation Guide](#nvm-installation-guide)
    + [NVM commands](#nvm-commands)
- [NVM (Node Version Manager)](#nvm-node-version-manager-)

## NVM (Node Version Manager)

### NVM Installation Guide
To install nvm in Linux Follow this link
```
https://github.com/nvm-sh/nvm#installing-and-updating
```
To install nvm in Windows Follow this link
```
https://github.com/coreybutler/nvm-windows/releases
```


### NVM commands
| Command | Description |
| ------- | ----------- |
| `nvm version` |Show current nvm version|
| `nvm list` |list all the node version available in pc|
| `nvm list available` |list all the node version available in cloud|
| `nvm install <node_version>` |Install nodeJS with specific version|
| `nvm use <node_version>` |Use specific node version|

____

## HTTP & HTTPS module

### Creating Server with HTTP
```
const http = require('http')

const app = http.createServer((req, res)=>{
    res.end('Hello World')
})

app.listen(PORT, ()=>{
    console.log('app listening on PORT')
})
```

### Creating Server with HTTPS
Generate new SSL certificate 

```
openssl req -newkey rsa:2048 -new -nodes -x509 -days 3650 -keyout key.pem -out cert.pem
```

Initiate server with Https 
```
const https = require('https')

const httpsServerOptions = {
    key: fs.readFileSync(path.join(__dirname, '..', 'https', 'key.pem' )),
    cert: fs.readFileSync(path.join(__dirname, '..', 'https', 'cert.pem'))
}

const app = https.createServer(server.httpsServerOptions, (req, res)=>{
    res.end('Running from https server')
})

app.listen(PORT, ()=>{
    console.log('app listening on PORT')
})
```