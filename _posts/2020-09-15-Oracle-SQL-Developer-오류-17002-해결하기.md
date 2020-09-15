---
title: Oracle SQL Developer 오류 17002 해결하기
date: 2020-09-15 15:45:34 +0900
categories: [공부, SQL]
tags: [oracle, sql, 공부, 오류, 해결]
---

## IO 오류: The Network Adapter could not establish the connection(업체 코드 17002)

---

![오류 사진](/assets/img/post/2020-09-15-154921.png)

### 오류 발생 경위

---

오늘 데이터베이스 수업에서 `Oracle Database Express Edition(XE)`와 `Oracle SQL Developer`를 설치하는 것을 배웠는데 시키는 데로 했더니 위와 같은 오류가 발생하였다.\
그래서 방화벽 인바운드 규칙도 추가하고 재설치도 하고 리스너 서비스 재시작도 해보고 별 짓을 다 해봤다.

![리스너 서비스 명령어](/assets/img/post/2020-09-15-155901.png)

하지만 위와 같이 리스너는 제대로 동작하고 있는 것을 보면 리스너의 문제는 아닌 것 같았다.\
그래서 `Oracle SQL Developer`의 설정이 잘못되었나 살펴보았다.

![Oracle SQL Developer 설정 화면](/assets/img/post/2020-09-15-160332.png)

호스트 이름에 `localhost`라고 적혀 있었지만 분명히 `listener.ora`, `tnsnames.ora` 파일에는 다른 IP주소가 적혀 있었다.\
그래서 혹시나 해서 해당 파일들에 적혀 있는 주소로 바꿔봤는데 정상적으로 작동하였다.

### 해결방법

---

#### 1. IPv4 주소 알아내기

![ipconfig](/assets/img/post/2020-09-15-163107.png)

먼저 `cmd`를 실행시키고 `ipconfig` 명령어를 쳐서 `IPv4` 주소을 알아낸다.\
위 이미지에서 빨간색 부분을 메모해두면 된다.

#### 2. ora 파일 수정하기

그다음 `Oracle XE`를 설치한 곳에 있는 `listener.ora`, `tnsnames.ora` 파일을 찾으면 된다.\
저 같은 경우에는 `C:\app\dayong\product\18.0.0\dbhomeXE\network\admin`에 해당 파일들이 위치해있다.\
만약에 설치 경로는 찾았지만 해당 폴더에 해당 파일들이 없다면 다른 문제로 오류가 발생하는 것이기 때문에 이 글로는 해결이 불가능하다. _(다시 구글링 하시러 가시는 것을 추천합니다..)_

![ora 파일 수정 방법](/assets/img/post/2020-09-15-163745.png)

해당 파일들을 열어서 흰색 부분 즉 (HOST = **이 부분**)을 아까 메모한 `IPv4` 주소를 넣어주면 된다.\
`localhost`를 넣어줘도 된다고 하는데 저 같은 경우에는 안 되가지고 그냥 `Ipv4` 주소를 넣어줬다.

#### 3. 서비스 재시작하기

![서비스 재시작](/assets/img/post/2020-09-15-164150.png)

이제 설정은 마무리했으니 설정을 적용하기 위해 리스너를 재시작하는 과정이 필요하다.\
먼저 `윈도우 + R` 단축키를 누른 후에 `services.msc`를 입력하고 확인을 눌러주면 위의 사진처럼 서비스 프로그램이 실행된다.\
그 후 `OracleOraDB18Home1TNSListener`를 찾아준다.\
저 같은 경우에는 18 버전을 깔아서 위와 같은 이름으로 되어있는데 아마 다른 버전은 이름이 다를 것이다.\
어쨌든 해당 리스너 서비스를 오른쪽 클릭하고 **중지**를 누른 후 다시 오른쪽 클릭하고 **시작**을 눌러주면 재시작이 되어 설정이 적용되었을 것이다.

#### 4. Oracle SQL Developer 호스트이름 수정하기

![Oracle SQL Developer 설정 화면](/assets/img/post/2020-09-15-160332.png)

이제 `Oracle SQL Developer`으로 돌아가서 접속 설정 중 호스트 이름을 localhost 또는 다른 값으로 설정되어 있는 것을 아까 설정한 호스트 이름으로 바꿔주면 된다.\
아마 같은 이유로 문제가 일어났으면 위의 방법들로 바로 해결될 것이다.\
하지만 구글링 한 결과 정말 다양한 해결 방법이 존재하기 때문에 차근차근 하나씩 해보는 것이 중요한 것 같다.

**해결되었기를 바랍니다.
감사합니다.**
