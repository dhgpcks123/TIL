# batch, quartz, scheduler?

서버에서 일괄처리를 하기 위해 spring에서 제공하는 3가지 방법이 있다.
batch, quartz, scheduler

batch는 대용량 job을 순차적으로 처리
quartz scheduler 특정 job을 특정 시간에 처리
하기 위해 사용된다

quarts는 scheduler보다 구현하기 복잡하고,
스케쥴링의 세밀한 제어가 필요하다.

스프링 프레임워크에서 Quartz 라이브러리를 사용할 수 있다. 스프링 프레임워크에서 기본적으로 제공하는 스케줄러보다 보다 정교하게 잡 스케줄링을 다룰 수 있다고 한다. 반대로 설정과 사용에 번거롭고, 무엇보다 현재 필요한 기능은 기본 제공 스케줄러에서 모두 처리할 수 있어 Quartz 라이브러리 없이 스프링 스케줄러를 사용하기로 결정하였다.

혹시 당신이 스케줄링으로 필요한게 단순히 특정 시간에 또는 특정 간격으로 비동기 수행 정도이면 그냥 스프링에서 제공하는 @Scheduled를 사용해도 좋을 것 같다.

동적으로 수행 시간 변화를 필요하면 Quartz를 사용하라는 답변이 있는데 동적으로 수행 간격, 수행 시점 변경도 @Scheduled로 처리가 가능하니 해당 부분에 관해선 다른 문서를 참고

## scheduler

Spring Scheduler는 별도의 추가적인 의존성이 필요하지 않다. Spring Boot starter에 기본적인 의존성으로 제공됩니다. 사용하기 위해서는 @EnableScheduling 어노테이션을 붙여준다

```
@EnableScheduling
@SpringBootApplication
public class Application() {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

```
@Component

public class LetterScheduler {

    @Scheduled(fixedDelay = 1000) // scheduler 끝나는 시간 기준으로 1000 간격으로 실행
    public void scheduleFixedDelayTask() {

    }

    @Scheduled(fixedRate = 1000) // scheduler 시작하는 시간 기즌으로 1000 간격으로 실행
    public void scheduleFixedRateTask(){

    }

    @Scheduled(cron = "0 15 10 15 * ?") // cron에 따라 실행
//    @Scheduled(cron = "0 15 10 15 * ?", zone = "Europe/Paris") // cron에 TimeZone 설정 추가
    public void scheduleTaskUsingCronExpression(){

    }

    @Scheduled(cron = "* /1 * * * *") // 매 1분 마다 실행
//    @Scheduled(cron = "* /10 * * * *") // 매 1분 마다 실행
    public void scheduleTaskUsingCronExpression(){
        System.out.println("LetterScheduler.scheduleTaskUsingCronExpression");
    }

```

    초, 분, 시간, 일, 월, 요일

    //scheduler 는 기본적으로 thread 1개를 이용하여 동기형식으로 진행된다.
    // 1번이 끝나지 않으면 2번 스케쥴 시작 시간이 되어도 시작하지 않는다.
    // 비동기 방식으로 진행하고 싶으면 @EnableAsync 어노테이션을 사용해야 한다.
