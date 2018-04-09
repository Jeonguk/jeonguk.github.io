---
layout: post
title:  "how-to-read-a-file-in-java"
date:   2018-04-09 13:42:10 +0700
categories: [java,language]
---

# How to Read a File in Java

### 표준 Java 클래스를 사용하여 클래스 패스, URL 또는 JAR 파일에서 파일을 읽는 방법을 살펴 본다

```Setup```

- using Hamcrest


```java
private String readFromInputStream(InputStream inputStream)
  throws IOException {
    StringBuilder resultStringBuilder = new StringBuilder();
    try (BufferedReader br
      = new BufferedReader(new InputStreamReader(inputStream))) {
        String line;
        while ((line = br.readLine()) != null) {
            resultStringBuilder.append(line).append("\n");
        }
    }
  return resultStringBuilder.toString();
}
```

```Read File from Classpath```
- This section explains how to read a file that is available on a classpath. We will read the “fileTest.txt” available under src/main/resources:

```java
@Test
public void givenFileNameAsAbsolutePath_whenUsingClasspath_thenFileData() {
    String expectedData = "Hello World from fileTest.txt!!!";
     
    Class clazz = FileOperationsTest.class;
    InputStream inputStream = clazz.getResourceAsStream("/fileTest.txt");
    String data = readFromInputStream(inputStream);
 
    Assert.assertThat(data, containsString(expectedData));
}
```

- 위 코드에서 현재 클래스를 사용하여 getResourceAsStream 메소드를 사용하여 파일을로드하고로드 할 파일의 절대 경로를 전달했습니다.

ClassLoader 인스턴스에서도 같은 방법을 사용할 수 있습니다.

```java
ClassLoader classLoader = getClass().getClassLoader();
InputStream inputStream = classLoader.getResourceAsStream("fileTest.txt");
String data = readFromInputStream(inputStream);
```

- getClass (). getClassLoader ()를 사용하여 현재 클래스의 classLoader를 가져옵니다.

가장 큰 차이점은 ClassLoader 인스턴스에서 getResourceAsStream을 사용할 때 경로는 클래스 경로의 루트에서 시작하여 절대적으로 처리된다는 것입니다.

클래스 인스턴스에 대해 사용되는 경우 경로는 패키지와의 상대 경로이거나 슬래시로 암시되는 절대 경로 일 수 있습니다.

```Read File with JDK7```
- JDK7에서는 NIO 패키지가 크게 업데이트되었습니다. Files 클래스와 readAllBytes 메서드를 사용하는 예제를 살펴 보겠습니다. readAllBytes 메서드는 Path를 받아들입니다. Path 클래스는 java.io.File의 업그레이드로 생각할 수 있습니다.

```java
@Test
public void givenFilePath_whenUsingFilesReadAllBytes_thenFileData() {
   String expectedData = "Hello World from fileTest.txt!!!";
        
   Path path = Paths.get(getClass().getClassLoader()
     .getResource("fileTest.txt").toURI());       
   byte[] fileBytes = Files.readAllBytes(path);
   String data = new String(fileBytes);
 
   Assert.assertEquals(expectedData, data.trim());
}
```

```Read File with JDK8```
- JDK8은 Files 클래스 내에 lines () 메소드를 제공합니다. 문자열 요소의 스트림을 반환합니다.

바이트로 데이터를 읽고 UTF-8 charset을 사용하여 디코딩하는 방법의 예를 살펴 보겠습니다.

```java
@Test
public void givenFilePath_whenUsingFilesLines_thenFileData() {
    String expectedData = "Hello World from fileTest.txt!!!";
          
    Path path = Paths.get(getClass().getClassLoader()
      .getResource("fileTest.txt").toURI());
          
    StringBuilder data = new StringBuilder();
    Stream<String> lines = Files.lines(path);
    lines.forEach(line -> data.append(line).append("\n"));
    lines.close();
          
    Assert.assertEquals(expectedData, data.toString().trim());
}
```

```Read File with FileUtils```
- 또 다른 일반적인 옵션은 Commons 패키지의 FileUtils 클래스를 사용하는 것입니다. 다음 종속성을 추가해야합니다.

```xml
<dependency>
    <groupId>commons-io</groupId>
    <artifactId>commons-io</artifactId>
    <version>2.5</version>
</dependency>
```

```java
@Test
public void givenFileName_whenUsingFileUtils_thenFileData() {
    String expectedData = "Hello World from fileTest.txt!!!";
         
    ClassLoader classLoader = getClass().getClassLoader();
    File file = new File(classLoader.getResource("fileTest.txt").getFile());
    String data = FileUtils.readFileToString(file);
         
    Assert.assertEquals(expectedData, data.trim());
}
```

- 여기에서는 File 객체를 FileUtils 클래스의 readFileToString () 메서드에 전달합니다. 이 유틸리티 클래스는 InputStream 인스턴스를 만들고 데이터를 읽는 데 필요한 상용구 코드를 작성할 필요없이 내용을로드하도록 관리합니다.

```Read Content from URL```
- URL에서 내용을 읽으려면이 예에서 "/"URL을 다음과 같이 사용합니다.

```java
@Test
public void givenURLName_whenUsingURL_thenFileData() {
    String expectedData = "Baeldung";
 
    URL urlObject = new URL("/");
    URLConnection urlConnection = urlObject.openConnection();
    InputStream inputStream = urlConnection.getInputStream();
    String data = readFromInputStream(inputStream);
 
    Assert.assertThat(data, containsString(expectedData));
}
```
- URL에 연결하는 다른 방법도 있습니다. 여기에서는 표준 SDK에서 사용할 수있는 URL 및 URLConnection 클래스를 사용했습니다.

```Read File from JAR```
- JAR 파일 안에있는 파일을 읽으려면 파일 안에 JAR 파일이 있어야합니다. 예를 들어 "hamcrest-library-1.3.jar"파일에서 "LICENSE.txt"를 읽습니다 :

```java
@Test
public void givenFileName_whenUsingJarFile_thenFileData() {
    String expectedData = "BSD License";
 
    Class clazz = Matchers.class;
    InputStream inputStream = clazz.getResourceAsStream("/LICENSE.txt");
    String data = readFromInputStream(inputStream);
 
    Assert.assertThat(data, containsString(expectedData));
}
```
- 여기서 우리는 Hamcrest 라이브러리에있는 LICENSE.txt를로드하려고합니다. 따라서 자원을 얻는 데 도움이되는 Matcher 클래스를 사용합니다. 동일한 파일을 클래스 로더를 사용하여로드 할 수도 있습니다.

