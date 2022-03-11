# 1 Introduction
이 문서는 자바 프로그래밍 언어에서 소스 코드에 대해 구글의 코딩 표준을 완전히 정의하는 역할을 한다.  
다른 프로그래밍 스타일 가이드와 마찬가지로, 해당 이슈들은 형식화의 미학적인 이슈 뿐만 아니라, 다른 타입의 코딩 표준이나 컨벤션 또한 아우른다. 그러나, 이 문서는 주로 우리가 대개 따르는 **hard-and-fast** rules에 집중해 있다.

## 1.1 Terminology notes
1. class라는 용어는 "일반적인" 클래스,enum 클래스, interface 또는 annotation type(@interface)와 같은 를 뜻한다. 
2. (클래스의)member라는 용어는 중첩(집합?) 클래스, 필드, 메서드 또는 생성자를 통틀어서 말한다. 즉, initializers와 comments를 제외한 모든 top-level의 내용이다.
3. comment란 용어는 항상 구현 comments를 말한다. 'Javadoc'이라는 용어 대신 "documentation comments"라는 용어를 쓰지 않는다.

다른 용어 노트들은 간간히 문서를 통해 보여줄 것이다.

## 1.2. Guide notes
예시 코드는 **non-normative** 하다. 즉 구글 스타일이지 코드의 표현에 우아한 방법만 사용하지는 않았다. 부가적인 포매팅 선택지들이 규칙을 강제하지 않는 예시들을 보여줄 것이다.

# 2 Sources file basics
## 2.1. File name
소스파일 이름은 .java 확장자를 가지는 top-level class의 이름을 포함한다.
## 2.2. File encoding: UTF-8
UTF-8로 인코딩된다.
## 2.3. Special characters
### 2.3.1. Whitespace characters (공백문자)
특히 행을 끝내는 나열에서, the ASCII horizontal space character (0x20) 는 소스파일 어디에서든 나타날 수 있는 유일한 공백문자이다. 의미:
1. 문자열과 문자 리터럴들에 있는 다른 모든 공백문자들은 escaped 된다.
2. Tab 문자는 들여쓰기에 사용되지 않는다.
### 2.3.2. Special ecape sequences
8진법(e.g. `/012`)아나 유니코드(e.g. `/u000a`)에 일치시키기 보다 해당 sequence를 사용한다.

###
추가로 계속 수정 예정
[숑구님 블로그 번역](https://shongnote.tistory.com/8)