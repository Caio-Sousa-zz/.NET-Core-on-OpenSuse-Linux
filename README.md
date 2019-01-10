# .NET Core OpenSuse-Linux
This guide provides a step by step on installing and configuring .NET core and running a ASP.NET Application on Open Suse Linux.

For this tutorial open suse Leap version 42.1 has been used.

## Installing .NET Core 2.1.4
This section contains the first step to install the packages necessary for running .NET core in linux distribution this tutorial.

### For Key registration, product repository and depencies

Open the terminal and run the following commands:

```
~$ sudo zypper install libicu
~$ sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
~$ wget -q https://packages.microsoft.com/config/opensuse/42.2/prod.repo
~$ sudo mv prod.repo /etc/zypp/repos.d/microsoft-prod.repo
~$ sudo chown root:root /etc/zypp/repos.d/microsoft-prod.repo
```

### Installing the .NET SDK:

Open the terminal and run the following commands:

Update all installed packages with newer available versions:
```
~$ sudo zypper update
```

To update individual packages, specify the package with either the update or install command:
```
~$ sudo zypper install libunwind libicu
~$ sudo zypper install dotnet-sdk-2.1.4
```

When installing the SDK the following prompt might occur: nothing provides libcurl needed by dotnet-runtime choose the option to ignore and continue usually option 2.


### Creating a .NET Core Console Application
After installing .NET Core SDK we will do a quick test to make sure it is running properly.

Open the terminal type the following commands:

Create a new test folder:
```
~$ mkdir testClient
~$ cd testClient
```

Running .NET console application
```
~$ dotnet new console
~$ dotnet restore
~$ dotnet run
```
The text 'Hello World' should appear in the terminal and we know that .net is running correctly.

## Running And Deploying ASP.NET Core Application
This section provides an example of a running asp.net core application the deployment process in the linux environment.

### Running a Web Application

Open the terminal type the following commands:

Create a development folder:
```
~$ mkdir dev
~$ cd dev
~$ mkdir webApp
~$ cd webApp
```

Create a empty web application in the webApp folder:
```
~$ dotnet new web
~$ dotnet restore
~$ dotnet run
```

For packing the app and its dependencies into a folder for deployment to a hosting system:
```
~$ dotnet publish
```

### Installing Visual Studio Code
A recommended code editor for .NET applications is VS Code.

To install VS code run the following commands in the terminal:
```
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ntype=rpm-md\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/zypp/repos.d/vscode.repo'
```

```
sudo zypper refresh
sudo zypper install code
```

### Configuring the Web Application
We will need to configure a reverse proxy for canceling the HTTP request and fowarding to the ASP.NET Application.

Include the Startup.cs file update the ConfigureServices method to update the foward headers.

```
services.Configure<ForwardedHeadersOptions>(options =>
    {
        options.ForwardedHeaders = 
            ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto;
    });
```
Then to call this option on the Configure method include:

```
 app.UseForwardedHeaders();
```

### Hosting a ASP.NET Core in Apache

First we will install the apache server, open the terminal and type:

```
~$ zypper in apache2
```

To start and enable apache:

```
~$ systemctl start apache2
~$ systemctl enable apache2
```

The apache folder should then be available at

```
~$ cd /srv/www/htdocs
```

If we copy a index.html to the htdocs the file should be available in the browser on localhost


### Creating a configuration file for the application

Configuration files for Apache are located within the /etc/apache2 directory where the *.conf are located.

First navigate to the configuration folder:
```
~$ cd /stc/apache2
```

Then, create a configuration file we will create on called webApp.conf
```
~$ touch webApp.conf


<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ServerName www.example.com
    ServerAlias *.example.com
    ErrorLog ${APACHE_LOG_DIR}helloapp-error.log
    CustomLog ${APACHE_LOG_DIR}helloapp-access.log common
</VirtualHost>
```

Edit the file and

## References
* [.NET Core Installation](https://dotnet.microsoft.com/download/linux-package-manager/opensuse/sdk-2.1.4) - Install .NET Core in Linux.
* [Deploying .NET Core](https://www.youtube.com/watch?v=z5dnNthXwzE) - ASP.NET Core 1.0 Cross-Platform - Deploying to a Linux Server.
* [Adding user on OpenSuse](https://www.youtube.com/watch?v=lkVeMLwYHkY) - Add new user.
