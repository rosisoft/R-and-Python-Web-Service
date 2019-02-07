# R-and-Python-Web-Service
The R and Python Web Service is a program developed to allow script execution in real-time applications.

## Introduction
There are several web services avalaible for R and Python, but there isn't a solution available for the following requirements:

1. The Web Service has to be installed as a Windows Service
2. The service has to be self hosted with a simple install\uninstall mode
3. The calculation has to support multi core or threads</br>
R and Python code for advanced analytics has latency between milli-seconds and seconds and a single instance cannot scale to hundreds of consumers
4. Simple end points with only oine GET operation and JSON data input
5. Performance metrics such as service and execution latencies
6. Minimize client-server latency; the data model should support the push-execute-pull sequence
7. Unified data model across different scripting languages; currently these are R, Python and PowerShell(beta)

## Installation
All commands are executed from the command line in administrative mode:</br>
</br>
--install (installs the service)</br>
--uninstall (uninstalls the service)</br>
--cmd (starts the program as a Windows command line app)</br>
</br>
In addition, the following software has has to be installed:

[R Version 5.2.2, 64bit](https://cran.r-project.org/bin/windows/base/R-3.5.2-win.exe)</br>
[Python Version 3.7.2, 64 bit](https://www.python.org/ftp/python/3.7.2/python-3.7.2-amd64.exe)</br>

It is recommended to install both programs under the Windows x64 directory: C:\Program Files\


## Configuration
The service is configured using a JSON file in the install directory named "Analytic.Server.json".
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
The JSON contains both the configuration for the server and the script cores. The server requires to specify both the **address** and the **port**. Each core provides the following configuration items:</br>
</br>
"Executable" - Name of the core exe</br>
"ExecutablePath" - Installation path of the web service</br>
"CorePath1" - Configuration path; needs to be specified for R</br>
"CorePath2" - Configuration path; needs to be specified for R</br>
"WorkingDirectory" - Working directory for scripts</br>
"StartUpScripts" - List of start-up scripts that need to be executed when the core is initialized</br>
"Cores" - **Number of core or threads for calculations**</br>
"Language" - Enum specifying the scriping language</br>


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

## JSON Data Model

The following data model is used to submit calculations to the web service and retrieve responses.

### Dimension</br>
**Value = 0**</br> 
**Vector = 1**</br>
**Matrix = 2**</br>
Frame = 3</br>
</br>
### LanguageType</br>
None = 0</br>
**R = 1**</br>
**Python = 2**</br>
PowerShell = 3</br>
</br>
### OperationType</br>
None = 0</br>
**Push = 1**</br>
**Pull = 2**</br>
**Execute = 3**</br>
Information = 4</br> 
Error = 5</br>
Stop = 6</br>
</br>
### ValueType</br>
None = 0</br>
**Double = 1**</br>
**Int = 2**</br>
**String = 3**</br>
**Bool = 4**</br>
Frame = 5</br>
</br>
### Operation</br>
double? DoubleValue</br>
int? IntValue</br>
string StringValue</br>
bool? BoolValue</br>
double[] DoubleVector</br>
int[] IntVector</br>
string[] StringVector</br>
bool[] BoolVector</br>
DateTime[] TimeVector</br>
double[,] DoubleMatrix</br>
int[,] IntMatrix</br>
string[,] StringMatrix</br>
bool[,] BoolMatrix</br>
Frame Frame</br>
OperationType Type</br>
ValueType ValueType</br>
Dimension Dimension</br>
string Command</br>
string</br>
string Message</br> 

### Sequence
List<Operation> Operations</br>
double? ServiceTime</br>
double? CoreTime</br>
double? ExecutionTime</br>
LanguageType Language</br>
string UniqueId</br>
string Message</br>
int? QueueLength</br>
string ClientId</br>
string CoreId</br>

## Quick Start

The Web service does not require the entire data model for each operation and just a few parameter are needed for basic operations. The main idea is that each calculation can be divided into the following 3 operation types:</br>

+ Push; submite data such as bool, int, double or string to the server
+ Execute; execute a script or function
+ Pull; retrieve results from the server

A simple calculation such as c = a + b can be translated into the following 4 operations:

+ Push a
+ Push b
+ Execute c = a + b
+ Pull c

The JSON looks then like this:

http://ServerName:8080/api/language/r?json={Operations:[{Type:1,DoubleValue:1,Name:a},{Type:1,DoubleValue:2,Name:b},{Type:3,Command:c=sum(a,b)},{Type:2,Dimension:0,ValueType:1,Name:c}],Language:1} 
