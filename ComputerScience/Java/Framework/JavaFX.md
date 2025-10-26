# 概述

## 简介

JavaFX 是一个使用 Java 开发桌面应用程序的框架，提供了丰富的 UI 控件、布局管理、动画、图形、媒体和样式

JavaFX 是 Java 官方的 GUI 库

## 要求

JavaFX 23 要求 JDK 最低版本为 21

## 与 Swing 的区别

> JavaFX 与 Swing 都是 Java 用于开发图形界面的技术，但两者有显著的区别：
> 
> - **Swing** 是一种轻量级 GUI 库，它是在 `AWT`（Abstract Window Toolkit）的基础上构建的，主要用于传统的桌面应用。
> - **JavaFX** 是一个较新的 GUI 库，提供了更现代、更丰富的界面开发工具，支持高效的硬件加速（通过 OpenGL 或 DirectX），并具有动画和 3D 图形等高级功能。

## 基本结构

JavaFX 应用程序的结构主要由以下几个部分组成：

- **Stage**：表示应用程序的主窗口。每个 JavaFX 应用程序必须有一个 `Stage`，它是容器，所有的 UI 控件都在其上展示。
- **Scene**：表示在 `Stage` 中显示的内容。每个 `Stage` 至少有一个 `Scene`，`Scene` 可以包含图形、控件、布局等内容。
- **Node**：`Node` 是所有 JavaFX 图形元素的基本类。`Node` 类的子类包括 `Control`（UI 控件）、`Shape`（图形）、`Group`（容器）等。所有显示在 `Scene` 上的元素都是 `Node` 对象。

## 控件

- **Label**：显示静态文本。
- **Button**：按钮控件。
- **TextField**：用于接收用户输入的文本框。
- **TextArea**：多行文本框。
- **ComboBox**：下拉框控件。
- **ListView**：显示列表的控件。
- **TableView**：显示表格的控件，支持数据绑定和排序。
- **TreeView**：显示树形结构的控件。

## 布局管理器

JavaFX 提供了多个布局容器来帮助自动化管理控件的位置和大小。常见的布局管理器包括：

- **HBox**：水平排列子控件。
- **VBox**：垂直排列子控件。
- **BorderPane**：包含五个区域（上、下、左、右、中心）的布局，适合常见的页面结构。
- **GridPane**：基于网格的布局管理器，可以控制控件在网格中的位置。
- **StackPane**：将子控件堆叠在一起，通常用于需要覆盖显示的控件。

## 事件处理

JavaFX 提供了灵活的事件处理机制，用户交互事件（例如点击按钮、输入文本等）会触发事件，开发者可以为这些事件定义相应的处理逻辑。

- **事件类型**：JavaFX 支持多种事件类型，如鼠标事件、键盘事件、窗口事件等。
- **事件监听**：通过 `setOnAction()`、`setOnMouseClicked()` 等方法为控件设置事件处理程序。

```java
Button button = new Button("Click Me");
button.setOnAction(e -> System.out.println("Button clicked!"));
```

## 数据绑定

数据绑定是 JavaFX 的一个强大功能，它使得 UI 控件与数据模型之间保持同步。当数据变化时，UI 会自动更新。常见的绑定方法包括：

- **单向绑定**：一个对象的属性值绑定到另一个对象的属性值。
- **双向绑定**：两个对象的属性互相绑定，当一个对象的值变化时，另一个对象的值也会自动更新。

```java
IntegerProperty value1 = new SimpleIntegerProperty(5);
IntegerProperty value2 = new SimpleIntegerProperty();
value2.bind(value1);  // value2 将与 value1 绑定
```

## 动画与过渡

JavaFX 提供了强大的动画和过渡功能，可以为 UI 控件添加动画效果，增加应用的互动性和视觉吸引力。常用的动画类有：

- **Timeline**：控制时间轴上的动画效果。
- **KeyFrame**：定义一个动画的关键帧。
- **FadeTransition**、**TranslateTransition** 等：这些类是过渡动画的具体实现，用于处理特定的动画效果（如淡入淡出、平移、缩放等）。

```java
FadeTransition fadeTransition = new FadeTransition(Duration.seconds(2), button);
fadeTransition.setFromValue(1.0);
fadeTransition.setToValue(0.0);
fadeTransition.play();
```

## CSS 样式

JavaFX 允许使用 CSS 来样式化控件和布局，类似于 Web 开发中的样式设置。通过外部 CSS 文件或内联 CSS 样式来设置控件的外观，如字体、颜色、边框等。

```css
.button {
    -fx-background-color: #ff0000;
    -fx-text-fill: white;
}
```

## 3D 图形

JavaFX 支持 3D 图形的渲染，能够创建和渲染三维对象。你可以使用 `Shape3D` 类及其子类来构建 3D 图形，并通过摄像机（`Camera`）来控制视角。

- **Box**：表示一个 3D 的立方体。
- **Sphere**：表示一个 3D 的球体。
- **Cylinder**：表示一个 3D 的圆柱体。
- **PhongMaterial**：用于设置 3D 图形的材质和纹理。

## FXML 和 **Scene Builder**

- **FXML** 是 JavaFX 提供的一种 XML 格式的文件，用于描述用户界面的结构。通过 FXML 文件，可以将界面布局与应用逻辑分离，使得界面设计和代码逻辑更清晰。
- **Scene Builder** 是 JavaFX 提供的一个可视化界面设计工具，开发者可以通过拖拽控件的方式快速构建 JavaFX 应用的 UI，生成 FXML 文件。

# 使用

## 入门

首先需要添加依赖，其中 `javafx-graphics` 已经被依赖传递到了 `javafx-controls`

- pom.xml
    
    ```xml
     <dependency>
         <groupId>org.openjfx</groupId>
         <artifactId>javafx-controls</artifactId>
         <version>23.0.1</version>
     </dependency>
     <dependency>
         <groupId>org.openjfx</groupId>
         <artifactId>javafx-fxml</artifactId>
         <version>23.0.1</version>
     </dependency>
    ```
    

因为 JavaFX 是基于模块化开发的，需要在虚拟机参数中传入项目中依赖的库

- VM Options
    
    ```bash
    --module-path
    "D:\\Development\\JavaFX\\javafx-sdk-23.0.1\\lib"
    --add-modules
    javafx.controls,javafx.fxml
    ```
    

最后就是开发一个小功能了，有一个按钮，每次点击都会在控制台打印 Hello World

- JavaFXExample
    
    ```java
    public class JavaFXExample extends Application {
        @Override
        public void start(Stage stage) throws Exception {
            Button button = new Button("Click Me!");
            button.setOnAction(e -> System.out.println("Hello, World!"));
    
            StackPane root = new StackPane();
            root.getChildren().add(button);
    
            Scene scene = new Scene(root, 300, 250);
            stage.setTitle("JavaFX Example");
            stage.setScene(scene);
            stage.show();
        }
    
        public static void main(String[] args) {
            launch(args);
        }
    }
    ```