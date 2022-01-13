# wait함수

> 디토랜드에서는 Lua언어를 사용합니다!
> 

저희가 배울 공간에서 사용하는 언어는 Lua언어를 사용합니다. 

> wait은 무엇일까요?
> 

저희가 배울 wait함수 이 wait 함수는 무엇일까요?

바로 대기 함수입니다. 이 함수는 동기 메서드 입니다. 

여기서 **동기(Synchronized)**란 쉽게 설명해서 프로그램이 작성된 순서대로 실행되는 것으로 A, B, C 순서로 함수가 작성되어 있다면 함수는 반드시 A, B, C순서대로 A의 동작이 끝나면 B함수가 실행되고, B가 끝나면 C함수가 동작하는 것을 동기적 실행이라고 할 수 있습니다. 즉 본인이 짠 코드가 순서대로 실행되는 것이라고 생각하면 됩니다. 

# Lua언어에서의 wait와 디토랜드 Wait의 차이

> Lua언어에서의 Wait()
> 

루아에서는 시간지연 함수인 wait()을 만들어서 써야합니다.

```lua
function wait(seconds)
	local start = os.time()
	repeat until os.time() > start + seconds
	end
```

> 디토랜드에서의 Wait()
> 

디토랜드에서는 Wait() 함수를 내장하고 있어서 Lua언어처럼 길게 안 적어도 됩니다. 

```lua
print("wait 3s") //출력
wait(3) //3초를 기다린 후
print("end wait!") //출력
```

# Coroutine(코루틴)

동시에 다른 일을 처리할 수 있도록 독립적인 흐름을 만들어 줄 수 있는 방법입니다.
일반적으로 하나의 일을 끝내고 다른 일을 할 수 있다면,Coroutine은 동시에 일을 처리할 수 있도록 해줍니다.

![Untitled](wait%E1%84%92%E1%85%A1%E1%86%B7%E1%84%89%E1%85%AE%209298f4a2969a42f6bc15b60718d59dff/Untitled.png)

# Coroutine함수 목록

| 함수명 | 설명 |
| --- | --- |
| coroutine.create() | coroutine 생성 |
| coroutine.resume() | coroutine (재)실행 |
| coroutine.yield() | coroutine 일시정지 |
| coroutine.status() | coroutine의 상태 반환 |
| coroutine.wrap() | 함수 형태로 호출 가능한 coroutine 생성 |

# Coroutine사용법

### coroutine.create(함수 or 익명함수)

> 새로운 코루틴을 생성하지만, 실행하지는 않아요.
> 

```lua
--매개변수를 사용하지 않을 경우
local co1 = coroutine.create(function()
    print("hi")
end)

--매개변수를 사용할 경우
local co2 = coroutine.create(function(message)
    print(message)
end)
```

> 여기서 매개변수란?
> 
- 매개변수란  그림에서 보시는 것처럼 함수를 정의할 때 사용되는 변수를 의미합니다.

![Untitled](wait%E1%84%92%E1%85%A1%E1%86%B7%E1%84%89%E1%85%AE%209298f4a2969a42f6bc15b60718d59dff/Untitled%201.png)

### coroutine.resume(코루틴, 매개변수)

생성/일시정지 된 코루틴을 (재)실행 합니다
일시정지 후, 재실행을 하면 일시정지 된 부분에서부터 시작합니다

```lua
--매개변수를 사용하지 않을 경우
local co1 = coroutine.create(function()
    print("hi")
end)
coroutine.resume(co1)
>hi

--매개변수를 사용할 경우
local co2 = coroutine.create(function(message)
    print(message)
end)
coroutine.resume(co2, "hi")
>hi
```

### coroutine.yield()

실행중인 코루틴 일시정지합니다. yield는 코루틴 내에서 사용해야합니다.

```lua
local co1 = coroutine.create(function()
    for i = 1, 5 do
        if i == 4 then
            coroutine.yield() --coroutine 내에서 사용해요. (i == 4일 때 정지)
        end
        print("num : " .. i)
    end
end)
coroutine.resume(co1)  --1 ~ 3까지 출력

wait(1)                --1초 후 
print("재시작")         --재시작
coroutine.resume(co1)  --4 ~ 5 출력 (일시정지한 i == 4부터 다시 시작)
>num : 1
num : 2
num : 3
```

### coroutine.status(코루틴)

코루틴 상태를 반환합니다. 코루틴의 상태는 running (실행 중) / suspend (정지) / dead (종료) 가 있습니다.

```lua
local co = coroutine.create(function()
    for i = 1, 5 do
        if i == 4 then
            coroutine.yield() --coroutine 내에서 사용해요.
        end
        print("num : " .. i)
    end
end)
coroutine.resume(co)         --1 ~ 3까지 출력
print(coroutine.status(co))  --suspend

print("재시작")               --재시작

coroutine.resume(co)         --4 ~ 5 출력
print(coroutine.status(co))  --dead
>num : 1
num : 2
num : 3
suspended
재시작
num : 4
num : 5
dead
```

### coroutine.wrap(함수 or 익명함수)

함수로 이용가능한 코루틴 생성합니다.wrap을 사용하면 yield로 정지 후 resume을 사용하지 않고 재시작할 수 있습니다. 

```lua
--wrap을 사용하면 yield로 정지 후 재시작할 때 resume을 사용할 필요가 없어요.

local co = coroutine.wrap(function()
    for i = 1, 5 do
        print("num : " .. i)
        coroutine.yield() --coroutine 내에서 사용해요.
    end
end)
co() -- num : 1
co() -- num : 2
co() -- num : 3
co() -- num : 4
co() -- num : 5
>num : 1
num : 2
num : 3
num : 4
num : 5
```

# Coroutine 과 wait 을 이용한 예제 (신호등 만들기)

초록과 빨강으로 만들기

상열이가 가져온 예제를 보며 살펴보겠습니다.

> 다음 회의
> 

다음 회의까지 신호등 예제를 사용하여 초안을 만들면 좋을 거 같습니다.

유니티 교제 처럼 하나 하나 캡처해서 교안을 만드는 건 어떨 까요?

또한 그 예제를 사용하기 전에 코루틴에 대해 추가로 설명하는 것도 보완해야할거 같습니다.