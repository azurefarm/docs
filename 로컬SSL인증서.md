로컬 SSL 인증서

외부 연동의 작업시 콜백 지정 URL이 Https 일 경우에는 로컬환경에서도 반드시 SSL 인증서 발급이 필요함

1.Chocolater란?
SSL인증서를 만들기 위해서는 리눅스 기반의 라이브러리 패키지 관라자가 필요합니다.
윈도우용 리눅스 라이브러리 패키지를 관리할 수 있는 기능 제공

2.Chocolatey 패키지 관리자
"PowerShell 관리자 모드" 에서 입력 및 설치

Set-ExecutionPolicy Bypass -Scope Process -Force;
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072;
iex ((New-Object System.Net.webClient).DownloadString('https://commnity.chocolatey.org/install.ps1'))

3.설치확인
Choco -v

4.MKCERT 라이브러리 설치
choco install mkcert

5.MKCERT CA 모듈 설치
mkcert -install

6.SSL 인증서 생성
mkcert localhost

localhost.pem
localhost-key.pem



로컬 프록시 설정
1.로컬 프록시 설정을 사용해야 하는 이유
CORS정책을 위회하기 위해서는 콜백(POST)처리가 필요한데 이를 API서버에서 하기 때문에 로컬 환경에서도 외부인증을 사용하기 위해서는 설정이 필요하다

2.프록시 패턴 추가
"/api" 접두사와 동일한 경우 해당 target, header 등의 프로토콜 설정을 스위칭이 가능

setupProxy.js

app.use(
 ['api']
 createProxyMiddleware({
   target: 'https://localhost:44370',
   changeOrigin: true,
   secure: false,
   headers: {
     'Acess-Control-Allow-Origin': '*',
     'Acess-Control-Allow-Credentials': '*',
     'Acess-Control-Allow-Methods': '*'
   }
  })
 );
 app.listen(8713);

3.블드 옵션
package.json 

"start": "set HTTPS=true&&set SSL_CRT=localhost&set SSL_KEY_FILE=localhost-key.pen&&react-scripts start"
