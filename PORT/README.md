## PORT



### 한 IP에서 둘 이상의 APP을 연결해야 하면은?

<img src="./images/network" alt="image-20211206140425707" style="zoom:75%;" /> 

IP의 목적 : 목적지 서버를 찾는다.

PORT의 목적 : 해당 목적지서버에서 어떤 APP인지를 찾는다.

<img src="./images/packet1" alt="image-20211206140846105" style="zoom:50%;" /> 

실제로는 이렇게 계층마다 한꺼풀 한꺼풀 캡슐화가 되지만, 이제는 그냥 편하게

<img src="./images/packet2" alt="image-20211206140914894" style="zoom:50%;" /> 

이렇게, TCP/IP패킷이라고 부르자!!

<img src="./images/port" alt="image-20211206141013644" style="zoom:85%;" /> 

웹브라우저 앱으로 요청을 보낼때는 IP : 200.200.200.3의 80port로 요청 메시지데이터를 보내는 것이고

그러면 목적지 서버에서는 요청에 관한 html을 만들어서 그 만든 데이터 결과를 응답을 할텐데 이때는  IP : 100.100.100.1의 10010port로 보내준다.



#### 근데 서버에서 어떻게 출발지의 port를 아는가?

출발지에서 처음에 요청패킷을 보낼때 출발지IP와 요청을 하는 APP의 PORT번호도 담겨져 있다. 그래서 응답할때 그 출발지 정보를 써서 그곳으로 응답해준다.



### PORT 번호 정보

+ 0 ~ 65535 할당 가능
+ 0 ~ 1023 : 잘 알려진 포트, 사용하지 않는 것이 좋음
  +  FTP - 20, 21
  + TELNET - 23
  + HTTP -  80
  + HTTPS - 443





-----> 다음 내용 : [DNS](../DNS/README.md)






