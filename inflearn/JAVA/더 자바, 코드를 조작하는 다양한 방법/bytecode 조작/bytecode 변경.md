# 예제 코드

```java
public class Moja {
    public String pullOut() {
        return "";
    }
}

public class Masulsa {
    public static void main(String[] args) {
        //현재는 빈 문자열이 출력되지만 바이트코드를 직접 변경하면 다른 문자열 출력이 가능.
        System.out.println(new Moja().pullOut());
    }
}
```

# 1. bytebuddy 라이브러리

* 기존에 ASM이라는 라이브러리가 많이 쓰이고 있으나, 사용이 어려움.
    * api 문서가 PDF.
    * 디자인 패턴을 알아야 함.
* bytebuddy는 api 사용법이 쉬우며, 자바 바이트 코드나 클래스 파일에 대한 포맷을 몰라도 사용 가능.

[bytebuddy document](https://bytebuddy.net)

### gradle 설정

```java
dependencies {
    implementation 'net.bytebuddy:byte-buddy:1.12.10'
}
```

### 바이트코드 변경

```java
public class Masulsa {
    public static void main(String[] args) {
        try {
            new ByteBuddy().redefine(Moja.class)
                    //Moja 클래스의 pullOut이라는 메서드를 재정의. "Rabbit!"가 반환되도록 수정.
                    .method(named("pullOut")).intercept(FixedValue.value("Rabbit!"))
                    //재정의한 Moja 클래스 교체. 해당 클래스의 풀패키지 경로를 제외한 경로를 지정.
                    .make().saveIn(new File("/Users/jeff/study/inflearn-the-java-codes/bytecode-changing/out/production/classes/"));
        } catch (IOException e) {
            e.printStackTrace();
        }

        //마치 컴파일하는 것처럼 바이트 코드를 변경하는 작업이 선행 필요하기 떄문에 주석처리.
        //실행되는 시점에 이미 클래스로더에는 재정의하기 전의 클래스가 로드되어있기 때문.
        //System.out.println(new Moja().pullOut());
    }
}
```

### 바이트코드 변경 확인

<img src="./images/bytecode changing bytebuddy.png">

## 번외 : bytebuddy 라이브러리의 클래스 로딩 기능사용시 이슈

* FQCN은 풀패키지외에 클래스 로더도 포함이 되어 있음. (어떤 클래스로더가 로딩했는지)
* 애플리케이션 클래스 로더가 Moja를 읽고, bytebuddy가 만들어주는 클래스로더로 Moja를 읽으면 JVM상에는 2개의 Moja가 로드됨.
    * 풀 패키지 경로까지도 같지만, 서로 다른 클래스임. (호환 불가)