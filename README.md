# suricata
suricata Example

---

## arp-spoofing 코드리뷰

case문 쓸 때는 enum

---

## snort 개요

sourcefire사에서 만든 NDIS

실사긴 traffic 분석 & packet logging용으로 적합

원쓰레드 처리

---

## Suricata 개요

멀티코어, 멀티 쓰레딩 지원

---

**요점 : snort는 싱글쓰레드 / 수리카타는 멀티쓰레드**

---

## Suricata 설치

``` console
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository ppa:oisf/suricata-stable
sudo apt update
sudo apt install suricata
```

---

## Suricata로 로그 확인하기

```
alert tcp any any -> any 80 (msg:"gilgil.net access"; content:"GET /"; content:"Host: "; content:"gilgil.net"; sid:10001; rev:1;)
alert tcp any any -> any 80 (msg:"netflix.com access"; content:"GET /"; content:"Host: "; content:"netflix.com"; sid:10002; rev:1;)
alert tcp any any -> any 80 (msg:"qt.io access"; content:"GET /"; content:"Host: "; content:"qt.io"; sid:10003; rev:1;)
```

다음과 같이 `test.rules` 파일 작성

이후 `suricata -s test.rules -i eth0(interface name)` 입력하여 수리카타 실행

로그 확인하는 법 : `tail -f /var/log/suricata/fast.log`

---
## 아호 코라식(Aho-Corasick)  알고리즘

아호코라식(Aho-Corasick)은 현재 광범위하게 알려진 거의 유일한 일대다 패턴매칭 알고리즘
아호 코라식은 또 S를 한 번만 훑어서 결과를 내기 때문에 O(N+m1+m2+...+mk)입니다. (S의 길이 N / 각 단어의 길이를 m)

[아호 코라식 정리 블로그](https://pangtrue.tistory.com/305)

![image](https://user-images.githubusercontent.com/37138188/128498259-308cd56e-680e-49ac-abc6-5e54752b0eb3.png)
트라이에서의 매칭 과정 

![image](https://user-images.githubusercontent.com/37138188/128498285-1126de9e-c0b0-4fce-bd25-6a56333fbbf0.png)
트라이에서 매칭 중 실패한 상황

![image](https://user-images.githubusercontent.com/37138188/128498385-c03aab04-9660-408d-aa69-779a414290ed.png)
매칭에 실패했을 땐 그전까지 일치한 문자열의 접두사-접미사가 같은 지점으로 이동

그녕 서치하면 : O(N)
aho-corasick 쓰면 : O(1)

단점은 메모리를 많이 먹는다.

시그니처가 아무리 많아져도 aho-corasick을 돌리면 된다.

---

## Boyer-Moore 알고리즘

[Boyer-Moore 알고리즘 완벽 정리](https://devwooks.tistory.com/12)

Boyer-Moore알고리즘은 패턴의 마지막 문자부터 역순으로 검사를 진행하면서 일치하지 않는 문자가 나타나면 미리 준비된 표(skip table)에 따라 건너뛸 위치를 정한다.

strnstr 함수를 호출하면 tcp 영역을 찾아서 리턴 해주는 함수.

Boyer-moore
오른쪽에서 왼쪽으로 검색을 한다.
BC(Bad Charater)와 GS(Good Suffix) 테이블을 생성한다.

**ex)**

abcde << 이런 패턴을 찾아야한다.

strnstr은 인덱스를 증가시켜서 찾아낸다. 이 함수는 big 문자열에 len 길이 중에서 little 문자열을 찾는 것이다.

반환 값
- 만약 little 값이 비어 있으면 big를 반환한다.
- big 문자열에서 little 문자열을 찾지 못하면 NULL을 반환한다.
- little 문자열을 찾으면 big에 little 문자열 시작 부분 위치 주소를 반환한다.

boyer moore-search
![image](https://user-images.githubusercontent.com/37138188/128499370-8ac347d8-f9cb-41c9-b138-360504e85991.png)
나머지는 전부 5

---
