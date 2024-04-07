---
title: 'mac M1 에서 MongoDB setup 하기 (homebrew 이용)'
categories: setup
tag: ['MongoDB','homebrew']
---

# mac M1 에서 mongodb setup 하기

## 1. homebrew 설치  
homebrew를 통해 필요한 것들을 설치 할 것이기 떄문에, 우선 homebrew를 설치한다.  
mac의 iterm을 켜서 아래와 같이 입력 후 homebrew를 설치한다.

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

만약 안된다면 아래 사이트로 들어가서 설치하자.  
[homebrew 사이트 접속](https://brew.sh/)


## 2. MongoDB setup
homebrew 설치가 완료되었다면 아래와 같이 차례대로 iterm 에 입력한다.
```bash
brew tap mongodb/brew
brew install mongodb-community@5.0
```
여기서 @5.0은 MongoDB version 에 대한 것으로 원하는 버전을 입력하면 된다.


## 3. MongoDB Service 
MongoDB 가 다 설치가 되었더라도 DB가 실행되고 있는것은 아니다.   
Background 에서 서비스가 시작되게 아래와 같이 입력한다.
```bash
brew services start mongodb/brew/mongodb-community@5.0
```


## 4. MongoDB Shell
설치가 완료되면 terminal 에 mongo라고 입력한다.
```bash
mongo
```


## 5. mongosh setup
mongosh를 설치해서 좀 더 편의성을 더한다.  
4번 과정이 안되더라도 mongosh 를 설치하고 나서 mongosh 접속이 가능할 수 있으니 mongosh 를 설치해본다.
```bash
brew install mongosh
```

## 6. mongosh 실행
terminal에 mongosh 를 쳐서 mongo shell에 접속이 성공하면 setup이 끝난다.
```bash
mongosh
```