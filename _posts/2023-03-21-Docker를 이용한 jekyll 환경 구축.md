---
title: Docker를 이용한 jekyll 환경 구축
date: 2023-03-21 20:00:00 +0900
categories: [jekyll]
tags: [docker, jekyll]     # TAG names should always be lowercase
---
---

이전 jekyll 블로그를 운영 하였을 때에는<br> 로컬 LAMP 서버 위에 공유기 도메인으로<br> 호스팅을 하였으나 공유기 교체와 서버 운용 중단으로<br> 자료 소실 후 블로그 운영은 중단한 상태였다 <br>

이번에는 운용 중인 NAS서버의 Docker를 이용하여 로컬 환경을 구축하기로 하였다<br>


~~윈도우 상에서 Docker로 구축 하여도 되지만<br>평소에 놀고있는 장비 좀 굴려 보겠다는 생각에~~

1. jekyll theme 설치 및 로컬 실행<br>
[Chirpy Starter](https://github.com/cotes2020/chirpy-starter/generate)
를 이용하여 레포지토리 생성 후<br>
NAS상의 디렉토리에 clone 후 권한 설정을 해준 뒤 컨테이너를 생성하면 로컬 서버를 실행한다<br>

	``` bash
	docker run --rm --volume=$path:"/srv/jekyll" -p 4000:4000  -it jekyll/jekyll jekyll serve
	```
	`$path는 clone한 디렉토리`<br><br>
	로컬에서 확인 후 push 할 예정이고<br> 굳이 컨테이너는 유지 할 필요가 없어 `--rm`<br>
	포트포워딩 후 접속 해보면 정상적으로 출력된다<br><br><br>
2. Github에 Deploy<br>
	변경사항 수정 후 github-pages에서 선택한 branch로 push 하면 자동으로 deploy 된다<br><br><br>
3. Posting<br>
[jekyll admin](https://github.com/jekyll/jekyll-admin) 플러그인을 이용하여 포스팅 하는 방법도 있지만 docker file을 수정하여 이미지를 만들어야 하기에 일단 보류 <br>
마크다운도 익숙해 질겸 [write a new post](https://chirpy.cotes.page/posts/write-a-new-post/)를  참고하여 포스팅 중 <br> 추후에는 덜 귀찮은 쪽을 선택할 듯<br><br><br>

4. etc
	1. image 저장 장소 선택
		1. 윈드라이브에 저장 : 구독 종료 후 용량이 괜찮을까
		2. nas에 저장 : HDD가 뻑나면 위험
		3. 1 + 2 백업 : 정석<br>
		

	2. docker image 수정