---
title: Jekyll posting시 future date 문제
date: 2023-03-21 20:30:00 +0900
categories: [jekyll]
tags: [jekyll]     # TAG names should always be lowercase
---
---

# 문제

`Skipping: _posts/2023-03-21-Docker를 이용한 jekyll 환경 구축.md has a future date`

_post 디렉토리에 업로드하였지만 해당 문장을 출력하며 정상적으로 글이 등록 되지 않았다<br>

jekyll은 글 상단에 front fomatter을 적어줘야 하는데

```
---
title: TITLE
date: YYYY-MM-DD HH:MM:SS +/-TTTT
categories: [TOP_CATEGORIE, SUB_CATEGORIE]
tags: [TAG]     # TAG names should always be lowercase
---
```
date값이 미래 시간이면 등록하지 않고 skip해 버린다

시간대 정보 생략시<br>
원격서버의 경우 서버의 시간을 따른다<br>

# 해결
---
front matter에 시간대를 명시<br>
`_config.yml`의 `timezone` 항목을 Asia/Seoul로 수정