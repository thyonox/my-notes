# IO与NIO
## IO概述


### 结构体系


## 文件操作





## IO类
### IO基类




### 字节流







### 字符流






### 转换流




## NIO




# 基础概念

Java中的I/O（输入/输出）是指Java程序与文件、网络、控制台等数据源进行读写操作的机制。Java提供了丰富的I/O类库，这些类库位于`java.io`和`java.nio`包中。I/O的基本概念主要围绕输入和输出操作，分为字节流和字符流。

## 输入流和输出流

- **输入流（Input Stream）**: 用于从数据源（如文件、网络连接等）读取数据。输入流的方向是从外部数据源到Java程序内部。
- **输出流（Output Stream）**: 用于将数据从Java程序写入到目标数据源（如文件、网络连接等）。输出流的方向是从Java程序内部到外部数据源。

## 字节流和字符流

- **字节流（Byte Stream）**:
    - 用于处理原始的二进制数据（如图像、音频、视频文件）。
    - 以字节（8位）为单位进行数据传输和处理。
    - 字节流类以`InputStream`和`OutputStream`为基类，分别用于输入和输出操作。
- **字符流（Character Stream）**:
    - 用于处理文本数据（如读取或写入文本文件）。
    - 以字符（16位）为单位进行数据传输和处理，支持不同字符编码（如UTF-8, UTF-16等）。
    - 字符流类以`Reader`和`Writer`为基类，分别用于输入和输出操作。

## 节点流和处理流

- **节点流：是直接连接到数据源或数据目标的流。它们用于直接从数据源读取数据（如文件、字节数组、字符串、网络连接等）或者向数据目标写入数据。**
    - 直接与数据源或目标相连。
    - 负责从数据源读取数据或将数据写入目标。
    - 不能进行数据的缓冲、转换或过滤等处理。
    - 节点流包括：访问文件、数组、管道、字符串、套接字等。
- **处理流：（也称为过滤流）是包装在节点流之上的流，它们用于对从节点流读取的数据或写入节点流的数据进行处理（如缓冲、转换、过滤等）。**
    - 不能直接连接到数据源或目标，必须依赖于节点流。
    - 提供数据的缓冲、转换、过滤等高级处理功能。
    - 可以嵌套多个处理流来实现复杂的数据处理操作。
    - 关闭外层处理流，也会相应关闭内层节点流。

## 结构层次

|分类|字节输入流|字节输出流|字符输入流|字符输出流|
|---|---|---|---|---|
|抽象基类|`InputStream`|`OutputStream`|`Reader`|`Writer`|
|访问文件|`FileInputStream`|`FileOutputStream`|`FileReader`|`FileWriter`|
|访问数组|`ByteArrayInputStream`|`ByteArrayOutputStream`|`CharArrayReader`|`CharArrayWriter`|
|访问管道|`PipedInputStream`|`PipedOutputStream`|`PipedReader`|`PipedWriter`|
|访问字符串|||`StringReader`|`StringWriter`|
|访问套接字|`SocketInputStream`|`SocketOutputStream`|||
|缓冲流|`BufferedInputStream`|`BufferedOutputStream`|`BufferedReader`|`BufferedWriter`|
|转换流|||`InputStreamReader`|`OutputStreamWriter`|
|对象流|`ObjectInputStream`|`ObjectOutputStream`|||
|筛选流|`FilterInputStream`|`FilterOutputStream`|`FilterReader`|`FilterWriter`|
|打印流||`PrintStream`||`PrintWriter`|
|推回输入流|`PushbackInputStream`||`PushbackReader`||
|数据流|`DataInputStream`|`DataOutputStream`|||

# 抽象基类

Java中的I/O抽象基类主要包括四个：`InputStream`、`OutputStream`、`Reader`和`Writer`。这些类定义了Java I/O系统的核心接口，提供了对输入和输出操作的基本方法和行为规范。

## `InputStream`

`InputStream`是所有字节输入流的抽象基类，用于从数据源（如文件、网络连接、内存等）中读取字节数据。

**常用方法：**

- `int read()`: 从输入流中读取一个字节，返回读取的字节数据（0到255之间的整数）。如果到达流的末尾，返回`-1`。
- `int read(byte[] b)`: 从输入流中读取若干字节数据，存储到字节数组`b`中，返回实际读取的字节数。如果到达流的末尾，返回`-1`。
- `int read(byte[] b, int off, int len)`: 从输入流中读取最多`len`个字节数据，存储到字节数组`b`中从索引`off`开始的位置，返回实际读取的字节数。如果到达流的末尾，返回`-1`。
- `long skip(long n)`: 跳过并丢弃输入流中的`n`个字节，返回实际跳过的字节数。
- `int available()`: 返回可以不受阻塞地读取的字节数。
- `void close()`: 关闭输入流并释放与该流相关的所有系统资源。
- `void mark(int readlimit)`: 标记当前流中的位置，之后可以使用`reset()`方法回到该位置。`readlimit`参数表示在该标记失效前可以读取的字节数。
- `void reset()`: 将输入流重新定位到上次调用`mark()`时的位置。
- `boolean markSupported()`: 判断输入流是否支持`mark()`和`reset()`方法。

## `OutputStream`

`OutputStream`是所有字节输出流的抽象基类，用于将字节数据写入到目标数据源（如文件、网络连接、内存等）。将构造方法第二个参数设置为`true`，输出流将不会覆盖文件，而是会**追加到文件末尾**。

**常用方法：**

- `void write(int b)`: 将指定的字节写入输出流。
- `void write(byte[] b)`: 将字节数组`b`中的所有字节写入输出流。
- `void write(byte[] b, int off, int len)`: 将字节数组`b`从索引`off`开始的`len`个字节写入输出流。
- `void flush()`: 刷新输出流，强制将所有缓冲数据写入到目标数据源。
- `void close()`: 关闭输出流并释放与该流相关的所有系统资源。

## `Reader`

`Reader`是所有字符输入流的抽象基类，用于从数据源（如文件、网络连接、内存等）中读取字符数据。

**常用方法：**

- `int read()`: 读取单个字符并返回，如果到达流的末尾，则返回`-1`。
- `int read(char[] cbuf)`: 从输入流中读取若干字符并存储到字符数组`cbuf`中，返回实际读取的字符数。如果到达流的末尾，返回`-1`。
- `int read(char[] cbuf, int off, int len)`: 从输入流中读取最多`len`个字符，并将它们存储在字符数组`cbuf`中从索引`off`开始的位置，返回实际读取的字符数。如果到达流的末尾，返回`-1`。
- `long skip(long n)`: 跳过并丢弃输入流中的`n`个字符，返回实际跳过的字符数。
- `boolean ready()`: 判断输入流是否可以在不阻塞的情况下读取字符。
- `void close()`: 关闭输入流并释放与该流相关的所有系统资源。
- `void mark(int readAheadLimit)`: 标记当前流中的位置，之后可以使用`reset()`方法回到该位置。
- `void reset()`: 将输入流重新定位到上次调用`mark()`时的位置。
- `boolean markSupported()`: 判断输入流是否支持`mark()`和`reset()`方法。

## `Write`

`Writer`是所有字符输出流的抽象基类，用于将字符数据写入到目标数据源（如文件、网络连接、内存等）。将构造方法第二个参数设置为`true`，输出流将不会覆盖文件，而是会**追加到文件末尾**。

**常用方法：**

- `void write(int c)`: 写入单个字符。
- `void write(char[] cbuf)`: 将字符数组`cbuf`中的所有字符写入输出流。
- `void write(char[] cbuf, int off, int len)`: 将字符数组`cbuf`从索引`off`开始的`len`个字符写入输出流。
- `void write(String str)`: 将字符串`str`写入输出流。
- `void write(String str, int off, int len)`: 将字符串`str`从索引`off`开始的`len`个字符写入输出流。
- `void flush()`: 刷新输出流，强制将所有缓冲字符写入到目标数据源。
- `void close()`: 关闭输出流并释放与该流相关的所有系统资源。

# 文件操作类

在Java中，文件操作类主要用于处理文件和目录的创建、删除、重命名、读取、写入、以及属性的检查等操作。Java为这些操作提供了多个类，其中最常用的是`File`类。此外，Java 7引入的NIO包（`java.nio.file`）提供了更现代化的文件操作方法，如`Files`和`Paths`类。

## 构造方法

- `File(String pathname)`: 通过文件路径名创建一个新的`File`实例。
- `File(String parent, String child)`: 根据父路径名字符串和子路径名字符串创建一个新的`File`实例。
- `File(File parent, String child)`: 根据父路径名（`File`对象）和子路径名字符串创建一个新的`File`实例。

## 常用方法

- **文件和目录操作**
    - `boolean createNewFile()`: 创建一个新的空文件，如果文件已存在则返回`false`。
    - `boolean mkdir()`: 创建一个目录，如果目录已存在则返回`false`。
    - `boolean mkdirs()`: 创建多级目录，如果上级目录不存在，也会创建。
    - `boolean delete()`: 删除文件或空目录，成功返回`true`。
    - `boolean renameTo(File dest)`: 将文件或目录重命名为指定的目标名称。
- **文件和目录信息**
    - `String getName()`: 获取文件或目录的名称。
    - `String getPath()`: 获取文件或目录的路径。
    - `String getAbsolutePath()`: 获取文件或目录的绝对路径。
    - `String getParent()`: 获取文件或目录的父路径。
    - `boolean exists()`: 判断文件或目录是否存在。
    - `boolean isFile()`: 判断是否为文件。
    - `boolean isDirectory()`: 判断是否为目录。
    - `long length()`: 获取文件的长度（字节数）。如果文件不存在或是一个目录，返回`0`。
- **文件和目录的权限**
    - `boolean canRead()`: 判断文件是否可读。
    - `boolean canWrite()`: 判断文件是否可写。
    - `boolean canExecute()`: 判断文件是否可执行。
    - `boolean setReadable(boolean readable)`: 设置文件的可读权限。
    - `boolean setWritable(boolean writable)`: 设置文件的可写权限。
    - `boolean setExecutable(boolean executable)`: 设置文件的可执行权限。
- **遍历目录**
    - `String[] list()`: 返回一个字符串数组，表示目录中的文件和目录名称。
    - `File[] listFiles()`: 返回一个`File`对象数组，表示目录中的文件和目录。
- **文件路径的操作**
    - `boolean isAbsolute()`: 判断路径是否为绝对路径。
    - `File getAbsoluteFile()`: 返回绝对路径形式的文件对象。
    - `File getCanonicalFile()`: 返回文件的规范路径形式（去除冗余的符号，如`.`和`..`）。

# 常用IO类

## 文件流

文件流是Java I/O中用于读写文件数据（图像、视频、音频和文本等数据）的流。

- **`FileInputStream`/`FileOutputStream`**: 将文件提供字节流操作。
- **`FileReader`/`FileWriter`**: 为文件提供字符流操作。

```java
public static void fileWriter()  {
 FileReader fileReader = null;
 try {
	 fileReader = new FileReader(new File("E:\\\\Desktop\\\\a.txt"));
	 // 缓存字符的字符数组
	 char[] chars = new char[1024];
	 // 读取的长度，控制打印范围
	 int len;
	 while((len = fileReader.read(chars)) != -1) {
		 System.out.print(new String(chars ,0 ,len));
	 }
 } catch (IOException e) {
	 e.printStackTrace();
 }finally {
			 // 关闭流
	 if (fileReader != null) {
		 try {
			 fileReader.close();
		 } catch (IOException e) {
			 e.printStackTrace();
		 }
	 }
 }
}
```

## 缓冲流

是对字节流和字符流的包装。缓冲流使用内存中的一个缓冲区来存储数据。数据在缓冲区中进行临时存储，从而减少对底层I/O设备的读写次数，优化性能。

- **`BufferedInputStream`/`BufferedOutputStream`**: 为字节流提供缓冲。
- **`BufferedReader`/`BufferedWriter`**: 为字符流提供缓冲。
- 缓冲区默认大小8KB，可以通过构造方法指定缓冲区大小。
- **读取数据**: 当从缓冲输入流读取数据时，首先从缓冲区中获取数据。如果缓冲区为空，则从底层输入流中读取数据并填充缓冲区。
- **写入数据**: 当向缓冲输出流写入数据时，数据首先被写入到缓冲区中。只有当缓冲区满或显式刷新（flush）时，数据才会被实际写入到底层输出流。
- 关闭流时会自动调用`flush()`方法，但推荐显示调用，避免潜在的数据遗失。

**特有方法：**

- **`String readLine()`**:读取一行文本，直到读取到行尾（由换行符或回车符标识）或到达流的结尾。返回读取到的行内容（不包含行结束符），如果流的结尾已到达则返回`null`。
- **`void newLine()`**:写入一个平台无关的新行符。根据操作系统的不同，具体的换行符可能是`\\n`（Unix/Linux），`\\r\\n`（Windows）或其他形式。

```java
// 文件复制
public static void copyFile(String starFile, String endFile) {
 BufferedInputStream in = null;
 BufferedOutputStream out = null;
 BufferedReader reader = null;
 BufferedWriter writer = null;
 File file = new File(starFile);
 // 判断源文件是否存在
 if (file.exists()) {
	 String str = file.getName().split("\\\\.")[1];
	 // 字符文件
	 if (str.equals("txt") || str.equals("java") || str.equals("c") || str.equals("cpp")) {
		 try {
			 reader = new BufferedReader(new FileReader(starFile));
			 writer = new BufferedWriter(new FileWriter(endFile));
			 String s;
			 // 每次读一行数据
			 while ((s = reader.readLine()) != null) {
				 writer.write(s);
				 // 写入行分割符
				 writer.newLine();
			 }
			 // 强制刷出缓冲区数据
			 writer.flush();
		 } catch (IOException e) {
			 e.printStackTrace();
		 }finally {
			 closeFile(reader,writer);
		 }
	 } else {
		 // 字节文件
		 try {
			 in = new BufferedInputStream(new FileInputStream(starFile));
			 out = new BufferedOutputStream(new FileOutputStream(endFile));
			 int len;
			 while ((len = in.read()) != -1) {
				 out.write(len);
			 }
			 out.flush();
		 } catch (IOException e) {
			 e.printStackTrace();
		 } finally {
			 closeFile(in,out);
		 }
	 }
 } else {
	 System.out.println("没有此文件，即将创建！");
	 try {
		 System.out.println("是否创建成功："+file.createNewFile());
		 copyFile(file.getName(),endFile);
	 } catch (IOException e) {
		 e.printStackTrace();
	 }
 }

// 关闭字符缓冲
public static void closeFile(BufferedReader reader,BufferedWriter writer){
 if (reader != null) {
	 try {
		 reader.close();
	 } catch (IOException e) {
		 e.printStackTrace();
	 }
 }
 if (writer != null) {
	 try {
		 writer.close();
	 } catch (IOException e) {
		 e.printStackTrace();
	 }
 }
}

// 关闭字节缓冲
public static void closeFile(BufferedInputStream in,BufferedOutputStream out){
 if (in != null) {
	 try {
		 in.close();
	 } catch (IOException e) {
		 e.printStackTrace();
	 }
 }
 if (out != null) {
	 try {
		 out.close();
	 } catch (IOException e) {
		 e.printStackTrace();
	 }
 }
}
```

## 数据流

在Java中，数据流（Data Streams）用于以平台无关的方式处理Java基本数据类型（如`int`、`double`、`char`等）和字符串。数据流在读写过程中保持数据的完整性和兼容性，因此可以在不同的机器和操作系统之间传输数据。

### 基本概念

- **数据输入流（Data Input Stream）**: 允许应用程序以机器无关的方式从基础输入流中读取Java的基本数据类型。数据流以**字节序列**的方式读取数据，通常用于文件、网络连接等数据源。
- **数据输出流（Data Output Stream）**: 允许应用程序以机器无关的方式向基础输出流中写入Java的基本数据类型。数据流以**字节序列**的方式写入数据，通常用于文件、网络连接等目标数据源。

### 特点

- **平台无关性**: 数据流以[大端字节序](https://www.notion.so/6474470260ea4626affd5ee8f702f121?pvs=21)（Big-Endian）写入或读取数据，无论在何种平台上，这种顺序保持不变，因此数据具有平台独立性。
- **高效性**: 数据流直接处理原始的字节数据，因此比字符流更高效，特别是在读写大量原始数据时。
- **顺序性**: 数据流以顺序的方式读写数据，这意味着读数据的顺序必须与写数据的顺序一致（IFIO），否则会导致数据读取错误。

### 使用场景

- 适用于需要读写基本数据类型的应用程序，如数据存储、网络通信、游戏数据存储、文件存储等。
- 在网络通信中，数据流可用于确保不同平台之间的数据交换兼容性。

### 常用类

- **`DataInputStream`**: 从输入流中读取Java基本数据类型和字符串。
    - `readBoolean()`: 读取一个布尔值。
    - `readByte()`: 读取一个字节。
    - `readChar()`: 读取一个字符。
    - `readDouble()`: 读取一个双精度浮点数。
    - `readFloat()`: 读取一个单精度浮点数。
    - `readInt()`: 读取一个整数。
    - `readLong()`: 读取一个长整数。
    - `readShort()`: 读取一个短整数。
    - `readUTF()`: 读取一个用UTF-8编码的字符串。
- **`DataOutputStream`**: 向输出流中写入Java基本数据类型和字符串。
    - `writeBoolean(boolean v)`: 写入一个布尔值。
    - `writeByte(int v)`: 写入一个字节。
    - `writeChar(int v)`: 写入一个字符。
    - `writeDouble(double v)`: 写入一个双精度浮点数。
    - `writeFloat(float v)`: 写入一个单精度浮点数。
    - `writeInt(int v)`: 写入一个整数。
    - `writeLong(long v)`: 写入一个长整数。
    - `writeShort(int v)`: 写入一个短整数。
    - `writeUTF(String s)`: 写入一个用UTF-8编码的字符串。

### 代码示例

```java
import java.io.*;

public class ReadLargeDatFile {
    public static void main(String[] args) {
        try (DataInputStream dis = new DataInputStream(new BufferedInputStream(new FileInputStream("largefile.dat")))) {
            // 假设文件结构是：每组数据由一个整数、一个双精度浮点数和一个UTF字符串组成
            while (dis.available() > 0) { // 只要还有可用字节，就继续读取
                int intValue = dis.readInt();
                double doubleValue = dis.readDouble();
                String utfString = dis.readUTF();
                
                // 对读取到的数据进行处理
                System.out.println("Int: " + intValue + ", Double: " + doubleValue + ", String: " + utfString);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## 对象流

Java中的对象流用于读写对象，即序列化（将对象转换为字节流）和反序列化（将字节流恢复为对象）。

### 基本概念

用于在Java应用程序之间传输对象或将对象保存到文件中。对象流的核心操作是序列化和反序列化。

- **序列化（Serialization）**：将Java对象的状态转换为字节流，以便存储或传输。
- **反序列化（Deserialization）**：将字节流恢复为Java对象。

### 常用类

- `ObjectInputStream`：从输入流中读取字节流并将其反序列化为Java对象。
    - `readObject()`：从输入流中读取字节流并将其反序列化为对象。
- `ObjectOutputStream`：将Java对象序列化为字节流并写入到输出流中。
    - `writeObject(Object obj)`：将对象序列化并写入输出流。

### 要求与规则

- **`Serializable`接口**：对象序列化的类必须实现`java.io.Serializable`接口。该接口是一个**标记接口**（没有任何方法），用来标识该类的对象是可序列化的。
- **`transient`关键字**：用于标识不希望被序列化的字段。在序列化过程中，`transient`修饰的字段会被忽略。
- **`serialVersionUID`**：序列化版本号，用于标识序列化类的版本。如果类的版本不匹配，可能会导致反序列化失败。
- **`.ser`**：序列化后的对象数据将被保存在该后缀名的文件中。

### 注意事项

- **序列化版本不匹配**：如果在序列化之后修改了类结构（如增加或删除字段），反序列化时可能会抛出`InvalidClassException`。此时，可以通过显式声明`serialVersionUID`来避免该问题。
- **性能和安全性**：序列化可能会影响性能，特别是在大型对象上。此外，从不可信源反序列化数据可能会引入安全漏洞。
- **自定义序列化**：可以通过实现`Externalizable`接口来自定义序列化逻辑。

### 示例代码

**序列化：**

```java
import java.io.FileOutputStream;
import java.io.ObjectOutputStream;
import java.io.Serializable;

class Person implements Serializable {
    private static final long serialVersionUID = 1L; // 序列化版本号
    private String name;
    private int age;
    transient private String password; // 不希望被序列化的字段

    public Person(String name, int age, String password) {
        this.name = name;
        this.age = age;
        this.password = password;
    }

    // Getters and toString()...
}

public class SerializeExample {
    public static void main(String[] args) {
        Person person = new Person("Alice", 30, "secret");

        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("person.ser"))) {
            oos.writeObject(person); // 序列化对象
            System.out.println("Object serialized successfully.");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

**反序列化：**

```java
import java.io.FileInputStream;
import java.io.ObjectInputStream;

public class DeserializeExample {
    public static void main(String[] args) {
        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream("person.ser"))) {
            Person person = (Person) ois.readObject(); // 反序列化对象
            System.out.println("Object deserialized successfully.");
            System.out.println(person);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

## 转换流

Java中的转换流（转换流，或称为字符编码流）是用于在字节流和字符流之间进行转换的流。

### 基本概念

- **转换流**的主要功能是将字节流（`InputStream`/`OutputStream`）与字符流（`Reader`/`Writer`）相互转换。
- 它们提供了一种在不同字符编码之间进行转换的机制，使得程序可以在不同平台和语言之间处理文本数据。

### 常用类

- **`InputStreamReader`**：将字节输入流（`InputStream`）转换为字符输入流（`Reader`）
    - `InputStreamReader(InputStream in)`: 使用平台默认字符编码，将字节输入流转换为字符输入流。
    - `InputStreamReader(InputStream in, String charsetName)`: 使用指定的字符编码，将字节输入流转换为字符输入流。
- **`OutputStreamWriter`** ：将字符输出流（`Writer`）转换为字节输出流（`OutputStream`）。
    - `OutputStreamWriter(OutputStream out)`: 使用平台默认字符编码，将字符输出流转换为字节输出流。
    - `OutputStreamWriter(OutputStream out, String charsetName)`: 使用指定的字符编码，将字符输出流转换为字节输出流。

### 使用场景

- **读取和写入文本文件**: 读取和写入需要指定特定字符编码的文本文件，如UTF-8或ISO-8859-1。
- **网络数据传输**: 在客户端和服务器之间传输文本数据时，使用转换流来确保正确的字符编码。
- **与数据库交互**: 处理需要指定字符编码的数据库文本数据（如从二进制BLOB字段读取或写入文本数据）。

### 示例代码

```java
import java.io.*;

public class ConvertStreamExample {
    public static void main(String[] args) {
        try {
            // 使用转换流读取文件，指定编码为UTF-8
            InputStreamReader reader = new InputStreamReader(new FileInputStream("input.txt"), "UTF-8");
            BufferedReader bufferedReader = new BufferedReader(reader);

            // 使用转换流写入文件，指定编码为UTF-8
            OutputStreamWriter writer = new OutputStreamWriter(new FileOutputStream("output.txt"), "UTF-8");
            BufferedWriter bufferedWriter = new BufferedWriter(writer);

            String line;
            while ((line = bufferedReader.readLine()) != null) {
                bufferedWriter.write(line);
                bufferedWriter.newLine();
            }

            // 关闭流
            bufferedReader.close();
            bufferedWriter.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

# 性能优化

- **使用缓冲流**: 缓冲流通过减少对底层系统的直接访问次数来提高性能。
- **合并I/O操作**: 减少对文件或网络的多次小型I/O操作，将其合并为批量操作。
- **使用NIO**: NIO的通道和缓冲区提供了非阻塞式I/O操作，更适合于需要高性能和高并发的应用。

# 工具类
---
## File
---



## Files

## Paths

## Path

## StandardOpenOption

## FileVisitor

## FileSystems

# AIO

# NIO



