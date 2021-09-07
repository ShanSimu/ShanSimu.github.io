## 构建基础

#### projects和tasks

任何一个 Gradle 构建都是由一个或多个 projects 组成。每个 project 包括许多可构建组成部分。 这完全取决于你要构建些什么。举个例子，每个 project 或许是一个 jar 包或者一个 web 应用，它也可以是一个由许多其他项目中产生的 jar 构成的 zip 压缩包。一个 project 不必描述它只能进行构建操作。它也可以部署你的应用或搭建你的环境。不要担心它像听上去的那样庞大。 Gradle 的 build-by-convention 可以让您来具体定义一个 project 到底该做什么。

每个 project 都由多个 tasks 组成。每个 task 都代表了构建执行过程中的一个原子性操作。如编译，打包，生成 javadoc，发布到某个仓库等操作。