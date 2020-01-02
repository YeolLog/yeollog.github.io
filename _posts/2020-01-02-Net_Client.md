---
title:  "[Network] 네트워크 프로그래밍 기본 (클라이언트 소켓)"
excerpt: "Socket"
toc: true
toc_sticky: true
header:
  teaser: "/assets/images/200101_Network/tcp,ip_socket.jpg"

categories:
  - Study
tags:
  - Network
last_modified_at: 2020-01-02T10:18:00

---

![열혈TCP/IP](https://yeollog.github.io/assets/images/200101_Network/tcp,ip_socket.jpg "열혈TCP/IP"){: .align-center}
소켓의 기본 및 서버소켓 코드 : [[Network] 네트워크 프로그래밍 기본(서버 소켓)](https://yeollog.github.io/study/Net_Server/)

## 클라이언트 소켓
서버 소켓이 있다면 역시 클라이언트 소켓 또한 존재한다.  
클라이언트 소켓 또한 서버 소켓처럼 socket() 함수를 호출함으로써 소켓을 생성한다.  

다만 클라이언트 소켓은 연결을 요청하는 것이 필요할 뿐이다.  
연결 요청 가능 및 수락은 서버 소켓에서 할 일이지, 클라이언트 소켓에서는 필요하지 않다.  
또 서버의 IP와 PORT가 필요할 뿐, 그 서버의 IP와 PORT는 인자로 받아내면 되기 때문에
정리하자면 bind(), listen(), accept() 함수는 필요하지 않는 것이다.  

그리고 연결을 요청하기 위한 다른 함수를 사용한다.

### 연결을 요청

```c
#include <stdio.h>
int connect(int sockfd, struct sockaddr *serv_addr, socklen_t addrlen);
/* 성공 시 : 0 반환
   실패 시 : -1 반환 */
```

sockfd는 마찬가지로 socket() 함수를 호출 했을 때 반환 받은 소켓 디스크립터 번호고,
서버 소켓의 주소 구조체를 매개변수로 필요하다.  

클라이언트 소켓은 서버 소켓과는 달리 매우 간단히 구현할 수 있다.  

## 정리
 + 연결을 요청하는 소켓의 생성과정
  1. 소켓의 생성 : socket() 함수 호출
  2. 연결을 요청 : connect() 함수 호출

## 클라이언트 소켓 코드
**hello_client.c**
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <arpa/inet.h>
#include <sys/socket.h>

void error_handling(char *message);   // 에러 발생 시 호출

int main(int argc, char *argv[]) {
  int sock;
  struct sockaddr_in serv_addr;
  char message[30];
  int str_len;

  if (argc!=3) {
	 printf("Usage : %s <IP> <port>\n", argv[0]);
    exit(1);
  }     // 클라이언트 파일 실행 시 : ./<파일명> <IP주소> <포트번호>

  sock=socket(PF_INET, SOCK_STREAM, 0);    // socket 함수 호출
	
  if(sock == -1)
	  error_handling("socket() error");      // 실패 시 -1 반환

  memset(&serv_addr, 0, sizeof(serv_addr));
  serv_addr.sin_family=AF_INET;
  serv_addr.sin_addr.s_addr=inet_addr(argv[1]);       // IP 인자 값
  serv_addr.sin_port=htons(atoi(argv[2]));            // PORT 인자 값

  if(connect(sock, (struct sockaddr*)&serv_addr, sizeof(serv_addr))==-1)      // connect 함수 호출
	  error_handling("connect() error!");
	
  str_len=read(sock, message, sizeof(message)-1);
    
	if(str_len==-1)
	  error_handling("read() error!");
	
  printf("Message from server: %s %d \n", message,str_len);
  close(sock);          // 소켓 닫음
  return 0;
}

void error_handling(char *message) {
    fputs(message, stderr);
    fputc('\n', stderr);
    exit(1);
}
```

보기에도 서버 소켓에 비해 간단하다는 것을 알 수 있다.
마찬가지로 클라이언트 소켓에 대한 세부적인 사항은 이후에 더 알아보자.

## 서버 소켓과 클라이언트 소켓 예제 실행 결과

__hello_server.c__
```
$ gcc hello_server.c -o hello_server    // 서버 c 파일 컴파일
$ ./hello_server 9090                   // port번호 9090
```
__hello_client.c__
```
$ gcc hello_client.c -o hello_client    // 클라이언트 c 파일 컴파일
$ ./hello_client 127.0.0.1 9090         // ip주소(서버) 127.0.0.1, port번호 9090
                                        // 127.0.0.1 은 실행하는 로컬 컴퓨터.
                                        // 즉, 자신의 컴퓨터로 루프백하는 것이다.
Message from server: Hello World!       // 서버 소켓 코드에 char message[] = "Hello World!" 가 출력되었다.
```