# 자동 잔디 심기

---

[//]: # (1. [metrics]&#40;#metrics&#41; 만들기 )

[//]: # (2. [githubProfile]&#40;#githubprofile&#41; 설정 )

[//]: # (3. [readme]&#40;#readme&#41; 설정)

[//]: # (4. [Token]&#40;#token&#41; 설정 )

[//]: # (5. [yaml]&#40;#yaml&#41; 파일 설정)

[//]: # (6. [Action]&#40;#action&#41; 설정  )

[//]: # ()


옮겨와야할 필수 파일

> / <br>
> > .eslintrc.js <br>
> action.yml<br>
> package.json <br>
> plug.yml <br>
> settings.json<br> 
>
> .github/workflows/
> > metrics.yaml (커스텀 필요)<br> 
> auto-metrics.yaml (커스텀 필요)<br>
> index.mjs <br>


> .github/workflows 경로는 git Action 이 실행될 위치이다.


auto-metrics.yaml : 자동으로 커밋 하는 파일

아래에 나의 깃 정보를 입력하자.
```yaml
env:
  GIT_NAME: "홍길동"
  GIT_EMAIL: "홍길동.dev@gmail.com"
```

metrics.yaml : 가장 핵심인 깃 허브 프로필 svg 액션

> ${{ secrets.METRICS_TOKEN }} 등록은 아래 에서 설명함 

```yaml
  token: ${{ secrets.METRICS_TOKEN }}
  user: git_name
```

아래는 유튜브가 포함되어있지 않은 metrics.yaml 
```yaml
name: Metrics
on:
  schedule: [{cron: "0 * * * *"}]
  workflow_dispatch:
  push: {branches: ["master", "main"]}
jobs:
  github-metrics:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: lowlighter/metrics@latest
        with:
          token: ${{ secrets.METRICS_TOKEN }}
          user: JHKoder
          template: classic
          base: header, activity, community, repositories, metadata
          config_timezone: Asia/Seoul

          plugin_habits: yes
          plugin_habits_charts_type: classic
          plugin_habits_days: 14
          plugin_habits_facts: yes
          plugin_habits_from: 200
          plugin_habits_languages_limit: 8
          plugin_habits_languages_threshold: 0%
          plugin_isocalendar: yes
          plugin_isocalendar_duration: half-year
          plugin_languages: yes
          plugin_languages_analysis_timeout: 15
          plugin_languages_analysis_timeout_repositories: 7.5
          plugin_languages_categories: markup, programming
          plugin_languages_ignored: html,css,js
          plugin_languages_limit: 8
          plugin_languages_recent_categories: markup, programming
          plugin_languages_recent_days: 14
          plugin_languages_recent_load: 300
          plugin_languages_sections: most-used
          plugin_languages_threshold: 0%
```

###  ${{ secrets.METRICS_TOKEN }} 설정 방법

깃 토큰을 가져오고

액션을 등록할 레파지토리에서 

1. Settings 클릭
2. 좌측 security 목록중 [Secrets and variables]  화살표 클릭
3. Actions 클릭
4. 오른쪽에 [new repository secret] 

> 대충 경로 : https://github.com/gitname/gitname/settings/secrets/actions/new

name 에는 METRICS_TOKEN 
Secret 에는 발급받은 토큰값 