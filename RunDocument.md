# Decca运行文档

## 快速开始（Quick Start）

项目地址：

`https://github.com/wangcwangc/Decca`

克隆仓库到本地：

`git clone https://github.com/wangcwangc/Decca.git`

部署soot依赖包：

`cd soot-4.1.0所在位置`

`mvn install:install-file -Dfile=soot-4.1.0.jar -DgroupId=neu.lab -DartifactId=soot -Dversion=4.1.0 -Dpackaging=jar -DpomFile=soot-4.1.0.pom`

Maven构建项目：

`mvn clean package`

部署Decca到运行机器上：

`cd Decca所在的位置`

`mvn install:install-file -Dfile=target\riddle-1.0.jar -DgroupId=neu.lab -DartifactId=riddle -Dversion=1.0 -Dpackaging=maven-plugin -DpomFile=pom.xml`

Maven构建需要检测的项目，项目生成**target**文件夹：

`mvn clean package` 或 `mvn package -Dmaven.test.skip=true` （对于有多个子程序的项目，建议使用 `mvn install` 或 `mvn install -Dmaven.test.skip=true`）

 使用Decca针对目标项目生成依赖冲突检测报告（需要了解）：
 
 1. `cd 目标项目地址\`

    `mvn -f=pom.xml -Dmaven.test.skip=true -DuseAllJar=false -DfilterSuper=true neu.lab:riddle:1.0:printRiskLevel -DresultPath=目标结果存放位置\ -e`

 2. `mvn -f=目标项目地址\pom.xml -Dmaven.test.skip=true -DuseAllJar=false -DfilterSuper=true neu.lab:riddle:1.0:printRiskLevel -DresultPath=目标结果存放位置\ -e`
 
 使用Decca针对目标项目生成依赖冲突问题的修复补丁(可稍后了解)：
 
 1. `cd 目标项目地址\`

    `mvn -f=pom.xml -Dmaven.test.skip=true -DuseAllJar=false -DfilterSuper=true neu.lab:riddle:1.0:repair -DresultPath=目标结果存放位置\ -e`
    
 2. `mvn -f=目标项目地址\pom.xml -Dmaven.test.skip=true -DuseAllJar=false -DfilterSuper=true neu.lab:riddle:1.0:repair -DresultPath=目标结果存放位置\ -e`

## 项目结构（Project Structure）

建议IDE使用idea，可以通过《Maven实战》了解下Maven，可以从PrintRiskLevelMojo类一步一步的看检测逻辑，对于一些方法如果不太懂，可以找一个测试项目，在TestMojo类进入，实现你想运行的方法功能，查看运行输出

<pre><code>
├─src
│  ├─main
│  │  ├─java
│  │  │  ├─abandon.neu.lab.conflict -->与检测无关，不需要动
│  │  │  └─neu
│  │  │      └─lab
│  │  │          ├─conflict
│  │  │          │  │  CircularDetectMojo.java -->循环依赖检测入口
│  │  │          │  │  ClassDupRiskMojo.java -->类粒度检测入口
│  │  │          │  │  ClsDetectMojo.java
│  │  │          │  │  ConflictMojo.java -->maven插件的总入口，其他的Mojo都继承自它，mojo类中声明的name即为插件运行该功能的指令
│  │  │          │  │  ConflictRepairMojo.java
│  │  │          │  │  Debug2Mojo.java -->打印风险方法路径入口
│  │  │          │  │  DebugMojo.java
│  │  │          │  │  DetectMojo.java
│  │  │          │  │  FindVersionMojo.java
│  │  │          │  │  GeneMojo.java
│  │  │          │  │  GlobalVar.java
│  │  │          │  │  JarDupRiskMojo.java
│  │  │          │  │  PathMojo.java
│  │  │          │  │  PrintRiskLevelMojo.java -->包粒度版本冲突检测入口
│  │  │          │  │  PrintSizeMojo.java
│  │  │          │  │  RepairMojo.java -->依赖冲突修复入口
│  │  │          │  │  RiddleMojo.java
│  │  │          │  │  RiskLevelDebugMoJo.java
│  │  │          │  │  StaMojo.java
│  │  │          │  │  TestCaseGenerator.java
│  │  │          │  │  TestMojo.java
│  │  │          │  │  UpdateVersionMojo.java
│  │  │          │  │  
│  │  │          │  ├─container -->存放各个数据结构的容器
│  │  │          │  │      AllCls.java -->所有依赖的类的集合
│  │  │          │  │      AllRefedCls.java -->所有被使用类的集合
│  │  │          │  │      Conflicts.java -->包粒度版本冲突的集合
│  │  │          │  │      DepJars.java	-->项目所有依赖的集合
│  │  │          │  │      FinalClasses.java
│  │  │          │  │      JarClsThrowRisks.java
│  │  │          │  │      NewNodeAdapters.java
│  │  │          │  │      NodeAdapters.java -->解析项目依赖树节点的集合
│  │  │          │  │      TempCls.java
│  │  │          │  │      TempRefedCls.java
│  │  │          │  │      
│  │  │          │  ├─graph -->检测内核
│  │  │          │  │      Book4distance.java
│  │  │          │  │      Book4path.java
│  │  │          │  │      Cross.java
│  │  │          │  │      Dog.java
│  │  │          │  │      Graph4distance.java
│  │  │          │  │      Graph4path.java
│  │  │          │  │      GraphPrinter.java
│  │  │          │  │      IBook.java
│  │  │          │  │      IGraph.java
│  │  │          │  │      INode.java
│  │  │          │  │      IRecord.java
│  │  │          │  │      MthdCfgs.java
│  │  │          │  │      Node4distance.java
│  │  │          │  │      Node4path.java
│  │  │          │  │      Record4distance.java
│  │  │          │  │      Record4path.java
│  │  │          │  │      RepairCg.java
│  │  │          │  │      
│  │  │          │  ├─repair -->与修复相关，目前不需要看
│  │  │          │  │      AnalysePom.java
│  │  │          │  │      CheckCorrect.java
│  │  │          │  │      Level3ChangePom.java
│  │  │          │  │      Level3Repair.java
│  │  │          │  │      Level4ChangePom.java
│  │  │          │  │      Level4Repair.java
│  │  │          │  │      MavenCrawler.java
│  │  │          │  │      
│  │  │          │  ├─risk -->版本冲突检测的功能入口
│  │  │          │  │  └─jar
│  │  │          │  │          ConflictJRisk.java -->检测版本冲突风险等级的入口
│  │  │          │  │          DepJarJRisk.java	-->检测版本冲突风险等级的具体实现
│  │  │          │  │          
│  │  │          │  ├─soot -->检测内核
│  │  │          │  │  │  ClassThrowTf.java
│  │  │          │  │  │  JarAna.java
│  │  │          │  │  │  SootAna.java
│  │  │          │  │  │  SootJRiskCg.java
│  │  │          │  │  │  SootNRiskCg.java
│  │  │          │  │  │  SootRiskMthdFilter.java
│  │  │          │  │  │  SootRiskMthdFilter2.java
│  │  │          │  │  │  ThrowRiskTf.java
│  │  │          │  │  │  
│  │  │          │  │  └─tf
│  │  │          │  │          CgTf.java
│  │  │          │  │          JRiskCgTf.java
│  │  │          │  │          JRiskDistanceCgTf.java
│  │  │          │  │          JRiskMthdPathCgTf.java
│  │  │          │  │          MthdCgTf.java
│  │  │          │  │          PreNodeJarTf.java
│  │  │          │  │          
│  │  │          │  ├─spider
│  │  │          │  │      L4SafeJarSpider.java
│  │  │          │  │      PreNodeVerSpider.java
│  │  │          │  │      SafeJarSpider.java
│  │  │          │  │      SafePreNodeSpider.java
│  │  │          │  │      
│  │  │          │  ├─util
│  │  │          │  │      ClassDupRiskMemoryUnit.java
│  │  │          │  │      ClassifierUtil.java
│  │  │          │  │      Cmp.java
│  │  │          │  │      Conf.java -->一些全局参数的定义
│  │  │          │  │      DebugUtil.java
│  │  │          │  │      LibCopyInfo.java
│  │  │          │  │      MathUtil.java	
│  │  │          │  │      MavenUtil.java -->包括获取Maven项目相关信息，解析新的依赖，打印运行日志等方法
│  │  │          │  │      MySortedMap.java
│  │  │          │  │      NodeAdapterCollector.java -->j解析由DependencyNode获取的项目依赖节点，存放于NodeAdapters
│  │  │          │  │      NodeInfoMemory.java
│  │  │          │  │      PomOperation.java
│  │  │          │  │      SootUtil.java -->包括soot解析类、方法等的接口
│  │  │          │  │      UserConf.java -->用户配置文件，主要定义了检测输出路径
│  │  │          │  │      
│  │  │          │  ├─vo
│  │  │          │  │      ArgsVO.java
│  │  │          │  │      ClassVO.java	-->解析类的数据结构
│  │  │          │  │      Conflict.java -->包粒度版本冲突的数据结构
│  │  │          │  │      DependencyInfo.java
│  │  │          │  │      DepJar.java	-->依赖包的数据结构
│  │  │          │  │      JarClsThrowRisk.java
│  │  │          │  │      ManageNodeAdapter.java -->被<dependencyManagement>覆盖的节点数据结构
│  │  │          │  │      MethodCall.java -->soot解析的call graph的数据结构
│  │  │          │  │      MethodCalls.java	
│  │  │          │  │      MethodVO.java -->解析的方法的数据结构
│  │  │          │  │      NewNodeAdapter.java	
│  │  │          │  │      NodeAdapter.java -->解析项目依赖树节点的数据结构
│  │  │          │  │      PathVO.java
│  │  │          │  │      
│  │  │          │  └─writer
│  │  │          │          CircularNodeWriter.java -->检测循环依赖的输出
│  │  │          │          ClassDupRiskWriter.java -->检测类粒度冲突的输出
│  │  │          │          ConflictRepairWriter.java
│  │  │          │          DetailedSuggestionWriter.java
│  │  │          │          DistanceWriter.java
│  │  │          │          JarDupRiskWriter.java
│  │  │          │          JarRchedWriter.java
│  │  │          │          JarRiskWriter.java
│  │  │          │          L3MulShadowedWriter.java
│  │  │          │          L3OneShadowedWriter.java
│  │  │          │          L4MulShadowedWriter.java
│  │  │          │          L4OneShadowedWriter.java
│  │  │          │          NewRepairWriter.java
│  │  │          │          RepairSuggestionWriter.java
│  │  │          │          RepairWriter.java
│  │  │          │          ReWriter.java
│  │  │          │          RiskLevelDebugWriter.java
│  │  │          │          RiskLevelWriter.java -->检测包粒度版本冲突的输出
│  │  │          │          RiskPathWriter.java
│  │  │          │          StaWriter.java
│  │  │          │          UpVerWriter.java
│  │  │          │          VersionWriter.java
│  │  │          │          
│  │  │          └─evoshell
</code></pre>
 
