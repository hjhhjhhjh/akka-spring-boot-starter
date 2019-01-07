Spring Boot Starter Akka
========================
Spring Boot 2.0 starter for Akka.

### Why Spring Boot starter for nats
NATS is very simple, why you create a starter?

* Settings easy: just nats.url
* Spring Kafka like: @NatsSubscriber
* Metrics & endpoints: NATS states
* Health indicator for NATS

### How to use

* please add following dependency in your pom.xml
```xml
          <dependency>
                 <groupId>org.mvnsearch.spring.boot</groupId>
                 <artifactId>akka-spring-boot-starter</artifactId>
                 <version>1.0.0-SNAPSHOT</version>
          </dependency>
```

* Write Akka Actor like spring with @ActorComponent

```
 @ActorComponent
 public class GreetingActor extends AbstractActor {
     @Autowired
     private GreetingService greetingService;
 
     @Override
     public Receive createReceive() {
         return receiveBuilder()
                 .match(String.class, text -> {
                     System.out.println(greetingService.greet(text));
                 })
                 .build();
     }
 
 }
```

* Add actor ref in init method

```
@RestController
public class PortalController extends AkkaSpringSupport {
    private ActorRef greetingActor;

    @PostConstruct
    public void init() {
        greetingActor = actorOf(GreetingActor.class);
    }

    @RequestMapping("/hello/{name}")
    public String hello(@PathVariable(name = "name") String name) {
        greetingActor.tell(name, ActorRef.noSender());
        return "sent";
    }
}
```
### References

* Akka Quickstart with Java: https://developer.lightbend.com/guides/akka-quickstart-java/index.html
* Introduction to Spring with Akka: https://www.baeldung.com/akka-with-spring
