# 14 값 하나로 갈림길 만들기: `switch`

지금까지 우리는 `if`와 `else if`를 써서 조건에 따라 어떤 코드를 실행할지 작성해왔다. 

그런데 조건에 따라 실행 흐름을 나누다 보면 종종 등장하는 패턴이 있다. 
하나의 변수나 수식이 몇 가지 정해진 값 중 하나를 가질 때,
그 값에 따라 다른 코드를 실행하는 패턴이다.

예를 들어 게임 시작 화면에서 메뉴를 선택하는 상황을 프로그래밍 해보자.

```c
/* if 로 조건을 하나씩 검사 */
if (menu == 1) {
        start_game();
} else if (menu == 2) {
        load_game();
} else if (menu == 3) {
        show_settings();
} else if (menu == 4) {
        show_help();
} else if (menu == 5) {
        quit_game();
} else {
        show_error();
}
```

`menu`의 값이 `1`인 경우, `2`인 경우, `3`인 경우, `4`인 경우, `5`인 경우를 확인하기 위해서 
`menu == 1`, `menu == 2`, `menu == 3`, `menu == 4`, `menu == 5`라는 조건을 만들었다.

같은 변수 `menu`를 여러 상수 값과 비교하는 패턴은 프로그래밍을 할 때 자주 등장한다.
C 언어에서는 이 패턴을 더 분명하게 표현하기 위해 `switch` 문법을 제공한다.

`switch` 문법에서는 처음에 검사할 값 `menu`를 한 번 정하고, 
그 값이 어느 `case`에 해당하는지를 나열한다.

```c
/* switch로 케이스 나누기 */
switch (menu) {
case 1: 
        start_game();     
        break;
case 2: 
        load_game();      
        break;
case 3: 
        show_settings();  
        break;
case 4: 
        show_help();      
        break;
case 5: 
        quit_game();      
        break;
default: 
        show_error();    
        break;
}
```

마치 자판기 버튼처럼 원하는 값에 해당하는 `case`로 바로 점프하는 구조다.
점프한 다음부터는 순서대로 실행되는데, 보통 각 `case` 끝에 `break`를 적어서 `switch` 블록을 빠져나간다. 
해당하는 `case`가 없는 경우에 실행할 내용은 `default`에 적는다. 

> [!NOTE]
> **`switch`의 용도**
>
> `switch`는 하나의 값이 여러 상수 중 어느 것과 같은지 비교할 때 적합하다.
> 반대로 `x > 10`, `age >= 20 && age < 30`처럼 범위 조건이나 복합 조건을 검사할 때는 
> 여전히 `if`와 `else if`가 더 적합하다.
>
> ```c
> /* switch보다 if가 더 적합한 조건 */
> if (age < 0) {
>         /* 에러 */        
> } else if (age < 20) {
>         /* 미성년자 */       
> } else if (age < 30) {
>         /* 20대 */      
> } else {
>         /* 30대 이상 */       
> }
> ```

> [!NOTE]
> **기계어 관점: `switch`의 장점**
>
> `switch`는 하나의 값을 기준으로 여러 경우를 명확하게 나열할 수 있다는 장점이 있다.
> 
> 범용적인 `if`-`else if` 패턴보다 형태가 제한되어 있지만, 
> 이 제한 덕분에 컴파일러는 경우에 따라 더 효율적인 기계어 코드를 만들어 내기도 한다.

> [!CAUTION]
> **보안 관점: 비밀번호 비교는 `switch`나 일반 `if`로 직접 구현하지 말자**
>
> 비밀번호를 비교할 때는 값에 따라 실행 시간이 달라지는 코드를 쓰면 위험하다.
> `switch`, `if-else`, 조기 종료되는 반복 문법은 해커의 표적이 될 수 있다.
>
> 실제 보안 코드에서는 직접 비교 함수를 만들기보다, 검증된 상수 시간 비교 함수를 사용한다.

## 14.1 `switch`의 구성 요소: `case`, `break`, `default`

`switch`에서는 괄호 `()` 안에 검사할 대상을 적는다. 
검사할 대상과 비교할 정수 상수 수식은 `case` 다음에 적고, 그 뒤에 콜론 `:`을 적는다. 
검사 결과에 해당하는 값이 있으면 해당 `case`로 점프해서 문장을 실행한다. 
일치하는 `case`가 없으면 `default`로 점프해서 문장을 실행한다.
일치하는 `case`도 없고 `default`도 없으면, `switch` 블록 안으로 들어가지 않는다.

```c
switch (value) {       /* 1. 검사할 대상. 결과값이 정수 계열이어야 하고, 변수를 쓸 수 있다 */

case 1:                /* 2. 비교할 정수 상수 수식. 변수를 쓸 수 없다. 검사할 대상의 값이 같으면 여기로 점프한다 */
        command_1();   /* 점프해서 들어온 뒤 순서대로 실행된다 */
        break;         /* 3. 탈출. 다음 명령들은 실행하지 않고 switch를 종료한다 */
case 2:
        command_2();
        break;         
default:               /* 4. 기타. 일치하는 case가 없으면 여기로 이동한다 */
        command_default();
        break;
}
```

**동작 원리: "점프 후 직진"**
1. **점프 _jump_:** `switch` 괄호 안의 값을 계산해서, 일치하는 `case` 라벨로 이동한다.
2. **직진 _flow_:** 그 위치부터 아래로 코드를 실행한다.
3. **탈출 _break_:** `break`를 만나면 `switch` 블록을 나간다. 

> [!NOTE]
> **보안 관점: `default`는 기본 차단 지점이다**
>
> 외부에서 들어온 명령, 권한, 상태 값을 `switch`로 처리할 때는 
> 예상한 값만 명시적으로 허용하고, 나머지는 `default`에서 차단하는 습관이 좋다 _fail-closed_.
> 해커가 예상하지 못한 값을 넣었을 때 아무 처리 없이 지나가게 두면, 
> 프로그램이 안전하지 않은 상태로 계속 실행될 수 있다.
>
> ```c
> switch (command) {
> case 1:
>       /* 허용된 명령 처리 */
>       break;
> case 2:
>       /* 허용된 명령 처리 */
>       break;
> default:
>       /* 알 수 없는 명령 거부 */
>       return -1;
> }
> ```
>
> `default`에서는 필요하다면 요청 거부, 로그 기록, 상태 초기화 등을 수행하기도 한다.

> [!NOTE]
> `case`의 들여쓰기: 정렬식 vs 계단식
>
> `switch` 문법을 처음 작성할 때 가장 많이 하는 고민이 `case`를 들여써야 하는지다. 
> `switch` 코딩 스타일은 크게 정렬 방식과 계단 방식이 있다.
>
> - **정렬 방식:** `switch`와 `case`를 같은 열에 두기
>
>   `switch`와 `case`의 줄을 맞추는 방식이다. 
>    C 코드에서 오래전부터 널리 쓰인 방식이며, 가로 공간을 아낄 수 있다.
>
>   ```c
>   switch (x) {
>   case 1: /* switch와 case의 라인을 맞춘다 */
>           /* x가 1일 때 */
>           break;
>   case 2:
>           /* x가 2일 때 */
>           break;
>   }
>   ```
>
> - **계단 방식:** `case`를 들여쓰기
>   
>   `switch`보다 `case`를 한 번 들여쓰고, 내용물은 거기서 또 들여쓴다.
>
>   ```c
>   switch (x) {
>           case 1: /* case를 들여쓴다 */
>                   /* x가 1인 경우 */
>                   break;      /* 내용물은 들여쓰기를 한 번 더 한다 */
>      
>           case 2:
>                   /* x가 2인 경우 */
>                   break;
>   }
>   ```
>
> 어느 쪽이든 정답은 하나가 아니다. 
> 중요한 것은 프로젝트 안에서 한 가지 방식을 일관되게 적용하는 것이다.

## 14.2 `case` 라벨의 규칙: 컴파일러가 아는 값만

`case` 뒤에는 계산 결과값이 정수 상수인 수식 _integer constant expression_ 만 올 수 있다. 
소스 코드를 기계어로 번역하는 시점 _compile time_ 에 값을 확정할 수 있는 것이어야 한다.

- **가능한 것:** 컴파일러가 미리 계산할 수 있는 정수 상수 수식은 허용된다.

  ```c
  case 3:         /* 정수 리터럴. */
  case 'A':       /* 문자 리터럴. 내부적으로 정수값을 갖는다. */
  case 10 + 5:    /* 상수 수식. 컴파일러가 15로 미리 계산할 수 있다. */
  ```

- **불가능한 것:** 변수는 C 문법상 정수 상수 수식으로 취급되지 않는다.

  ```c
  int x = 3;
  
  switch (input) {
  case x:         /* 에러 발생: 변수는 안 된다 */
          break;
  }
  ```

> [!CAUTION]
> **`case`에는 `const` 변수도 금지**
>
> C 언어에서는 변수에 `const`가 붙어도 저장된 값을 바꿀 수 없을 뿐, 여전히 변수로 취급한다.
>
> ```c
> const int limit = 10;
>         
> switch (n) {
> case limit:  /* 에러 발생: case의 값에 변수 사용 */
>         break;
> }
> ```
> C 언어의 `case`에는 정수 리터럴, 문자 리터럴, 뒤에서 다룰 _enumeration constant_, 
> 그리고 이들을 연산하는 정수 상수 수식을 쓸 수 있다. 
> 또한 `#define` 매크로도 전처리 후 정수 상수 수식으로 바뀐다면 사용할 수 있다.

> [!WARNING]
> **`case` 값이 중복되면 안 된다**
>
> `case` 값은 코드의 어느 부분으로 이동할지 정하는 데 쓰이기 때문에, 
> 하나의 `switch` 안에 중복해서 `case` 값이 나오면 컴파일 에러가 발생한다. 
> 계산 결과가 같아도 중복이다.
>
> ```c
> switch (x) {
> case 3:
>         break;
> case 1 + 2:    /* 오류: 계산 결과가 3이므로 중복 */
>         break;
> }
> ```
> 
> `default`도 하나의 `switch` 안에서 최대 한 번만 쓸 수 있다.

## 14.3 `break`의 대상: 가장 가까운 `switch` 또는 반복 문법

`break`는 탈출 명령어다. 그런데 `for`에서도 쓰고, `switch`에서도 쓴다.

그렇다면 `switch`가 `for` 안에 들어있다면 어떻게 될까?

```c
for (;;) {                /* 바깥쪽 for */
        switch (x) {      /* 안쪽 switch */
        case 1:
                break;    /* switch만 빠져나간다 */
        }

        /* case 1에서 break를 만나면 여기로 온다 */
        /* 바깥쪽 for는 아직 끝나지 않았다 */
}
```

`break`는 자신이 들어 있는 가장 가까운 `switch` 또는 반복 문법 하나만 빠져나간다.

따라서 `for` 안에 `switch`가 있을 때, `case` 안의 `break`는 `switch`만 빠져나가고 바깥쪽 `for`는 계속된다.

그렇다면 `switch`의 어떤 `case`에서 바깥쪽 `for`를 빠져나가려면 어떻게 해야 할까?

**해결책: 상태 플래그 _flag_ 를 써라**

반복을 진행할지 말지에 대한 신호를 사용하면 된다. 
바깥쪽에서 반복을 멈춘다는 신호를 확인하면, `break`를 해서 종료한다. 

```c
int is_running = 1; /* 1: 반복을 계속 진행, 0: 반복을 멈춤 */
int menu;

for (;;) {
        menu = get_menu();

        switch (menu) {
        /* ... */
        case 5:
                /* 반복을 멈춘다는 의미의 값인 0을 is_running에 저장하고 switch를 종료한다 */
                is_running = 0;    /* 1. 반복을 멈춘다는 정보를 저장한다 */
                break;             /* 2. switch를 종료한다 */
        default:
                show_error();
                break;
        }

        /* switch가 끝나고, for 루프가 다시 돌기 전에 반복을 할지 확인한다 */
        if (is_running == 0) {
                break;             /* 3. is_running이 멈춘다는 뜻인 0이면 for를 종료한다 */
        }
}
```

1. 안쪽에 있는 `switch`에서 `is_running`을 0으로 만든다.
2. `break`로 `switch`를 나온다.
3. `switch` 바깥의 `if`에서 `is_running == 0`인지 확인하고, 참이면 `break`로 바깥쪽 `for`를 끝낸다.

> [!NOTE]
> **`break`는 `if`를 빠져나가지 않는다**
>
> 1990년 AT&T의 미국 장거리 전화망에서 대규모 장애가 발생했다.
>
> 사고의 원인으로 널리 소개되는 코드 구조는 `switch` 안에 있는 `if`에서 `break`를 하는 것이다.
> `break`가 `if`를 빠져나가는 것이 아니라, 가장 가까운 `switch` 또는 반복 문법에 작용한다는
> 점을 보여주는 대표적인 사고 사례로 자주 소개된다. 
>
> ```c
> switch (message_type) {
> case 1:
>         if (hardware_busy) {
>                 /* break의 의도: if만 건너뛰고 update_plan()을 실행하려고 했다 */
>                 /* break의 실제 동작: switch를 종료해서 update_plan()은 실행이 안 된다 */
>                 break;
>         } else {
>                 process_message();
>         }
>
>         /* 메시지를 처리했는지 여부와 관계없이 항상 실행되는 공통 작업 */
>         update_plan();
>         break;
> }
> ```
>
> `if`만 건너뛰고 싶다면 `break`를 쓰는 것이 아니라, 
> 조건식을 반대로 만들거나 `if`, `else` 구조를 다시 짜야 한다.
>
> ```c
> switch (message_type) {
> case 1:
>         if (!hardware_busy) {
>                 process_message();
>         }
>
>         /* 메시지를 처리했는지 여부와 관계없이 항상 실행되는 공통 작업 */
>         update_plan();
>         break;
> }
> ``` 

**또 다른 방법: `return`**

중첩된 반복 문법과 `switch`를 한 번에 빠져나가야 할 때는,
해당 로직을 하나의 함수로 묶고, `return`으로 함수 실행 자체를 종료하는 방법도 있다.

```c
void run_game(void) 
{
        int menu;

        for (;;) {
                menu = get_menu();

                switch (menu) {
                /* ...중략... */

                case 5:
                        return; /* 함수 전체를 종료한다 */

                /* ...중략... */
                }                        
        }
}
```

`return`은 `switch`나 `for`만 빠져나가는 것이 아니라 현재 함수 전체를 종료한다.
함수 끝부분에서 반드시 해야 할 정리 작업이 있다면, 정리 작업을 하지 않고 `return`으로 바로 빠져나가면 안 된다.

```c
switch (menu) {
/* ... */
case 5:
        return;
/* ... */
}                        

/* switch를 return으로 종료할 경우 이 코드는 실행되지 않는다 */
save_game_data();
```

정리 작업을 하지 않고 빠져나가면 리소스를 조금씩 잃어버리는 
_cwe-404: improper resource shutdown or release_ 보안 취약점이 발생한다. 
공격자가 이런 경로를 반복적으로 실행할 수 있으면 시스템 자원을 고갈시키는 서비스 거부 공격 
_denial of service_ 으로 이어질 수 있다.

> [!NOTE]
> **심화 학습: `return` 전 정리 작업을 하는 `goto cleanup;` 패턴**
>
> 실무 C 코드에서는 여러 경로에서 같은 정리 작업을 할 때 `goto cleanup;` 패턴을 쓰기도 한다. 
> `goto`는 실행 흐름을 복잡하게 만들 수 있으므로 아무 곳으로나 점프하는 용도로 쓰면 안 된다.
> 함수 끝부분의 정리 코드로 이동하는 제한된 용도로만 이해하자.
>
> ```c
> void run_game_with_cleanup(void)
> {
>         int menu;
> 
>         for (;;) {
>                 menu = get_menu();
> 
>                 switch (menu) {
>                 /* ... */
>                 case 5:
>                         goto cleanup;
>                 default:
>                         show_error();
>                         break;
>                 }                        
>         }
> 
> cleanup:
>         /* 파일 닫기, 메모리 해제, 로그 기록 등 정리 작업 */
>         save_game_data();
>         return;
> }
> ```
>
> C 언어에는 다른 언어들과 달리 예외 처리 문법이 없기 때문에 이러한 `goto` 패턴이
> 실무적인 에러 처리 구조로도 널리 쓰인다.

## 14.4 `break`가 없으면 아래로 흐른다: _fall-through_

`switch`는 일치하는 `case` 라벨로 이동한 뒤, 그 위치부터 아래 방향으로 코드를 계속 실행한다.
따라서 각 `case` 끝에 `break`를 적지 않으면 다음 `case`의 코드까지 실행된다 _fall-through_.

의도적으로 `break`를 쓰지 않고 다음 `case`에 속한 코드도 실행할 때는 전통적으로 `/* fall through */` 같은 주석을 달아서 실수가 아님을 분명하게 밝힌다. 

```c
switch (menu) {
case 1:
        play_opening_video();
        /* fall through */        /* 오프닝 동영상이 재생된 후에 이어서 게임이 시작된다 */
case 2:
        start_game();
        break;
}
```

> [!NOTE]
> **최신 C23 표준에서 도입된 `[[fallthrough]];`**
>
> 최신 표준인 C23에서는 컴파일러에게 직접 힌트를 주는 속성 _attribute_ 문법 
> `[[ ... ]]`을 도입했다. 
> `/* fall through */` 대신 `[[fallthrough]];`라고 적을 수 있다.
>
> 사람은 실수로 `break`를 누락할 수 있기 때문에 실무에서는 컴파일러의 도움을 받는다. 
> _gcc_ 나 _clang_ 등의 컴파일러에서 `-Wimplicit-fallthrough` 옵션을 켜면, 
> `[[fallthrough]];` 속성이나 명시적인 주석 없이 `break`가 빠진 코드를
> 컴파일 단계에서 경고하거나 에러로 처리할 수 있다. 

> [!NOTE]
> **보안 관점: `default`로 조용히 흘러내리는 버그**
>
> 잘 짜인 `switch` 문에서 `default`는 보통 맨 아래에서 '에러 처리기' 역할을 한다. 
> 문제는 프로그래머가 새로운 기능을 추가할 때 발생한다. 
> 개발자들은 보통 `default` 바로 위에 새로운 `case`를 끼워 넣는 경향이 있는데, 
> 실수로 `break`를 빼먹으면 정상 처리 후 `default`의 에러 처리 코드까지 실행될 수 있다.
>
> ```c
> switch (protocol) {
> case PROTO_TCP:
>         route_tcp(packet);
>         break;        /* 정상적인 경우 패킷이 처리되면 switch를 나간다 */
> 
> /* 개발자가 default 바로 위에 ICMP 처리 기능을 추가했다 */
> case PROTO_ICMP:
>         route_icmp(packet);
>         /* 버그: 여기에 break; 를 쓰는 것을 깜빡했다! */
> 
> default:
>         /* 알 수 없는 패킷을 처리하는 에러 핸들러 */
>         log_security_warning("비정상 패킷 감지. 삭제합니다.");
>         free_memory(packet);
>         return -1;
> }
> ```
>
> **조용한 실패와 이중 삭제 _double free_ 보안 취약점**
>
> 만약 `PROTO_TCP`에서 `break`를 쓰지 않았다면 
> TCP 패킷이 ICMP로도 한 번 더 라우팅되어 오작동하므로 테스트 과정에서 쉽게 발견된다. 
> 하지만 `default` 바로 앞에서 `break`를 쓰지 않은 경우에는
> 핵심 기능인 `ICMP` 패킷 처리는 정상적으로 되기 때문에 성공으로 판정될 가능성이 높다.
> 문제는 정상 처리 중에 `route_icmp(packet)`에서 `packet`을 이미 삭제했는데 
> `default`의 `free_memory(packet)`에서도 `packet`을 삭제하는 경우다.
> 중복 삭제는 메모리 관리 구조를 망가뜨리는 _cwe-415: double free_ 보안 취약점으로 분류된다. 
>
> **리눅스 커널에서 `switch`에 `break`가 누락된 버그**
> 2019년, 리눅스 커널 개발진은 소스 코드의 `switch` 문법을 검토하여 
> `break`가 없어서 발생한 버그를 여러 건 수정했다.
> 그 뒤로 의도적으로 `break`를 생략한 경우에는 자체적으로 만든 매크로로 표시하기로 정했다. 
> C23의 `[[fallthrough]]`도 같은 문제의식을 표준 문법으로 표현했다고 볼 수 있다.

> [!WARNING]
> **보안 관점: _fall-through_**
>
> `switch`에서 `break`가 빠져 의도하지 않은 코드가 함께 실행되는 것은 보안 취약점 
> _cwe-484: omitted break statement in switch_ 로 분류되어 있다. 
> 보안 코드에서는 각 `case`의 끝이 눈에 보이게 닫혀 있어야 한다. 
> `break`, `return`, `goto cleanup;`, 또는 의도적 `[[fallthrough]];` 중 하나로 끝나야 한다.

**`/* fall through */`를 알리지 않아도 되는 예외 상황: 여러 케이스를 묶을 때 _case stacking_**

`case`에 해당하는 문장 없이 바로 다음 `case`가 이어지는 경우는 
일반적으로 _fall through_ 표시를 따로 하지 않는다.
여러 `case`를 같은 코드로 처리하려는 의도가 분명하기 때문이다.

```c
switch (char_input) {
case 'y':
case 'Y': 
        /* 'y'인 케이스와 'Y'인 케이스에서 똑같이 동작한다 */
        accept_terms();
        break;
}
```

## 14.5 숫자에 이름을 붙이는 방법: `enum`

프로그램을 만들다 보면 상태나 종류를 숫자로 표현할 일이 많다. 
예를 들어 HTTP 응답 상태 코드에 따라 적절한 메시지를 반환하는 함수를 보자.

```c
const char *http_status_message_naive(int status)
{
        switch (status) {
        case 200:                    
                return "200 OK";
        case 404:             
                return "404 Not Found";
        case 503:   
                return "503 Service Unavailable";
        default:                                
                return "Unknown HTTP status"; 
        }
}
```

여기서 `200`, `404`, `503`이 정확히 무엇을 의미할까? 
HTTP 응답 상태 코드에는 표준적으로 정해진 번호가 있기는 하지만, 
숫자를 직접 쓰면 의도가 명확히 드러나지 않는다.

정해진 표준이 없을 경우는 문제가 더 심각하다.
코드를 작성한 사람은 당장 알겠지만, 며칠만 지나도 기억나지 않을 것이다. 
코드 안에 의미를 알기 힘든 숫자 _magic number_ 를 직접 적는 것은 
코드를 읽기 어렵게 만들기 때문에 권장되지 않는 방식이다.

관련된 정수 상수들에 의미 있는 이름을 붙이기 _enumeration_ 위해 
C 언어에서는 `enum` 이라는 타입을 제공한다.

### 14.5.1 `enum` 타입을 만들고 쓰는 방법

지금까지 썼던 `int`, `char`는 그 자체로 C에서 제공되는 타입이었다.
반면 `enum`은 프로그래머가 직접 이름을 붙여 만드는 타입이다.

예를 들어 `enum HttpStatusCode` 전체가 하나의 타입처럼 쓰인다.

`enum` 타입을 쓸 때 한 번 나열할 상수들의 이름과 정수 값을 정한다.
나열할 상수들을 중괄호 `{}` 안에 적는데, 
이름만 적을 수도 있고, `상수이름 = 정수값`처럼 값을 정할 수도 있다.

```c
/* HTTP 응답 상태 코드를 나타내는 enum 타입 만들기 */
enum HttpStatusCode {
        HTTP_STATUS_OK = 200,                     /* 요청 성공 */
        HTTP_STATUS_NOT_FOUND = 404,              /* 페이지 없음 */
        HTTP_STATUS_SERVICE_UNAVAILABLE = 503     /* 서버 과부하 또는 점검 중 */
};
```

위 코드는 `enum HttpStatusCode`라는 타입과, 그 타입의 값으로 쓸 상수들을 정한다.
문법은 `enum 새로운_enum_타입_이름 { enum_상수_목록 };`이다.

> [!NOTE]
> **`enum`에서 이름을 정하는 규칙**
> 
> `enum` 타입 이름은 `HttpStatusCode`, `PlayerState`처럼 
> 단어의 첫 글자를 대문자로 적는 방식이 읽기 좋다. 
>
> `enum` 상수는 일반 상수처럼 모두 대문자로 쓰는 경우가 많다. 
> 또한 C 언어에서는 `enum` 상수 이름이 서로 충돌할 수 있으므로, 
> `HTTP_STATUS_OK`, `STATE_IDLE`, `PROTOCOL_TCP` 처럼 접두사를 붙이는 습관이 좋다.

`enum` 타입의 이름과 구성하는 이름들을 정해놓았으면, 
그 다음부터는 값을 저장할 상자를 만들 때, 
기존에 쓰던 `int` 대신 `enum HttpStatusCode`를 타입으로 쓸 수 있다.

```c
enum HttpStatusCode http_response_status; /* HTTP 응답 상태 코드를 저장할 변수 */
```

또한, `enum` 상수들은 정수 상수값을 쓸 수 있는 자리에 대신 쓸 수 있다.

```c
http_response_status = HTTP_STATUS_OK;    /* 성공적으로 처리됨을 의미 */
```

`switch` 코드의 `case`에서도 정수 상수값이 필요하다.
`enum` 상수는 컴파일 시점에 값이 정해지는 정수 상수식이므로, 
`case`에 쓸 수 있다.

```c
/* http 응답 상태 코드에 따라 적절한 메시지 문장을 반환하는 함수 */
const char *http_status_message(enum HttpStatusCode status)
{
        switch (status) {
        case HTTP_STATUS_OK:                    
                return "200 OK";

        case HTTP_STATUS_NOT_FOUND:             
                return "404 Not Found";

        case HTTP_STATUS_SERVICE_UNAVAILABLE:   
                return "503 Service Unavailable";

        default:                                
                return "Unsupported or unknown HTTP status"; 
        }
}
```

예제 코드에서는 각 `case`에서 `return`을 만나면 함수 전체가 끝나므로, 
뒤에 `break`를 따로 쓰지 않았다.

**`enum`의 값 지정 규칙**

`enum` 타입을 만들 때, 상수의 값을 정하지 않아도 된다.

1. 첫 번째 `enum` 상수의 값을 정하지 않으면 0이 된다.
2. 그 다음 `enum` 상수부터는 값을 정하지 않으면 이전 상수의 값보다 1씩 크게 설정된다.
3. 프로그래머가 원하는 `enum` 상수의 값을 직접 정할 수도 있다.

```c
/* enum 타입 상수의 정수값을 자동으로 정하기 */
enum PlayerState {
        STATE_IDLE,     /* 자동으로 0이 된다 */
        STATE_RUNNING,  /* 자동으로 1이 된다 */
        STATE_JUMPING   /* 자동으로 2가 된다 */
};
```

> [!WARNING]
> **C 언어의 `enum`은 값의 범위를 자동으로 검사하지 않는다**
>
> C 표준에서 `enum`은 정수 계열 타입과 호환되는 타입으로 표현된다고만 되어있다.
> `enum HttpStatusCode` 타입이라고 해서 값이 항상
> `HTTP_STATUS_OK`, `HTTP_STATUS_NOT_FOUND`, `HTTP_STATUS_SERVICE_UNAVAILABLE`
> 중 하나라고 보장되지는 않는다.
>
> ```c
> enum HttpStatusCode bad_value = 99; /* 정해진 enum 상수가 아닌 값도 저장될 수 있다 */
> ```
>
> 따라서 `enum` 타입의 값을 `switch`에서 다룰 때는 반드시 `default`를 작성하여 
> 예상치 못한 값이나 `case`에서 처리하지 않는 값이 들어올 가능성에 대비하는 습관을 들이자.

