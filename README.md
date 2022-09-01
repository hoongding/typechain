# Typescript로 블록체인 만들기 - 실습

## ts로 프로젝트만들기

1. npm init -y 로 프로젝트일단 만들기
2. src 폴더 만들어주기
3. tsconfig.json 파일 만들기.
    - 이 파일이 있으면 우리가 TS로 작업한다는 것을 바로 알수있게끔함!
    - 아주 훌륭한 자동완성기능을 제공!
    - 이 json파일에서는 어디에 타입스크립트 파일이 위치하는지 알려줘야함!
    
    - **include
    의 배열에는 우리가 JS로 컴파일하고 싶은 모든 디렉토리를 넣어줌.**
    
    - **compilerOptions**
        
        
        → **outDir** : JS 파일이 생성될 디렉터리를 지정.
        
        - outDir에 build라고 써주고
        - package.json에 가서 script에 “build” : “tsc”라고 적어준다!
        
        → **target:** 어떤 버전의 JS로 TS를 컴파일 하고 싶은지 써줄 수 있다!
        
        - es3 : const 없고 var밖에 없음
        - es6 : Arrow Func도 있음! (이걸 추천! nodeJS와 브라우저가 es6 지원한다!)
        - 만약 서버만들때 옛날거로 만들고싶으면 호환되는 JS버전을 써줘야 할거임!
        
        → **lib**: 합쳐진 라이브러리의 정의 파일이 목표로 하는 실행환경을 나타낸다.
        
        - 나의 JS 코드가 **어디에서 동작할지를 알려준**다는 뜻.
        - JS의 어떤버전이 그 환경에서 사용되는지
        
        ```json
        "lib": ["ES6", "DOM"]
        ```
        
        → ES6를 지원하는 서버와 DOM(브라우저환경)에서 코드를 실행시킬것이다! 라는 뜻임.
        
        → DOM을 lib에 포함시켜두고 TS코드에서 document를 쓰면 브라우저에서 사용 가능한 모든 자동완성을 제공해준다! ( 브라우저의 API와 타입들을 알고있기 때문! )
        
        TS는 내장된 JS API를 위한 **기본적인 타입 정의는 가지고** 있다!
        
        - 타입정의 : TS가 JS 코드와 API의 타입을 설명할 수 있도록 해준다.
        - 타입정의는 TS를 사용하는 목적과 연관이 있다.
        - TS는 JS를 대체해서 쓰게 해주는데 JS에서 정의된 여러 라이브러리나 API를 쓰기위해선 TS가 알아들을 수 있도록 타입정의를 해줘야한다!
        
        **그래서 JS의 파일과 모듈을 위한 타입정의를 어떻게 작성하는지 알아보겠당!**
        
        - myPackage.**js** 파일을 만들어서 node_module 인 것처럼 사용할거임.
        - Github와 npm에 푸시해 둔것이고 이걸 우리가 설치했다고 가정!
        
        - strict모드 → 타입스크립트가 성가시게한다(보호해준다)
        - **정의파일** : JS 코드의 모양을 TS에게 설명해주는 파일이다!
        - d.ts → 정의파일!!
        - myPackage.d.ts라는 파일을 만들자!
        - 여기에서 myPackage에서 선언한 함수에 대한 호출 시그니처만 써주면 된다!
        - 호출 시그니처, 타입만 써주면 된다!
        
        만약 내가 npm 파일이나 프로젝트를 다운받았는데 타입이 없어서 TS가 오류를 내고있으면 .d.ts에 JS 라이브러리의 함수 모양을 설명해야한다.
        
        스스로 정의 파일을 써야할 일은 많지 않다!
        
        - tsconfig파일에 allowJs true해주고
        - ./myPackage → 앞에 ./ 를 붙여준다!
            - 이 타입스크립트 파일에 ./myPackage파일을 불러온다는 뜻.
            - 대신 .d.ts파일은 없어야한다
        
         만약 JS파일을 TS파일한테 확인하라고 알리고싶다. 즉 기존에 존재하던 JS파일에서 타입스크립트의 보호를 받게하고 싶으면 `// @ts-check` 라고 써주면 된다!
        
        타입스크립트를 이용하면 입력값이나 리턴값 모두 타입을 지정할 수 있다.  
        
        그리고 JSDoc을 사용한다!
        
        - JSDoc은 코멘트 코드이다.
        - TS가 코멘트 코드를 읽어서 타입을 확인해준다!
        - 이때 코드를 완전히 TS코드로 바꿀필요는 없다.
        
        ```tsx
        //@ts-check
        /**
         * Initializes the project
         * @param {object} config
         * @param {boolean} config.debug
         * @param {string} config.url
         * @returns {boolean}
         */
        export function init(config) {
          return true;
        }
        /**
         * Exits the project
         * @param {number} code
         * @returns {number}
         */
        export function exit(code) {
          return code + 1;
        }
        ```
        
        **npm i -D ts-node**
        
        → build없이 TS를 실행할 수 있게한다.
        
        → 빌드없이 빠르게 새로고침하고 싶을 때 사용.
        
        → ts-node가 컴파일할 필요없이 TS코드를 대신 실행해준다.
        
        **nodemon**
        
        → 자동으로 커맨드를 재실행해준다.
        
        → 서버를 재시작 시켜줄 필요없이 알아서해준다!
        
        <tsconfig>
        
        **module**
        
        → 브라우저 앱을 만들고 있다면 umd선택
        
        → nodeJS앱을 만들면 CommonJs
        
        **DefinitelyTyped**
        
        - 깃 레포임.
        - only 타입 정의로만 이루어져 있다.
        - npm에 존재하는 거의 모든 패키지들에 대해 타입 정의해주는 레포이다.
        
        - 만약 내가 쓰고 싶은 npm라이브러리가 있는데 type처리한 파일이 없다면(정의파일이 없다면)
        `npm i -D @types/받고싶은라이브러리`이렇게 쓰면됨.