# CICD
## 一、对 CICD 的理解
CICD ：持续集成，持续交付，持续部署的过程。    

持续集成 (CI):将不同开发人员的代码代码更改合并到一个 main 分支的流程。

持续交付 (CD):使代码始终处于可随时发布的状态。在持续集成阶段建立的测试和构建自动化为基础。

持续部署 (CD): CI/CD 流程的最后阶段，在这个阶段满足所有要求后，新版本的软件将交付给最终用户。  

## 二、CI 配置流程（ Web ）
teamcity网址: https://teamcity.sjfood.us/project/Smarties?branch=mixture_v3&buildTypeTab=overview&mode=builds

找到正在做的项目，Run一下看看有没有报错
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/bd04f1c0-c8db-44d7-a14b-abb03a09ecd0)
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/b8c9fd30-58a7-4841-83de-b9e15b383aaa)
如果分支没有报错，就可以将分支合并到大杂烩里（ Mixture_v3 ），到 GitHub里合到大杂烩中，再跑一下大杂烩看看有没有报错。   
可在 IssueLog 里查看日志
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/cbb523f3-122e-43c4-9b2c-fd5f96a1661d)
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/b687b20c-0355-4b7b-b5ef-7f0e1f617e1e)
⚠️如果有报错的话需要另外开分支进行修改

### 三、CD 的配置流程（web）
小章鱼🐙网址: https://octopus.sjfood.us/app#/Spaces-1/projects/smarties-api/deployments/releases/create

小章鱼 -> 在 Projects 里选择项目 -> Releases -> Channel 选择 Default 。    
Version 填入在 teamcity 里 Run 完的版本号，+号改-号 。      
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/70517e7f-2c13-4167-b343-d04e2074e0ce)
Deploy 选最后一个，需要点进去里面去查到自己的版本号，不可以直接输入❌

需要 ☑️ 选择非正式的版本号 => 🆗        
下一列也是填一样的‼️ 最后 Save 保存
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/9d83ecee-d7fb-40e3-a79a-199beba7bb37)

最后需要选择 Test => 完成✅
⚠️：核对自己的版本号
