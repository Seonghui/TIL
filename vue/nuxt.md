nuxt
---

넉스트는 뷰, 뷰 라우터, 뷰엑스, 뷰 서버 렌더러와 뷰 메타의 조합임. 뷰 서버 렌더러는 spa 모드일 때 제외됨. 매뉴얼상의 넉스트는 universal app을 생성하기 위한 프레임워크임. 유니버셜 앱은 클라이언트 사이드와 서버 사이드에 대한 fancy word임. 그런데 서버 사이드에서 동작한다고 해서 넉스트가 익스프레스 같은 백엔드 프레임워크는 아니고, 서버 사이드 넉스트는 실제적으로 미리 구성된 vue 서버 렌더러임.

# SSR (Server side Rendering)
## 장점
* SEO. 검색 엔진 크롤러가 렌더된 페이지를 바로 볼 수 있음.
* Faster time to content. 특히 느린 인터넷이나 기기에서 빠름. 서버에서 렌더된 마크업은 모든 자바스크립트가 실행될때까지 기다릴 필요가 없음. 따라서 유저는 렌더링된 페이지를 보다 빨리 볼 수 있음. 

## ssr를 사용하면 감수해야하는 것들
* Browser-specific 코드는 특정 라이프사이클 훅에서만 사용될 수 있음. 그래서 ssr 지원 안 하는 외부 라이브러리는 서버 렌더링 앱에서 별도로 처리해줘야 함.
* 설치와 배포 설정이 복잡. SPA는 정적 파일 서버라면 어디서든 배포가 되는데 서버 렌더링 앱에서는 Node.js 서버가 실행될 수있는 환경이 필요함 (뭐????)
* 서버 사이드 로드가 증가. node.js에서 전체 앱을 렌더링하면 정적 파일 렌더링할때보다 cpu 사용량이 증가. 따라서 만약 트래픽이 많은 사이트라면 이러한 서버 로드에 대비하고 캐싱 등을 사용해서 대처해야 함.

# lifecycle
Request가 들어오면..

![Nuxt lifeCycle](https://cdn-images-1.medium.com/max/1600/1*CM9tZI28r0sJjb53MtjmYw.png)

## nuxtServerInit
nuxtServerInit이 Store에 정의되어 있으면 Nuxt.js는 컨텍스트 (서버 측에서만 해당)를 사용해 호출해야 함. 서버의 데이터를 클라이언트에게 직접 주고 싶을때 유용.

## middleware
* 미들웨어는 미리 정의된 함수임.
* 특정 페이지나 전체 페이지에 적용 가능
* 로그인이 체크되었는지, 유효성 검사 등에 사용
* 하나의 미들웨어는 하나의 js 파일에 정의되어야 함

### 미들웨어 파일 파일 생성하기
js파일에는 익명 함수를 export default한다. 이 익명함수는 middleware 함수임. nuxt는 익명 함수에 argument value로 context를 할당하는데 이게 베리베리 유용함. 왜냐면 컨텍스트는 미들웨어 함수를 거의 전체 페이지에 접근할 수 있게 만들어주기 떄문임. context key는 [여기](https://nuxtjs.org/api/context/)서 확인 가능.

```js
//  context의 store, redirect
export default function ({ store, redirect }) {
  // 사용자가 인증을 하지 않은 경우.
  if (!store.state.authenticated) {
    return redirect('/login')
  }
}

//그냥 context
export default function (context) {
  console.log(context) // context.params, context.query, context.store...
}
```

### 미들웨어 사용하기
- 특정 페이지에서 사용하기
![특정 페이지에서 미들웨어 사용하기](https://cdn-images-1.medium.com/max/2400/1*zNEFZjo0KQXWaoOAMO6xSQ.png)
앞서 말했듯 미들웨어는 특정 페이지나 전체 페이지에서전부 사용할 수 있는데, 특정 페이지에서 미들웨어를 사용하는 경우에는 페이지에 미들웨어 프로퍼티를 직접 작성하면 됨. 플러그인과 비슷하게 작성하면 되고 미들웨어 이름은 미들웨어 폴더에 있는 파일명임

- 전체 페이지에서 사용하기
![전체 페이지에서 미들웨어 사용하기](https://cdn-images-1.medium.com/max/2400/1*kaobLl8tIPPU1A0bvPYrcw.png)
nuxt.config.js에서 router 프로퍼티를 추가하고 그 안에 미들웨어를 추가하면 됨. 이렇게 하면 전체 페이지에 미들웨어 적용됨

## validate() - Pages & children
* 사용자가 제출한 모든 데이터는 validate 되어야함. nuxt는 이걸 메소드로 제공함. 
* Validate method는 .vue 파일들의 exported object에 설치되어야 함. 
* 참고로 Validate method는 _id.vue에 설치되어야 함. 
* Validate method는 3가지 프로퍼티를 가진 object를 argument값으로 가짐 - params, query, store(vuex로 접근 가능)
* validate method는 유효성 검사를 위해서 존재한다. 그러니까 뭐 바꾸려고 하지말자.
* 리턴값으로 true, false를 가지며 true일 경우 _id.vue 파일은 정상적으로 로드되지만 false의 경우 미리 정의되어있는 에러 페이지로 이동한다.

## asyncData() & Fetch() - Pages & children

## Render

# 폴더 구성
## Plugin
plugin config 파일 있는 곳임. 플러그인은 js file을 config로 파일로 요구함.

플러그인은 plugin폴더에 config 파일을 만들고 그걸 nuxt.config.js 파일에 등록해야 함. nuxt.config.js 파일에 등록된 컴포넌트는 글로벌 컴포넌트로 생성이 되는데, 여기서의 글로벌이라는 단어의 의미는 전체 next program을 의미함. 따라서 어느 파일에서나 이 플러그인을 사용할 수 있음. 그런데 여기서 문제가 생기는데, pages 디렉토리의 모든 .vue 파일은 독립적인 SPA임. 만약 pages 디렉토리에 .vue 파일이 2개면 이 nuxt program은 2개의 SPA들로 구성되어있다는 것을 의미함. 결과적으로 우리가 이 프로그램을 output(?)할 때, 플러그인은 2번이나 bundled 될 것임. (이게 뭔소리야... 배포파일 만들 때 2번 불러온다는 건가) 아무튼 이렇게 중복이슈 발생하는데 이거 해결하려면 nuxt.confug.js의 build 프로퍼티에 vender라는 새로운 프로퍼티를 추가하면 됨. vendor에는 플러그인 이름이 들어감.

# Refs
https://nuxtjs.org/guide/
https://medium.com/@onlykiosk/the-complete-nuxt-guide-940751e1a6a5
https://ssr.vuejs.org/