## 개요
개발 프로젝트의 필수 요소, 테스팅에 대해 설명한 글

## 키워드
Junit5, TDD, 학습 테스트, FIRST원칙

## 중요도
별 3개

## 서론
"테스트 해야지"라는 말은 개발 프로젝트를 진행하면 자주 듣게 되는 말입니다. 개발 프로젝트에서 테스트가 왜 중요한 지와 실제로 어떻게 테스트를 할 수 있을 지에 관해 설명하고자 합니다. 학습 키워드가 4개가 있지만 이번 글에서는 TDD와 Junit5에 관해 다룰 예정이고 나머지 키워드는 아래 '나머지 키워드에 대한 글' 을 참고하면 됩니다.

### TDD
TDD는 Test Driven Development의 약자입니다. 테스트를 기반으로 하는 개발이라 해석할 수 있다. TDD가 적용된 프로젝트의 주인공은 테스트입니다. 연극이라 생각하면 API라는 배우가 등장하기 전에는 항상 테스트라는 배우가 등장해야 하는 연극입니다. 테스트보다 API가 먼저 등장했다면 그 연극은 TDD라 할 수 없고 단위 테스트를 작성했다고 말할 수 있습니다.

그럼 TDD는 어떤 원리가 있고 장점은 무엇인지 간략하게 살펴보겠습니다. TDD의 핵심은 내가 만들고 싶은 프로그램의 결과를 알고 있어야 한다는 점입니다. 예를 들어, 더하기 프로그램을 만든다 생각해봅시다. TDD를 적용하지 않으면 더하기 프로그램을 바로 만들기 시작합니다. 반면에 TDD를 적용하면 더하기 프로그램의 결과를 예측하는 테스트를 ``먼저`` 작성한다. 여기서 중요한 말은 실제 프로그램보다 `먼저` 테스트를 작성한다는 말입니다.

#### TDD의 과정
더하기 프로그램 프로젝트에 TDD를 적용해보면 아래와 같은 순서로 진행됩니다.
- 테스트 작성 : 3 + 5 = 8
  - 더하기 프로그램에 3과 5를 더하면 8이라는 결과 나오는 테스트를 작성한다.  
- 테스트를 검증 한다 : 3 + 5 = 8
  - 테스트 결과는 당연히 실패다. 왜냐하면 테스트만 존재할 뿐 실제 더하기 프로그램은 없기 때문이다.
- 실패한 테스트를 통과하기 위해서 실제 더하기 프로그램 코드를 작성한다.
-  코드를 작성한 후 다시 테스트를 검증한다. 3 + 5 = 8
  - 이번에도 실패했다면 내가 작성한 코드가 잘못됐다는 증거이다.
- 테스트가 통과될 수 있게 다시 실제 더하기 프로그램의 코드를 수정한다.
- 테스트를 검증한다. : 3 + 5 = 8
- 테스트를 통과했다!
  - 내가 만든 더하기 프로그램이 정상적으로 동작한다는 의미이다.

장황하게 TDD 과정을 설명했지만 핵심은 테스트를 `먼저` 작성한 뒤에 실제 코드를 작성합니다. 테스트가 통과됐다면 내가 작성한 코드도 잘 동작하는 코드라는 증거가 생기기 때문입니다. 주의할 점은 내가 테스트의 결괏값을 정확히 알고 있어야 한다는 점입니다. 위 예시에서 내가 3 더하기 5를 7로 알고 있었다면 테스트가 통과되더라도 내가 만든 더하기 프로그램이 정상적인 프로그램이라고 확신할 수 없습니다.

### Junit5
#### Junit5에 대해서
[Junit5](https://junit.org/junit5/)는 자바에서 테스팅을 할 때 사용할 수 있는 도구입니다. 내가 사용하는 언어가 자바가 아니라면 테스트 도구는 달라질 수 있습니다.

> Junit5 = Junit Platform + Junit Jupiter + Junit Vintage

- Junit Platform은 JVM에서 실행되는 테스트 프레임워크를 제공해줍니다. TestEngine이라는 API도 제공합니다. 이는 Platform 위에서 동작하는 테스트 프레임워크를 개발하는데 사용됩니다.
- Junit Jupiter는 Junit5에서 테스트를 작성할 때 필요한 프로그래밍 모델과 확장 프로그램 모델의 조합입니다. 테스트할 때 필요한 문법을 의미한다. @Test 어노테이션 등.
- Junit Vintage는 Junit5 이전 버전인 Junit3~4도 동작할 후 있는 기능을 의미한다.

#### Junit 사용법
> 환경은 Gradle 기반 프로젝트입니다. 메이븐 설정은 공식문서를 참고해주세요!

- build.gradle에 아래 설정을 추가합니다.
```
test {
    useJUnitPlatform()
}
```

- 스프링부트 프로젝트가 아니라 기본 스프링 프로젝트라면 아래 의존성을 추가합니다.
```
dependencies {
    testImplementation("org.junit.jupiter:junit-jupiter-api:5.6.2")
    testRuntimeOnly("org.junit.jupiter:junit-jupiter-engine:5.6.2")
}
```

- 스프링부트 프로젝트라면 자동으로 아래와 같은 의존성이 추가됩니다. exclude group에 `org.junit.vintage`가 있는데 스프링 부트에서는 junit 3 ~ 4보단 5 사용을 추천하기 때문입니다.
```
testImplementation('org.springframework.boot:spring-boot-starter-test') {
        exclude group: 'org.junit.vintage', module: 'junit-vintage-engine'
    }
```

- 테스트 클래스의 패키지 위치는 아래 사진처럼 src-test-java 패키지 밑에 생성합니다. 위에 실제 main패키지와 동일한 구조로 만듭니다.

![테스트클래스_패키지위치](/jack/assets/testing/테스트클래스_패키지위치.png)

- 테스트 코드 작성 (아래 예시는 기본적인 테스트 코드이다)

![기본적인_테스트코드](/jack/assets/testing/기본적인_테스트코드.png)

위 사진처럼 @Test을 사용하면 테스트를 실행할 수 있다. @DisplayName으로 어떤 테스트인지 설명을 적을 수 있습니다.

## 결론
이번 글에선 간략하게 TDD가 무엇인지, Junit5에 대해서 살펴봤습니다. 얕게 개념을 살펴봤기 때문에 테스팅에 대해서 관심이 있다면 책이나 강의를 통해 깊게 공부하시면 될 것 같습니다.


## 나머지 키워드에 관한 글
- [학습테스트](https://jaeyeolshin.github.io/2016-03-13/training-test/)
- [F.I.R.S.T 원칙](https://brocess.tistory.com/212)