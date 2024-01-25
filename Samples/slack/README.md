# 설정방법

[이 문서 참고했음](https://velog.io/@devand/%EC%8A%AC%EB%9E%99-Web-API-%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%B4%EB%B3%B4%EC%9E%90-1)

## 헤멘부분 
위 참고 문서에서는 permission을 chat:write, groups:write만 주면 된다고 되어 있지만 실제로 deploy를 해서 실행시켜보면 not_in_channel이라는 오류가 발생한다.

[이 문서](https://surprisecomputer.tistory.com/49)에서 말한 대로 chat:write.public이 있어야만 제대로 메세지가 전달이 됨.

