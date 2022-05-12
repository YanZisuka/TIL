# Vue.js_03

## Vuex

-   "Statement management pattern + Library" for vue.js
    -   상태 관리 패턴 + 라이브러리
-   상태(state)를 전역 저장소로 관리할 수 있도록 지원하는 라이브러리
    -   상태가 예측 가능한 방식으로만 변경될 수 있도록 보장하는 규칙 설정
    -   애플리케이션의 모든 컴포넌트에 대한 **중앙 집중식 저장소** 역할
-   Vue의 공식 devtools와 통합되어 기타 고급 기능을 제공



>   State

-   state는 곧 data이며 해당 어플리케이션의 핵심이 되는 요소
-   중앙에서 관리하는 모든 상태 정보



>   상태 관리 패턴

-   컴포넌트의 공유된 상태를 추출하고 이를 전역에서 관리하도록 함
-   컴포넌트는 커다란 view가 되며 모든 컴포넌트는 트리에 상관없이 상태에 액세스 하거나 동작을 트리거 할 수 있음
-   상태 관리 및 특정 규칙 적용과 관련된 개념을 정의하고 분리함으로써 코드의 구조와 유지 관리 기능 향상



>   기존 Pass props & Emit event

-   각 컴포넌트는 독립적으로 데이터를 관리
-   데이터는 단방향 흐름으로 부모 -> 자식 간의 전달만 가능하며 반대의 경우 이벤트를 트리거
-   [장점] 데이터의 흐름을 직관적으로 파악 가능
-   [단점] 컴포넌트 중첩이 깊어지는 경우 동위 관계의 컴포넌트로의 데이터 전달이 불편해짐
-   단방향 데이터 흐름
    -   state는 앱을 작동하는 원본 소스 **(data)**
    -   view는 state의 선언적 매핑
    -   action은 view에서 사용자 입력에 대해 반응적으로 state를 바꾸는 방법 **(methods)**



>   Vuex management pattern

-   중앙 저장소(store)에 state를 모아놓고 관리
-   규모가 큰 (컴포넌트 중첩이 깊은) 프로젝트에서 매우 효율적
-   각 컴포넌트에서는 중앙 집중 저장소의 state만 신경쓰면 됨
    -   동일한 state를 공유하는 다른 컴포넌트들도 동기화 됨



## Vuex Core Concepts

![image-20220511092441871](vuejs_03.assets/image-20220511092441871.png)

```javascript
import Vue from 'vue'
import Vuex from 'vuex'

import _ from 'lodash'
import axios from 'axios'

const API_KEY = process.env.VUE_APP_YOUTUBE_API_KEY
const API_URL = 'https://www.googleapis.com/youtube/v3/search'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    query: '',
    videos: [],
    selectedVideo: {},
  },
  getters: {
    isVideos: state => !!state.videos.length,
    isSelectedVideo: state => !_.isEmpty(state.selectedVideo),
    videoSrc: state => {
      const videoId = state.selectedVideo.id?.videoId
      return `https://www.youtube.com/embed/${videoId}`
    },
  },
  mutations: {
    SET_QUREY: (state, query) => state.query = query,
    SET_VIDEOS: (state, videos) => state.videos = videos,
    SET_SELECTED_VIDEO: (state, video) => state.selectedVideo = video,
  },
  actions: {
    setQuery({ commit, dispatch }, query) {
      commit('SET_QUREY', query)
      dispatch('fetchVideos')
    },
    fetchVideos({ state, commit }) {
      const params = {
        key: API_KEY,
        part: 'snippet',
        type: 'video',
        q: state.query,
      }

      axios.get(API_URL, { params })
        .then(res => commit('SET_VIDEOS', res.data.items))
        .catch(err => console.error(err))

      },
    setSelectedVideo({ commit }, video) {
      commit('SET_SELECTED_VIDEO', video)
    },
  },
  modules: {
  }
})

```



>   State

-   "중앙에서 관리하는 모든 상태 정보 (data)"
    -   Vuex는 single state tree를 사용
    -   즉, 이 단일 객체는 모든 애플리케이션 상태를 포함하는 "원본 소스(single source of truth)"의 역할을 함
    -   이는 각 애플리케이션마다 하나의 저장소만 갖게 된다는 것을 의미함



-   여러 컴포넌트 내부에 있는 특정 state를 중앙에서 관리하게 됨
    -   이전의 방식은 state를 찾기 위해 각 컴포넌트를 직접 확인해야 했음
    -   Vuex를 활용하는 방식은 Vuex Store에서 각 컴포넌트에서 사용하는 state를 한 눈에 파악 가능



-   State가 변화하면 해당 state를 공유하는 여러 컴포넌트의 DOM은 알아서 렌더링됨
-   각 컴포넌트는 이제 Vuex Store에서 state 정보를 가져와 사용



>   Mutations

-   "실제로 state를 변경하는 유일한 방법"
-   Mutation의 handler는 반드시 동기적이어야 함
    -   비동기적 로직(ex. 콜백함수)은 state가 변화하는 시점이 의도한 것과 달라질 수 있으며, 콜백이 실제로 호출될 시기를 알 수 있는 방법이 없음 (추적할 수 없음)



-   첫번째 인자로 항상 **state를** 받음
-   Actions에서 `commit()` 메서드에 의해 호출됨



>   Actions

-   Mutations와 유사하지만 다음과 같은 차이점이 있음
    1.   state를 변경하는 대신 mutations를 `commit()` 메서드로 호출해서 실행
    2.   mutations와 달리 비동기 작업이 포함될 수 있음 (Backend API와 통신하여 Data Fetching 등 작업 수행)



-   **context** 객체 인자를 받음
    -   context 객체를 통해 store/index.js 파일 내에 있는 모든 요소의 속성 접근 & 메서드 호출이 가능
    -   **단, state를 직접 변경하지 않음** (가능하지만)



-   컴포넌트에서 `dispatch()` 메서드에 의해 호출됨



>   Getters

-   state를 변경하지 않고 활용하여 계산을 수행 (computed 속성과 유사)
    -   compute를 사용하는 것처럼 getters는 저장소의 상태(state)를 기준으로 계산
    -   e.g., state에 todoList라는 해야 할 일의 목록의 경우 완료된 todo 목록만을 필터링해서 출력해야 하는 경우



-   computed 속성과 마찬가지로 getters의 결과는 state 종속성에 따라 캐시(cached)되고, 종속성이 변경된 경우에만 다시 재계산됨
-   getters 자체가 state를 변경하지는 않음
    -   state를 특정한 조건에 따라 구분(계산)만 함
    -   즉, 계산된 값을 가져옴



## 시작하기

```
$ vue create [app_name]
```

```
$ vue add vuex
```



## Component Binding Helper

-   `mapState`

    -   computed와 Store의 state를 매핑

    -   Vuex Store의 하위 구조를 반환하여 component 옵션을 생성함

    -   매핑된 computed 이름이 state 이름과 같을 때 문자열 배열을 전달할 수 있음

    -   하지만 다른 computed 값을 함께 사용할 수 없기 때문에 최종 객체를 computed에 전달할 수 있도록 다음과 같이 Object Spread Operator로 객체를 복사하여 작성

        ```javascript
        computed: {
            ...mapState([
                'todos'
            ])
        }
        ```

    -   `mapState()`는 객체를 반환함

-   `mapGetters`

    -   computed와 Getters를 매핑
    -   getters를 Object Spread Operator로 계산하여 추가
    -   해당 컴포넌트 내에서 매핑하고자 하는 이름이 `index.js`에 정의한 getters의 이름과 동일하면 배열의 형태로 해당 이름만 문자열로 추가

-   `mapActions`

    -   actions를 Object Spread Operator로 전개하여 추가

    -   [주의] mapActions를 사용하면, 이전에 `dispatch()`를 사용했을 때 payload로 넘겨줬던 `this.todo`를 pass prop으로 변경해서 전달해야 함

        ```html
        <template>
          <div>
            <span
              @click="updateSomething(some)"
            ></span>
          </div>
        </template>
        ```

        

-   `mapMutations`



## Local Storage

>   vuex-persistedstate

-   Vuex state를 자동으로 브라우저의 Local Storage에 저장해주는 라이브러리 중 하나
-   페이지가 새로고침 되어도 Vuex state를 유지시킴

```
$ npm i vuex-persistedstate
```

```javascript
// index.js

import createPersistedState from 'vuex-persistedstate'

export default new Vuex.Store({
    plugins: [
        createPersistedState(),
    ],
    ...
})
```

