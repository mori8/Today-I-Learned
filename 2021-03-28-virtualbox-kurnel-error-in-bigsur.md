## [Big Sur]Virtualbox에서 OS 업그레이드 이후 “Kernel Driver Not Installed (rc=-1908)” 에러 해결 방법
### 문제 상황
- Catalina -> Big Sur 업그레이드 이후 Virtualbox에서 CentOS 실행하면 “Kernel Driver Not Installed (rc=-1908)” 에러 발생
- VirtualBox v.6.1.18(최신)
- 시스템 환경설정 > 보안 및 개인 정보 보호에서 Oracle에 권한 허용해주면 된다는데 아예 허용 버튼이 뜨지를 않음
- virtualbox를 재설치하면 첫 실행만 에러 없이 잘 되고 두번째 실행부터는 똑같은 에러 뜸

### 문제 해결 방법
virtualbox 최신 버전 설치하면서 **Extention Pack까지 새 버전으로 재설치** 해줘야 한다.
그럼 아무 일도 없었던 것처럼 잘 돌아간다.
