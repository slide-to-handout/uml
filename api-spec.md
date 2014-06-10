`POST /login`
-------------

요청 body: <https://developers.facebook.com/docs/graph-api/reference/v2.0/user>에서 주는 응답을 그대로.

응답:
```
200 OK
Content-Type: application/json
Set-Cookie: (사용자 세션 정보)

{"result": "ok"}
```



`GET /users/`
-------------

쿼리 인자:
- q: 닉네임 혹은 이름을 검색할 검색어

응답:
```
200 OK
Content-Type: application/json

{
    "result": [
        {
            "id": "(사용자의 unique ID)",
            "name": "(사용자의 이름)",
            "nickname": "(짧은 영문자로 된 사용자의 별명)"
        },
        ...
    ]
}
```

`GET /users/<user_id>/channels`
-------------------------------

응답:
```
200 OK
Content-Type: application/json

{
    "result": [
        {
            "id": "(채널의 unique ID)",
            "title": "(채널 제목)",
            "download_URL": "(PDF 파일을 다운받을 수 있는 URL)",
        },
        ...
    ]
}
```

`POST /channels/<channel_id>/join`
----------------------------------

쿼리 인자:
- user_id: 채널에 접속할 사용자의 ID

응답:
```
200 OK
Content-Type: application/json

{
    "result": {
        "session": "(세션 키)"
    }
}
```
