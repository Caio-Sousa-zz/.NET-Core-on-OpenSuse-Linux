# .NET Core OpenSuse-Linux
This guide provides a step by step on installing and configuring .NET core and running a ASP.NET Application on Open Suse Linux.

For this tutorial open suse Leap version 42.1 has been used.

## Installing .NET Core 2.1.4
The first step is to install the packages necessary for running .NET core in linux distribution this tutorial.

### For Key registration, product repository and depencies run

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


## References
* [.NET Core Installation](https://dotnet.microsoft.com/download/linux-package-manager/opensuse/sdk-2.1.4) - Install .NET Core in Linux.
