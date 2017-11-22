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

