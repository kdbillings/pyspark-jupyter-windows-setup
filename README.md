# Pyspark and Windows Setup
This is a step by step guide to try to set up a Spark environment on Windows using Docker. A brief summary of items of what will be setup include the following:
  1. chocolatey - Windows package manager which makes installing things a breeze
  2. Docker for Windows
  3. That's it! The Docker setup will contain Spark and a linked Jupyter notebook

## Instructions
1. Install chocolatey. Open the command line but before opening, make sure you right-click and select `Run as Administrator`. Execute the following commmand in the command line:
```
@"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
```

2. Now that chocolatey is setup, we are now ready to install Docker for Windows. Run the following command:
```
choco install docker-desktop
```

3. Open up Docker Desktop. You may have to register and login.

4. Open up a new command line window. Execute the following command. This will pull down a Docker image and run the image. This will take a little while depending on your internet connection. This is a one time install and download
```
docker run -p 8888:8888 jupyter/all-spark-notebook
```

5. Pay attention to server logs as the container comes up. You'll notice it in the form of `http://<hostname>:8888/?token=<token>`. In my case, it shows up something like this `http://127.0.0.1:8888/?token=bccdc1af8f9be1360297bf0cedd8d0ecf0ce5e84e960e054`. Each person's token will be different on each subsequent run of this container. Go this address in your browser to bring up the Jupyter notebook.


6. Test a hello world application in your notebook by running the following code in a new Python notebook:
```python
from pyspark import SparkContext
from operator import add
 
sc = SparkContext()
data = sc.parallelize(list("Hello World"))
counts = data.map(lambda x: (x, 1)).reduceByKey(add).sortBy(lambda x: x[1], ascending=False).collect()
for (word, count) in counts:
    print("{}: {}".format(word, count))
sc.stop()
```

