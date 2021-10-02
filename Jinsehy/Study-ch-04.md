# <strong>4. 제어문
### __GOAL__: 자바가 제공하는 제어문을 학습하세요.
---

## <strong>4.1 제어문 - 조건문

### if문
```java
if(condition)
{
   // If condition is true then this block of statements will be executed
}
```

### if-else문
```java
if(condition)
{
   // If condition is true then this block of statements will be executed
}
else if(condition)
{
   // If condition is true then this block of statements will be executed
}
else 
{
   // If none of condition is true, then this block of statements will be executed
}
```
<br><br><br>

## <strong>4.2 제어문 - 선택문

### switch문
```java
switch(조건식){
     case 값1:
     실행 코드
     break;
     case 값2:
     실행 코드
     break;
     case 값3:
     실행 코드
     break;
     default: case에 해당하는 값이 없을 때 실행할 코드 
      break;        
}
```
여기서 `break`를 사용하지 않으면 밑의 나머지 case까지 전부 실행. 사용한다면 바로 switch문 탈출. (`continue` 사용 불가능)


<br><br><br>

## <strong>4.3 반복문

`break`는 반복문 탈출

`continue`는 다음 반복 분기로 이동

### for문
```java
for(initialization; condition; increment/decrement)
{
   // Body of for loop
}
```

### while문
```java
while(condition)
{
   // Body of while loop
}
```

### do-while문
```java
do
{
   // Body of loop
} while(condition);
```

### for each문
```java
for (변수타입 변수명 : 루프를 돌릴 객체) {
    // 실행 코드
}
```
Python의 `for (변수타입 변수명) in (루프를 돌릴 iterable 객체):` 과 비슷한 동작을 한다.

<br><br><br>

## Reference

https://kils-log-of-develop.tistory.com/349?category=923003