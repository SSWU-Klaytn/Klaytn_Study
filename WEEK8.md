# Klaytn-study-8

## 목표
기부형 크라우드 펀딩 BApp : 합치기

## DApp 주제
기부형 크라우드 펀딩 BApp

## 개발 환경

- 인프런의 "Klaytn 클레이튼 블록체인 어플리케이션 만들기 - 이론과 실습" 강의 BApp 개발환경과 같다.
  - [Klaytn 클레이튼 블록체인 어플리케이션 만들기 - 이론과 실습](https://www.inflearn.com/course/%ED%81%B4%EB%A0%88%EC%9D%B4%ED%8A%BC#)
  - 웹 : HTML, css, javascript
  
## 합치기

### 문제 상황

1. 이미지가 로드되지 않았다.
<img width="904" alt="10" src="https://user-images.githubusercontent.com/53432869/72123199-d259ae80-33a3-11ea-92e0-5c3ffdb4b6bf.PNG">

2. 페이지 이동이 불가능하였다. 
<img width="668" alt="13" src="https://user-images.githubusercontent.com/53432869/72123203-d4bc0880-33a3-11ea-9f03-553dcc2d59e4.PNG">

- server.js를 추가하였다. 

```
var http = require('http');
var fs = require('fs');
var url = require('url');
var mime = require('mime');

http.createServer( (request, response) => {
    // App.start();

    require('./src/views/logo.png');
    require('./src/views/img/project01.jpg');
    require('./src/views/img/project02.jpg');
    require('./src/views/img/project03.jpg');
    require('./src/views/img/project04.jpg');
    require('./src/views/img/project05.jpg');
    require('./src/views/img/project06.jpg');
    require('./src/views/img/project07.jpg');
    require('./src/views/img/project08.jpg');
    require('./src/views/img/project09.jpg');

    var pathname = url.parse(request.url).pathname

    console.log('pathname: ' + pathname)

      // pathname으로 router 분기함 
    if (pathname == '/') {
        pathname = '/views/index.html'
    } else if (pathname == '/mypage.html') {
        pathname = '/views/mypage.html'
    } else if (pathname == '/AllProjects.html') {
        pathname = '/views/AllProjects.html'
    } else if (pathname == '/PopulerProjects.html') {
        pathname = '/views/PopulerProjects.html'
    } else if (pathname == 'projectstory.html') {
        pathname = '/view/projectstory.html'
    } else if (pathname == 'myproject.html') {
        pathname = '/view/myproject.html'
    } else if (pathname == 'investment.html') {
        pathname = '/view/investment.html'
    } else if (pathname == 'projectstory.html') {
        pathname = '/view/projectstory.html'
    } else if(pathname == 'projectcommunity.html') {
        pathname = '/view/projectcommunity.html'
    } else if(pathname == 'projectinform.html') {
        pathname = '/view/projectinform.html'
    }

    fs.readFile(pathname.substr(1), (err, data) => {
        if (err) {
            console.log(err)
            response.writeHead(404, {'Content-Type': 'text/html'})
        } else {
            response.writeHead(200, {'Content-Type': 'text/html'})
            response.write(data.toString())
        }
        response.end()
    });

    fs.readFile('.\img\logo.png', function(error, data) {
        response.writeHead(200, { 'Content-Type': 'image/png'});
        response.write(data);
        response.end();
    });


}).listen(8080)
```

3. 이미지와 페이지는 정상적으로 동작하였지만 자바스크립트가 동작하지 않는 문제 발생

<img width="900" alt="11" src="https://user-images.githubusercontent.com/53432869/72123466-b0146080-33a4-11ea-81ea-b4170f6fe43e.PNG">
<img width="700" alt="12" src="https://user-images.githubusercontent.com/53432869/72123472-b4d91480-33a4-11ea-92ab-d6c19c5eb2de.PNG">

- 시행착오를 반복하다 처음으로 돌아와 webpack.config.js 파일을 수정해주었다.

 ```
plugins: [   
    new webpack.DefinePlugin({
      DEPLOYED_ADDRESS: JSON.stringify(fs.readFileSync('deployedAddress', 'utf8').replace(/\n|\r/g, "")),
      DEPLOYED_ABI: fs.existsSync('deployedABI') && fs.readFileSync('deployedABI', 'utf8'),
    }),
    new CopyWebpackPlugin([{ from: "./src/index.html", to: "index.html"}]),
    
    // 추가
    new CopyWebpackPlugin([{ from: "./src/AllProjects.html", to: "AllProjects.html"}]),
    new CopyWebpackPlugin([{ from: "./src/investment.html", to: "investment.html"}]),
    new CopyWebpackPlugin([{ from: "./src/mypage.html", to: "mypage.html"}]),
    new CopyWebpackPlugin([{ from: "./src/myproject.html", to: "myproject.html"}]),
    new CopyWebpackPlugin([{ from: "./src/PopulerProjects.html", to: "PopulerProjects.html"}]),
    new CopyWebpackPlugin([{ from: "./src/projectcommunity.html", to: "projectcommunity.html"}]),
    new CopyWebpackPlugin([{ from: "./src/projectinform.html", to: "projectinform.html"}]),
    new CopyWebpackPlugin([{ from: "./src/projectstory.html", to: "projectstory.html"}]),
    new CopyWebpackPlugin([{ from: "./src/img/logo.png", to: "logo.png"}]),
    new CopyWebpackPlugin([{ from: "./src/img/project01.jpg", to: "project01.jpg"}]),
    new CopyWebpackPlugin([{ from: "./src/img/project02.jpg", to: "project02.jpg"}]),
    new CopyWebpackPlugin([{ from: "./src/img/project03.jpg", to: "project03.jpg"}]),
    new CopyWebpackPlugin([{ from: "./src/img/project04.jpg", to: "project04.jpg"}]),
    new CopyWebpackPlugin([{ from: "./src/img/project05.jpg", to: "project05.jpg"}]),
    new CopyWebpackPlugin([{ from: "./src/img/project06.jpg", to: "project06.jpg"}]),
    new CopyWebpackPlugin([{ from: "./src/img/project07.jpg", to: "project07.jpg"}]),
    new CopyWebpackPlugin([{ from: "./src/img/project08.jpg", to: "project08.jpg"}]),
    new CopyWebpackPlugin([{ from: "./src/img/project09.jpg", to: "project09.jpg"}])
  ],
```

## 완성
[togetherwegetall](https://github.com/SSWU-Klaytn/togetherwegetall)
