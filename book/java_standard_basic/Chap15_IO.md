입출력
========
- 컴퓨터 내부 또는 외부 장치와 프로그램간의 데이터를 주고 받는 것
# 1. 스트림
## 1.1. 개념
- 입출력을 수행하려면 두 대상을 연결하고 데이터를 전송하기 위해 필요
- 14장의 스트림과 다른 개념
- **데이터를 운반하는데 사용하는 연결통로**
- 입출력을 동시에 수행하려면 입력스트림과 출력스트림, 모두 2개 필요
- 먼저 보낸 데이터를 먼저 받는다.
## 1.2. 바이트 기반 스트림 - InputStrema, OutputStream
- File/ByteArray/Piped/Audio + InputStream/OutputStream
- 경우에 따라 선택
    - 어떠한 대상에 대해서 작업을 할 것인지
    - 입력인지 출력인지
- 모두 InputStream, OutputStream의 자손
- 입출력 메서드 625p 표
    - read()의 반환타입이 byte가 아니라 int인 이유는 read()의 반환값의 범위가 0~255와 -1이기 때문
## 1.3. 보조 스트림
- 실제 데이터를 주고받는 스트림이 아니기 때문에 입출력 처리 불가
- 스트림 기능을 향상시키거나 새로운 기능 추가
- 스트림을 먼저 생성한 후 이를 이용해 생성
```
// 먼저 기반 스트림을 생성한다.
FileInputStream fis = new FileInputStream("test.txt");

// 기반 스트림을 이용해서 보조 스트림을 생성한다.
BufferedInputStream bis = new BufferedInputStream(fis);
bis.read();
```
- 실제 입력 기능은 FileInputStream이 수행하고 보조스트림은 버퍼만을 제공
- **모든 보조 스트림**도 결국 InputStrema과 OutputStream의 자손들이므로 입출력 방법이 같다.
## 1.4. 문자기반 스트림 - Reader, Writer
- 바이트기반 : 입출력 단위가 1byte
- char형 : 2byte -> 바이트기반 스트림 사용하기 곤란 -> 문자기반 스트림 제공
- InputStream -> Reader / OutputStream -> Writer / ByteArray -> CharArray
## 1.5. 바이트 기반 스트림과 문자 기반 스트림의 비교
- 628p
## 1.6. InputStream과 OutputStream
### 1.6.1. InputStream 메서드
- int available() : 스트림으로부터 읽어 올 수 있는 데이터의 크기를 반환
- void close()
- void mark(int readlimit) : 현재위치 표시, 후에 reset()에 의해서 표시해 놓은 위치로 돌아갈 수 있다. readlimit은 돌아갈 수 있는 byte수
- boolean markSupported() : mark()와 reset() 지원 여부
- abstract int read() : 1byte 읽어 옴(0~255). 더 이상 읽어올 데이터가 없으면 -1 반환
- int read(byte[] b) : 배열의 bz크기 만큼 읽어 배열을 채우고 읽어 온 데이터의 수를 반환. 반환 값은 항상 배열의 크기보다 작거나 같다.
- int read(byte[] b, int off, int len) : 최대 len개의 byte를 읽어서, 배열의 b의 지정된 위치(off)부터 저장. 실제로 읽어올 수 있는 데이터가 len개보다 적을 수 있다.
- void reset() : 마지막 mark()위치로 되돌림
- long skip(long n)
### 1.6.2. OutputStream 메서드
- void close() : 입력소스를 닫음으로써 사용하고 있던 자원을 반환
- void flush() : 스트림 버퍼에 있는 모든 내용을 출력소스에 쓴다.
- abstract void write(int b) : 주어진 값을 출력소스에 쓴다.
- void write(byte[] b) : 주어진 배열 b에 저장된 모든 내용을 출력소스에 쓴다.
- void write(bytew[] b, inf off, int len) : 주어진 배열 b에 저장된 내용 중에서 off번째부터 len개 만큼만을 읽어서 출력소스에 쓴다.
+ flush()는 버퍼가 있는 출력스트림의 경우에만 의미가 있다. OutputStream에 정의된 flush()는 아무일도 하지 않는다.
+ 프로그램이 종료될 때, 사용하고 닫지 않은 스트림을 JVM이 자동저긍로 닫아주기는 하지만, 스트림을 사용해서 모든 작업을 마치고 난 후에는 close()를 호출해서 반드시 닫아주어야 한다.
    + ByteArrayInputStream과 같이 **메모리를 사용하는 스트림**과 Systeme.in, System.out과 같은 **표준 입출력 스트림**은 닫아 주지 않아도 된다.
### 1.6.3. 예제
### ByteArrayInputStream ByteArrayOutputStream 예제 1
- 메모리, 즉 바이트배열에 데이터를 입출력하는데 사용하는 스트림
- 주로 다른 곳에 입출력하기 전에 데이터를 임시로 바이트 배열에 담아서 변환 등의 작업을 하는데 사용
- 바이트 배열은 사용하는 자원이 **메모리** 밖에 없으므로 가비지컬렉터에 의해 **자동적으로 자원을 반환** -> close() 안써도 됨
- read()와 write(int b) 사용 시 한 번에 1byte만 사용하므로 효율 떨어짐
- toByteArray() : 스트림의 내용 바이트배열로 반환
- 배열 출력 Arrays.toString([]) 기억하자
### ByteArrayInputStream ByteArrayOutputStream 예제 2
- int read(byte[], int off, int len) : 객체 생성 시 넣어논 배열에서  매개변수로 넣은 배열에 읽는다.
- void write(byte[] b, int off, int len) : 매개변수로 주어진 배열에 값을 쓴다.
- 바구니를 사용해 한번에 더 많은 물건 옮기기
### ByteArrayInputStream ByteArrayOutputStream 예제 3
- read()나 write()이 IOException 발생 가능 -> try-catch
- 성능을 위해 내용을 덮어쓰는 방식이라 운반되는 배열에 길이보다 읽는 값이 적으면 이전 값들이 남아있음(632예제 다시보기)
- 633p 읽고 바로 쓰는 것이 **대신**, 읽은 값ㅇ르 변수에 저장하고, 쓸때 범위를 지정하면 이 문제 해결 가능
## 1.7. FileInputStream과 FileOutputStream
- 파일 입출력을 위한 스트림 
- 실제 많이 씀
### 1.7.1. FileInputStream 생성자
- FileInputStream(String name) : 지정된 파일이름을 가진 실제 파일과 연결된 FileInputStream을 생성
- FileInputStream(File file) : 이름 대신 File 인스턴스로 지정해야 함. 그 외엔 바로 위와 동일
- FileInputSTream(FileDescriptor fdObj) : 파일 디스크럽터로 생성
### 1.7.2.FileOutputStream 생성자
- FileOutputStream(String name) : 지정된 파일이름을 가진 실제 파일과 연결된 스트림 생성
- FileOutputStream(String name, boolean append) : append를 true하면 출력 시 기존의 파일 내용의 마지막에 덧붙임, false면 내용을 덮어쓰게 된다.
- FileOutputStream(File file)
- FileOutputStream(File file, boolean append)
- FileOutputStream(FileDescriptor fdObj)
### FileInputStream과 FileOutputStream 예제 1
- read()의 반환값 int이긴 하지만, -1을 제외하고, 0~255(1byte)범위의 정수값이기 때문에 char형(2byte)로 변환해도 손실 값 없음
### FileInputStream과 FileOutputStream 예제 2
- 텍스트파일을 다루는 경우에는 문자 기반의 스트림인 FileReader/FileWriter를 사용하는 것이 좋다.
## 1.8. FilterInputStream과 FilterOutputStream
### 1.8.1. 설명
- Input/OutputStream의 자손이면서 모든 보조 스트림의 조상
- 보조 스트림은 자체적 입출력 수행 불가  

        protected FilterInputStream(InputStream in)
        public FilterOutputStream(OutputStream out)

```
public class FilterINputStream etends InputStream{
    protected volatile InpuStream in;
    
    protected FilterINputStream(InputStream in){
        this.in = in;
    }

    public int read() throws IOException{
        return in.read();
    }
    ...
}
```
- 접근제어자가 protected이기 때문에 인스턴스를 생성해서 사용할 수 없고, 상속을 통해서 오버라이딩되어야 한다.
- 기반스트림에 보조기능을 추가한 자손들
    - BufferedInputStream, DataInputStream, PushbackInputStream 등
    - BufferedOutputStream, DataOutputStream, PushbacOutputStream 등
### 1.8.2. BufferedInputStream
### 설명
- 스트림의 입출력 효율을 높이기 위해 버퍼를 사용하는 보조스트림
- 버퍼를 이용해 한 번에 여러 바이트를 입출력하는 것이 바르기 떄문에 대부분 입출력 작업에 사용
### 생성자
- BufferedInputStream(InputStream in, int size) : 주어진 InputStream인스턴스를 입력소스로 하며 지정된 크기(byte단위)의 버퍼를 갖는 인스턴스 생성 
- BufferedInputStream(InputStream in) : 크기를 지정해주지 않으므로 기본적으로 8192byte 크기의 버퍼를 갖는다.
+ 파일의 경우 size값을 8192로 하는게 보통이며, **버퍼의 크기를 변경해가면서 테스트하면 최적 버퍼크기를 알아낼 수 있다.**
+ 과정
    + 입력소스로부터 데이터 읽기 위해 처음 read() 호출 -> BufferedInputStream이 입력소스로부터 버퍼 크기만큼 데이터 읽어다 자신의 내부 버퍼에 저장 -> 프로그램에서 버퍼에 저장된 데이터 읽음
    + 버퍼에 저장된 데이터 다 읽고 다음 데이터를 읽기 위해 read() 호출 -> 다시 버퍼크기만큼 읽어옴 -> 반복
    + 외부 데이터 읽기보다 내부의 버퍼로 부터 읽는게 훨씬 빠름, 효율 높아짐
### 1.8.3. BufferedOutputStream
### 설명
- 과정
    - 읽을 때와는 반대
    - write을 이용한 출력이 BufferedOutputStream의 버퍼에 저장 -> 버퍼가 가득 차면 모든 내용을 출력소스에 출력 -> 버퍼를 비우고 다시 프로그램의 출력을 저장할 준비
- 모든 출력작업을 마친 후 BufferedOutputStream에 close()나 flush()를 호출해서 마지막에 버퍼에 있는 모든 내용이 출력소스에 출력되도록 해야 한다.
### 예제
- 예제에서 보조스트림을 닫지 않고 기반스트림만 닫았더니 버퍼에 남아있는 데이터가 출력되지 못하고 프로그램이 종료되었다.
- FilterOutputStream은 close() 사용 시 flush()를 자동 호출하도록 되어있다.
    - 자손인 BufferedOutputStream은 close()를 오버라이딩 없이 상속받음
    - 기반 스트림 호출없이 보조스트림만 닫아주면 된다.
### 1.9. SequenceInputStream
### 설명
- 여러 개의 입력스트림을 연속적으로 연결해서 하나의 스트림으로부터 데이터를 읽는 것과 같이 처리할 수 있도록 도와준다.
### 메서드(642p 예시있음)
- SequenceInputStream(Enumeration e) : Enumeration에 저장된 순서대로 입력스트림을 하나의 스트림으로 연결한다.
- SequenceInputStream(InputStream s1, INputStream s2) : 두 개의 입력스트림 s1, s2를 하나로 연결한다.
### 643p 예제
- 벡터에 3개(ByteArrayInputStream 인스턴스) 넣어서 연결시켰더니, Vector에 저장된 순서대로 입력됨
## 1.10. PrintStream
### 1.10.1. 역할
- 문자기반 스트림의 역할
- print, println, printf 같은 메서드 오버로딩하여 제공
- 향상된 PrintWriter가 나왔지만, 빈번하게 사용되는 System.out이 PrintStream이다 보니 둘 다 사용
    - 가능하면 PrintWriter사용하는게 좋음, 다양한 문자 처리하는데 적합
### 1.10.2. 메서드 644p
## 1.11. 문자 기반 스트림 - Reader
### 1.11.1. 설명
- 바이트기반 스트림의 조상이 InputStream/OutputStream라면 문자기반 스트림에서는 Reader/Writer가 그 역할을 한다.
### 1.11.2. 메서드
- byte배열 대신 char배열을 사용한다는 것 외에는 InputStream/OutputStream의 메서드와 별반 다르지 않음
- boolean ready() : 입력소스로부터 데이터를 읽을 준비가 되어있는지 알려 준다.
## 1.12. 문자 기반 스트림 - Writer
- 문자기반 스트림은 단순히 2byte로 스트림을 처리하는 것만을 의미하지는 않는다. -> 인코딩
- Reader/Writer 그리고 그 자손들은 여러 종류의 인코딩에서 바자에서 사용하는 유니코드(UTF-16)간의 변환을 자동적으로 처리해준다. 
    - Reader는 특정 인코딩을 읽어서 유니코드로 변환
    - Writer는 유니코드를 특정 인코딩으로 변환하여 저장.
    - FileInputStream으로 txt를 그냥 읽어올 시 한글이 꺠져서 출력됨
### 예제 1 FileInputStream 한글 깨짐
### 예제 2 FileReader/Writer 공백제거해보기

## 1.13. String Reader와 StringWriter
- CharArrayReader/CharArrayWriter와 같이 입출력 대상이 메모리인 스트림이다.
- StringWriter에 출력되는 데이터는 내부의 StringBuffer에 저장되며 다음과 같은 메서드로 저장된 데이터를 얻을 수 있다.  
        
        StringBuffer getBuffer() StringWriter에 출력한 데이터가 저장된 StringBuffer를 반환
        String toString() StringWriter에 출력된 (StringBuffer에 저장된) 문자열을 반환
## 1.14. BufferedReader와 BufferedWriter
- BufferedReader
    - readLine() : 라인단위 읽음 
- BufferedWriter
    - newLine() : 줄바꿈해줌
## 1.15. InputtreamReader, OutputStreamWriter
### 1.15.1. 설명
- 바이트기반 스트림을 문자기반 스트림으로 연결시켜주는 역할
- 바이트기반 스트림의 데이터를 지정된 인코딩의 문자데이터로 변환하는 작업 수행
### 1.15.2. 메서드

- InputStreamReader(InputStream in) : OS에서 사용하는 기본 인코딩의 문자로 변환하는 - InputStreamReader를 생성한다.
- InputStreamReader(InputStream in, String encoding) : 지정된 인코딩을 사용하는 InputStreamReader생성
- String getEncoding() : InputStreamReader의 인코딩을 알려 준다.
+ OutputStreamWriter도 out으로 다 바꾸면 똑같음
***

# 2. 표준 입출력(Standard I/O)
## 2.1. 설명
- 표준 입출력은 콘솔(console, 도스창)을 통한 데이터 입력과 콘솔로의 데이터 출력을 의미
- 자바에서는 System.in, System.out, System.err을 제공
    - 자바 어플리케이션 실행과 동시에 자동 생성
    - 별도로 스트림을 생성하는 코드를 작성할 필요 없음 
```
public final class System{
    public final static InputStream in = nullInputStream();
    public final static PrintStream out = nullInputStream();
    public final static PrintStream err = nullInputStream();
}
```
- 선언부만 봐서는 InputStream과 PrintStream이지만 실제로는 버퍼를 이용하는 BufferedInputStream과 BufferedOutputStream의 인스턴스를 사용한다.
## 2.2. 표준 입출력의 대상 변경
    static void setOut(PrintStream out) 
    static void setErr(PrintStream err)
    static void setIn(InputStream in)
- 출력을 지정된 PrintStream으로 변경
- 입력을 지정된 InputStream으로 변경
## 2.3. 표준 입출력 대상변경 예제
- setOut을 통해 txt파일에 연결시켰더니, sout하면 txt에 입력, err 사용 시 콘솔에 출력됨
***

# 3. File
## 3.1. 생성자, 메서드
- File(String fileName) :주어진 문자열을 이름으로 갖는 파일을 위한 File 인스턴스를 생성한다. 파일 뿐만 아니라 **디렉토리도** 같은 방법으로 다룬다. 여기서 fileName은 주로 경로(path)를 포함해서 지정해주지만, 파일 이름만 사용해도 되는 데 이 경우 **프로그램이 실행되는 위치**가 경로로 간주된다.
- File(String pathName, String fileName)/(File pathName, String fileName)
- File(URI uri) : 지정된 uri로 파일 생성
- String getName()/getPath()
- String getAbsolutePath()/getParent()/getCanonicalPath() : 파일의 절대경로/조상 디렉토리/정규경로를 String으로 반환
- File getAbsoluteFile()/getParentFile()/getCanonicalFile() : 파일의 절대경로/조상 디렉토리/정규경로를 File로 반환
>파일경로 메서드들 정리
[codechacha]https://codechacha.com/ko/java-getpath-getabsolutepath-getcanonicalpath/

- getPath는 입력한대로, getAbsolutePath는 절대, getCanonicalPath는 다듬은 경로
## 3.2. 멤버변수
|멤버변수|설명|
|--|--|
|static String pathSeparator|OS에서 사용하는 경로 구분자. 윈도우";", 유닉스":"|
|static char pathSeparatorChar|OS에서 사용하는 경로 구분자. 윈도우";", 유닉스":"|
|static String separator|OS에서 사용하는 이름 구분자. 윈도우"₩", 유닉스"/"|
|static char separatorChar|OS에서 사용하는 이름 구분자. 윈도우"₩", 유닉스"/"|
|||
## 3.3.1. 예제 1
- 정규경로 : 기호나 링크 등을 포함하지 않는 유일한 경로, 단 하나뿐
- 시스템 속성 (System.getProperty("");)
    - user.dir : 실행중인 디렉토리
    - sun.boot.class.path : 기본적인 classpath
- File 인스턴스를 생성했다고 해서 파일이나 디렉토리가 생성되는 것은 아니다.
    - 지정된 문자열이 유효하지 않더라도 컴파일이나 에러를 발생시키지 않음
- 새로운 파일 생성 : File인스턴스 생성 후 출력스트림 생성하거나 createNewFile()사용
## 3.3.2. 예제 2
- isDirectory, exists
## 3.3.3. 예제 3
- 재귀 호출로 하위 파일만 다 지우기
- File 객체로 디렉토리 다루기  
```
String currDir = System.getProperty("user.dir");
File dir = new File(currDir);
File[] files = dir.listFiles();
```
- 해당 디렉토리에 listFiles() 사용해서 파일 및 경로 목록 뽑아내기
## 3.3.4. 예제4
- renameTo(File f)를 이용해서 파일의 이름을 바꾸는 내용
- 파일 순서 바로잡기
- 왜 length-7인데 001이 아닌 00001일까

## 3.4. 직렬화(serialization)
### 3.4.1. 설명
- 객체를 데이터 스트림으로 만드는 것
- 저장된 데이터를 스트림에 쓰기(write)위해 연속적인 데이터로 변환하는 것
- 데이터를 읽어서 객체를 만드는 것을 역직렬화(deserialization)라고 함
### 3.4.2. ObjectInputStream, ObjectOutputStream
- 보조스트림
- InputStream, OutputStream 직접 상속받음
- 직렬화에는 ObjectOutputStream
- 역직렬화에는 ObjectInputStream
```
ObjectInputStream(InputStream in)
ObjectOutputStream(OutputStream out)
```
- 파일에 객체를 저장(직렬화)하는 경우
```
FileOutputStream fos = new FileOutputStream("objectfile.ser");
ObjectOutputStream out = new ObjectOutputStream(fos);

out.writeObject(new UserInfo());
```
-> objectfile.ser이라는 파일에 UserInfo객체를 직렬화하여 저장한다.  

- 역직렬화
```
FileInputStream fis = new FileInputStream("objectfile.ser");
ObjectInputStream in = new ObjectInputStream(fis);

UserInfo info = (UserInfo)in.readObject();
```
### ObjectInputStream, ObjectOutputStream 메서드
- 664p 메서드
- 직렬화와 역직렬화를 직접 구현할 때 주로 사용
- defaultReadObject(), defaultWriteObject()는 자동 직렬화를 수행
- readObject()와 writeObject()를 사용한 자동 직렬화가 편리하기는 하지만 직렬화 작업시간을 단축시키려면 직렬화하고자 하는 클래스에 추가적으로 다음과 같은 2개의 메서드를 직접 구현
```
private void writeObject(ObjectOutputStream out)
    throws IOException{
        //write메서드를 사용해서 직렬화를 수행한다.
    }
private void readObject(ObjectInputStream in)
    throws IOException, ClassNotFoundException{
        //read메서드를 사용해서 직렬화를 수행한다.
    }
```
### 3.4.3. 직렬화 가능한 클래스 만들기
- Serializable인터페이스를 구현하도록 하면 됨
- Serializable은 아무 내용도 없는 빈 인터페이스, **직렬화를 고려하여 작성한 클래스인지 판단**하는 기준이 됨  

`public inferface Serializable{}`  

- Serializable을 구현하지 않았어도, 조상 클래스가 Sereializable을 구현했다면 직렬화 가능
- 조상클래스만 Serializable을 구현하지 않은 경우, 자손클래스를 직렬화할때 조상클래스에 정의된 인스턴스 변수는 직렬화 대상에서 제외
- 머리부터 적용되는 이미지로 외움 headset()?
### 3.4.4. 직렬화 대상에서 제외시키기 - transient
```
public class ...{
    ...
    //직렬화 할 수 없는놈 참조
    Object obj = new Object();      //Object는 직렬화할 수 없다.
    //실제 저장 객체가 직렬화 가능
    Object obj = new String("abc"); //String은 직렬화 가능
}
```
- Serializable이 구현되어 있어도, **직렬화 할 수 없는 클래스의 객체**를 인스턴스변수가 **참조**하고 있다면 직렬화에 실패
    - java.io.NotSerializableException 발생
- 인스턴스변수가 직렬화가 안돼도, **실제 저장 객체**가 직렬화가 가능하다면 직렬화 가능
- **transient** 붙이면 직렬화 대상에서 제외
    - 위처럼 직렬화 안되는 객체를 참조 시 제외
    - 보안상 직렬화되면 안되는 값에 붙임
    - transient가 붙은 인스턴스변수의 값은 그 타입의 기본값으로 직렬화 됨 -> 역직렬화 하면 Object형, String형 값이 null로 반환
### 직렬화와 역직렬화 예제 1
- 예제에 앞서 클래스 컴파일
### 직렬화와 역직렬화 예제 2
- writeObject()사용
- 객체를 직렬화하는 코드는 간단하지만, 객체에 정의된 모든 인스턴스변수에 대한 참조를 찾아들어아기 때문에 상당히 복잡하고 시간이 걸리는 작업이 될 수 있다.
    - ArrayList같은 객체 직렬화 시 저장된 모든 객체들과 각 객체의 인스턴스 변수가 참조하고 있는 객체들까지 모두 직렬화
### 직렬화와 역직렬화 예제 3
- readObject()로 읽어옴
    - 읽어올 때(역직렬화) 형변환 `UserInfo u1 = (UserInfo)in.readObject();` 
    - 역직렬화 시 직렬화할 때의 순서와 일치해야 함 -> 개별적 직렬화보다 ArrayList와 같은 컬렉션에 저장해서 직렬화하는게 좋다.(순서 고려 안해도 됨)

### 연습문제
- split(String regex)