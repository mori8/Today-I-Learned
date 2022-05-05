## Python requirements.txt 파일 생성과 패키지 설치
파일명을 꼭 `requirements.txt`로 할 필요는 없으나, 대부분의 파이썬 개발자가 이 이름을 사용하고 있으므로 따르는 것이 좋음 
```
$ pip freeze > requirements.txt
```

requirements.txt에 있는 패키지들을 한꺼번에 설치하기
```
$ pip install -r requirements.txt
```
