---
layout: post
title: React
category: Frontend
tag: react
---

# React 

## Install 

* install nodejs 

    linux 또는 MacOS인 경우, [nvm](https://github.com/nvm-sh/nvm)에서 nvm을 설치하면 node 버전에 따른 관리가 가능하다. 
    ```
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash

    export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
    [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm

    nvm install --lts
    ```

* install yarn

    nodejs를 설치하면, node module을 관리하는 `npm`이 설치된다. </br>
    프로젝트에서 사용되는 라이브러리들의 버전을 관리하는 역할을 하며, `yarn`은 `npm`의 개선된 버전이라고 보면 된다. </br>

    https://classic.yarnpkg.com/en/docs/install#debian-stable

    ```
    npm install --global yarn
    ```

## Build a project

* create-react-app
    ```
    npx create-react-app [project name]
    ```

    * to execute 
        ```
        yarn start
        ```
    
    



refer:
https://react-anyone.vlpt.us/

* Virtual Dom

* Webpack

* Babel
    is a javascript compiler

* [codesandbox](https://codesandbox.io/s/4r6lqrlvj9)

* JSX
    * 태그는 꼭 닫아주어야 한다.
    * 두 개 이상의 엘리먼트는 무조건 하나의 엘리먼트에 감싸져 있어야 한다. (혹은 Fragment 사용)
    * JSX 안에는 javascript 값을 `{}`로 사용할수 있다. 
        * 변수 선언에 있어서 `var`는 함수 단위의 scope를 가진다. (react에서는 더 이상 사용하지 않음!!!)
        * 변수 선언에 있어서 `let`과 `cosnt`는 block단위로 scope를 가진다. 
            * `let`: 유동적인 값
            * `const`: 한 번 선언 후 고정적인 값

    * 렌더링을 하는 데에 있어서 조건부를 직접적으로 사용할 수 있다. 

    * 주석: `{}`로 `//` 또는 `/* ... */`을 감싸주어야 한다. 

* Props & State: 데이터를 다룰 때 쓰는 기능
    * Props: 주로 부모 component에서 자식 component로 값을 전달할 때 사용 (자식 입장에서는 읽기 전용)

    * State: Component 내부의 변수를 바꿀 때 사용하며, `setState()`를 사용한다. 

* LifeCycle API: 생명주기
    * Mounting
    * Updating
    * Unmounting

    <img src='/assets/react_intro/lifecycle.png'>

