# Today I Learn

저번주차 멘토링때 멘토님께서 레디스를 사용해보라하셧고, 웹소켓을 공부하면서 레디스도 따로 공부해봤다.
멘토님께서 레디스를 사용하라 했던이유는 레디스의 key,value 를 이용해서 어떤식으로든 이득을 보라는거였고,
웹소켓을 하면서 레디스를 공부한것은 pub/sub기능 때문이었다. 여기서 redis를 key,value를 어떤식으로 우리프로젝트에 적용할수 있을지 고민을 많이 했었는데 3가지정도 떠올랐다.

1. 채팅기능에서 최신순으로 채팅방을 정렬하는기능(레디스에서 정렬기능이 있는 자료구조가 있어서 생각남..)
2. 채팅방목록을볼때 안읽은 새로운 메시지가 있는지 판별해주는기능
3. 1:1 채팅인데 채팅방에 인원수가 한명이고 그한명이 메세지를 보냈을때 상대방에게 알람을 보내는 기능

레디스는 싱글 스레드라고한다 그래서 여러동작을 하는건 위험할수 있다고한다.
처음에는 채팅메시지 insert기능을 레디스에 100개가 쌓일때까지 들고있다가 100개가 되었을때 db에저장하려고했는데.
100개의 데이터를 한번에 읽어오는게 올바른방식이 아닐것같다는생각이 들어서 포기했고 더생각해보기로했다.


느낀점 - 재밌는것같다