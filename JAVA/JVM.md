# JVM 구조

<img src="./images/jvm.png" width="90%">

## 클래스 로더

* .class 파일의 바이트 코드를 읽어들여서 메모리에 적절하게 배치하는 역할을 담당

### Loading : 클래스를 읽어오는 과정

* 클래스 파일을 읽고 그 내용에 따라 바이너리 데이터를 만들어 메소드 영역에 저장
* 메소드 영역에 저장하는 데이터
    * 풀 패키지 이름을 포함한 클래스 이름 (FQCN)
    * 클래스, 인터페이스, 이늄 여부
    * 메소드와 변수
* 로딩이 끝나면 해당 클래스 타입의 Class 객체가 Heap 영역에 저장됨.
    * MyClass라는 클래스를 정의하면 소스상에서 MyClass.class로 class 객체에 접근할 수 있음.
* 각 단계별 로더
    * Bootstrap Class Loader : jre의 lib폴더에 있는 rt.jar 파일을 찾아 자바 기본 클래스들을 로드.
    * Extension Class Loader : jre의 lib폴더에 있는 ext 폴더에 위치한 확장 코어 클래스 파일들을 로드.
    * Application Class Loader : 사용자가 직접 정의한 클래스 파일들을 로드. Classpath 환경변수에 있는 클래스 파일이나 -classpath 또는 -cp 명령어 옵션이 있는 파일들을 로드.



