---
layout: post
title: "[실전] ReactJS로 웹 서비스 만들기"
categories:
  -	노마드코더
tags:
  -	ReactJS
---

### 리액트가 왜 핫하냐?

1.	자바스크립트 기반(별도의 다른 프레임워크를 배울 필요없어)
2.	Composition
3.	Unidirectional Dataflow (데이터가 변하면 UI가 변함, UI에서 데이터를 바꿀 수 없어.)
4.	리액트는 프레임워크가 아니라 UI라이브러리다.
5.	Model, View, Controller에서 View를 담당하기 때문에 Model과 Controller를 골라서 사용할 수 있다.

### 영화 평가 사이트를 만들어보자.

-	YTS이 API를 사용할 것이다. - 영화와 관련된 정보들을 긁어온다.(영화리스트, 추천, 별점등..)

### 트랜스파일러

-	우리가 사용하는 코드는 리액트 코드이기 때문에, 이걸 자바스크립트로 바꿔주는 도구.
-	우리가 사용할 것은 웹팩 - 리액트코드를 브라우저가 이해하는 언어로 바꿔준다.
-	다양한 파일들을 기본 .js .css .png로 바꿔준다.
-	이번 강의에서 자세히 다루지는 않는다. (인스타그램 풀스택 강의에서 다룸.)

#### 리액트로 작업하려면 웹팩과 같은 모듈번들러가 필요하다.

-	이 강의에서는 시간이 없기 때문에 그래서 페이스북에서 제공하는 create react app을 사용한다.

#### Create React app

-	웹팩과 같은 툴을 사용할 필요 없이 손쉽게 리액트 앱을 만들어주는 툴.
-	이번강의에서는 이것을 이용해 설정 작업없이 한번에 작업을 시작한다.
-	configuration을 하지 않아도 된다.

##### Install

-	오랜만에 node와 npm을 사용하다 보니 최신 버전으로 upgrade를 했는데 npm은 node 9버전에서는 동작하지 않았다.
-	그래서 n 모듈을 사용하여 8.9.1버전으로 다운그레이드 후 다시 create-react-app을 설치하였다.

```
  npm install -g create-react-app
```

#### 사용법

```
create-react-app movie_app
```

#### yarn start

-	이 명령어를 사용하기 전 yarn이 설치되어 있어야한다.
-	react-script를 실행 시키는 명령어

#### 리액트 프로젝트를 시작할때 마다 해야하는 것.

-	Design components!
-	무비리스트 - 무비 - 이미지 컴포넌트 순으로 분리.
-	리액트는 컴포넌트에 기반한다!

#### JSX

-	자바스크립트안에 html코드가 자리잡고 있는 것.
-	리액트 컴포넌트를 만들때 사용하는 언어.
-	css의 class가 아니라 className을 사용한다.

#### 모든 컴포넌트는 render function을 갖고 있다.

-	render의 기능은 '뭔가를 보여주는, 출력하는' 기능이다. - '이 컴포넌트가 나에게 보여주는 것이 무엇인가'

#### react vs reactDom

-	react 는 UI 라이브러리
-	리액트DOM은 리액트를 웹사이트에 출력(render)하는 걸 도와주는 모델. DOM, Document object model.
-	리액트를 사용해서 웹사이트에 올려놓고 싶다 -> reactDOM을 사용.
-	모바일 앱 -> reactNative

#### 컴포넌트 > render > return > JSX

-	모든 컴포넌트는 무조건 RENDER 해야 하고, RETURN 해야함.
-	JSX는 리액트로 작성하는 html이다.

#### 리액트의 주요 컨셉 2개 (state, props)

-	props - Data flow with Props

	-	부모 컴포넌트가 자식 컴포넌트에게 정보를 주는 것.
	-	타이틀, 영화 포스터 정보를 메인에 다 집어넣고, 그걸 각각 컴포넌트에 props를 이용해 정보를 출력하는 것.
	-	JSX의 경우 명령을 실행시키려면 괄호를 꼭 쳐야한다.
	-	dataflow : 큰 컴포넌트가 전체 정보를 갖고있고, 그리고 각기 칠드런에게 정보를 전달함. 이를 이용해 강력한 UI를 구축한다. 한개의 데이터 소스를 가지고 각 컴포넌트별로 출력한다.

	```
	  App.js
	  const movieTitles  = ["~", "~", "~", "~"]


	  const movieImages = ["~", "~", "~", "~"]


	  class App extends Component {
	    render() {
	      return (
	        <div className="App">
	          <Movie title={movieTitles[0]} poster={movieImages[0]} />
	          <Movie title={movieTitles[1]} poster={movieImages[1]} />
	          <Movie title={movieTitles[2]} poster={movieImages[2]} />
	          <Movie title={movieTitles[3]} poster={movieImages[3]} />
	        </div>
	      );
	    }
	  }


	  Movie.js
	  class Movie extends Component {
	      render() {
	          return (
	              <div>
	                  <MoviePoster poster={this.props.poster}/>
	                  <h1>{this.props.title}</h1>
	              </div>
	          )
	      }
	  }


	  class MoviePoster extends Component {
	      render() {
	          return(
	              <img src={this.props.poster} />
	          )
	      }
	  }
	```

#### .map 사용법

```
  const movies = [
  {
    title : "Matrix",
    poster: "https://upload.wikimedia.org/wikipedia/en/c/c1/The_Matrix_Poster.jpg"
  },
  {
    title: "Full Metal Jacket",
    poster: "https://upload.wikimedia.org/wikipedia/en/9/99/Full_Metal_Jacket_poster.jpg"
  },
  {
    title: "Oldboy",
    poster: "https://www.languagetrainers.com/reviews/foreign-film-reviews/uploads/9214-Oldboy.jpg"
  },
  {
    title: "Star Wars",
    poster: "http://starwarsblog.starwars.com/wp-content/uploads/sites/6/2017/10/the-last-jedi-theatrical-blog.jpg"
  }
  ]

  이런 데이터 배열을 새로운 배열로 만들어준다.

  {movies.map(movie => {
          return <Movie title={movie.title} poster={movie.poster} />
  })}

  이걸 통해 각각의 title과 poster이 담긴 새로운 배열이 생성된다.
```

#### PropTypes

-	자식 컴포넌트에서 부모 컴포넌트에게서 받는 정보를 체크 할 수 있다.
-	isRequire이라고 지정해서 만약 데이터가 없을 경우 메세지를 받을 수 있다.

#### Lifecycle Events on React

-	컴포넌트는 여러기능들을 정해진 순서대로 실행함.
	1.	Render: componentsWillMount() -> render() -> conponentDidMount()
	2.	Update: componentWillReceiveProps() -> shouldComponentUpdate() -> componentWillUpdate() -> render() -> compoenetDidUpdate()
-	컴포넌트가 '존재'하기 시작하면 위의 순서대로 작동한다. (willmount -> did render -> did mount)
-	영화앱과 같은 어플리케이션을 만든다면 willmount에서 api를 작업 요청하고 완료되면 다른 작업을 요청한다.
-	shouldComponentUpdate에서는 이전 컴포넌트와 현재 컴포넌트를 비교해서 차이점이 있으면 update를 실행한다.
-	컴포넌트는 많은 functions을 가지고 있고 그들은 순서대로, 자동으로 작동한다.

#### Thinking in React: Component State

-	state : 리액트 컴포넌트 안에 있는 오브젝트이다. functions 처럼 모든 컴포넌트 안에 들어 있음.
-	컴포넌트 안에 state가 바뀔 때 마다, render이 발생  
-	state를 직접 변경할려고 하면 위의 render 설정들이 작동을 안한다. 그래서 this.setState({})를 통해서 state를 변경한다.
-	this.setState({})를 할때 이전에 정보를 그대로 두고 정보를 추가만 하고 싶을 때 ...this.state.변수명, 을 선언하면 이전에 정보가 그대로 남아있다.
-	이것을 이용해서 페이지가 로딩될 때 스크롤을 아래로 내릴수록 더 많은 영화가 로딩되는 효과와 같은 infinite scroll을 구현할수 있어. (끝까지 스크롤하면 20개가 더 추가되는. 예를들면 페이스북 등등)

#### Loading States

-	필요한 데이터가 항상 바로 즉시 존재하지는 않다. 그래서 데이터 없이 컴포넌트가 로딩되고 데이터를 위해 API가 실행되고 API가 데이터를 넘겨주면 너의 컴포넌트가 state를 update한다.

```
    {this.state.movies ? this._renderMovies() : 'Loading'}
    3항연산자를 사용해서 구현한 LoadingStates
    this.state.movies가 있으면  this._renderMovies()를 실행 없으면 Loading을 출력하게 한다.
```

-	\_renderMovies에 \_가 붙은 이유는 리액트에는 내장함수가 많기 때문에 내가 직접 선언한 함수와 내장함수와의 차이점을 두기위해서 선언한다.

#### Smart vs Dumb Components

-	모든 컴포넌트가 state가 있는 것이 아니다. (stateless components)
-	Smart 컴포넌트는 state가 있고 Dumb 컴포넌트는 state가 없다.
-	Dumb 컴포넌트가 존재하는 이유는 그저 return만 하는 컴포넌트(functional Components)가 필요할 때 가 있어서다.

```
  function MoviePoster({poster}) {
    return (
        <img src={poster} alt="Movie Poster" />
    )
  }

  class가 아닌 function으로 선언
```

-	dumb만 갖고 있으면 render function, will mount, did mount 이런건 사용하지 않고 그저 html만 return 한다. 그리고 state를 잃게되고 업데이트하는 기능들이 다 없어 진다.

#### Ajax in React

-	Ajax: Asyncronous JavaScript And XML.
-	XML은 안쓰고 JSON포맷을 알아야한다.
-	JSON: JavaScript Object Notation.
-	오브젝트를 자바스크립트로 작성하는 기법.
-	리액트에 AJAX를 적용하는 방법은 간단하다. FETCH라는 것을 사용한다. fetch는 뭔갈 잡는걸 의미함.
-	fetch request를 만들어서 뭔가를 GET한다.

```
  componentDidMount() {
    fetch('https://yts.ag/api/v2/list_movies.json?sort_by=rating')    
  }
```

-	컴포넌트가 mount되면, 저 url을 가서 fetch 해온다.

#### Promises

-	자바스크립트 컨셉. asynchronous programming.
-	첫번째 라인이 다 끝나든 말든, 2번째 라인이 작업을 한다.
-	이것의 장점은 다른일을 계속 스케쥴해놓을 수 있기 때문.

```
fetch('https://yts.ag/api/v2/list_movies.json?sort_by=rating')
    .then(response => response.json())
    .then(json => console.log(json))
    .catch(err => console.log(err))
```

-	then을 사용해서 다음에 할일을 지정해 놓으면 완료될대까지 다른 일을 한다.

#### Async Await in React

-	Await, Async는 위의 라인들을 좀더 분명하게 작성해주는 도구.
-	위의 코드대로 선언하면 json에서 데이터를 처리하기위해 .then() .then() .then(.then())등 .then을 남용하게 되어 CallBack Hell에 빠지게 된다.

```
  _getMovies = async () => {
    const movies = await this._callApi()
    this.setState({
      movies
    })
  }
```

-	async를 작성하지 않으면 await가 작동하지 않는다.
-	fetch를 \_callApi()에 넣은 후 didMount()에는 \_getMovies를 선언해서 추가한다.
-	\_getMovies function는 asynchronous function이다.
-	await는 this.\_callApi() 작업이 완료되고 return 하라는 뜻이다. (실패를 하든 성공을 하든 Finish되고.)
-	컴포넌트의 key는 인덱스를 사용하는 것은 좋지 않다. 느리기때문!!..
