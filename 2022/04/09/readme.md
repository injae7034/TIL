# 9일에 공부한 내용을 적었습니다.

## 스프링5 프로그래밍 입문 처음부터 챕터7까지 다시 복습하기

## 코드숨 코드리뷰 개선 사항
### 1. TaskControllerMock_MVC_Test에서 list에 테스트 개선
TaskServie에 10개의 항목을 채운다음 이를 list 메소드테스트에서 확인하기 위해<br>
1~10까지 다 있는지 확인하기 위해 10줄을 작성했는데 굉장히 비효율적임.<br>
그래서 처음에는 Task에서 taskToJson메소드를 생성하고,<br>
TaskService에서 tasksToJson메소드를 새로 정의함.<br>
하지만 1주차 때 리뷰해주신 내용 중에 task에서 json 양식으로 변경하는 역할을 분리하라고 했던 것이 생각남.<br>
그래서 PrinterService 클래스를 새로 만들고 여기에서 taskToJson과 tasksToJson 메소드를 정의함.<br>
JsonPrinterService보다 PrinterService로 명명한 이유는 지금은 json양식 하나뿐이지만<br>
나중에 task를 다른 양식으로 출력할 일이 생겼을 때<br>
그 때 마다 PrinterService 를 따로 생성하는 것보다는 출력과 관련된 처리는 모두 PrinterService 에서 해주기 위해<br>
범용적인 이름을 지음.<br>
그러나 이것이 자칫 잘못하면 오버엔지니어링이 될 수도 있을 것 같다는 생각이 들어 그 경계를 아직 잘모르겠음.

### 2. HelloController JavaDoc 한줄 내용 변경

### 3. TaskServiceTest에서 getTasks 메소드 테스트 취약점 개선
TASKS_SIZE = 5로 상수를 선언하고 setUP에서 5개만큼 반복을 돌려 task를 채워 넣은 다음에 테스트 메소드에서 hasSize(TASKS_SIZE )를 이용하여 테스트하도록 변경

### 4. TaskControllerMock_MVC_Test list의 context 상황 설명 변경
"처음에는"이라는 표현이 애매하여 "Tasks에 할 일 목록이 하나도 없으면"으로 수정하였습니다.
<br><br>

# \@Autowired 질문사항
스프링에 대해 아직 모르는 것이 너무 많아서 최근에 혼자서 조금 공부를 하고 있습니다.<br>
최근에 \@Component와 \@ComponentScan, \@Autowired에 대해서 알게 되었습니다.<br>
현재 TaskController 클래스는 생성자에서 taskService를 주입받고 있습니다.
## 기존 TaskController 코드
```java
@RestController
@RequestMapping("/tasks")
@CrossOrigin
public class TaskController {
    private TaskService taskService;

    public TaskController(TaskService taskService) {
        this.taskService = taskService;
    }
}
```
TaskService는 \@Service로 인해 스프링빈의 검색 대상에 등록이 됩니다.
## TaskService 코드
```java
@Service
public class TaskService {
    private List<Task> tasks = new ArrayList<>();
    private Long newId = 0L;
}
```

그래서 TaskController에서 \@Autowired를 통해 스프링이 자동으로 TaskService에 대한 의존주입을 해주도록 바꿨습니다.<br>
그럼 더이상 생성자에서 의존주입을 할 필요가 없으니 (기본)생성자 정의를 없앴습니다. 
## 변경된 TaskController 코드
```java
@RestController
@RequestMapping("/tasks")
@CrossOrigin
public class TaskController {
    @Autowired
    private TaskService taskService;
}
```
앱에서는 \@SpringBootApplication이 있고 여기에 \@ComponentScan이 있기 때문에<br>
App은 정상적으로 돌아갈 것이라 예상되고 실제로 콘솔창에 실험을 해보니 정상적으로 돌아갑니다.<br>
## App 코드
```java
@SpringBootApplication
public class App {
    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }
}
```
## 테스트 코드
TaskControllerTest의 경우 기존코드는 아래와 같습니다.
setUp에서 TaskController에 의존 주입을 해주고 있습니다.
## 기존 TaskControllerTest 코드
```java
@DisplayName("TaskController 클래스")
class TaskControllerTest {
    private static final String TASK_TITLE_ONE = "testOne";
    private static final String TASK_TITLE_TWO = "testTwo";
    private static final String UPDATE_TITLE = "otherTest";

    private TaskController controller;
    private TaskService taskService;

    @BeforeEach
    void setUp() {
        taskService = new TaskService();
        controller = new TaskController(taskService);

        Task task = new Task();
        task.setTitle(TASK_TITLE_ONE);
        controller.create(task);
    }
}
```
이를 아래처럼 바꿨습니다. Autowired를 통해 스프링이 자동으로 의존주입을 해주기 때문에 더이상
생성자를 호출하지 않았습니다.<br>
ComponentScan이 없어서 안되나 싶어 SpringBootApplication 애노테이션을 임시적으로 사용하였습니다.
## 변경된 TaskControllerTest 코드
```java
@SpringBootApplication
@DisplayName("TaskController 클래스")
class TaskControllerTest {
    private static final String TASK_TITLE_ONE = "testOne";
    private static final String TASK_TITLE_TWO = "testTwo";
    private static final String UPDATE_TITLE = "otherTest";

    @Autowired
    private TaskController controller;
    @Autowired
    private TaskService taskService;

    @BeforeEach
    void setUp() {
        Task task = new Task();
        task.setTitle(TASK_TITLE_ONE);
        controller.create(task);
    }
}
```

TaskControllerMock_MVC_Test 의 경우 처음에는 아래와 같이 작성하여<br>
mockMvc이 생성될 때 TaskController에 TaskService를 의존주입해주고 있습니다.
## 기존 TaskControllerMock_MVC_Test  코드
```java
@SpringBootTest
@AutoConfigureMockMvc
public class TaskControllerMock_MVC_Test {
    private static final String TASK_TITLE_ONE = "testOne";

    MockMvc mockMvc;

    private TaskService taskService;

    @BeforeEach
    void setUp() {
        taskService = new TaskService();

        this.mockMvc = MockMvcBuilders
                .standaloneSetup(new TaskController(taskService))
                .setControllerAdvice(TaskErrorAdvice.class)
                .build();
    }
```
이후 TaskController의 taskService에 Autowired 애노테이션을 선언해주고 생성자에서 의존주입하는 것을 없앴습니다.<br>
## 변경된 TaskControllerMock_MVC_Test 코드
```java
@SpringBootTest
@AutoConfigureMockMvc
public class TaskControllerMock_MVC_Test {
    private static final String TASK_TITLE_ONE = "testOne";

    MockMvc mockMvc;

    private TaskService taskService;

    @BeforeEach
    void setUp() {
        taskService = new TaskService();

        this.mockMvc = MockMvcBuilders
                .standaloneSetup(new TaskController())
                .setControllerAdvice(TaskErrorAdvice.class)
                .build();
    }
}
```
## 변경된 테스트 코드 결과
두 테스트의 결과는 모든 테스트가 NullPointerException이 발생했습니다.<br>
즉, 아예 생성이 안되었다는 말인데 이유를 잘모르겠습니다.<br><br>
혹시 App에서는 AutoWired가 잘되어 정상적으로 잘돌아가는데<br>
테스트에서는 왜 안돌아가는지 이유를 알 수 있을까요?<br>
SpringBootTest 애노테이션에 ComponentScan이 없어서 안되나 싶어<br>
SpringBootAplication을 임시방편으로 사용했으나 테스트에 실패하였습니다ㅠ
