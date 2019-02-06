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

## Web Service Endpoints

The web service is hosted on Windows OS. The address and port are configurable, the following end points are supported:

Shows the supported script Modules
http://ServerName:8080/api/language

<p align="center">
  <img src="Image/Web Service Available Cores.PNG">
</p>

Calls the different cores
+ http://ServerName:8080/api/language/r?json={}
+ http://ServerName:8080/api/language/python?json={}
+ http://ServerName:8080/api/language/powershell?json={}

Each call provides this basic information

<p align="center">
  <img src="Image/Web Service Result.PNG">
</p>


