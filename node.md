# NodeJs_CheatSheet

## Table Of Contents
- [NVM (Node Version Manager)](#nvm-node-version-manager-)
    + [NVM Installation Guide](#nvm-installation-guide)
    + [NVM commands](#nvm-commands)
- [HTTP & HTTPS module](#http--https-module)
    * [Creating Server with HTTP](#creating-server-with-http)
    * [Creating Server with HTTPS](#creating-server-with-https)
- [URL Module](#url-module)
- [String Decoder](#string-decoder)
- [File System](#fs-module)
    * [Reading a File Synchronously](#reading-a-file-synchronously)
    * [Reading a File Asynchronously](#reading-a-file-asynchronously)
    * [writing a File Synchronously](#writing-a-file-synchronously)
    * [writing a File Asynchronously](#writing-a-file-asynchronously)
    * [Appending to a File](#appending-to-a-file)
    * [Deleting a file](#deleting-a-file)
    * [Open a File](#open-a-file)
- [Crypto Module](#crypto-module)
  * [Hash a string with crypto](#hash-a-string-with-crypto)
  * [Create Random String](#create-random-string)

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

____

## URL Module
Url module is used to parse, construct, normalize and encode URLs.
```
const url = require('url')

const parsedUrl = url.parse('http://localhost:8080/default?year=2017&month=february', true);

const host = parsedUrl.host // 'localhost:8080'
const pathName = parsedUrl.pathname // '/default'
const query = parsedUrl.query // '{ year: 2017, month: 'february' }'
```
____

## String Decoder
Decode a stream of binary data (a buffer object) into a string
```
const {StringDecoder} = require('string_decoder)

const decoder = new StringDecoder(utf8)
const buffer = Buffer('abc') // Creating buffer using Buffer()

const result = decoder.write(buffer) // Decoding the buffer back to string
decoder.end()

```
_____

## FS Module
File System module allows us to read write delete files in the disk

### Reading a File Synchronously
```
const fs = require('fs')
const str = fs.readFileSync('path', 'utf8')
console.log(str)
```

### Reading a File Asynchronously
```
const fs = require('fs')

fs.readFile('path', 'encType', (err, str)=>{
    console.log('str')
})
```
### writing a File Synchronously
```
const fs = require('fs')

fs.writeFileSync(path, str)
```

### writing a File Asynchronously
```
const fs = require('fs')

fs.writeFile('path', str, (err)=>{
    ...
})
```

### Appending to a File
```
const fs = require('fs')

fs.appendFileSync(path, str)

fs.appendFile(path, str, (err)=>{
    console.log(err)
})
```

### Deleting a file
```
const fs = require('fs')

fs.unlink(path, ()=>{
    ...
})
```

### Open a File
Opening a File allows to do all the operations to a file
```
const fs = require('fs')

fs.open(path, flag, (err, fd)=>{
    ...
})
```


## Crypto Module

### Hash a string with crypto
```
const crypto = require('crypto')

crypto.createHmac('SHA256', config.hashingSecret).update(str).digest('hex')

```

### Create Random String
```
const crypto = require('crypto')

const hash = crypto.randomBytes(strLength).toString('hex');
```