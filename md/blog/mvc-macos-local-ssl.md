# Debugging dotnet core Kestrel web MVC apps locally with ssl on macos

If your working on any sort of authentication with an external resource, it pays dividends to be able to debug locally. Getting setup to communicate over ssl with an external resource is a pain, but if you're on MacOS and building an MVC app, I have the Tylenol. Following are straightforward steps to ensuring you setup your MVC app to debug against a secure endpoint.

### setup hosts

```sh
cd /etc
sudo vi hosts
```

add the line to the bottom of the hosts file

```
127.0.0.1   [your dummy domain].dev
```

Make sure you have Brew, so that you can make sure you have openssl.

```sh
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew install openssl
```

### setup certificate

It's a lot of commands to get going.

```sh
cd ~\Source\app\
openssl genrsa -des3 -passout pass:x -out server.pass.key 2048
openssl rsa -passin pass:x -in server.pass.key -out server.key
rm server.pass.key
openssl req -new -key server.key -out server.csr
openssl x509 -req -sha256 -days 365 -in server.csr -signkey server.key -out server.crt
openssl pkcs12 -export -out server.pfx -inkey server.key -in server.crt
```

### tell MacOS to trust the certificate

1. open the `keyachain access` app from spotlight search and click the `certificates` category
2. open `finder` and navigate to your app folder.
3. drag `server.crt` from `finder` into the `keychain access` app.
4. double click your cert, open the trust section, in the first dropdown select `Always Trust`

### configure kestrel.

```sh
dotnet add package Microsoft.AspNetCore.Server.Kestrel -v 1.1.3;
dotnet add package Microsoft.AspNetCore.Server.Kestrel.Https -v 1.1.3;
dotnet restore
```

in dotnet core v1, open `program.cs` and configure the following lines

```c#
if ((Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT") ?? "Production").Equals("Development")) {
   var host = new WebHostBuilder()
      .UseKestrel(options => { 
         options.UseHttps("server.pfx", "password"); 
      })
      .UseUrls("https://*:5000");
}
```

in dotnet core 2, it's:

```sh
dotnet add package Microsoft.AspNetCore.Server.Kestrel;
dotnet add package Microsoft.AspNetCore.Server.Kestrel.Https;
dotnet restore
```

```c#
var host = new WebHostBuilder()
   .UseKestrel((hostingContext, options) => {
      if (!(Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT") ?? "Production").Equals("Development")) return;
      options.Listen(IPAddress.Loopback, 9002, listenOptions => {
         listenOptions.UseHttps("server.pfx", "password");
      });
   })
   .UseUrls("https://*:5000");
```
