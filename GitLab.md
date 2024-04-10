# gitlab 、 Sourcetree 和 Rider 的使用流程
## 1.新建一个里程 （ Milestones ）：Issue -> Milestones
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/926f2ffc-14ad-4c3d-a117-5bcd5480de35)
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/a4479185-4924-444e-abe7-75eef5a88e82)

## 2.克隆新仓库 （ 选择 HTTPS 进行 Clone 到 Sourcetree ）
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/abee8e18-c0c5-43cd-a4df-7fa560b878aa)
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/5db66c2e-3ee0-455d-8ef0-32ca2f1a9a21)
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/aac3db3e-57b2-4878-9935-ff04071d3c4c)

## 3.在 Sourcetree 中建立一个新分支（ New Branch ）
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/80aa6e1d-b4b3-4d05-9b84-4f30d6eb4ebc)

## 4.打开 Rider 选择保存的路径文件
- ⚠️ Sourcetree 中 clone 下来打开只可以是文件，若想生成解决方案，需要另建一个解决方案 从访达进入 放入clone的文件夹
- ⚠️ 需要留住 .sln 的文件和 PractiseForSerena 的空文件（访达中可以看见）
  
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/cb60e409-99e0-4e82-9dfc-c0de79bd86d9)

## 5.Rider 中新建一个解决方案
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/f1096235-53e7-4486-9419-c6ac5dee1d80)

## 6.再将 Facade、Libraries、Tests 构建起来
新建Facade、Libraries、Tests 的解决方案文件夹   

![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/4349d54d-4c7d-449b-8369-0cfaa9d8c9ef)

## 7.从 Facade 中添加 PracticeForSerena.Api 项目
点击 Facade 添加新建项目
需要选择ASP.NET Core Web 应用程序
需要添加到 src 路径下
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/4c9c64f6-cb82-4bdd-a5d0-022458ca596d)

## 8.Libraries、Tests 构建文件下面的项目
Libraries 和 Tests 的构建方法都相同
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/f0471bec-002d-4bfb-892b-b036fa76c5b4)

## 9.建好大概框架后在 Sourcetree 中查看是否更改，进行提交和推送
Sourcetree 中 提交 后 推送
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/35e8dcc6-d108-45c5-8aa9-c0288f0ab77c)

## 10.进入 gitlab 建立 Issue （pr 需要Issue编码 如#1）
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/ea4d19a5-23d9-476a-b64f-6836f882567c)
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/ffb39158-432c-44f6-a8b7-3660cbed1916)

## 11.建立pr
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/add9ce8b-fe57-448a-ad4d-22d2fb0485d0)
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/5d2c1c85-0690-4ccc-ba29-f20f53b20890)


