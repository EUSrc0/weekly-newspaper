# 网络安全工作室周报
个人文件夹命名格式：21级-Flyg
## 上传步骤
+ 首先将仓库fork到自己账号下
  ![image](https://github.com/EUSrc0/weekly-newspaper/assets/108129440/a167a510-3398-4ab4-b601-05f8cf09a63b)
+ 将对应的仓库push到自己本地，如Flyg2/weekly-newspaper,一定是自己id下的那个仓库。
命令行下：
```
git@github.com:Flyg2/weekly-newspaper.git
或者
gh repo clone Flyg2/weekly-newspaper
```
+ 在自己本地weekly-newspaper文件夹下建一个自己id的文件夹，周报直接写在id下的文件夹内，如2024年4月第三周周报.md，md名字安装此例命名。
+ 将增添或者修改之后的文件或者文件夹先添加至暂存区,然后将修改提交到本地
```
git add . 这个命令会将当前目录下的所有修改（包括新增、修改和删除的文件）添加到Git的暂存区。.代表当前目录。你也可以指定具体的文件或目录来添加，比如 git add filename 或 git add directory/。
git commit -m "2024年4月第三周周报" 这个命令会将暂存区中的修改提交到本地的Git仓库中，并添加一条提交信息。-m选项用于指定提交信息，其中 "2024年4月第三周周报" 是你的提交信息。提交信息应该清晰地描述本次提交的内容,如例所示。
```
+ 将本地文件pull 到远端仓库，注：一定先pull在push
```
git pull 
git push weekly-newspaper main
```
