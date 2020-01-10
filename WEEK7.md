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

- index.html

```
<div class="collapse bg-dark" id="collapsibleNavbar2" style="margin: auto; text-align: center;">
      <div class="container">
        <div class="navbar navbar-inverse">
        
          <!-- keystore file -->
          <div class="form-group" style="float: top; height: 105px; width: 300px;">
            <label for="keystore" style="color:orange; font-size: large; float: left; padding-bottom: 5%;">Keystore</label></br>
            <input type="file" class="form-control" id="keystore" style="height: 37px; color: black; background-color: white; font-size: small; float: left;" onchange="App.handleImport()">          
            <p class="help-block"style="color:orange; font-size: small; float: left;" ></p>
          </div>
          
          <!-- password -->
          <div class="form-group" style="float: top; height: 105px; width: 300px;">
            <label for="input-password" style="color:orange; font-size: large; float: left; padding-bottom: 5%;">비밀번호</label>
            <input type="password" class="form-control" id="input-password" style="color:orange; float: left; height: 37px;" onchange="App.handlePassword()" onKeypress="javascript:if(event.keyCode==13) {App.handleLogin()}"> 
            <p class="help-block" id="message" style="color:orange; font-size: small; float: left;" ></p>
          </div>
          
          <!-- submit -->
          <div class="form-group" style="height: 105px; width: 300px;">
            <button type="button" class="btn btn-default" id="submit" style="font-size: large; color:orange; " onclick="App.handleLogin()">제출</button>
            <button type="button" class="btn btn-default" data-dismiss="modal" style="font-size: large; color:orange;" >닫기</button>
          </div>
          
        </div>
      </div>
    </div>

```

- index.js
  - handleImport
  
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

- index.js
  - handlePassword

```
handlePassword: async function () {
    this.auth.password = event.target.value;
  }
```

- index.js
  - handleLogin

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

### 2. 마이페이지

![capture_mypage html](https://user-images.githubusercontent.com/53432869/72121344-1fd31d00-339e-11ea-852f-9520fe5aab58.png)

- mypage.html

```
<div class="media text-muted pt-3">
              <svg class="bd-placeholder-img mr-2 rounded" width="32" height="32" xmlns="http://www.w3.org/2000/svg" preserveAspectRatio="xMidYMid slice" focusable="false" role="img" aria-label="Placeholder: 32x32"><title>Placeholder</title><rect width="100%" height="100%" fill="#e83e8c"/><text x="50%" y="50%" fill="#e83e8c" dy=".3em">32x32</text></svg>
              <p class="media-body pb-3 mb-0 small lh-125 border-bottom border-gray">
                <!-- 계정 주소 -->
                <strong class="d-block text-gray-dark">당신의 계정 주소는</strong>
                <strong class="d-block text-gray-dark" id="address"></strong></p>
              </p>
            </div>

            <div class="media text-muted pt-3">
              <svg class="bd-placeholder-img mr-2 rounded" width="32" height="32" xmlns="http://www.w3.org/2000/svg" preserveAspectRatio="xMidYMid slice" focusable="false" role="img" aria-label="Placeholder: 32x32"><title>Placeholder</title><rect width="100%" height="100%" fill="#007bff"/><text x="50%" y="50%" fill="#007bff" dy=".3em">32x32</text></svg>
              <p class="media-body pb-3 mb-0 small lh-125 border-bottom border-gray">
                <!-- 지갑 잔액 -->
                <strong class="d-block text-gray-dark">당신의 잔액은</strong>
                <strong class="d-block text-gray-dark" id="walletBalance"></strong></p>
            </div>
            
 ```
 
- index.js

```
changeUI: async function (walletInstance) {

    // 계정 주소
    $('#address').append(walletInstance.address + ' 입니다.');
    
    // 지갑 잔액
    $('#walletBalance').append(await this.callBalance(walletInstance) + ' 입니다.');
    
  },
```

### 3. 후원 기능

<img width="634" alt="capture_projectstory html_후원" src="https://user-images.githubusercontent.com/53432869/72121604-e64ee180-339e-11ea-9590-986e7af0d3a3.png">

- projectstory.html

```
<!-- 총 후원액 확인>
<small>후원금액</small><br/>
<text id="contractBalance"></text><br/>
<hr/>

<!--후원 기능-->
<label for="ex_input">후원할 금액</label><br/>
<input type="text" id="amount" style="margin-bottom: 10px;" ><br/>
<button class="btn btn-sm btn-outline-secondary" onclick="App.deposit()">프로젝트 밀어주기</button>
```


- index.js

```
changeUI: async function (walletInstance) {

    // 컨트랙트 잔액
    $('#contractBalance').append(cav.utils.fromPeb(await this.callContractBalance(), "KLAY") + ' KLAY');
    
  },
  
```

```
deposit: async function () {
    const walletInstance = this.getWallet();

    if (walletInstance) {
        var amount = $('#amount').val();
        if (amount) {
          agContract.methods.deposit().send({
            from: walletInstance.address,
            gas: '250000',
            value: cav.utils.toPeb(amount, "KLAY")
          })        
          .once('transactionHash', (txHash) => {
            console.log(`txHash: ${txHash}`);
          })
          .once('receipt', (receipt) => {
            console.log(`(#${receipt.blockNumber})`, receipt); //Received receipt! It means your transaction(calling plus function) is in klaytn block                          
            alert(amount + " KLAY를 프로젝트에 후원하였습니다.");               
            location.reload();      
          })
          .once('error', (error) => {
            alert(error.message);
          }); 
        }
        return;    
    }
  },

```

4. 후원 확인

![screencapture-file-C-Users-jkc60-projects-c-src-investment-html-2020-01-10-01_08_55](https://user-images.githubusercontent.com/53432869/72121829-86a50600-339f-11ea-93c5-0642a71fb3e0.png)

-index.html

```
<td><a href="" id='invest'>후원 현황 확인하기</a></td>
```

- index.js

```
myurl: function (walletInstance) {
    var scp = "https://baobab.scope.klaytn.com/account/";
    console.log(typeof(walletInstance.address));
    // var account = walletInstance.address;
    var url = scp + walletInstance.address;
    return url;
  },

changeUI: async function (walletInstance) {

    $("#invest").attr("href", await this.myurl(walletInstance));
    
  },
```
