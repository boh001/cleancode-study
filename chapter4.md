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

### 좋은 주석의 예

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
