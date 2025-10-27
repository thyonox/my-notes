# 基本概念

Maven 是一个用于**构建和管理** Java 项目的工具，它简化了项目的构建过程，并自动管理项目的依赖。Maven 采用 POM（Project Object Model）文件来描述项目结构和配置信息。

- 管理项目：不同的IDE使用不同的项目结构，而Maven基于约定大于配置的思想，提供一套标准化的项目结构，确保项目可读性和构建过程的一致性。
    
    ![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668694426091-74450940-d0f5-424b-927c-070e7feafcc6.png#averageHue=%23f3eeed&clientId=u790262c5-7cb6-4&from=paste&height=258&id=u12373c2d&originHeight=258&originWidth=994&originalType=binary&ratio=1&rotation=0&showTitle=false&size=77980&status=done&style=none&taskId=u6bb818a4-5375-4575-a170-603c95ce8fd&title=&width=994)
    
- 构建项目：Maven提供了一套标准化的项目构建流程，并通过插件自动化调用，简化构建过程。
    
    ![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668694433663-1cbfd508-57a0-4117-9d1e-1807b019f394.png#averageHue=%23fbf5f5&clientId=u790262c5-7cb6-4&from=paste&height=188&id=u9b23fe39&originHeight=188&originWidth=776&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21187&status=done&style=none&taskId=uc7b65d54-c1ba-47f5-93d7-6c589707d52&title=&width=776)
    
- POM（Project Object Model）：POM 是 Maven 项目的核心文件，使用 `pom.xml` 来定义项目的配置信息和依赖管理。POM 文件中包含了项目的基本信息、依赖的库、插件以及构建的详细信息。
    
    ![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668694469075-22e6df88-e379-426a-94ba-a459ee855cd2.png#averageHue=%23e1dfbd&clientId=u790262c5-7cb6-4&from=paste&height=303&id=u209827b8&originHeight=303&originWidth=679&originalType=binary&ratio=1&rotation=0&showTitle=false&size=40709&status=done&style=none&taskId=u08b9790d-e169-465c-b88b-74e9d698249&title=&width=679)
    
- 坐标：Maven项目的坐标是项目的唯一标识，通过坐标进行依赖管理，而不再需要手动引入外部库和配置项目依赖包。
    
    ![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668694451019-5bf0cb52-f641-45ae-8164-50a1917ad188.png#averageHue=%23f2efee&clientId=u790262c5-7cb6-4&from=paste&height=109&id=uc40448b8&originHeight=109&originWidth=636&originalType=binary&ratio=1&rotation=0&showTitle=false&size=28602&status=done&style=none&taskId=u450faa93-e981-4f5a-a124-df45bf02080&title=&width=636)
    
- 仓库： Maven 仓库是存储构建输出物的地方，如 JAR 包和插件。POM中引入的坐标对应的依赖jar包存储在这里。
    

# 安装配置

- **下载Maven压缩包**
    
    - [https://maven.apache.org/download.cgi](https://maven.apache.org/download.cgi)
        
    - 下载编译好的二进制文件，如果要自定义编译，可以下载源文件。
        
        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/4c425732-c903-4686-874b-0d25ac03db11/image.png)
        
- **添加到环境变量**
    
    虽然在Idea中可以设置Maven位置，但有时会在终端调用Maven命令，将Maven添加到环境变量会更加方便。
    
- **`setting.xml`配置**
    
    - **本地仓库配置**：使用`localRepository`标签定义本地仓库位置。
        
    - **镜像仓库配置**：在`mirrors`中添加镜像仓库，加速依赖的下载。
        
        ```xml
        <mirror>
            <id>alimaven</id>
            <name>aliyun maven</name>
            <url><http://maven.aliyun.com/nexus/content/groups/public/></url>
            <mirrorOf>central</mirrorOf>
        </mirror>
        ```
        
D:\\Development\\Maven\\apache-maven-3.9.9\\conf\\settings.xml
D:\\Development\\Maven\\apache-maven-3.9.9\\conf\\settings.xml
# 生命周期

Maven 的生命周期（Lifecycle）是指 Maven 项目从开始构建到结束所经过的多个阶段和任务序列。Maven 提供了一组预定义的生命周期和各个阶段，来帮助开发者自动化地执行项目的构建、测试、打包、部署等任务。Maven 的构建生命周期分为三个主要部分，每个部分包含多个阶段，如果调用某个阶段，Maven 会执行该阶段及之前的所有阶段。

## Clean生命周期

Clean 生命周期用于清理项目的构建目录，通常在执行新的构建之前运行。

- **pre-clean**: 执行清理工作之前的操作。
- **clean**: 删除上一次构建生成的文件（如 `target` 目录）。
- **post-clean**: 执行清理工作之后的操作。

```powershell
mvn clean
```

## Default生命周期

Default 生命周期是 Maven 最核心的构建生命周期，涵盖了项目从验证、编译、测试、打包、安装、部署等完整的构建过程。其包含以下常用的阶段：

- **validate**: 验证项目是否正确并且所有必要信息是否可用（如 `pom.xml` 配置是否正确）。
- **initialize**: 初始化构建状态，执行一些项目初始化操作。
- **generate-sources**: 生成项目的源代码，如从模板或注解中生成代码。
- **process-sources**: 处理项目的源代码，如进行代码转换、合并等操作。
- **generate-resources**: 生成项目的资源文件。
- **process-resources**: 处理项目的资源文件，将资源文件从 `src/main/resources` 复制到目标目录。
- **compile**: 编译项目的源代码，将 `.java` 文件编译为 `.class` 文件。
- **process-classes**: 处理编译生成的 `.class` 文件。
- **generate-test-sources**: 为测试生成源代码。
- **process-test-sources**: 处理测试源代码，准备进行测试。
- **test-compile**: 编译测试代码，将 `src/test/java` 中的测试类编译为 `.class` 文件。
- **process-test-classes**: 处理编译后的测试类。
- **test**: 执行单元测试，运行 `src/test/java` 中的测试类。
- **prepare-package**: 在打包之前执行的操作，通常用于准备阶段。
- **package**: 将编译后的代码打包成可发布的格式，如 JAR、WAR 文件。
- **pre-integration-test**: 在集成测试之前进行一些操作，如准备集成测试环境。
- **integration-test**: 执行集成测试，将构建的包部署到环境中并进行测试。
- **post-integration-test**: 在集成测试之后进行的操作，如清理环境。
- **verify**: 验证构件是否符合质量标准，并确保构建成功。
- **install**: 将打包后的构件安装到本地仓库，供本地项目依赖。
- **deploy**: 将最终构件发布到远程仓库，供其他开发者或项目使用。

```powershell
mvn package
```

## Site生命周期

Site 生命周期用于生成项目的站点文档，Maven 支持自动生成 HTML 格式的项目文档（如项目报告、Javadoc、依赖树等）。

- **pre-site**: 在站点生成之前执行的操作。
- **site**: 生成项目的站点文档。
- **post-site**: 在站点生成之后执行的操作。
- **site-deploy**: 将生成的站点文档部署到指定的服务器。

```bash
mvn site
```

## 生命周期和插件

Maven 的构建过程依赖插件完成具体的任务。每个阶段都会与一个或多个插件绑定。例如：

- `compile` 阶段会使用 `maven-compiler-plugin` 来编译代码。
- `test` 阶段会使用 `maven-surefire-plugin` 来执行单元测试。
- `package` 阶段会使用 `maven-jar-plugin` 来打包项目为 JAR 文件。

这些插件可以通过 `pom.xml` 文件中的 `<build>` 部分进行配置，以自定义某些阶段的行为。

## 跳过生命周期阶段

如果希望跳过某些阶段，例如跳过测试，可以使用以下命令：

```bash
mvn install -D skipTests
```

这会跳过 `test` 阶段，但仍然执行从 `validate` 到 `install` 的所有其他阶段。也可以对跳过生命周期进行细粒度的控制。

```xml
<plugin>
    <artifactId>maven-surefire-plugin</artifactId>
    <version>2.22.1</version>
    <configuration>
        <skipTests>true</skipTests>
        <includes><!--设置跳过测试-->
		        <include>**/User*Test.java</include><!--包含指定的测试用例-->
        </includes>
        <excludes>
            <exclude>**/User*TestCase.java</exclude><!--排除指定的测试用例-->
        </excludes>
    </configuration>
</plugin>
```

## 自定义生命周期

Maven 允许开发者通过自定义生命周期和插件来扩展构建过程。你可以绑定自定义的插件到某个生命周期的阶段，或者创建新的构建任务。例如：

```bash
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-antrun-plugin</artifactId>
            <executions>
                <execution>
                    <phase>package</phase> <!-- 自定义的插件绑定到 package 阶段 -->
                    <goals>
                        <goal>run</goal>
                    </goals>
                    <configuration>
                        <target>
                            <echo message="Running custom task during the package phase!" />
                        </target>
                    </configuration>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

## 使用场景

- **持续集成/持续交付 (CI/CD)**: Maven 的生命周期可轻松集成到 Jenkins、GitLab CI 等 CI/CD 管道中，自动化项目的构建、测试和部署。
- **模块化项目构建**: 大型项目可以通过父 POM 结合多模块 (multi-module) 构建，将多个子项目的构建过程整合在一起。
- **测试驱动开发 (TDD)**: 使用 Maven 的 `test` 阶段进行单元测试，集成测试工具（如 Surefire 插件）自动执行测试并生成报告。

# Maven插件

Maven 插件是 Maven 项目构建过程中执行特定任务的工具。每一个 Maven 插件都封装了一组功能，帮助自动化执行构建、测试、打包、部署等任务。Maven 插件可以绑定到生命周期的不同阶段，从而在执行某些阶段时自动运行相应的任务。

## 核心插件

Maven 自带的一些插件是核心插件，它们负责执行最基础的构建任务。

- maven-clean-plugin：用于清理项目的构建目录（通常是 `target` 目录），这个插件与 `clean` 生命周期绑定。
- maven-compiler-plugin：用于编译 Java 源代码，通常与 `compile` 和 `test-compile` 阶段绑定。
- maven-surefire-plugin：用于执行单元测试，通常与 `test` 生命周期阶段绑定。Surefire 插件支持 JUnit 和 TestNG。
- maven-install-plugin：用于将项目打包后的构件安装到本地仓库，通常与 `install` 阶段绑定。
- maven-deploy-plugin：用于将项目打包后的构件发布到远程仓库，通常与 `deploy` 阶段绑定。
核心插件若没有指定版本，使用Maven自带的版本。使用idea创建maven项目时，会在properties标签中指定maven.compiler.source、maven.compiler.target和project.build.sourceEncoding，这些事标准maven属性，会传递给maven-clean-plugin，从而使用正确的java编译项目。

## 常用插件

- maven-jar-plugin：用于打包项目为 JAR 文件，通常与 `package` 阶段绑定。
- maven-war-plugin：用于打包 Web 项目为 WAR 文件。
- maven-shade-plugin：用于将多个依赖打包到一个“可执行的 JAR 文件”中，同时也支持 JAR 文件的混淆和减少大小。
- maven-site-plugin：用于生成项目的站点文档，通常与 `site` 生命周期绑定。通过该插件可以生成项目的依赖报告、Javadoc、测试报告等。
- maven-checkstyle-plugin：用于检查代码是否符合规范，根据 Checkstyle 的规则进行代码风格检查。
- spring-boot-maven-plugin：用于 **打包 Spring Boot 应用**，而 **不是编译 Java 源码**。默认情况下，Spring Boot 会使用它生成 **可执行 JAR**，并嵌入 Tomcat、Jetty 等 Web 服务器

## 插件配置

- 绑定生命周期：插件可以绑定到 Maven 生命周期的某个阶段，这意味着当 Maven 运行该生命周期时，插件的相应任务会自动执行。
    
    ```xml
    <execution>
      <phase>compile</phase> <!-- 绑定到 compile 阶段 -->
      <goals>
        <goal>compile</goal> <!-- 指定要执行的目标 -->
      </goals>
    </execution>
    ```
    
- 自定义配置：许多插件允许通过 `<configuration>` 标签来自定义插件的行为。例如， `maven-compiler-plugin` 允许设置源代码和目标的 Java 版本：
    
    ```xml
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>3.8.1</version>
      <configuration>
        <source>1.8</source> <!-- 指定 Java 源版本 -->
        <target>1.8</target> <!-- 指定 Java 目标版本 -->
      </configuration>
    </plugin>
    ```
    

## 插件的执行目标

Maven 插件通过目标（Goal）来执行特定任务。一个插件可以包含多个目标。开发者可以手动执行插件的某个目标，也可以通过绑定目标到生命周期阶段来自动执行。

例如，要手动执行 `maven-compiler-plugin` 的 `compile` 目标，可以运行以下命令：

```bash
mvn compiler:compile
```

要执行 `maven-clean-plugin` 的 `clean` 目标，可以运行：

```bash
mvn clean:clean
```

## 插件管理

通过 `pluginManagement` 元素，可以为整个项目或父子模块提供插件的版本和默认配置。这在管理多模块项目时特别有用。

```xml
<build>
  <pluginManagement>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.8.1</version>
        <configuration>
          <source>1.8</source>
          <target>1.8</target>
        </configuration>
      </plugin>
    </plugins>
  </pluginManagement>
</build>
```

# 依赖管理

Maven 的依赖管理是其最强大和重要的功能之一，它自动处理项目所需的外部库，简化了项目的构建和管理。通过 `pom.xml` 文件，Maven 能够自动下载、解析和管理依赖库，并确保项目运行所需的所有依赖都能够正确解析和加载。

## 依赖坐标

每个 Maven 依赖都由以下几个属性唯一标识，统称为 **依赖坐标**（Dependency Coordinates）：

- **`groupId`**：表示组织或项目的唯一标识，一般使用反转的域名。例如，`org.springframework`。
- **`artifactId`**：表示具体的库或项目名称。它与 `groupId` 结合，唯一标识一个依赖。例如，`spring-core`。
- **`version`**：指定依赖的版本号。例如，`5.3.9`。
- **`scope`** (可选)：定义依赖的使用范围（compile、test、provided 等）。默认为 `compile`。
- **`type`**（可选）：指定依赖的打包类型（如 `jar`、`war` 等），默认为 `jar`。
- **`classifier`**（可选）：用于区分同一个 artifact 的不同变体，如区分 `sources`、`javadoc` 版本。

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
    <version>5.3.9</version>
</dependency>
```

## 依赖范围

Maven 中可以为依赖设置不同的 **范围**（Scope），以控制依赖的使用阶段和打包方式。常见的依赖范围有：

- **`compile`**（默认值）：依赖在编译、测试和运行阶段都需要使用。这是大多数依赖的默认范围，并会打包进生成的输出（如 JAR 或 WAR）。
- **`provided`**：类似于 `compile`，但不会打包到最终的输出中。这种依赖通常在编译和测试时可用，但在运行时需要由运行环境提供，如 `Servlet API`（由服务器容器提供）。
- **`runtime`**：依赖在运行时需要，但在编译时不需要。典型的例子是 JDBC 驱动程序，编译时不需要，而运行时才会用到。
- **`test`**：仅在测试编译和运行时使用，不会在正常运行时使用。常见的例子包括 JUnit 或 TestNG 测试库。
- **`system`**：依赖需要明确提供，并不会通过远程仓库下载，必须通过本地路径手动引入。这种用法较少见。

```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>servlet-api</artifactId>
    <version>2.5</version>
    <scope>provided</scope>
</dependency>
```

## 依赖传递

**依赖传递** 是 Maven 自动管理依赖关系的一大优势。当项目 A 依赖于库 B，而库 B 又依赖于库 C，Maven 会自动下载库 C，而无需在 A 的 `pom.xml` 中显式声明库 C。这一机制极大地简化了依赖管理。

```xml
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-core</artifactId>
    <version>5.4.12.Final</version>
</dependency>
```

## 依赖的版本冲突

由于 **依赖传递**，一个项目可能会间接依赖于不同版本的同一个库，这时就会发生 **依赖版本冲突**。可以通过被动与主动方式解决冲突：

- **被动方式**：Maven 会自动通过一定的规则解决依赖冲突，具体如下：
    - **最短路径优先**：Maven 会选择路径最短的依赖版本（即最接近当前项目的依赖版本）。
    - **声明顺序优先**：如果路径长度相同，Maven 会选择在 `pom.xml` 中声明的第一个依赖版本。
- **主动方式**：需要手动解决冲突问题。
    - **强制解决版本冲突**：通过 `dependencyManagement` 来强制指定依赖的版本号。即使在其他子模块中不同版本的依赖被声明，也会采用在 `dependencyManagement` 中声明的版本。主要用于管理多模块项目的依赖版本。这种方式引入的依赖，子模块将不能继承，必须手动引入。
        
        ```xml
        <dependencyManagement>
            <dependencies>
                <dependency>
                    <groupId>org.hibernate</groupId>
                    <artifactId>hibernate-core</artifactId>
                    <version>5.4.12.Final</version>
                </dependency>
            </dependencies>
        </dependencyManagement>
        ```
        
        子项目无需指定版本号。
        
        ```xml
        <dependency>
            <groupId>com.google.guava</groupId>
            <artifactId>guava</artifactId>
        </dependency>
        ```
        
    - **排除依赖**：通过 `exclusions` 元素，可以显式地从传递依赖中排除某个库。以避免冲突或减少不必要的依赖体积。
        
        ```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-logging</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>org.apache.logging.log4j</groupId>
                    <artifactId>log4j-api</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        ```
        
    - **可选依赖**：通过`optional`元素，可以显示地排除掉依赖库，让该库的依赖不再具有传递性。例如在B库中定义可选依赖，则A库将不能利用依赖传递自动下载B库的任何依赖库。
        
        ```xml
        <dependency>
            <groupId>com.itheima</groupId>
            <artifactId>maven_03_pojo</artifactId>
            <version>1.0-SNAPSHOT</version>
            <optional>false</optional>
        </dependency>
        ```
        

## 依赖范围继承

在多模块项目中，子模块会继承父模块的依赖配置。继承时，子模块的依赖范围会受到父模块配置的影响。比如，如果父模块中指定了 `provided` 范围的依赖，子模块中默认会继承相同的范围。

## 多模块下的依赖管理

# Maven仓库

Maven 仓库（Repository）是用于存储项目依赖包、构建输出和插件的存储位置。Maven 通过仓库的管理机制帮助项目自动获取所需的依赖和插件。Maven 仓库主要有三种类型：本地仓库、中央仓库和远程仓库。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668694519755-c9705fb8-c43b-42de-b23b-ddb60ad69d93.png#averageHue=%23fbf7f7&clientId=u790262c5-7cb6-4&from=paste&height=345&id=u43f657ea&originHeight=345&originWidth=857&originalType=binary&ratio=1&rotation=0&showTitle=false&size=53825&status=done&style=none&taskId=u3a18311e-9dd9-4f9f-99e0-67dd28ae8d6&title=&width=857)

## 本地仓库

- **概念**：本地仓库是存储在开发者机器上的 Maven 仓库，通常用于缓存远程仓库中下载的依赖包。每次构建时，Maven 首先检查本地仓库是否已经存在所需的依赖，如果没有才会从远程仓库下载并缓存到本地仓库。
    
- **默认位置：**本地仓库的默认位置为用户目录下的 `.m2/repository`，具体路径：
    
    - **Windows**: `C:\\Users\\{username}\\.m2\\repository`
    - **Linux/MacOS**: `/home/{username}/.m2/repository`
- **配置**：可以通过 Maven 的全局配置文件 `settings.xml` 来自定义本地仓库的位置：
    
    ```xml
    <settings>
      <localRepository>/path/to/local/repo</localRepository>
    </settings>
    ```
    
- **作用：**
    
    - **缓存依赖**: 当你第一次运行 Maven 命令时，Maven 会从远程仓库下载依赖并缓存到本地仓库，后续使用时直接从本地读取，避免重复下载。
    - **加速构建**: 使用本地仓库可以大幅减少构建时间，尤其是当网络环境不理想或依赖库较大时。
    - **离线构建**: 在离线模式下，Maven 只能使用本地仓库中的依赖进行构建（通过命令 `mvn -o`）。

## 中央仓库

中央仓库是 Maven 官方维护的一个公共仓库，包含了大量常用的开源库。Maven 默认会从中央仓库获取项目的依赖，中央仓库的地址是：

- [https://repo.maven.apache.org/maven2](https://repo.maven.apache.org/maven2)

## 远程仓库

- 概念：远程仓库是开发者或公司维护的一个仓库，用于存储自定义的构件或内部项目的构建成果。远程仓库可以是私有仓库、公司内部仓库或者第三方开源项目的仓库。
    
- 配置：在 `pom.xml` 中可以自定义远程仓库地址：
    
    ```xml
    <repositories>
      <repository>
        <id>my-repo</id>
        <url><http://repo.mycompany.com/maven2></url>
      </repository>
    </repositories>
    ```
    
- 类型：
    
    - **公共仓库**: 例如 JCenter 或者 Sonatype，这些仓库类似于中央仓库，但可能包含某些特定的开源项目。
        
    - **私有仓库**: 企业或团队可以搭建私有仓库来存储内部项目的构件或定制的第三方库。常见的私有仓库解决方案有 Nexus、Artifactory 等。
        
    - **镜像仓库**: 可以使用镜像仓库来提高下载速度，尤其是在网络不佳的情况下。通过 `settings.xml` 配置镜像仓库：
        
        ```xml
        <mirrors>
          <mirror>
            <id>aliyun-maven</id>
            <mirrorOf>central</mirrorOf>
            <url><https://maven.aliyun.com/repository/public></url>
          </mirror>
        </mirrors>
        ```
        

## 仓库的检索顺序

Maven 在构建时，会按照以下顺序查找依赖：

1. **本地仓库**: 首先检查本地仓库是否已经存在所需的依赖。
2. **远程仓库**: 如果本地仓库没有，Maven 会根据 `pom.xml` 中的配置从远程仓库获取依赖。
3. **中央仓库**: 如果没有配置远程仓库，Maven 默认会从中央仓库获取依赖。

## 部署到远程仓库

开发者可以将自己项目的构件部署到远程仓库，使其他项目能够使用这些构件。要将项目构件部署到远程仓库，通常需要配置 `distributionManagement` 部分，并指定仓库信息：

```xml
<distributionManagement>
  <repository>
    <id>releases</id>
    <url><http://repo.mycompany.com/releases></url>
  </repository>
  <snapshotRepository>
    <id>snapshots</id>
    <url><http://repo.mycompany.com/snapshots></url>
  </snapshotRepository>
</distributionManagement>
```

使用 `mvn deploy` 命令将项目部署到远程仓库。

## 远程仓库解决方案

# Maven配置

## 属性管理

Maven 中的属性管理允许你在 `pom.xml` 文件中定义和使用变量，以提高构建配置的灵活性和可维护性。通过属性，开发者可以避免硬编码配置值，便于项目的统一管理和调整。

属性（Properties）在 Maven 中可以用 `${property-name}` 的形式引用，它们可以用来定义项目的配置信息，如版本号、文件路径等。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668695232650-c305b274-b8b4-44d1-99a5-2ee2138e5ba0.png#averageHue=%23fdfde2&clientId=u790262c5-7cb6-4&from=paste&height=525&id=u7a1d8728&originHeight=525&originWidth=979&originalType=binary&ratio=1&rotation=0&showTitle=false&size=65813&status=done&style=none&taskId=u97283062-1925-483f-b73b-7aa969202b8&title=&width=979)

- 属性分类：尽量不要使用默认属性，可能会导致Idea无法正常识别项目。
    
    - 默认属性：Maven 自带了一些默认属性，供项目在 `pom.xml` 中使用。常见的默认属性包括：
        
        - `${project.basedir}`：项目根目录。
        - `${project.version}`：项目的版本。
        - `${project.artifactId}`：项目的 Artifact ID。
        - `${project.groupId}`：项目的 Group ID。
        - `${project.build.directory}`：构建目录，默认为 `target`。
    - 自定义属性：可以在 `pom.xml` 的 `<properties>` 标签中定义自定义属性，并在其他地方引用。
        
        ```xml
        <properties>
            <java.version>17</java.version>
            <encoding>UTF-8</encoding>
        </properties>
        ```
        
        ```xml
        <build>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.8.1</version>
                    <configuration>
                        <source>${java.version}</source>
                        <target>${java.version}</target>
                    </configuration>
                </plugin>
            </plugins>
        </build>
        ```
        
        ```xml
        jdbc.driver=com.mysql.jdbc.Driver
        jdbc.url=${jdbc.url}
        jdbc.username=root
        jdbc.password=root
        ```
        
    - 环境变量属性：Maven 允许引用操作系统的环境变量作为属性。格式为 `${env.VARIABLE_NAME}`。
        
        ```xml
        <properties>
            <path>${env.PATH}</path>
        </properties>
        ```
        
    - 命令行属性：在运行 Maven 命令时，可以使用 `-Dproperty=value` 的方式为属性赋值。这些属性只在当前执行的命令中生效。
        
        ```bash
        mvn clean install -DskipTests=true
        ```
        
        ```xml
        <properties>
            <skipTests>false</skipTests>
        </properties>
        <build>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-surefire-plugin</artifactId>
                    <version>2.22.2</version>
                    <configuration>
                        <skipTests>${skipTests}</skipTests>
                    </configuration>
                </plugin>
            </plugins>
        </build>
        ```
        
- 属性传递
    
    在多模块（子模块）项目中，子项目可以继承父项目中的属性。父项目中的属性可以在子项目的 `pom.xml` 中被引用和使用。
    
    ```xml
    <properties>
        <version.spring>5.3.8</version.spring>
    </properties>
    ```
    
    ```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${version.spring}</version>
        </dependency>
    </dependencies>
    ```
    
- profile属性
    
    在不同的 Maven 构建配置文件（Profile）中，可以定义不同的属性集，以适应不同的构建环境（如开发环境、测试环境、生产环境）。
    
    ```xml
    <profiles>
        <profile>
            <id>dev</id>
            <properties>
                <db.url>jdbc:mysql://localhost/devdb</db.url>
            </properties>
        </profile>
        <profile>
            <id>prod</id>
            <properties>
                <db.url>jdbc:mysql://localhost/proddb</db.url>
            </properties>
        </profile>
    </profiles>
    ```
    
- settings.xml属性
    
    在 Maven 的 `settings.xml` 配置文件中也可以定义属性。这些属性通常用于全局或用户特定的配置，优先级比 `pom.xml` 中的低。
    
- 属性优先级
    
    在 Maven 中，属性的优先级顺序是：
    
    1. **命令行属性**（最高优先级）。
    2. **`pom.xml` 中定义的属性**。
    3. **环境变量属性**。
    4. **父 POM 传递的属性**。
    5. **`settings.xml` 中的属性**（全局配置）。
    6. **Maven 默认属性**（最低优先级）。

## 多环境配置

在实际的 Java 项目中，开发、测试、生产等不同的环境可能需要不同的配置。Maven 提供了 **多环境配置**（Profiles）功能，允许根据环境动态切换项目的配置，如不同的数据库连接、日志级别、依赖包等。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668695273615-8afaf8d9-7dfb-4fd6-9d0b-593e76150c2b.png#averageHue=%23f8f7f7&clientId=u790262c5-7cb6-4&from=paste&height=509&id=u009d244f&originHeight=509&originWidth=989&originalType=binary&ratio=1&rotation=0&showTitle=false&size=49141&status=done&style=none&taskId=u7937ada5-c149-4edf-8874-d3f8c0e9cb1&title=&width=989)

- 配置
    
    Profiles 一般定义在 `pom.xml` 中的 `<profiles>` 标签中，可以在其中根据不同环境来设置插件、属性、依赖等。
    
    ```xml
    <project>
        <!-- 项目的一般配置 -->
        <modelVersion>4.0.0</modelVersion>
        <groupId>com.example</groupId>
        <artifactId>multi-environment-app</artifactId>
        <version>1.0-SNAPSHOT</version>
    
        <!-- Profile 配置 -->
        <profiles>
            <!-- 开发环境 -->
            <profile>
                <id>dev</id>
                <properties>
                    <db.url>jdbc:mysql://localhost/devdb</db.url>
                    <db.username>devuser</db.username>
                    <db.password>devpassword</db.password>
                </properties>
            </profile>
    
            <!-- 测试环境 -->
            <profile>
                <id>test</id>
                <properties>
                    <db.url>jdbc:mysql://localhost/testdb</db.url>
                    <db.username>testuser</db.username>
                    <db.password>testpassword</db.password>
                </properties>
            </profile>
    
            <!-- 生产环境 -->
            <profile>
                <id>prod</id>
                <properties>
                    <db.url>jdbc:mysql://localhost/proddb</db.url>
                    <db.username>produser</db.username>
                    <db.password>prodpassword</db.password>
                </properties>
            </profile>
        </profiles>
    
        <!-- 使用属性的地方 -->
        <build>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-resources-plugin</artifactId>
                    <version>3.2.0</version>
                    <configuration>
                        <filtering>true</filtering>
                        <properties>
                            <db.url>${db.url}</db.url>
                            <db.username>${db.username}</db.username>
                            <db.password>${db.password}</db.password>
                        </properties>
                    </configuration>
                </plugin>
            </plugins>
        </build>
    </project>
    ```
    

# Maven多模块项目

## 继承与聚合

- **继承**：Maven 允许一个项目继承另一个项目的 `POM` 文件，以复用父项目中的配置。通过继承，可以减少重复代码，实现项目的统一配置管理。继承机制主要通过 `parent` 元素来实现。
    
    - 子项目继承了父项目的依赖、插件、版本信息等配置。
        
    - 子项目可以覆盖或扩展父项目的某些配置。
        
    - 只能继承一个父项目，不能多重继承。
        
        ```xml
        <parent>
            <groupId>com.dong</groupId>
            <artifactId>Maven_demo</artifactId>
            <version>1.0-SNAPSHOT</version>
        </parent>
        ```
        
- 聚合：聚合指的是通过一个父项目（聚合项目）来同时管理多个子模块。父项目可以将多个子模块聚合到一起，在构建父项目时，可以一次性构建所有子模块。聚合通过 `modules` 元素来实现。
    
    - 父项目本身不包含源代码，通常是一个 `pom` 类型的项目（打包方式设置为pom）。
        
    - 父项目的构建会自动触发所有子模块的构建。
        
    - 聚合和继承可以结合使用，但它们并不是必须一起使用的概念。
        
        ```xml
        <modules>
          <module>maven_demo_1</module>
        </modules>
        ```
        

## 多模块项目设置

- 创建使用Maven管理的空的父项目（只保留pom.xml文件）。
- 添加模块，将父项目标记为空项目，这样在父、子项目的pom文件中，就会自动添加parent和modules元素，并将父项目打包方式设置为pom（不生成可执行文件）。
- 在父项目`dependencies` 元素中添加子项目通用的依赖，并使用`properties` 元素设置依赖的版本号，子项目将自动继承这些依赖。
- 在父项目`dependencyManagement` 元素中添加子项目的可选依赖，并使用`properties` 元素设置可选依赖的版本号，子项目添加这些可选依赖后，将自动继承父项目设置的版本号。

此依赖用于控制依赖版本，类似的还有spring和springcloud的。父项目中需要添加type和scope。

添加依赖之前，可以点进去搜一下，看是否已经指定了版本，避免依赖冲突

```sql
    <properties>
        <java.version>17</java.version>
        <spring-boot.version>3.0.0</spring-boot.version>
    </properties>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>${spring-boot.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```

在 Maven 的 `pom.xml` 文件中，使用 `type` 和 `scope` 是为了更好地管理依赖的使用方式和生命周期。具体来说，它们的作用如下：

### 1. `type`

- **定义依赖的类型**：`type` 元素用于指定依赖的类型，最常用的类型是 `jar`，但还有其他类型，比如 `pom`、`war`、`ear` 等。
- **示例**：
    - `jar`：普通的 Java 库。
    - `pom`：表示这是一个 POM 文件，通常用于依赖管理（如 `spring-boot-dependencies`），而不是直接使用的库。

在 `spring-boot-dependencies` 的情况下，设置 `type` 为 `pom` 是因为它只是一个用于版本管理的 POM 文件，而不是一个实际的可执行或可引用的 JAR 文件。

### 2. `scope`

- **定义依赖的可见性和使用范围**：`scope` 元素用于指定依赖的作用域，这会影响到项目编译、运行和打包时对依赖的可见性。常用的范围包括：
    - **compile**（默认）：所有阶段都可用，编译、测试、运行时都可用。
    - **provided**：在编译时可用，但在运行时由 JDK 或应用服务器提供，通常用于 Servlet API 等。
    - **runtime**：编译时不可用，但在运行时可用，适合数据库驱动等。
    - **test**：仅在测试时可用，编译和运行时都不可用，适合测试框架等。
    - **system**：在编译和运行时可用，依赖必须在系统路径中。

对于 `spring-boot-dependencies`，通常将 `scope` 设置为 `import`，表示要 **导入该 POM 文件中定义的依赖版本信息**，但不会直接添加该依赖本身。

应该仅对版本管理Pom设置`type=pom` 和 `scope=import`，例如一些带dependencies的依赖，这些 BOM 仅用于提供统一版本管理，并不会直接引入具体的 JAR 文件。