# fastapi_windows_deploy
deploy fastapi golang on windows server

## 1.Using NSSM to Run Python Scripts as a Windows Service


[How to Run a Python Script as a Windows Service using NSSM](https://github.com/semmars/fastapi_windows_deploy/blob/main/How%20to%20Run%20a%20Python%20Script%20as%20a%20Windows%20Service%20using%20NSSM.pdf)

### orignal url


[https://www.mssqltips.com/sqlservertip/7325/how-to-run-a-python-script-windows-service-nssm/](https://www.mssqltips.com/sqlservertip/7325/how-to-run-a-python-script-windows-service-nssm/)


#the trick to make fastapi runs as windows service via NSSM,

![enter image description here](https://github.com/semmars/fastapi_windows_deploy/blob/main/Screen%20Shot%202022-10-05%20at%205.27.02%20PM.png?raw=true)

Put the Path for uvcorn.exe:
In my server it's C:\datacenter\fastapi\venv\Scripts\uvicorn.exe
The Startup directory is where you fastapi python code in: C:\datacenter\fastapi\
Arguments:main:app --host 0.0.0.0 --port 8080

Since gunicorn doesn't work in Windows server, it's the simple way to go.

If you put univcorn inside python code:
Just put "C:\datacenter\fastapi\app.py" in the Arguments tab.

```
from fastapi import FastAPI
import uvicorn

app = FastAPI()

@app.get("/")
async def home():
    return {"message": "Hello World"}

if __name__ == "__main__":
    uvicorn.run("app:app", port=8000, log_level="info", workers=4)

```
## 2 next step is to make IIS as Reverse Proxy 
[IIS Reverse Proxy to Web Apps](https://github.com/semmars/fastapi_windows_deploy/blob/main/IIS%20Reverse%20Proxy%20to%20Web%20Apps%20(Nginx%2C%20Python%2C%20PHP%2C%20Golang%2C%20Java)%20by%20Robert%20Long%20Medium.pdf)

### orignal url


[https://robert-long.medium.com/iis-reverse-proxy-to-web-apps-nginx-python-php-golang-java-8bdb1bbd2281](https://robert-long.medium.com/iis-reverse-proxy-to-web-apps-nginx-python-php-golang-java-8bdb1bbd2281)

## 3 use wrk to test performance of Rest API

get wrk tool from [https://github.com/wg/wrk](https://github.com/wg/wrk)


```
wrk.method = "POST"

wrk.body = '{"firstKey": "somedata", "secondKey": "somedata"}'

wrk.headers["Content-Type"] = "application/json"

```

### save this in a run.lua script and pass it into the command line test with the -s flag.

```

wrk -s run.lua

```

