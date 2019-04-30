# Ubuntu zip
## 1.将 ./data 这个目录下所有文件和文件夹打包为当前目录下的 data.zip
```
zip -q -r data.zip ./data
``` 
## 2.解压 data.zip 到目录 ./datasets/data
```
unzip data.zip -d ./datasets/
```