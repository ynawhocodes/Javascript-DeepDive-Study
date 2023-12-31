## 1. 산술 연산자

- 이항 산술 연산자
    
    > +, -, *,/,%
    > 
- 단항 산술 연산자
    
    ++, —, +,-
    
    - [x]  +,- 단항 연산자는 부수효과가 없다
    - [ ]  +,- 단항 연산자는 부수효과가 있다
    
    ```jsx
    var x = '1'
    console.log(+x)
    console.log(x)
    
    x = true
    console.log(+x)
    console.log(x)
    ```
    
    - 문자열 연결 연산자
        
        피연산자 중 하나 이상이 문자열인 경우
        
        ```jsx
        '1' + 2; // '12'
        1 + true; // 2
        ```
        

## 2. 할당 연산자

> =, +=, -=, *=, /=, %=
> 
- 부수효과 있음
- 표현식인 문

## 3. 비교 연산자

- 동등비교, 일치 비교, 부동등 비교, 불일치 비교
    
    > ==, ===,!=, !==
    > 
    - [x]  False
    - [ ]  True
    
    ```jsx
    NaN === NaN
    ```
    
- 대소 관계 비교 연산자
    
    > < , >, <=, >=
    > 

## 4. 삼항 조건 연산자

> 조건식 ? true 일 때: false 일 때
> 

## 5. 논리 연산자

- 논리합, 논리곱, 부정
    
    > ||, &&, !
    > 
    
    ```jsx
    !0; // true
    !'Hello' //false
    'A' && 'B'; // 'B' (단축 평가)
    ```
    

## 6. 쉼표 연산자

```jsx
var x, y, z;
x = 1, y = 2, z = 3; // 3
```

## 7. 그룹 연산자

우선 순위 높이는 소괄호

## 8. typeof 연산자

피연사자의 데이터 타입을 문자열로 반환

```jsx
typeof NaN // "number"
typeof undefined // "undefined"
typeof null // object
```

- 주의

```jsx
var foo = null;
typeof foo === null; // false
foo === null; // true
```

## 9. 지수 연산자

> **
> 

```jsx
num **= 2 // 25, 할당 연산자와 함께 사용 가능
```

## 11. 연산자의 부수 효과

- 부수효과 있는 연산자
    
    > =, ++,—, delete
