---
layout: post
title: "Solidity"
date: 2022-06-22 00:00:00
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

## Private / Public 함수

*Solidity*에서 함수는 기본적으로 *public*으로 선언됨

다른 컨트랙트를 포함한 누구나 해당 컨트랙트의 함수를 호출할 수 있다는 것을 의미

따라서 기본적으로 함수를 *private*로 선언하여 공격으로 부터 보호가 필요

_private_ 함수명은 *\_(언더바)*로 시작하는 것이 관례

```solidity
pragma solidity ^0.4.19;

contract PrivateFunctionExample {
  uint[] numbers;

  function _addToArray(uint _number) private {
    numbers.push(_number);
  }
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

    function _createZombie(string _name, uint _dna) private {
        zombies.push(Zombie(_name, _dna));
    }
}
```

## 함수 더 알아보기 (반환값 & 함수 제어자)

함수에서 값을 반환 받기 위해서는 아래와 같이 선언

```solidity
pragma solidity ^0.4.19;

contract ReturnExample {
  string greeting = "What's up dog";

  function sayHello() public returns (string) {
    return greeting;
  }
}
```

상태(값)를 변경하지 않고 보기만하는 함수는 _view_ 제어자를 통해 함수를 선언

```solidity
pragma solidity ^0.4.19;

contract ReturnExample {
  string greeting = "What's up dog";

  function sayHello() public view returns (string) {
    return greeting;
  }
}
```

함수가 어떤 데이터에도 접근하지 않는 것을 나타내기 위해서는 _pure_ 제어자, 상태 변경은 물론이고 상태를 조회하지도 않음

_Solidity_ 컴파일러가 적절한 제어자 경고 메시지를 해줌 (_view_, _pure_ 등 무엇이 적절한 제어자인지에 대해)

```solidity
  function _multiply(uint a, uint b) private pure returns (uint) {
    return a * b;
}
```

*\_generateRandomDna*라는 함수를 *private*으로 *\_str*이라는 이름의 _string_ 타입을 인자로 받고, *uint*를 반환하는 함수를 선언

이 함수는 컨트랙트 변수를 참조할 것이지만, 변경하지는 않을 것이므로 *view*로 선언

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

    function _createZombie(string _name, uint _dna) private {
        zombies.push(Zombie(_name, _dna));
    }

    function _generateRandomDna(string _str) private view returns (uint){

    }
}
```

## Keccak256과 형 변환

해시 함수는 블록체인에서 여러 용도로 활용됨

해시함수를 의사 난수 발생기(psuedo-random number generator)로도 이용 가능

```solidity
  //6e91ec6b618bb462a4a6ee5aa2cb0e9cf30f7a052bb467b0ba58b8748c00d2e5
  keccak256("aaaab");
  //b1f078126895a1424524de5321b339ab00408010b7cf0e6ed451514981e58aa9
  keccak256("aaaac");
```

*Solidity*는 형 변환을 지원함

```solidity
  uint8 a = 5;
  uint b = 6;
  uint8 c = a * b;  // a * b가 uint8이 아닌 uint를 반환하기 때문에 에러 메시지가 난다
  uint8 c = a * uint8(b); // b를 uint8으로 형 변환해서 코드가 제대로 작동하도록 해야 한다
```

*\_str*을 이용하여 _keccak256_ 해시값을 받아서 의사 난수 16진수를 생성하고, 이를 uint로 형 변환한다음 *rand*라는 *uint*에 결과값 저장

이후, DNA를 16자리 숫자로 처리해야하므로, 위의 저장값을 *dnaModulus*를 모듈로(%) 연산하여 값을 반환함

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

    function _createZombie(string _name, uint _dna) private {
        zombies.push(Zombie(_name, _dna));
    }

    function _generateRandomDna(string _str) private view returns (uint) {
        uint rand = uint(keccak256(_str));
        return rand % dnaModulus;
    }
}
```

## 종합하기

*createRandomZombie*라는 _public_ 함수를 생성, 이 함수는 *\_name*이라는 _string_ 타입 인자를 하나 전달받음

함수의 첫 줄에서는 *\_name*을 전달받은 _\_generateRandomDna_ 함수를 호출하고, 이 함수의 반환값을 *randDna*라는 *uint*형 변수에 저장

두번째 줄에서는 _\_createZombie_ 함수를 호출하고, 이 함수에 *\_name*과 *randDna*를 전달해야 함

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

    function _createZombie(string _name, uint _dna) private {
        zombies.push(Zombie(_name, _dna));
    }

    function _generateRandomDna(string _str) private view returns (uint) {
        uint rand = uint(keccak256(_str));
        return rand % dnaModulus;
    }

    function createRandomZombie(string _name) public{
        uint randDna = _generateRandomDna(_name);
        _createZombie(_name, randDna);
    }
}
```

## 이벤트

*event*는 컨트랙트에 무언가의 이벤트 액션이 발생했을 때, 실행되는 코드를 정의

```solidity
  // 이벤트를 선언한다
  event IntegersAdded(uint x, uint y, uint result);

  function add(uint _x, uint _y) public {
    uint result = _x + _y;
    // 이벤트를 실행하여 앱에게 add 함수가 실행되었음을 알린다:
    IntegersAdded(_x, _y, result);
    return result;
  }
```

*NewZombie*라는 *evnet*를 선언

_zombieId_ (_uint_), _name_ (_string_), _dna_(_uint_)를 인자로 전달받음

_\_createZombie_ 함수를 변경하여, 새로운 좀비가 _zombies_ 배열에 추가된 후에 _NewZombie_ 이벤트를 실행하도록 함

이벤트를 위해 좀비의 *id*가 필요한데, *array.push()*가 배열의 새로운 길이를 *uint*형으로 반환하는 것을 이용함

기존에 *zombies.push(Zombie(\_name, \_dna));*의 부분을 이용한다는 뜻

배열의 첫 원소가 0 인덱스를 지니기 때문에, *array.push() - 1*은 막 추가된 좀비의 인덱스가 될 것임

*zombies.push() - 1*의 결과값을 *uint*형인 *id*에 저장하고, 이를 다음 줄에서 _NewZombie_ 이벤트 호출을 위해 활용

```solidity
pragma solidity ^0.4.19;

contract ZombieFactory {

    event NewZombie(uint zombieId, string name, uint dna); // 여기에 이벤트 선언

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    struct Zombie {
        string name;
        uint dna;
    }

    Zombie[] public zombies;

    function _createZombie(string _name, uint _dna) private {
        uint id = zombies.push(Zombie(_name, _dna)) - 1;
        NewZombie(id, _name, _dna);
    }

    function _generateRandomDna(string _str) private view returns (uint) {
        uint rand = uint(keccak256(_str));
        return rand % dnaModulus;
    }

    function createRandomZombie(string _name) public {
        uint randDna = _generateRandomDna(_name);
        _createZombie(_name, randDna);
    }
}
```

## Web3.js

컨트랙트와 상호작용하는 사용자 단의 자바스크립트 코드를 이더리움 자바스크립트 라이브러리인 *Web3.js*를 통해 작성

```javascript
// 여기에 우리가 만든 컨트랙트에 접근하는 방법을 제시한다:
var abi = /* abi generated by the compiler */
var ZombieFactoryContract = web3.eth.contract(abi)
var contractAddress = /* our contract address on Ethereum after deploying */
var ZombieFactory = ZombieFactoryContract.at(contractAddress)
// `ZombieFactory`는 우리 컨트랙트의 public 함수와 이벤트에 접근할 수 있다.

// 일종의 이벤트 리스너가 텍스트 입력값을 취한다:
$("#ourButton").click(function(e) {
  var name = $("#nameInput").val()
  // 우리 컨트랙트의 `createRandomZombie`함수를 호출한다:
  ZombieFactory.createRandomZombie(name)
})

// `NewZombie` 이벤트가 발생하면 사용자 인터페이스를 업데이트한다
var event = ZombieFactory.NewZombie(function(error, result) {
  if (error) return
  generateZombie(result.zombieId, result.name, result.dna)
})

// 좀비 DNA 값을 받아서 이미지를 업데이트한다
function generateZombie(id, name, dna) {
  let dnaStr = String(dna)
  // DNA 값이 16자리 수보다 작은 경우 앞 자리를 0으로 채운다
  while (dnaStr.length < 16)
    dnaStr = "0" + dnaStr

  let zombieDetails = {
    // 첫 2자리는 머리의 타입을 결정한다. 머리 타입에는 7가지가 있다. 그래서 모듈로(%) 7 연산을 하여
    // 0에서 6 중 하나의 값을 얻고 여기에 1을 더해서 1에서 7까지의 숫자를 만든다.
    // 이를 기초로 "head1.png"에서 "head7.png" 중 하나의 이미지를 불러온다:
    headChoice: dnaStr.substring(0, 2) % 7 + 1,
    // 두번째 2자리는 눈 모양을 결정한다. 눈 모양에는 11가지가 있다:
    eyeChoice: dnaStr.substring(2, 4) % 11 + 1,
    // 셔츠 타입에는 6가지가 있다:
    shirtChoice: dnaStr.substring(4, 6) % 6 + 1,
    // 마지막 6자리는 색깔을 결정하며, 360도(degree)까지 지원하는 CSS의 "filter: hue-rotate"를 이용하여 아래와 같이 업데이트된다:
    skinColorChoice: parseInt(dnaStr.substring(6, 8) / 100 * 360),
    eyeColorChoice: parseInt(dnaStr.substring(8, 10) / 100 * 360),
    clothesColorChoice: parseInt(dnaStr.substring(10, 12) / 100 * 360),
    zombieName: name,
    zombieDescription: "A Level 1 CryptoZombie",
  }
  return zombieDetails
}
```

## References

add here

<!-- [Array on mozzila.org](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array){: target="\_blank"} -->

<!-- ![permasecond](/assets/posts/2020-02-21-cmdcolor/permasecond.png) -->
