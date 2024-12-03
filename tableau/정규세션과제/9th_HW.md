# 9th Study Week

오늘은 별도의 문제가 없습니다.

여러 대시보드를 참고하시어, 학술제 주제 관련 데이터(없을 경우, 본인 관심 데이터)를 사용해 나만의 대시보드를 제작해주세요.

단, 워크시트 3개 이상의 그래프를 표시해야 하며 각 시트 간 상호작용성 필터 or 하이라이트 동작은 꼭 추가되어야 합니다

어떤 부분에 가중을 두었는지, 어떤 사용자 편의성을 고려하였는지에 대한 설명이 필요합니다.

---

**사용한 데이터셋** : DArt-B 학술제 '한국어 학습자를 위한 노래 추천 시스템'에 사용된 데이터셋\
**데이터 출처** : 스포티파이, 멜론\
[태블로 퍼블릭 링크](https://public.tableau.com/views/_17332297594600/1?:language=ko-KR&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)

### 기본 세팅
![img](https://github.com/dorxor/DartB-24-2/blob/main/tableau/img/%EB%8C%80%EC%8B%9C%EB%B3%B4%EB%93%9C_1.png?raw=true)
- 스포티파이 마크 이미지 + 스포티파이 url
- 대시보드의 제목
- 차트의 노래 개수에 따른 Artist 워드클라우드
- 인기도 기준으로 정렬한 노래 차트

### 워드 클라우드에서 아티스트명 선택 시
![img](https://github.com/dorxor/DartB-24-2/blob/main/tableau/img/%EB%8C%80%EC%8B%9C%EB%B3%B4%EB%93%9C_2.png?raw=true)
- 차트에 필터 동작을 걸어, 해당 아티스트의 노래만 표시되도록 함
- 웹페이지에 유튜브 링크("https://www.youtube.com/results?search_query=" + [Artist])를 넣어, 해당 아티스트 검색 결과 제시

### 차트에서 노래 제목 선택 시
![img](https://github.com/dorxor/DartB-24-2/blob/main/tableau/img/%EB%8C%80%EC%8B%9C%EB%B3%B4%EB%93%9C_3.png?raw=true)
- 선택한 노래의 가사 제시
- 해당 노래의 음악적 특징, 점수 등 제시\
각 점수의 값을 max값으로 나누어, 0~1 사이 값으로 스케일링
- 웹페이지에 유튜브 링크("https://www.youtube.com/results?search_query=" + [제목])를 넣어, 해당 노래 제목 검색 결과 제시
