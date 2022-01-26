# Docker, NodeJs로 간단한 서버 띄우기
Docker 가 설치 되어 있다는 가정에서 진행.

## Docker Linux alpine Image Pull
```shell
docker pull alpine
```
alpine 버전이란 가볍고 간단한, 보한성을 목적으로 개발한 linux 배포판 입니다.
용량이 80M 인 초 경량화된 배포판이므로 Embbeded나 네트워크 서버 등 특정 용도에 적합하며
특히 도커에 채택 되어 5M 크기의 리눅스 이미지로 유명하다.

## Docker Image Run
```shell
docker run -it -p 3000:3000 --name node alpine
```

## Install
```shell
apk update
apk upgrade
apk add vim
apk add nodejs
apk add npm
npm install express-generator -g
```

## NodeJs Server Create
```shell
mkdir node
cd node
express app
cd app
npm install
```

## NodeJs Server Update
```shell
cd routes
vi index.js
```

## NodeJs Start
```shell
cd app
npm start
```

## Check
URL : http://localhost:3000