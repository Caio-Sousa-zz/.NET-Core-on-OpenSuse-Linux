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


### Testing the .NET Core Console Application
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

### Testing a web application

Open the terminal type the following commands:

Create a development folder:
```
~$ mkdir dev
~$ cd dev
~$ mkdir webApp
~$ cd webApp
```

Create a web application in the webApp folder:





## References
* [.NET Core Installation](https://dotnet.microsoft.com/download/linux-package-manager/opensuse/sdk-2.1.4) - Install .NET Core in Linux.
* [Deploying .NET Core](https://www.youtube.com/watch?v=z5dnNthXwzE) - ASP.NET Core 1.0 Cross-Platform - Deploying to a Linux Server.
