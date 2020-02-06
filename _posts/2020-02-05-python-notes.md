## Installing python3.8.1 on ubuntu from source
```
 wget https://www.python.org/ftp/python/3.8.1/Python-3.8.1.tgz
 tar zxvf Python-3.8.1.tgz
 cd Python-3.8.1
 ./configure --enable-optimization --prefix /usr/
 make
 sudo make altinstall
 ```

 ### Update alternatives
 ```
 sudo update-alternatives --list python3
 sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.8.1 1
 python3 -V
 ```
