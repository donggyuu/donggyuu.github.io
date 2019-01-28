hazelcast and spring-boot cache settin

쓰는 이유가,  
분산형으로 메모리에 저장해서 속도가 빠르다(그냥 각 서버의 하드에 저장하는 것보다)  
백업 시스템으로 인해 한군데 노드가 날라가도 걱정없고 노드가 추가되어도 알맞게 데이터가 분산된다.  
https://www.slideshare.net/jaxlondon2012/clustering-your-application-with-hazelcast?next_slideshow=1
여기를 참고 


https://d2.naver.com/helloworld/106824
여기에 개념이 자세하게 나와있어.
실제 어떻게 할지는 spring boot와 캐쉬를 연계하는 -> 찾아보기.   
궁금증 : 분산형 노드는 그럼 테스트할 때 서버가 여러대여야 하는가?  그런듯? 


예제는 여기  
https://memorynotfound.com/spring-boot-hazelcast-caching-example-configuration/

 gradlew setting이랑은 좀 다른듯?   


 https://www.baeldung.com/java-hazelcast  
 여기도 참고. maven으로 예제 짜는 것. 