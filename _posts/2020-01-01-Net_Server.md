---
title:  "[Network] 네트워크 프로그래밍 기본 (서버 소켓)"
excerpt: "Socket"
toc: true
toc_sticky: true
header:
  teaser: "/assets/images/200101_Network/tcp,ip_socket.jpg"

categories:
  - Study
tags:
  - Network
last_modified_at: 2020-01-01T08:20:20

---

![열혈TCP/IP](https://yeollog.github.io/assets/images/200101_Network/tcp,ip_socket.jpg "열혈TCP/IP"){: .align-center}

## 네트워크 프로그래밍이란?
 - 네트워크로 연결된 둘 이상의 컴퓨터 사이에서의 데이터 송 수신 프로그램의 작성.

## 소켓이란?
 - 네트워크(인터넷)의 연결 도구
 - 운영체제에 의해 제공되는 소프트웨어적 장치

 쉽게 말해서, 통신을 하기 위한 프로그램 중 하나.
 
### 소켓의 생성
 
 ```c
 #include <sys/socket.h>
 int socket(int domain, int type, int protocol);
 /* 성공 시 : 파일 디스크립터 번호 반환
    실패 시 : -1 반환 */
 ```
 
 소켓은 socket() 함수의 호출을 통해서 생성한다.
 domain, type, protocol에 대해서는 이 후에 더 알아보자.

### 소켓의 주소 할당 및 연결
 
 ```c
 #include <sys/socket.h>
 int bind(int sockfd, struct sockaddr *myaddr, socklen_t addrlen);
 /* 성공 시 : 0 반환
    실패 시 : -1 반환 */
 ```

 bind() 함수로 IP와 PORT를 할당 할 수 있다.  
 sockfd에 대해서 눈치가 빠른 사람은 알 수 있겠지만, 바로 위 socket() 함수에서 반환 받은 파일 디스크립터 번호다.  

 마찬가지로 나머지 매개변수에 대해서는 이후에 더 알아보자.

### 연결 요청이 가능한 상태

 ```c
 #include <sys/socket.h>
 int listen(int sockfd, int backlog);
 /* 성공 시 : 0 반환
    실패 시 : -1 반환 */
 ```

 listen() 함수를 통해 클라이언트로부터 연결 요청이 가능한 상태가 된다.  
 마찬가지로 sockfd는 소켓의 파일 디스크립터 번호고, backlog는 연결 대기 줄의 갯수라고 생각하면 된다.

### 연결 요청에 대한 수락

 ```c
 #include <sys/socket.h>
 int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);
 /* 성공 시 : 파일 디스크립터 번호 반환
    실패 시 : -1 반환 */
 ```

 listen() 함수에서 대기 중인 연결 요청을 수락하는 accept() 함수이다.  
 accept() 함수 호출이 성공하게 된 후 데이터 송수신이 가능해진다.  
 수락된 이후에 데이터 송수신은 양방향으로 가능하다.

## 정리
 + 연결 요청을 허용하는 소켓의 생성 과정
  1. 소켓의 생성 : socket() 함수 호출
  2. IP/PORT 할당 : bind() 함수 호출
  3. 연결 요청 가능 상태 : listen() 함수 호출
  4. 연결 요청에 대한 수락 : accept() 함수 호출

## 서버 소켓 코드
 **hello_server.c**
 
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <arpa/inet.h>
#include <sys/socket.h>

void error_handling(char *message);   // 에러 발생 시 호출

int main(int argc, char *argv[]) {
  int svr_sock;
  int clt_sock;

  struct sockaddr_in svr_adr, clt_adr;
  socklen_t clt_adr_size;

  char message[] = "Hello World!";

  if(argc != 2) {
    printf("Usage : %s <PORT>\n", argv[0]);
    exit(1);
  }     // 서버 파일 실행 시 : ./<파일명> <포트번호>

  svr_sock = socket(PF_INET, SOCK_STREAM, 0); // socket 함수 호출
  if(svr_sock == -1)
    error_handling("socket() error!");        // 실패 시 -1 반환
  
  memset(&svr_adr, 0, sizeof(svr_adr));
  svr_adr.sin_family=AF_INET;
  svr_adr.sin_addr.s_addr=htonl(INADDR_ANY);
  svr_adr.sin_port=htons(atoi(argv[1]));

  if(bind(svr_sock, (struct sockaddr*)&svr_adr, sizeof(svr_adr)) == -1)       // bind 함수 호출
    error_handling("bind() error!");
  
  if(listen(svr_sock, 5) == -1)               // 대기 5
    error_handling("listen() error!");
  
  clt_adr_size=sizeof(clt_adr);

  clt_sock=accept(svr_sock, (struct sockaddr*)&clt_adr, &clt_adr_size);       // accept 함수 호출
  if(clt_sock == -1)
    error_handling("accept() error!");

  write(clt_sock, message, sizeof(message));

  close(clt_sock);        // 클라이언트 소켓 닫음
  close(svr_sock);        // 서버 소켓 닫음
  return 0;
}

void error_handling(char *message) {
  fputs(message, stderr);
  fputc('\n', stderr);
  exit(1);
}
```

서버 소켓의 코드 중 가장 기본적인 코드이다.  
더욱 자세한 내용은 이후에 설명하도록 하겠다.  
클라이언트 소켓 코드도 다음 게시물을 통해 설명하겠다.