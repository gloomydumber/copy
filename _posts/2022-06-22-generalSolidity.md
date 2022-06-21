---
layout: post
title: "Solidity"
date: 2022-06-22 24:00:00
categories: BLOCKCHAIN
permalink: /archivers/Solidity
nocomments: false
use_math: true
---

# Solidity

Solidity 정리

## 버전

버전 선언

```solidity
pragma solidity ^0.4.19;
// pragma solidity >=0.4.0 <0.7.0;
contract HelloWorld {

}
```

## 상태 변수 & 정수

*상태 변수*는 컨트랙트 저장소에 영구적으로 저장됨 (이더리움 블록체인에 기록됨)

데이터베이스에 데이터를 쓰는 것과 유사

*uint*는 부호 없는 정수(_unsigned int_) 타입

*Solidity*의 타입에 부호 있는 정수인 _int_ 또한 존재

*uint*는 *uint256*과 같음, _uint8_, _uint16_, _uint32_ 등 으로도 선언 가능

```solidity
pragma solidity ^0.4.19;

contract ZombieFactory {
  uint dnaDigits = 16;
}
```

## 수학 연산

덧셈, 뺄셈, 곱셈, 나눗셈, 나머지 연산 등을 지원 (+, -, \*, /, %)

지수 연산 또한 지원 ( \*\* )

```solidity
pragma solidity ^0.4.19;

contract ZombieFactory {
    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits; // 10의 16승
}
```

## 구조체

*Solidity*는 구조체를 지원

*struct*로 선언

```solidity
pragma solidity ^0.4.19;

contract ZombieFactory {
    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;
    struct Zombie {
        string name;
        uint dna;
    }
}
```

## 배열

*Solidity*에는 *정적 배열*과 *동적 배열*이 존재

*정적 배열*은 고정된 길이로 선언

*동적 배열*은 고정된 크기가 없으며, 계속 크기가 커질 수 있음

*public*을 이용하여, 다른 컨트랙트에서 해당 컨트랙트의 코드의 배열을 읽을 수 있도록 할 수 있음

*public*으로 배열을 선언하면, _getter_ 메소드를 자동적으로 생성함

```solidity
pragma solidity ^0.4.19;

contract ArrayExamples {
  // 2개의 원소를 담을 수 있는 고정 길이의 배열:
  uint[2] fixedArray;
  // 또다른 고정 배열으로 5개의 스트링을 담을 수 있다:
  string[5] stringArray;
  // 동적 배열은 고정된 크기가 없으며 계속 크기가 커질 수 있다:
  uint[] dynamicArray;
}
```

*구조체*의 배열 또한 생성 가능함

```solidity
pragma solidity ^0.4.19;

contract ZombieFactory {
    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;
    struct Zombie {
        string name;
        uint dna;
    }
    Zombie[] public zombies; // Zombie 구조체의 public 배열을 zombies라는 이름으로 생성
}
```

## 함수

함수 인자명을 *언더스코어(\_)*로 시작해서 전역 변수와 구별하는 것이 관례임

```solidity
pragma solidity ^0.4.19;

contract ZombieFactory {
    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;
    struct Zombie {
        string name;
        uint dna;
    }
    Zombie[] public zombies;
    function createZombie(string _name, uint _dna){

    }
}
```

## 구조체와 배열 활용하기

```solidity
pragma solidity ^0.4.19;

contract StructAndArrayExample {
  struct Person {
    uint age;
    string name;
  }

  Person[] public people;

  // 새로운 Person를 생성하고 people 배열에 추가하는 방법

  // 새로운 사람을 생성한다:
  Person satoshi = Person(172, "Satoshi");
  // 이 사람을 배열에 추가한다:
  people.push(satoshi);

  // 위 두 줄의 코드를 아래 한 줄과 같은 방식으로 표현 가능
  // people.push(Person(16, "Vitalik"));
}
```

```solidity
pragma solidity ^0.4.19;

contract ZombieFactory {
    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;
    struct Zombie {
        string name;
        uint dna;
    }

    Zombie[] public zombies;

    function createZombie(string _name, uint _dna) {
        zombies.push(Zombie(_name, _dna)); // 새로운 Zombie를 생성하여 zombies 배열에 추가함
    }
}
```

## References

add here

<!-- [Array on mozzila.org](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array){: target="\_blank"} -->

<!-- ![permasecond](/assets/posts/2020-02-21-cmdcolor/permasecond.png) -->
