통신 프로토콜
==========

* 참조: <http://www.jsonrpc.org/specification>

  위 프로토콜 문서에서 기본적인 형식 등을 많이 빌려왔습니다.


연결
----

* 액터
  * 사용자(speaker or audience)
* 요약
  * 채널에 접속할 때 부여받았던 세션 키를 사용해 WebSocket 연결을 맺는다
* 사전 조건
  * 사용자는 접속할 채널의 세션 키를 가지고 있음
  * 사용자는 다른 채널에 접속중인 상태가 아님
* 사후 조건
  * 사용자는 서버와 WebSocket 연결을 유지한다

### 흐름

* 사용자 -> 서버 요청

  ```
  { "method": "connect"
  , "params": {"session": "<session key>"}
  , "id": <요청 번호>
  }
  ```

* 서버 -> 사용자 응답
  * 성공

    ```
    { "result": "ok"
    , "id": <받았던 요청 번호>
    }
    ```

  * 실패

    ```
    { "error": { ... }
    , "id": <받았던 요청 번호>
    }
    ```

    * 응답 직후 연결을 종료함

### 비고

* 올바른 사용자가 요청하는지를 전적으로 세션 키를 통해 판단하게 되기 때문에, 사용자가 로그아웃하게 되면 사용자가 가지고 있던 모든 세션 키를 파기(expire)해야 한다.

슬라이드 메타데이터 요청
------------------

* 액터
  * 사용자(speaker or audience)
* 요약
  * 슬라이드에 대한 정보를 사용자에게 전송한다
* 사전 조건
  * 사용자는 서버와 연결을 유지 중임
* 사후 조건
  * 사용자는 슬라이드에 대한 정보를 받는다

### 흐름

* 사용자 -> 서버 요청

  ```
  { "method": "get_slide"
  , "params": {}
  , "id": <요청 번호>
  }
  ```

* 서버 -> 사용자 응답

  ```
  { "result":
    { "download_URL": "<PDF 파일을 다운받을 수 있는 URL>"
    , "pages": <슬라이드의 총 페이지 수>
    }
  , "id": <요청 번호>
  }
  ```

페이지 이동
--------

* 액터
  * 발표자(speaker)
  * 청중(audience)
* 요약
  * 슬라이드에 대한 정보를 사용자에게 전송한다
* 사전 조건
  * 발표자는 서버와 연결을 유지 중임
* 사후 조건
  * 사용자는 슬라이드에 대한 정보를 받는다

### 흐름

* 발표자 -> 서버 요청

  ```
  { "method": "move_page"
  , "params":
    { "page": <이동하려는 페이지 번호>
    }
  , "id": <요청 번호>
  }
  ```

* 서버 -> 발표자 응답

  ```
  { "result":
    { "page": <이동한 페이지 번호>
    }
  , "id": <요청 번호>
  }
  ```

* 서버 -> 청중 알림

  ```
  { "method": "move_page"
  , "params":
    { "page": <이동한 페이지 번호>
    }
  }
  ```