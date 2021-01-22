uberEatsChallenge-backend-deploy

## heroku 배포

- nest배포가 되기위해서는 nest build -> npm run start:prod 과정이 되야한다

- heroku 홈페이지 가서 new -> create new app

### heroku CLI 설치

```
brew tap heroku/brew && brew install heroku
```

### 로그인

```
heroku login
```

### git으로 배포

- heroku 브랜치 생성 후 연결

```
cd my-project/
git init // git 설정 안되있는 경우
heroku git:remote -a heroku에만든app이름 // 브랜치 remote로 heroku git 서버와 연결
```

- 배포

```
$ git add . && git commit -am "make it better"
// 변경 사항 있으면 커밋 필요
$ git push heroku master
```

- prebuild -> build -> start 순서대로 해준다
  - nest는 start가 아니라 stat:prod 실행 필요 -> Procfile 설정 필요
    - https://devcenter.heroku.com/articles/procfile

### 에러보기

```
heroku logs --tail
```
