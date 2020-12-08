https://reoim.tistory.com/entry/%EB%AC%B8%EC%A0%9C%ED%95%B4%EA%B2%B0-FAIL-TO-LOAD-SELINUX-POLICY-FREEZING
### CentOS 부팅시 Fail to load selinux policy, freezing 에러 메세지
리눅스 시험을 21시간 남기고 버추얼박스에서 클립보드 공유 설정을 양방향으로 해둔 다음 재부팅했는데 저 메세지와 함께 켜지질 않았다.
기다리면 켜질 줄 알고 새벽 2시부터 새벽 6시까지 켜놨는데도 그대로길래 구글에 찾아봤더니 selinux 관련 문제였다.
CentOS 부팅시(버추얼박스 로고 뜨면서 운영체제 선택할 때) e 키를 누르고 linux16이 있는 줄에다가 selinux=0만 넣어주면 언제 그랬냐는듯 잘 돌아간다.

근데 클립보드 공유 설정 적용 안됐다. 진짜 화난다
