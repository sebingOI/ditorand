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

# 코루틴과 wait (신호등 체계)

초록과 빨강으로 만들기

상열아 힘내~~~