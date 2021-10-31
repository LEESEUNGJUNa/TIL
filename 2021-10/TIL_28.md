# Today I Learn

이제 예외처리와 , 로그를 배웠으니까 실전에서 사용하기로 했다.
```java

public class ContapException extends RuntimeException {
    // 필요에 따라서 변수를 추가해주세요 !
    private final ErrorCode errorCode;
    private Object data = "";

    public ContapException(ErrorCode errorCode) {
        this.errorCode = errorCode;
    }

    public ContapException(ErrorCode errorCode,Object data) {
        this.errorCode = errorCode;
        this.data = data;
    }
}

```
값이 이상한 요청이 있을때 상황에맞는 message를 저장한후 클라이언트로 반환하는 예외를 정의해줬다.
message는 ErrorCode에 들어있다.
```java
@ExceptionHandler(value = { Exception.class })
    public ResponseEntity<Object> handleApiRequestException(
            HttpServletRequest request,
            Exception ex) throws Exception {
        RestApiException restApiException = new RestApiException();
        restApiException.setResult("fail");

        if (ex instanceof ContapException) // 처리한 예외이면
        {
            restApiException.setHttpStatus(HttpStatus.OK);
            restApiException.setErrorMessage(((ContapException) ex).getErrorCode().getMessage());
        } else // 아니면
        {
            AddLog.addExLog(request);
            restApiException.setHttpStatus(HttpStatus.INTERNAL_SERVER_ERROR);
            restApiException.setErrorMessage("서버에 문의해주세요");
        }
        return new ResponseEntity(
                restApiException,
                HttpStatus.OK
        );
    }
```
이런식으로 global exceptionhandler를 만들어줬는데, 개발자가 처리한 예외가 아닐경우에 로그를 남기도록 했다.
```java
public static void addExLog(
            HttpServletRequest request
            ) throws IOException {
        Logger log = LogManager.getLogger("AddExeption");
        StringBuilder sb = new StringBuilder();
        sb.append("[Method : ");
        sb.append(request.getMethod());
        sb.append("] [URI : ");
        sb.append(request.getRequestURI());
        sb.append("] [User : ");
        if(request.getUserPrincipal() != null)
        {
            sb.append(request.getUserPrincipal().getName() + "]  ");
        }
        else
        {
            sb.append("Null]  ");
        }

        Enumeration params = request.getParameterNames();
        while(params.hasMoreElements()) {
            String name = (String) params.nextElement();
            sb.append(name + " : " + request.getParameter(name) + "     ");
        }
        log.error(sb);
    }
```
이런식으로 누가 요청을보냈는지 , method는 무엇인지, 어느 uri인지 기록하도록했다.
아쉬운점은 Body에 있는 Json 데이터를 읽으려고하면 따로 처리가 필요하다고하는데 시간이없어서 일단 나중에 하기로했다.
여튼 서버 올려놓고 다음날 들어가서 확인해보니 로그가 잘남고있어서 뿌듯했다...

느낀점 - 일단은 생각한대로 잘된것같아서 기분이좋지만 이게 맞는방식인지 모르겠다. 

