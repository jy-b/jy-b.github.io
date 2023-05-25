---
title: HDD & SSD in DBMS
date: 2023-05-25 07:00:00 +0900
categories: [database]
tags: [database]     # TAG names should always be lowercase
---
---

대부분의 Database 수업에서 기초 개념을 설명할때 HDD를 기준으로 설명하곤 한다.<br>
물론 HDD가 여전히 현역인것은 자명하고 근본적인 설명을 위해서는 HDD로 설명하는것이 맞다고 생각한다.<br><br>
<img src="https://photo.coolenjoy.co.kr/bbs/data/jirum/1262248817/K_17.jpg"><br>
<font size="1">라떼는.... 80기가에 이하생략</font><br>
<img src="https://blocksandfiles.com/wp-content/uploads/2021/01/Wikibon-SSD-less-than-HDD-in-2026.jpg"><br>
<font size="1">낸드 플래시 가격의 하락으로 메모리와 ssd 가격은 하락 중</font><br>

가격 적인 측면에서 HDD가 SSD에 비해 여전히 우위를 점하고 있는것은 사실이지만<br>
그럼에도 불구하고 SSD가 메인스트림으로 자리잡았고 기업에서도 SSD를 많이 사용 하는 추세이기에<br>
DBMS에서 HDD 대신 SSD를 사용하였을때의 이점에 대해 조사해 보았다.<br><br>

>> SSDs offer substantial performance benefits over HDDs, particularly in terms of faster data access and transfer speeds. This speed advantage is especially valuable in database systems where quick data retrieval and processing are critical. As a result, many organizations and individuals have transitioned to SSDs for their DBMS environments to improve overall system performance and responsiveness.<br><br>
Additionally, the decreasing costs of SSDs have made them more accessible and cost-effective for a wider range of applications. While SSDs traditionally had higher price points compared to HDDs, the prices of SSDs have been steadily declining, narrowing the cost gap between the two technologies. This cost reduction has contributed to the increased adoption of SSDs in the DBMS market.<br><br>
However, it's important to note that HDDs still have their place in certain scenarios. HDDs are generally more cost-effective in terms of storage capacity, making them suitable for applications that require large amounts of data storage without the need for high performance. Additionally, in some cases, a combination of SSDs and HDDs is used, where the SSDs are used for caching or storing frequently accessed data, while the bulk of the data resides on HDDs.<br><br>
In summary, while SSDs have become increasingly prevalent in the DBMS market and offer numerous advantages, HDDs still have their role in specific situations. Ultimately, the choice between SSDs and HDDs in the DBMS market depends on the specific requirements of the application, including performance needs, storage capacity, and budget considerations.
>
도와줘요 Chat-gpt<br>
<font size = "1">대충 성능적인 측면에서 SSD가 HDD보다 우수하고 비용적인 부분도 해결되어 가고 있으나 요구사항에 따라 옵션을 선택하는 것이 좋다는 뜻</font><br><br>

# 성능적인 관점에서의 비교
<img src="https://byounghee.me/wp-content/uploads/2017/07/img_59701d998526b.png">
<font size="1">파랑-ssd, 빨강-sas hdd, 초록 sata hdd<br></font>
태생적인 차이로 인해 SSD와 HDD의 성능은 비교 할수 없을 정도로 많은 차이가 나며<br>
랜덤 작업뿐만 아니라 순차 작업에서의 속도 차이도 크다.<br><br>

![Mysql 환경에서의 Disk Type별 tpmP 성능 비교](https://phx02pap004files.storage.live.com/y4m9D5wA4jbMnnzsI3sMje3INpu1z35aq-3yK_1iW7vH0v0Yp1yNHrzlzhShp0OUWyCfovENjGfkgFzrLCa1M8DJeiWcVzZ2C2xc-R9mFbVvEgMwDNFLjhDzuo_LeivHcSSIwOVTAfiYM7XTVIBADiBYFaShxDRPNsZJ2ehRY8xoT7UTsIcb7H5M_8rCBLPGRRC?width=1103&height=477&cropmode=none)
<font size="1">tpmP= 분당 처리되는 신규 트랜잭션</font><br>
환경에 따라 격차가 감소 할 수는 있지만 SSD가 HDD보다 DBMS에서도 성능적인 관점에서 우위를 보인다<br><br>

# 결론

Google Cloud SQL의 SSD와 HDD 스토리지 선택 문서는 다음과 같이 정리 하고 있다<br><br>
확실하지 않은 경우 SSD 스토리지 선택
>>Cloud SQL 인스턴스에 SSD 스토리지를 사용하는 것이 가장 좋은 몇 가지 이유가 있습니다.<br><br>
SSD는 HDD보다 더 빠르며, 더 예측 가능한 성능을 가지고 있습니다.<br><br>
HDD 처리량이 SSD 처리량보다 훨씬 제한적입니다.<br><br>
HDD는 개별 행 읽기 속도가 느립니다. HDD 스토리지는 디스크 탐색 시간 때문에 SSD 스토리지의 초당 행 읽기의 5%만 지원합니다. 그러나 큰 다중 행 검색에는 부정적인 영향을 미치지 않습니다.<br><br>
많은 양의 데이터를 저장하는 경우가 아니라면 HDD의 비용 절감 효과는 미미합니다. 10TB 이상의 데이터를 저장하는 경우 HDD 스토리지를 사용하는 것이 좋습니다.<br>
>

HDD 스토리지가 적합한 사용 사례
>>HDD 저장소는 다음 조건을 충족하는 사용 사례에 적합합니다.<br><br>
10TB 이상의 데이터를 저장할 것으로 예상되는 경우<br><br>
사용자에게 표시되거나 지연 시간에 민감한 애플리케이션을 지원하는 데 데이터를 사용하지 않는 경우<br><br>
워크로드가 다음 카테고리 중 하나에 해당하는 경우<br><br>
  검색과 쓰기를 수행하고 가끔 적은 수의 행을 임의로 읽는 일괄 워크로드<br><br>
  많은 양의 데이터를 쓰고 거의 읽지는 않는 데이터 보관
>

위 사례 외에도 SSD의 수명 문제 등 고려해야 할 사항이 있으며<br>
상황에 맞추어 SSD와 HDD, 혹은 하이브리드로 구성 하는것이 바람직해 보인다.<br><br>

단순히 SSD의 절대적인 성능이 뛰어나기 때문에 성능이 좋다! 말고는<br>
구체적으로 설명하지 못하는 상황이라 더 자세한 내용은 Database 관련 공부를 더 하고 나서 작성해 보는걸로
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbwiy4o%2FbtryyCek0kA%2FIBQ3oumkK9UVaELr8SKfnK%2Fimg.png">
<br><br>
reference<br><br>
[SSD를 쓰면 DBMS가 빨라질까?](https://d2.naver.com/helloworld/7005)
[Google Cloud - SSD와 HDD 스토리지 중 선택](https://cloud.google.com/sql/docs/mysql/choosing-ssd-hdd?hl=ko)<br>
[Mysql Oracle - MySQL Performance Best Practice](https://www.youtube.com/watch?v=dF8qmCOSE7g)<br>
[Samsung Semiconductor - Best Practices for MySQL with SSDs](https://download.semiconductor.samsung.com/resources/white-paper/best-practices-for-mysql-with-ssds.pdf)<br>
[MariaDB vs MySQL SSD NVMe vs SATA Performance - Discussions on the Percona Benchmark](https://www.youtube.com/watch?v=T5nqr5-zIJE)
