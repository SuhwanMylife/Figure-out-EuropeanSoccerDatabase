# European Soccer Database 데이터 분석

- +25,000 matches
- +10,000 players
- 11 European Countries with their lead championship
- Seasons 2008 to 2016
- Players and Teams' attributes* sourced from EA Sports' FIFA video game series, including the weekly updates
- Team line up with squad formation (X, Y coordinates)
- Betting odds from up to 10 providers
- Detailed match events (goal types, possession, corner, cross, fouls, cards etc…) for +10,000 matches

**16th Oct 2016: New table containing teams' attributes from FIFA !*

여러 사이트에 흩어져 있는 데이터를 모은 게 이 결과물이다~

- [http://football-data.mx-api.enetscores.com/](http://football-data.mx-api.enetscores.com/) : scores, lineup, team formation and events
- [http://www.football-data.co.uk/](http://www.football-data.co.uk/) : betting odds. [Click here to understand the column naming system for betting odds:](http://www.football-data.co.uk/notes.txt) (학교에서 접속 시 URL 차단돼있음 ㄷㄷ)
- [http://sofifa.com/](http://sofifa.com/) : players and teams attributes from EA Sports FIFA games. *FIFA series and all FIFA assets property of EA Sports.*

크롤링 알고리즘 개선하여 NULL 값들을 채워넣겠다!

- [CLICK HERE TO ACCESS THE PROJECT GITHUB](https://github.com/hugomathien/football-data-collection/tree/master/footballData)

이 Dataset을 만든 사람의 의도는 크게 3가지

- 승리 예측
- 베팅 예측(?)
- 전술 혹은 선수 특성을 토대로 어떤 결과 도출
(전술, 선수 기용 등에 유의미한 결과를 만들어 봐라!)

# 테이블 구성

- Country
- League
- Match
- Player
- Player_Attributes
- Team
- Team_Attributes

# Country

- id
- name
    - 국가명

# League

1. id
2. country_id
    - FOREIGN KEY REFERENCES country (id)
3. name
    - 각 국가 별 리그 명칭

Country, League 데이터를 보면,
Country: 벨기에, 리그: 벨기에 리그
이런 식으로 매칭되어 있고, League의 id와 country_id도 같은 값을 가지고 있음. 아마 이는 DB 테이블 구성에 id가 필수로 들어가야 해서 중복으로 값이 들어간 것으로 추정됨. 정확한 건 알아봐야 하지만 중요한 건 아니니까 일단 넘김

# Match

1. id
2. country_id
    - FOREIGN KEY REFERENCES country (id)
3. league_id
    - FOREIGN KEY REFERENCES League (id)
4. season
    - 몇 시즌 경기인지 기록
    - 2008/2009 ~ 2015/2016
5. stage
    - 시즌 내에 몇 라운드에 치뤄진 경기인지 기록
6. date
    - 어느 날짜, 시간에 치뤄진 경기인지 기록
    - 시간은 00:00:00으로 통일한 것으로 보임
7. match_api_id
    - 잘 모르겠음. 그냥 id값인 듯?
8. home_team_api_id
    - FOREIGN KEY REFERENCES Team (team_api_id)
9. away_team_api_id
    - FOREIGN KEY REFERENCES Team (team_api_id)
10. home_team_goal
    - 홈 팀이 넣은 골 수
11. away_team_goal
    - 원정 팀이 넣은 골 수
12. home_player_X1 ~ X11 & away_player_X1 ~ X11
home_player_Y1 ~ Y11 & away_player_Y1 ~ Y11
    - 포메이션 / x, y: 좌표값 (으로 추정됨)
13. home_player_1 ~ 11 & away_player_1 ~ 11
    - FOREIGN KEY REFERENCES Player (player_api_id)
14. goal
    - 누가 골 넣었는지까지 정리되어 있는 듯
15. shoton
    - 유효슈팅 개수?
16. shotoff
    - 슈팅개수?
17. foulcommit
    - 반칙 횟수
    - 누가 반칙한 건지도 같이 크롤링 된 듯
18. card
    - 누가 옐로/레드 카드를 받았는지 정리되어 있는 듯
19. [cross]
    - 크로스 올린 갯수?
20. corner
    - 코너킥  갯수?
21. possession
    - 점유율
    - 크롤링이 참 더럽게 됐다...
22. H, D, A
    - 각 도박사이트별 홈 팀 승리 확률(H), 비길 확률(D), 원정팀 승리 확률(A)

# Player

1. id
2. player_api_id
    - 왜 별개의 api_id를 만든거지?
3. player_name
    - 플레이어 명
4. player_fifa_api_id
    - 피파(게임 상) api_id
5. birthday
    - 생일
6. height
    - 신장
7. weight
    - 몸무게

# Player_Attributes

1. id
2. player_fifa_api_id
    - FOREIGN KEY REFERENCES Player (player_fifa_api_id)
3. player_api_id
    - FOREIGN KEY REFERENCES Player (player_api_id)
4. date
    - 무슨 날짜지?
5. overall_rating
    - 전체 스텟 (평균 스텟)
6. potential
    - 잠재력
7. preferred_foot
    - 주 발 (선호하는 발, 오른발/왼발)
8. attacking_work_rate
    - 공격 가담률 (low, medium, high)
9. defensive_work_rate
    - 수비 가담률 (low, medium, high)
10. crossing
    - 크로스의 질
11. finishing
    - 마무리 능력 (골 결정능력)
12. heading_accuracy
    - 헤딩 정확도
13. short_passing
    - 짧은 패스의 질
14. volleys
    - 발리 킥(공중에 떠 있는 볼에 대한 킥)의 질
15. dribbling
    - 드리블의 질
    - 상대 선수를 제칠 수 있는 스킬을 가리킴
16. curve
    - 볼 회전의 질
17. free_kick_accuracy
    - 프리킥 정확도
18. long_passing
    - 롱 패스의 질
19. ball_control
    - 볼 컨트롤의 질
    - 볼 터치, 트래핑, 약간의 방향 변경 등을 나타냄
20. acceleration
    - 가속력
21. sprint_speed
    - 속력
22. agility
    - 민첩함
    - 밸런스가 무너지지 않고 얼마나 빠르게 다음 동작을 가져갈 수 있냐를 나타냄
23. reactions
    - 반응속도
    - 자신 외 다른 외적인 요소(상대 선수, 같은 팀 선수, 볼 흐름 등)에 대하여 얼마나 빠르게 올바른 반응을 가져가는지를 나타냄
24. balance
    - 특정 상황에서 얼마나 균형을 유지할 수 있는지를 수치로 나타냄
25. shot_power
    - 슈팅의 세기
26. jumping
    - 점프력
27. stamina
    - 체력
28. strength
    - 힘
    - 몸싸움 시 관여되는 수치로 추정됨
29. long_shots
    - 중장거리 슈팅의 위력을 수치화
30. aggression
    - 상대와의 몸싸움 적극성을 나타냄
    - 태클, 일반적인 볼 경합, 헤딩 경합 등
    - 이 수치가 높을수록 파울 발생률이 높아질 수 있음
31. interceptions
    - 상대 패스 경로를 예측하고 차단하는 능력
32. positioning
    - 얼마나 특정 상황에 맞는 위치를 가져가는지를 나타냄
    - 볼을 받는 위치, 골을 넣기 위한 위치 등
33. vision
    - 시야
    - 팀원의 위치를 얼마나 잘 파악하냐를 나타냄
34. penalties
    - 페널티킥 시 슈팅의 정확도
35. marking
    - 상대 선수가 공을 컨트롤하는 것을 막는 능력
36. standing_tackle
    - 선 자세에서 볼을 커트하는 능력
37. sliding_tackle
    - 슬라이딩하여 볼을 커트하는 능력
38. gk_diving
    - 다이빙 능력
39. gk_handling
    - 볼을 손으로 다루는 능력
    - 볼을 잡거나 하는 능력
40. gk_kicking
    - 골킥 능력
41. gk_positioning
    - 골키퍼의 위치선정 능력
42. gk_reflexes
    - 반사신경

# Team

1. id
2. team_api_id
3. team_fifa_api_id
4. team_long_name
    - 팀의 풀네임
5. team_short_name
    - 팀의 숏네임, 보통 알파벳 세 글자로 나타냄

# Team_Attributes

- 각 컬럼에 대해 100% 이해하지는 못했음. 수치가 높고 낮음에 따라 무엇을 의미하는 지에 대한 파악이 안 됨. 다른 사람의 Notebook 보면서 채워나가 보겠음
- 크롤링을 어디서 해 온 건지 모르겠음 (크롤링 이해도 부족)
- 아래 지표들은 주관적인 성격이 강하다고 판단. 따라서 크롤링한 해당 사이트로 가서 어디에서 긁어 온 건지 파악이 필요. 하지만 해당 사이트에 들어가서 확인했을 때 단계별(TEXT)로는 확인이 어느정도 가능하지만 수치로 나타낸 자료는 찾지 못했다. 추후에 크롤링을 통해 보완하거나 새로운 데이터로 대체가 필요한 부분
1. id
2. taem_fifa_api_id
    - FOREIGN KEY REFERENCES Team (team_fifa_api_id)
3. team_api_id
    - FOREIGN KEY REFERENCES Team (team_api_id)
4. date
5. buildUpPlaySpeed & buildUpPlaySpeedClass
    - 빌드업: 최후방에서 최전방까이 볼을 운반하는 과정 자체를 가리킴
    - 빌드업 시 볼을 운반하는 속도
    - Fast / Slow / Balanced
6. buildUpPlayDribbling & buildUpPlayDribblingClass
    - Normal / Little / Lots
7. buildUpPlayPassing & buildUpPlayPassingClass
    - 빌드업 시 주로 사용되는 패스
    - Mixed / Long / Short
8. buildUpPlayPositioningClass
    - 빌드업 시 포지션을 지키면서 혹은 자유롭게 하는지에 대한 여부
    - Organised / Free Form
9. chanceCreationPassing & chanceCreationPassingClass
    - 여기서 말하는 찬스는 슈팅으로 이어지는 찬스를 가리키는 것으로 추정됨 (이거는 50% 확신함. 걍 모르겠음)
    - 찬스로 이어지는 패스의 성향을 나타냄
    - Normal / Risky / Safe
10. chanceCreationCrossing & chanceCreationCrossingClass
    - 찬스로 이어지는 크로스의 빈도를 나타냄
    - Normal / Little / Lots
11. chanceCreationShooting & chanceCreationShootingClass
    - 찬스로 이어지는 슈팅의 빈도를 나타냄
    - Normal / Little / Lots
12. chanceCreationPositioningClass
    - 찬스로 이어지는 포지셔닝(?)의 방식
    - Organised / Free Form
13. defencePressure & defencePressureClass
    - 수비 시 상대를 압박하는 위치를 나타냄
    - Medium / Deep / High
14. defenceAggression & defenceAggressionClass
    - 수비 성향을 나타냄
    - Press / Double / Contain
15. defenceTeamWidth & defenceTeamWidthClass
    - 수비 시 팀 간격
    - Normal / Wide / Narrow
16. defenceDefenderLineClass
    - 수비 시 라인 컨트롤 방식
    - Cover / Offside Trap
    
- +25,000 matches
- +10,000 players
- 11 European Countries with their lead championship
- Seasons 2008 to 2016
- Players and Teams' attributes* sourced from EA Sports' FIFA video game series, including the weekly updates
- Team line up with squad formation (X, Y coordinates)
- Betting odds from up to 10 providers
- Detailed match events (goal types, possession, corner, cross, fouls, cards etc…) for +10,000 matches

**16th Oct 2016: New table containing teams' attributes from FIFA !*

여러 사이트에 흩어져 있는 데이터를 모은 게 이 결과물이다~

- [http://football-data.mx-api.enetscores.com/](http://football-data.mx-api.enetscores.com/) : scores, lineup, team formation and events
- [http://www.football-data.co.uk/](http://www.football-data.co.uk/) : betting odds. [Click here to understand the column naming system for betting odds:](http://www.football-data.co.uk/notes.txt) (학교에서 접속 시 URL 차단돼있음 ㄷㄷ)
- [http://sofifa.com/](http://sofifa.com/) : players and teams attributes from EA Sports FIFA games. *FIFA series and all FIFA assets property of EA Sports.*

크롤링 알고리즘 개선하여 NULL 값들을 채워넣겠다!

- [CLICK HERE TO ACCESS THE PROJECT GITHUB](https://github.com/hugomathien/football-data-collection/tree/master/footballData)

이 Dataset을 만든 사람의 의도는 크게 3가지

- 승리 예측
- 베팅 예측(?)
- 전술 혹은 선수 특성을 토대로 어떤 결과 도출
(전술, 선수 기용 등에 유의미한 결과를 만들어 봐라!)

# 테이블 구성

- Country
- League
- Match
- Player
- Player_Attributes
- Team
- Team_Attributes

# Country

- id
- name
    - 국가명

# League

1. id
2. country_id
    - FOREIGN KEY REFERENCES country (id)
3. name
    - 각 국가 별 리그 명칭

Country, League 데이터를 보면,
Country: 벨기에, 리그: 벨기에 리그
이런 식으로 매칭되어 있고, League의 id와 country_id도 같은 값을 가지고 있음. 아마 이는 DB 테이블 구성에 id가 필수로 들어가야 해서 중복으로 값이 들어간 것으로 추정됨. 정확한 건 알아봐야 하지만 중요한 건 아니니까 일단 넘김

# Match

1. id
2. country_id
    - FOREIGN KEY REFERENCES country (id)
3. league_id
    - FOREIGN KEY REFERENCES League (id)
4. season
    - 몇 시즌 경기인지 기록
    - 2008/2009 ~ 2015/2016
5. stage
    - 시즌 내에 몇 라운드에 치뤄진 경기인지 기록
6. date
    - 어느 날짜, 시간에 치뤄진 경기인지 기록
    - 시간은 00:00:00으로 통일한 것으로 보임
7. match_api_id
    - 잘 모르겠음. 그냥 id값인 듯?
8. home_team_api_id
    - FOREIGN KEY REFERENCES Team (team_api_id)
9. away_team_api_id
    - FOREIGN KEY REFERENCES Team (team_api_id)
10. home_team_goal
    - 홈 팀이 넣은 골 수
11. away_team_goal
    - 원정 팀이 넣은 골 수
12. home_player_X1 ~ X11 & away_player_X1 ~ X11
home_player_Y1 ~ Y11 & away_player_Y1 ~ Y11
    - 포메이션 / x, y: 좌표값 (으로 추정됨)
13. home_player_1 ~ 11 & away_player_1 ~ 11
    - FOREIGN KEY REFERENCES Player (player_api_id)
14. goal
    - 누가 골 넣었는지까지 정리되어 있는 듯
15. shoton
    - 유효슈팅 개수?
16. shotoff
    - 슈팅개수?
17. foulcommit
    - 반칙 횟수
    - 누가 반칙한 건지도 같이 크롤링 된 듯
18. card
    - 누가 옐로/레드 카드를 받았는지 정리되어 있는 듯
19. [cross]
    - 크로스 올린 갯수?
20. corner
    - 코너킥  갯수?
21. possession
    - 점유율
    - 크롤링이 참 더럽게 됐다...
22. H, D, A
    - 각 도박사이트별 홈 팀 승리 확률(H), 비길 확률(D), 원정팀 승리 확률(A)

# Player

1. id
2. player_api_id
    - 왜 별개의 api_id를 만든거지?
3. player_name
    - 플레이어 명
4. player_fifa_api_id
    - 피파(게임 상) api_id
5. birthday
    - 생일
6. height
    - 신장
7. weight
    - 몸무게

# Player_Attributes

1. id
2. player_fifa_api_id
    - FOREIGN KEY REFERENCES Player (player_fifa_api_id)
3. player_api_id
    - FOREIGN KEY REFERENCES Player (player_api_id)
4. date
    - 무슨 날짜지?
5. overall_rating
    - 전체 스텟 (평균 스텟)
6. potential
    - 잠재력
7. preferred_foot
    - 주 발 (선호하는 발, 오른발/왼발)
8. attacking_work_rate
    - 공격 가담률 (low, medium, high)
9. defensive_work_rate
    - 수비 가담률 (low, medium, high)
10. crossing
    - 크로스의 질
11. finishing
    - 마무리 능력 (골 결정능력)
12. heading_accuracy
    - 헤딩 정확도
13. short_passing
    - 짧은 패스의 질
14. volleys
    - 발리 킥(공중에 떠 있는 볼에 대한 킥)의 질
15. dribbling
    - 드리블의 질
    - 상대 선수를 제칠 수 있는 스킬을 가리킴
16. curve
    - 볼 회전의 질
17. free_kick_accuracy
    - 프리킥 정확도
18. long_passing
    - 롱 패스의 질
19. ball_control
    - 볼 컨트롤의 질
    - 볼 터치, 트래핑, 약간의 방향 변경 등을 나타냄
20. acceleration
    - 가속력
21. sprint_speed
    - 속력
22. agility
    - 민첩함
    - 밸런스가 무너지지 않고 얼마나 빠르게 다음 동작을 가져갈 수 있냐를 나타냄
23. reactions
    - 반응속도
    - 자신 외 다른 외적인 요소(상대 선수, 같은 팀 선수, 볼 흐름 등)에 대하여 얼마나 빠르게 올바른 반응을 가져가는지를 나타냄
24. balance
    - 특정 상황에서 얼마나 균형을 유지할 수 있는지를 수치로 나타냄
25. shot_power
    - 슈팅의 세기
26. jumping
    - 점프력
27. stamina
    - 체력
28. strength
    - 힘
    - 몸싸움 시 관여되는 수치로 추정됨
29. long_shots
    - 중장거리 슈팅의 위력을 수치화
30. aggression
    - 상대와의 몸싸움 적극성을 나타냄
    - 태클, 일반적인 볼 경합, 헤딩 경합 등
    - 이 수치가 높을수록 파울 발생률이 높아질 수 있음
31. interceptions
    - 상대 패스 경로를 예측하고 차단하는 능력
32. positioning
    - 얼마나 특정 상황에 맞는 위치를 가져가는지를 나타냄
    - 볼을 받는 위치, 골을 넣기 위한 위치 등
33. vision
    - 시야
    - 팀원의 위치를 얼마나 잘 파악하냐를 나타냄
34. penalties
    - 페널티킥 시 슈팅의 정확도
35. marking
    - 상대 선수가 공을 컨트롤하는 것을 막는 능력
36. standing_tackle
    - 선 자세에서 볼을 커트하는 능력
37. sliding_tackle
    - 슬라이딩하여 볼을 커트하는 능력
38. gk_diving
    - 다이빙 능력
39. gk_handling
    - 볼을 손으로 다루는 능력
    - 볼을 잡거나 하는 능력
40. gk_kicking
    - 골킥 능력
41. gk_positioning
    - 골키퍼의 위치선정 능력
42. gk_reflexes
    - 반사신경

# Team

1. id
2. team_api_id
3. team_fifa_api_id
4. team_long_name
    - 팀의 풀네임
5. team_short_name
    - 팀의 숏네임, 보통 알파벳 세 글자로 나타냄

# Team_Attributes

- 각 컬럼에 대해 100% 이해하지는 못했음. 수치가 높고 낮음에 따라 무엇을 의미하는 지에 대한 파악이 안 됨. 다른 사람의 Notebook 보면서 채워나가 보겠음
- 크롤링을 어디서 해 온 건지 모르겠음 (크롤링 이해도 부족)
- 아래 지표들은 주관적인 성격이 강하다고 판단. 따라서 크롤링한 해당 사이트로 가서 어디에서 긁어 온 건지 파악이 필요. 하지만 해당 사이트에 들어가서 확인했을 때 단계별(TEXT)로는 확인이 어느정도 가능하지만 수치로 나타낸 자료는 찾지 못했다. 추후에 크롤링을 통해 보완하거나 새로운 데이터로 대체가 필요한 부분
1. id
2. taem_fifa_api_id
    - FOREIGN KEY REFERENCES Team (team_fifa_api_id)
3. team_api_id
    - FOREIGN KEY REFERENCES Team (team_api_id)
4. date
5. buildUpPlaySpeed & buildUpPlaySpeedClass
    - 빌드업: 최후방에서 최전방까이 볼을 운반하는 과정 자체를 가리킴
    - 빌드업 시 볼을 운반하는 속도
    - Fast / Slow / Balanced
6. buildUpPlayDribbling & buildUpPlayDribblingClass
    - Normal / Little / Lots
7. buildUpPlayPassing & buildUpPlayPassingClass
    - 빌드업 시 주로 사용되는 패스
    - Mixed / Long / Short
8. buildUpPlayPositioningClass
    - 빌드업 시 포지션을 지키면서 혹은 자유롭게 하는지에 대한 여부
    - Organised / Free Form
9. chanceCreationPassing & chanceCreationPassingClass
    - 여기서 말하는 찬스는 슈팅으로 이어지는 찬스를 가리키는 것으로 추정됨 (이거는 50% 확신함. 걍 모르겠음)
    - 찬스로 이어지는 패스의 성향을 나타냄
    - Normal / Risky / Safe
10. chanceCreationCrossing & chanceCreationCrossingClass
    - 찬스로 이어지는 크로스의 빈도를 나타냄
    - Normal / Little / Lots
11. chanceCreationShooting & chanceCreationShootingClass
    - 찬스로 이어지는 슈팅의 빈도를 나타냄
    - Normal / Little / Lots
12. chanceCreationPositioningClass
    - 찬스로 이어지는 포지셔닝(?)의 방식
    - Organised / Free Form
13. defencePressure & defencePressureClass
    - 수비 시 상대를 압박하는 위치를 나타냄
    - Medium / Deep / High
14. defenceAggression & defenceAggressionClass
    - 수비 성향을 나타냄
    - Press / Double / Contain
15. defenceTeamWidth & defenceTeamWidthClass
    - 수비 시 팀 간격
    - Normal / Wide / Narrow
16. defenceDefenderLineClass
    - 수비 시 라인 컨트롤 방식
    - Cover / Offside Trap