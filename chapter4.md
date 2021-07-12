# 제 4장. 주석

- 프로그래머들이 주석을 유지하고 보수하기란 현실적으로 불가능하기에 주석은 오래될수록 코드에서 멀어진다.
- 부정확한 주석은 아예 없는 주석보다 훨씬 더 나쁘다.
- 주석을 가능한 줄이고 코드를 통해서 정확한 정보를 제공해야한다.
  
<br/>

## 주석은 나쁜 코드를 보완하지 못한다.
- 코드에 주석을 추가하는 이유는 코드의 품질이 나쁘기 때문이다.
- 난장판인 코드를 주석으로 설명하려고 노력할 시간에 난장판인 코드를 고쳐야된다.
  
<br/>

## 코드로 의도를 표현하라.
- 코드로만 의도를 설명하기 어려운 경우가 존재하지만 거의 없다.
- 몇 초만 더 생각하면 코드로 대다수 의도를 표현할 수 있다.
```java
// 직원에게 복지 혜택을 받을 자격이 있는지 검사한다.

//나쁜 코드
if((employee.flags & HOURLY_FLAG) && (employee.age > 65))

// 좋은 코드
if (employee.isEligibleForFullBenefits())
```
<br/>

## 좋은 주석의 예

1) 법적인 주석
    - 각 소스 파일 첫머리에 주석으로 들어가는 저작권 정보와 소유권 정보
    - 표준 라이선스나 외부 문서 참조
    
```java
// Copyright (c) 2003,2004,2005 by Object Mentor, Inc. All rights reserved.
// GNU General Public License 버전 2 이상을 따르는 조건을 배포한다.
```
<br/>

2) 정보를 제공하는 주석
    - 기본적인 정보를 주석으로 제공
    - 주석이 유용하다 할지라도 가능하다면 함수 이름에 정보를 담는편이 좋다
    
```java
// 테스트 중인 Responder 인스턴스를 반환한다.

// 나쁜 코드
protected abstract Responder responderInstance();

// 좋은 코드
protected abstract Responder responderBeingTested();
```
<br/>

3) 의도를 설명하는 주석
    - 주석은 구현을 이해하게 도와주는 선을 넘어 결정에 깔린 의도까지 설명한다
    
```java
public int compareTo(Object o)
{

  if(o instanceof WikiPagePath)
  {
    WikiPagePath p=(WikiPagePath)o;
    String compressedName=StringUtil.join(names,"");
    String compressedArgumentName=StringUtil.join(p.names,"");
    return compressedName.compareTo(compressedArgumentName);
  }
  return 1; // 오른쪽 유형이므로 정렬 순위가 더 높다.
}
```
<br/>

4) 의미를 명료하게 밝히는 주석
    - 표준 라이브러리나 변경하지 못하는 코드에 속한 인수나 반환 값의 의미를 명료하게 밝히는 주석은 유용하다
    - 주석이 올바른지 검증하기 어렵기 때문에 유용한 동시에 위험한 주석이다
    
```java
assertTrue(a.compareTo(a) == 0); // a === a
assertTrue(a.compareTo(b) !== 0); // a != b
assertTrue(ab.compareTo(ab) == 0); // ab == ab
```
<br/>

5) 결과를 경고하는 주석
    - 다른 프로그래머에게 결과를 경고할 목적으로 사용된다
    
```java
public static SimpleDateFormat makeStandardHttpDateFormat()
{
  // SimpleDateFormat은 스레드에 안전하지 못하다.
  // 따라서 각 인스턴스를 독립적으로 생성해야 한다.
  SimpleDateFormat df = new SimpleDateFormat("EEE, dd MM yyyy HH:mm:ss z");
  df.setTimeZone(TimeZone.getTimeZone("GMT"));
  return df;
}
```
<br/>

6) TODO 주석
    - 앞으로 할일을 주석으로 남겨두면 편하다
    - 주기적으로 TODO 주석을 점검해 없애도 괜찮은 주석은 없애야한다
    
```java
// TODO-MdM 현재 필요하지 않다.
// 체크아웃 모델을 도입하면 필요없다.
protected VersionInfo makeVersion() throws Exception
{
    return null;
}
```
<br/>

7) 중요성을 강조하는 주석
    - 대수롭지 않다고 여겨질 뭔가의 중요성을 강조하기 위해서도 주석을 사용한다
    
```java
String listItemContent = match.group(3).trim();
// 여기서 trim은 정말 중요하다. trim 함수는 문자열에서 시작 공백을 제거한다.
// 문자열에 시작 공백이 있으면 다른 문자열로 인식되기 때문이다.
new ListItemWidget(this, listItemContent, this.level + 1);
return buildList(text.substring(match.end()));
```
<br/>

## 나쁜 주석의 예

1) 주절거리는 주석
   - 특별한 이유 없이 의무감으로 혹은 마지못해 달은 주석
   
```java
public void loadProperties()
{
  try
  {
    String propertiesPath = propertiesLocation + "/" + PROPERTIES_FILE;
    FileInputStream propertiesStream = new FileInputStream(propertiesPath);
    loadedProperties.load(propertiesStream);
  }
  catch(IOException e)
  {
    //  속성 파일이 없다면 기본값을 모두 메모리로 읽어 들였다는 의미이다.
  }
}
```
<br/>

2) 같은 이야기를 중복하는 주석
   - 코드내용과 같은 것을 그대로 중복한 주석
   - 코드보다 부정확해 오히려 독자가 함수를 대충 이해하고 넘어가게 만듬
   
```java
// this.closed가 true일 때 반환되는 유틸리티 메서드다.
// 타임아웃에 도달하면 예외를 던진다.
public synchronized void waitForClose(final long timeoutMillis)
throws Exception
{
  if(!closed)
  {
    wait(timeoutMilis);
    if(!closed)
      throw new Exception("MockResponseSender could not be closed")
  }
}
```
<br/>

3) 오해할 여지가 있는 주석
   - 의도와는 다르게 작성된 주석
   - 살짝 잘못된 정보로 인해 잘못된 방식으로 함수를 호출할 수도 있다

```java
// this.closed가 true로 변하는 순간 반환되지 않는다.
// this.closed가 true여야 반환되고 아니면 무조건 타임아웃을 기다렸다가
// this.closed가 그래도 true가 아니면 예외를 던진다.
public synchronized void waitForClose(final long timeoutMillis)
throws Exception
{
  if(!closed)
  {
    wait(timeoutMilis);
    if(!closed)
      throw new Exception("MockResponseSender could not be closed")
  }
}
```
<br/>

4) 의무적으로 다는 주석
   - 모든 함수에 주서을 달아야한다는 규칙 때문에 생긴 괴물(javadocs)
   
```java
/**
 * @param title CD 제목
 * @param author CD 저자
 * @param tracks CD 트랙
 * @param durationinMinutes CD 길이
 */
public void addCD(String title, String author, int tracks, int durationInMinutes) {
  CD cd = new CD();
  cd.title = title;
  cd.author = author;
  cd.tracks = tracks;
  cd.duration = durationInMinutes;
  cdList.add(cd);
}
```
<br/>

5) 이력을 기록하는 주석
   - 모듈에 가한 변경을 모두 기록하는 일지 혹은 로그 같은 주석
   - 예전에는 관례였지만 현재는 소스코드 관리시스템이 있으니 제거하자

<br/>

6) 있으나 마나 한 주석
   - 너무 당연한 사실을 언급하며 새로운 정보를 제공하지 못하는 주석
   - 개발자가 주석을 무시하는 습관에 빠지고 코드가 바뀌면서 주석은 버려지면서 주석은 거짓말을 한다
   
```java
/**
 * 기본 생성자
 */
protected AnnualDateRule() {
}

/** 월 중 일자 */
private int dayOfMonth;
```
<br/>

7) 무서운 잡음
   - 문서를 제공해야한다는 잘못된 욕심으로 탄생한 잡음
   - 잘라서 붙여넣기 하다가 생긴 오타
   
```java
/** The name. */
private String name;

/** The version. */
private String version;

/** The version. */
private String info;
```
<br/>

8) 함수나 변수로 표현할 수 있다면 주석을 달지 마라
```java
// 전역목록 <smodule>에 속하는 모듈이 우리가 속한 하위 시스템에 의존하는가?
if (smodule.getDependSubsystems().contains(subSysMod.getSubSystem()))
  
// 좋은 코드
ArrayList moduleDependees = smodule.getDependSubsystems();
String ourSubSystem = subSysMod.getSubSystem();
if (moudleDependees.contains(ourSubSystem))
```
<br/>

9) 위치를 표시하는 주석
   - 소스 파일에서 특정 위치를 표시하려는 주석
   - 가독성만 낮춘다
   
```java
// Actions ///////////////////
```
<br/>

10) 닫는 괄호에 다는 주석
    - 중첩이 심하고 장황한 함수라면 의미가 있을 수도 있지만 함수를 줄이려 시도하자

```java
public class ws {
  public static void main(String[] args) {
    try {
      while ((line = in.readLin()) != null) {
        lineCount++;
        charCount += line.length();
        String words[] = line.split("\\W");
        wordCount += words.length;
      } // while
       System.out.printIn("wordCount = " + wordCount);
    } // try
     catch (IOException e) {
      System.err.printIn("Erro:" + e.getMessage());
     } // catch
  } // main
}
```
<br/>

11) 공로를 돌리거나 저자를 표시하는 주석
    - 소스 코드 관리 시스템은 누가 언제 무엇을 추가했는지 기억한다
    - 이런 주석은 그냥 오랫동안 코드에 방치되어 부정확하고 쓸모없는 정보로 변하기 쉽다
   
```java
/* 릭이 추가함 */
```
<br/>

12) 주석으로 처리하는 코드
    - 이유가 있어 남겨놓은것 같은 느낌이 들어 지우기 꺼려해서 쓸모 없이 계속 쌓여간다
   
```java
InputStreamResponse response = new InputStreamResponse();
response.setBody(formatter.getResultStream(), formatter.getByteCount());
// InputStream resultsStream = formatter.getResultStream();
// StreamReader reader = new StreamReader(resultsStream);
```
<br/>

13) HTML 주석
    - 소스 코드에서의 HTML 주석은 무슨 말인지 읽기조차 어렵다
   
```java
/**
 * 적합성 테스트를 수행하기 위한 과업
 * <p/>
 * <pre>
 * 용법:
 * &lt;taskdef name=&quot;execute-fitnesse-tests&quot; /&gt;
 * </pre>
 */
```
<br/>

14) 전역 정보
    - 코드 일부에 주석을 달면서 시스템의 전반적인 정보를 기술하지마라
   
```java
/**
 * 적합성 테스트가 동작하는 포트: 기본값은 8082
 * @param fitnessPort
 */
public void setFitnessePort(int fitnessPort)
{
  this.fitnessPort = fitnessPort;          
}
```
<br/>

15) 모호한 관계
    - 주석과 주석이 설명하는 코드 사이의 관계가 모호하다
    - 주석 자체를 다시 설명해야되는 주석
   
```java
/**
 * 모든 픽셀을 담을 만큼 충분한 배열로 시작한다(여기에 필터 바이트를 더한다)
 * 그리고 헤더 정보를 위해 200바이트를 더한다.
 */
this.pngBytes = new byte[((this.width + 1) * this.height * 3) + 200];
```
<br/>

16) 함수 헤더
    - 짧은 함수는 긴 설명이 필요없다
    - 짧고 한 가지만 수행하며 이름을 잘 붙인 함수는 주석이 필요없다
   
<br />

17) 비공개 코드에서 Javadocs
    - 공개 API는 주석은 유용하지만 공개하지않을 코드라면 주석은 필요없다
   

