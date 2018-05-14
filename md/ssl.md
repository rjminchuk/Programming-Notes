# macos ssl setup for dotnet core 1.1 kestrel web apps

### setup hosts

```sh
cd ~
cd ../../etc
sudo vi hosts
```

add the line to the bottom of the hosts file

```
127.0.0.1   richminchuk.dev
```

### setup certificate

```sh
cd ~\Source\app\
mkdir keys
cd keys
openssl genrsa -des3 -passout pass:x -out server.pass.key 2048
openssl rsa -passin pass:x -in server.pass.key -out server.key
rm server.pass.key
openssl req -new -key server.key -out server.csr
openssl x509 -req -sha256 -days 365 -in server.csr -signkey server.key -out server.crt
openssl pkcs12 -export -out server.pfx -inkey server.key -in server.crt
copy
```

### tell your MacOS to trust the certificate

1. open the `keyachain access` app from spotlight search and click the `certificates` category
2. open `finder` and navigate to your keys folder.
3. drag `server.crt` from `finder` into the `keychain access` app.
4. double click your cert, open the trust section, in the first dropdown select `Always Trust`


### configure kestrel.

```sh
dotnet add package Microsoft.AspNetCore.Server.Kestrel -v 1.1.3;
dotnet add package Microsoft.AspNetCore.Server.Kestrel.Https -v 1.1.3;
dotnet restore
```

open `program.cs` and configure the following lines

```c#
var host = new WebHostBuilder()
   .UseKestrel(options => { options.UseHttps("server.pfx", ""); })
   .UseUrls("https://*:5000");
```