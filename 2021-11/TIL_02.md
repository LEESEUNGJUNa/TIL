# Today I Learn

레디스에 대해서 알아봤는데 처음에 듣기로는 무슨 데이터베이스다 이런말이 있길래 데이터 베이스인가보다 라고 생각했었다.(누구한테 들었는지 기억은 안남)
근데 채팅기능을 구현하려고 하니까 레디스를 사용해야한다해서 공부해봤다 key,value 가있어서 key값으로 데이터에 빠르게 접근할수있는 ? 그런 기능을 한다는 정보가 많아서
이걸 어떻게 채팅으로구현을하지 라고생각햇었는데 redis에서 pub/sub 기능도 제공해준다고 한다.. 그럼 잠시만 stomp를 쓰면 되는게 아닌가 ? 라고 생각을 했었는데.
redis에서 pub/sub사용시 얻을수있는점이있다고한다. 일단 서버에서 pub/sub에대한 정보를 안들고있어도 되고, 여러서버에서 pub/sub에 접근할수도있다고한다.
그래서 어찌어찌해서 구현은 해놨고 내 로컬환경에서 테스트했을때는 문제없이 동작하는걸 확인했다. 하지만 클라이언트랑 연결해봐야 잘 동작하는지 알수있을것같다.
그리고 채팅기능 자체를 구현하는것보다 그외의것들이 더 어려운것같다 채팅방을 관리한다던지, 메시지를 저장하는 방식이라던지..(뭔가 메시지가 오고갈때마다 insert해주면 굉장히 잘못된 방식인것같다는 생각이..)


느낀점 - 항상 남의코드를 가져오면서 느끼는점은 내가 필요한 부분만을 고쳐서 쓸수있도록 만드는 능력이 중요한것같다.