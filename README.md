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
$ git push heroku main
// 배포
```

- prebuild -> build -> start 순서대로 해준다
  - nest는 start가 아니라 stat:prod 실행 필요 -> Procfile 설정 필요
    - https://devcenter.heroku.com/articles/procfile
    - web process type 쓰면 start단계 변경가능
    ```
    // Procfile
    web: npm run start:prod
    ```

### 에러보기

```
heroku logs --tail
```

### heroku에 env 옵션 사용하기

1. CLI로 세팅 하는 방법

   ```
   heroku config --help
   ```

   - NODE_ENV 설정

   ```
   heroku config:set NODE_ENV=production
   ```

2. Web에서 세팅하는 방법
   - https://dashboard.heroku.com/apps/프로젝트이름/settings -> Config Vars

### DB 설정 및 env 설정 완료

- https://dashboard.heroku.com/apps/프로젝트이름/resources -> addon -> 원하는 DB 애드온 선택
  - dyno : heroku 안에서 돌아가는 app 의미
- resources 가서 DB 클릭하면 실행된다
  - Setting -> view credential 가면 필요한 정보가 많이있다
  - ConfigVars가서 설정해주자
- 서버 포트 설정 필요
  - 히로꾸가 PORT 임의로 넣을수있음
  ```
  await app.listen(process.env.PORT || 4000);
  ```

### playground 띄우기 위한 graphQL 추가설정

```
GraphQLModule.forRoot({
      autoSchemaFile: true,
      introspection: true,
      playground: true,
      context: ({ req }) => {
        return { user: req["user"] };
      },
    }),
```

### DB URI를 써야한다

- 세부 credential 값이 계속 바뀔수도 있다

```
TypeOrmModule.forRoot({
      type: "postgres",
      ...(process.env.DATABASE_URI
        ? {
            url: process.env.DATABASE_URI,
          }
        : {
            host:
              process.env.NODE_ENV === "test"
                ? "localhost"
                : process.env.DB_HOST,
            port: 5432,
            username: process.env.USER,
            password: process.env.PASSWORD,
            database: process.env.DB_NAME,
            synchronize: true,
          }),
      logging: process.env.NODE_ENV !== "test",
      entities: [Podcast, Episode, User, Review],
    }),
```
