# R-and-Python-Web-Service
R and Python Web Service is a program developed to allow real-time calculations

## Introduction
There are several web services avalaible for R and Python, but there isn't a solution available for the following requirements:

1. The Web Service has to be installed as a Windows Service
2. The service has to be self hosted with a simple install\uninstall mode
3. The calculation has to support multi core or threads
R and Python code for advanced analytics has latency between milli-seconds and seconds and a single instance cannot scale to hundreds of consumers
4. Simple end points with only oine GET operation and JSON data input
5. Performance metrics such as service and execution latencies
6. Minimize client-server latency; the data model should support the push-execute-pull sequence
7. Unified data model across different scripting languages; currently these are R, Python and PowerShell(beta)

## Installation
All commands are executed from the command line in administrative mode:

+ --install (installs the service)
+ --uninstall (uninstalls the service)
+ --cmd (starts the program as a Windows command line app)

In addition, the following software has has to be installed:

[R Version 5.2.2, 64bit](https://cran.r-project.org/bin/windows/base/R-3.5.2-win.exe)</br>
[Python Version 3.7.2, 64 bit](https://www.python.org/ftp/python/3.7.2/python-3.7.2-amd64.exe)</br>

It is recommended to install bot program under the Windows x64 directory: C:\Program Files\


## Configuration
The service is configured using a JSON file in the instal, directory named "Analytic.Server.json".
The following shows an example:
```json
{
  "Port": 8080,
  "Address": "http://ServerName",
  "Cores": [
    {
      "Executable": "r.core.exe",
      "ExecutablePath": "C:\\Program Files\\Analytic.WebService\\",
      "CorePath1": "C:\\Program Files\\R\\R-3.5.2\\bin\\x64",
      "CorePath2": "C:\\Program Files\\R\\R-3.5.2",
      "WorkingDirectory": "C:/Temp",
      "StartUpScripts": [
      ],
      "Cores": 3,
      "Language": 1
    },
    {
      "Executable": "python.core.exe",
      "ExecutablePath": "C:\\Program Files\\Analytic.WebService\\",
      "CorePath1": "C:\\Program Files\\Python\\Python36",
      "CorePath2": null,
      "WorkingDirectory": "C:/Temp",
      "StartUpScripts": null,
      "Cores": 3,
      "Language": 2
    },
    {
      "Executable": "powershell.core.exe",
      "ExecutablePath": "C:\\Program Files\\Analytic.WebService\\",
      "CorePath1": null,
      "CorePath2": null,
      "WorkingDirectory": null,
      "StartUpScripts": null,
      "Cores": 0,
      "Language": 3
    }
  ]
}

```


## Web Service Endpoints

The web service is hosted on Windows OS. The address and port are configurable, the following end points are supported:

Shows the supported script Modules
http://ServerName:8080/api/language

<p align="center">
  <img src="Image/Web Service Available Cores.PNG">
</p>

Calls the different cores</br>
http://ServerName:8080/api/language/r?json={}</br>
http://ServerName:8080/api/language/python?json={}</br>
http://ServerName:8080/api/language/powershell?json={}</br>

Each call provides this basic information

<p align="center">
  <img src="Image/Web Service Result.PNG">
</p>


