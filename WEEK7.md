# Klaytn-study-7

## 목표
기부형 크라우드 펀딩 BApp : 기능 구현

## DApp 주제
기부형 크라우드 펀딩 BApp

## 개발 환경

- 인프런의 "Klaytn 클레이튼 블록체인 어플리케이션 만들기 - 이론과 실습" 강의 BApp 개발환경과 같다.
  - [Klaytn 클레이튼 블록체인 어플리케이션 만들기 - 이론과 실습](https://www.inflearn.com/course/%ED%81%B4%EB%A0%88%EC%9D%B4%ED%8A%BC#)
  - 웹 : HTML, css, javascript
  
## 기능 구현

### 1. 로그인
- key store file과 비밀번호를 통한 로그인 기능

<img width="900" alt="capture_login" src="https://user-images.githubusercontent.com/53432869/72120248-b00f6300-339a-11ea-881d-408208d2922d.png">

<img width="900" alt="capture_login2" src="https://user-images.githubusercontent.com/53432869/72120251-b30a5380-339a-11ea-9230-a0fea1c063eb.png">

```
<div class="collapse bg-dark" id="collapsibleNavbar2" style="margin: auto; text-align: center;">
      <div class="container">
        <div class="navbar navbar-inverse">
          <div class="form-group" style="float: top; height: 105px; width: 300px;">
            <label for="keystore" style="color:orange; font-size: large; float: left; padding-bottom: 5%;">Keystore</label></br>
            <input type="file" class="form-control" id="keystore" style="height: 37px; color: black; background-color: white; font-size: small; float: left;" onchange="App.handleImport()">          
            <p class="help-block"style="color:orange; font-size: small; float: left;" ></p>
          </div>
          <div class="form-group" style="float: top; height: 105px; width: 300px;">
            <label for="input-password" style="color:orange; font-size: large; float: left; padding-bottom: 5%;">비밀번호</label>
            <input type="password" class="form-control" id="input-password" style="color:orange; float: left; height: 37px;" onchange="App.handlePassword()" onKeypress="javascript:if(event.keyCode==13) {App.handleLogin()}"> 
            <p class="help-block" id="message" style="color:orange; font-size: small; float: left;" ></p>
          </div>
          <div class="form-group" style="height: 105px; width: 300px;">
            <button type="button" class="btn btn-default" id="submit" style="font-size: large; color:orange; " onclick="App.handleLogin()">제출</button>
            <button type="button" class="btn btn-default" data-dismiss="modal" style="font-size: large; color:orange;" >닫기</button>
          </div>
        </div>
      </div>
    </div>

```

```
handleImport: async function () {
    const fileReader = new FileReader();
    fileReader.readAsText(event.target.files[0]);
    fileReader.onload = (event) => {      
      try {     
        if (!this.checkValidKeystore(event.target.result)) {
          $('#message').text('유효하지 않은 keystore 파일입니다.');
          return;
        }    
        this.auth.keystore = event.target.result;
        $('#message').text('keystore 통과. 비밀번호를 입력하세요.');
        document.querySelector('#input-password').focus();    
      } catch (event) {
        $('#message').text('유효하지 않은 keystore 파일입니다.');
        return;
      }
    }   
  }
```


```
handlePassword: async function () {
    this.auth.password = event.target.value;
  }
```


```
handleLogin: async function () {
    if (this.auth.accessType === 'keystore') { 
      try {
        const privateKey = cav.klay.accounts.decrypt(this.auth.keystore, this.auth.password).privateKey;
        this.integrateWallet(privateKey);
      } catch (e) {      
        $('#message').text('비밀번호가 일치하지 않습니다.');
      }
    }
  }
```
