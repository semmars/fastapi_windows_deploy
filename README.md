# fastapi_windows_deploy
deploy fastapi golang on windows server

1.Using NSSM to Run Python Scripts as a Windows Service

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
