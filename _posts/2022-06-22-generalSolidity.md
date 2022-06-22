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

이후 Chapter 02 진행, 인간을 먹이로하여 인간의 DNA와 좀비 DNA를 결합하여 새로운 DNA의 좀비를 생성 (진화 개념)

## 매핑과 주소

온 체인에 저장된 좀비의 주인을 지정하기 위해 *mapping*과 _address_ 자료형을 활용

_mapping_ 또한 구조체와 배열과 같이 *Solidity*에서 구조화된 데이터를 저장하는 방법임

```solidity
// 금융 앱용으로, 유저의 계좌 잔액을 보유하는 uint를 저장한다:
mapping (address => uint) public accountBalance; // address가 key이고 uint가 값
// 혹은 userID로 유저 이름을 저장/검색하는 데 매핑을 쓸 수도 있다
mapping (uint => string) userIdToName; // uint가 key이고 string이 값
```

*mapping*은 기본적으로 키-값(key-value) 형식으로 저장되어 데이터를 저장하고 검색하는 데 이용됨

*zombieToOwner*라는 매핑을 키는 _uint_, 값은 *address*로 *public*으로 선언

*ownerZombieCount*라는 매핑을 키는 _address_, 값은 *uint*로 선언

```solidity
pragma solidity ^0.4.19;

contract ZombieFactory {
    event NewZombie(uint zombieId, string name, uint dna);

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    struct Zombie {
        string name;
        uint dna;
    }

    Zombie[] public zombies;

    mapping (uint => address) public zombieToOwner; // 여기서 매핑 선언
    mapping (address => uint) ownerZombieCount;

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

## Msg.sender

*msg.sender*를 통해 현재 함수를 호출한 사람 또는 호출한 스마트 컨트랙트 주소를 알 수 있음

*msg.sender*를 이용해 *mapping*을 업데이트 하는 예시

```solidity
mapping (address => uint) favoriteNumber;

function setMyNumber(uint _myNumber) public {
  // `msg.sender`에 대해 `_myNumber`가 저장되도록 `favoriteNumber` 매핑을 업데이트한다 `
  favoriteNumber[msg.sender] = _myNumber;
  // ^ 데이터를 저장하는 구문은 배열로 데이터를 저장할 떄와 동일하다
}

function whatIsMyNumber() public view returns (uint) {
  // sender의 주소에 저장된 값을 불러온다
  // sender가 `setMyNumber`을 아직 호출하지 않았다면 반환값은 `0`이 될 것이다
  return favoriteNumber[msg.sender];
}
```

우선, _zombieToOwner_ 매핑을 업데이트하여, *id*에 대하여 *msg.sender*가 저장되도록 함

이후 저장된 *msg.sender*를 고려하여 *ownerZombieCount*를 증가시킴

_Solidity_ 또한 *uint*를 *++*를 통해 증가시킬 수 있음

```solidity
pragma solidity ^0.4.19;

contract ZombieFactory {
    event NewZombie(uint zombieId, string name, uint dna);

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    struct Zombie {
        string name;
        uint dna;
    }

    Zombie[] public zombies;

    mapping (uint => address) public zombieToOwner;
    mapping (address => uint) ownerZombieCount;

    function _createZombie(string _name, uint _dna) private {
        uint id = zombies.push(Zombie(_name, _dna)) - 1;
        zombieToOwner[id] = msg.sender; // 여기서 시작
        ownerZombieCount[msg.sender]++;
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

## Require

함수 실행 간에 요구사항을 명시하여 _require_ 요건을 만족하지 못할 시 함수 실행을 중지하고 에러 메시지 반환

```solidity
function sayHiToVitalik(string _name) public returns (string) {
  // _name이 "Vitalik"인지 비교한다. 참이 아닐 경우 에러 메시지를 발생하고 함수를 벗어난다
  // (참고: 솔리디티는 고유의 스트링 비교 기능을 가지고 있지 않기 때문에
  // 스트링의 keccak256 해시값을 비교하여 스트링 값이 같은지 판단한다)
  require(keccak256(_name) == keccak256("Vitalik"));
  // 참이면 함수 실행을 진행한다:
  return "Hi!";
}
```

*require*를 활용하여, *ownerZombieCount[msg.sender]*가 *0*과 같은지 확인하고 함수 진행하도록 코딩

```solidity
pragma solidity ^0.4.19;

contract ZombieFactory {
    event NewZombie(uint zombieId, string name, uint dna);

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    struct Zombie {
        string name;
        uint dna;
    }

    Zombie[] public zombies;

    mapping (uint => address) public zombieToOwner;
    mapping (address => uint) ownerZombieCount;

    function _createZombie(string _name, uint _dna) private {
        uint id = zombies.push(Zombie(_name, _dna)) - 1;
        zombieToOwner[id] = msg.sender;
        ownerZombieCount[msg.sender]++;
        NewZombie(id, _name, _dna);
    }

    function _generateRandomDna(string _str) private view returns (uint) {
        uint rand = uint(keccak256(_str));
        return rand % dnaModulus;
    }

    function createRandomZombie(string _name) public {
        require(ownerZombieCount[msg.sender] == 0); // 여기서 시작
        uint randDna = _generateRandomDna(_name);
        _createZombie(_name, randDna);
    }
}
```

## 상속

*Solidity*는 _is_ 문법을 이용해 상속을 지원

```solidity
contract Doge {
  function catchphrase() public returns (string) {
    return "So Wow CryptoDoge";
  }
}
contract BabyDoge is Doge {
  function anotherCatchphrase() public returns (string) {
    return "Such Moon BabyDoge";
  }
}
```

_ZombieFactory_ 아래에 *ZombieFactory*를 상속하는 _ZombieFeeding_ 컨트랙트 생성

```solidity
pragma solidity ^0.4.19;

contract ZombieFactory {
    event NewZombie(uint zombieId, string name, uint dna);

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    struct Zombie {
        string name;
        uint dna;
    }

    Zombie[] public zombies;

    mapping (uint => address) public zombieToOwner;
    mapping (address => uint) ownerZombieCount;

    function _createZombie(string _name, uint _dna) private {
        uint id = zombies.push(Zombie(_name, _dna)) - 1;
        zombieToOwner[id] = msg.sender;
        ownerZombieCount[msg.sender]++;
        NewZombie(id, _name, _dna);
    }

    function _generateRandomDna(string _str) private view returns (uint) {
        uint rand = uint(keccak256(_str));
        return rand % dnaModulus;
    }

    function createRandomZombie(string _name) public {
        require(ownerZombieCount[msg.sender] == 0);
        uint randDna = _generateRandomDna(_name);
        _createZombie(_name, randDna);
    }
}

contract ZombieFeeding is ZombieFactory {  // 여기서 시작

}
```

## Import

*Solidity*는 코드를 여러 파일로 분리하여 정리할 수 있음

이 때 _import_ 키워드를 활용

```solidity
import "./someothercontract.sol";

contract newContract is SomeOtherContract {

}
```

*zombiefeeding.sol*에 *zombiefactory.sol*을 _import_ 함

```solidity
pragma solidity ^0.4.19;

import "./zombiefactory.sol"; // 여기에 import 구문을 넣기

contract ZombieFeeding is ZombieFactory {

}
```

## Storage vs Memory

*Solidity*에는 변수를 저장할 수 있는 공간으로 *storage*와 _memory_ 두 가지가 있음

*storage*는 블록체인 상에 영구적으로 저장되는 변수인 반면에, *memory*는 임시적으로 저장되는 변수임

대부분의 경우에는 *Solidity*가 변수를 알아서 처리해주기 때문에, *storage*나 _memory_ 키워드를 이용할 필요는 없음

함수 외부의 상태 변수로 선언된 변수는 *storage*로 선언되어 블록체인에 영구적으로 저장되나, 함수 내에 선언된 변수는 *memory*로 선언되어 함수 호출이 종료되면 사라짐

구조체와 배열을 처리할 때, _storage_ 및 _memory_ 키워드를 명시적으로 표시하여 유용하게 사용함

```solidity
contract SandwichFactory {
  struct Sandwich {
    string name;
    string status;
  }

  Sandwich[] sandwiches;

  function eatSandwich(uint _index) public {
    // Sandwich mySandwich = sandwiches[_index];

    // ^ 꽤 간단해 보이나, 솔리디티는 여기서
    // `storage`나 `memory`를 명시적으로 선언해야 한다는 경고 메시지를 발생한다.
    // 그러므로 `storage` 키워드를 활용하여 다음과 같이 선언해야 한다:
    Sandwich storage mySandwich = sandwiches[_index];
    // ...이 경우, `mySandwich`는 저장된 `sandwiches[_index]`를 가리키는 포인터이다.
    // 그리고
    mySandwich.status = "Eaten!";
    // ...이 코드는 블록체인 상에서 `sandwiches[_index]`을 영구적으로 변경한다.

    // 단순히 복사를 하고자 한다면 `memory`를 이용하면 된다:
    Sandwich memory anotherSandwich = sandwiches[_index + 1];
    // ...이 경우, `anotherSandwich`는 단순히 메모리에 데이터를 복사하는 것이 된다.
    // 그리고
    anotherSandwich.status = "Eaten!";
    // ...이 코드는 임시 변수인 `anotherSandwich`를 변경하는 것으로
    // `sandwiches[_index + 1]`에는 아무런 영향을 끼치지 않는다. 그러나 다음과 같이 코드를 작성할 수 있다:
    sandwiches[_index + 1] = anotherSandwich;
    // ...이는 임시 변경한 내용을 블록체인 저장소에 저장하고자 하는 경우이다.
  }
}
```

_feedAndMultiply_ 라는 함수를 생성, *public*으로 선언, *uint*형 *\_zombieId*와 *\_targetDna*를 인자로 받음

_require_ 구문을 이용하여, *msg.sender*가 좀비 주인과 동일하도록 함

먹이를 먹는 좀비 DNA를 얻기 위해, myZombie라는 Zombie형 변수를 _storage_ 키워드로 선언, 이 변수에 _zombies_ 배열의 _\_zombieId_ 인덱스가 가진 값을 할당

```solidity
pragma solidity ^0.4.19;

import "./zombiefactory.sol";

contract ZombieFeeding is ZombieFactory {
  function feedAndMultiply(uint _zombieId, uint _targetDna) public{ // 여기서 시작
    require(msg.sender == zombieToOwner[_zombieId]);
    Zombie storage myZombie = zombies[_zombieId];
  }
}
```

## 진행 챕터

해당 Chapter는 _Solidity_ 문법을 따로 다루지않고 함수 내용만 추가 (Zombie DNA)

먼저 *\_targetDna*가 16자리보다 크지 않게 해야함. 이를 위해, *\_targetDna*를 *\_targetDna % dnaModulus*와 같도록 해서, 마지막 16자리 수만 취하도록 함

그 다음, 함수가 *newDna*라는 *uint*를 선언하고 *myZombie*의 DNA와 *\_targetDna*의 평균 값 부여

새로운 DNA 값을 얻게 되면 _\_createZombie_ 함수 호출, 일단은 심시적으로 이름 인자값은 _NoName_ 으로 하도록 함

```solidity
pragma solidity ^0.4.19;

import "./zombiefactory.sol";

contract ZombieFeeding is ZombieFactory {
  function feedAndMultiply(uint _zombieId, uint _targetDna) public {
    require(msg.sender == zombieToOwner[_zombieId]);
    Zombie storage myZombie = zombies[_zombieId];
    _targetDna = _targetDna % dnaModulus; // 여기서 시작
    uint newDna = (myZombie.dna + _targetDna) / 2;
    _createZombie("NoName", newDna);
  }

}
```

## 함수 접근 제어자 더 알아보기 (Internal & External)

이전 Chapter 코드에서는 오류가 존재

_ZombieFeeding_ 컨트랙트 내에서 _\_createZombie_ 함수를 호출하려고 하면 오류가 발생

_\_createZombie_ 함수는 _ZombieFactory_ 컨트랙트 내의 _private_ 함수이기 때문임

_ZombieFactory_ 컨트랙트를 상속하는 컨트랙트라도 접근할 수 없음

*public*과 _private_ 이외에, *internal*과 *external*이라는 함수 접근 제어자가 있음

*internal*은 함수가 정의된 컨트랙트를 상속하는 컨트랙트에서도 접근이 가능하도록 하는 점을 제외하고는 *priavte*과 동일함

*external*은 함수가 컨트랙트 바깥에서만 호출될 수 있고, 컨트랙트 내의 다른 함수에 의해 호출될 수 없다는 점을 제외하면 *public*과 동일함

```solidity
contract Sandwich {
  uint private sandwichesEaten = 0;

  function eat() internal {
    sandwichesEaten++;
  }
}

contract BLT is Sandwich {
  uint private baconSandwichesEaten = 0;

  function eatWithBacon() public returns (string) {
    baconSandwichesEaten++;
    // eat 함수가 internal로 선언되었기 때문에 여기서 호출이 가능하다
    eat();
  }
}
```

_\_createZombie()_ 함수를 *private*에서 *internal*로 바꾸어 선언

```solidity
pragma solidity ^0.4.19;

contract ZombieFactory {
    event NewZombie(uint zombieId, string name, uint dna);

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    struct Zombie {
        string name;
        uint dna;
    }

    Zombie[] public zombies;

    mapping (uint => address) public zombieToOwner;
    mapping (address => uint) ownerZombieCount;

    // edit function definition below
    function _createZombie(string _name, uint _dna) internal {
        uint id = zombies.push(Zombie(_name, _dna)) - 1;
        zombieToOwner[id] = msg.sender;
        ownerZombieCount[msg.sender]++;
        NewZombie(id, _name, _dna);
    }

    function _generateRandomDna(string _str) private view returns (uint) {
        uint rand = uint(keccak256(_str));
        return rand % dnaModulus;
    }

    function createRandomZombie(string _name) public {
        require(ownerZombieCount[msg.sender] == 0);
        uint randDna = _generateRandomDna(_name);
        _createZombie(_name, randDna);
    }
}
```

## 진행 챕터

해당 Chapter는 _Solidity_ 문법을 따로 다루지않고 함수 내용만 추가 (좀비가 무엇을 먹나요?)

좀비가 크립토키티를 먹이로 하는 컨트랙트 코드를 작성

이를 위해 크립토키티의 DNA를 읽어와야함 (외부의 스마트 컨트랙트 데이터를 읽어와야함)

다른 컨트랙트와 상호작용하기 위해서는 우선 *인터페이스*를 정의 해야함

```solidity
contract LuckyNumber {
  mapping(address => uint) numbers;

  function setNum(uint _num) public {
    numbers[msg.sender] = _num;
  }

  function getNum(address _myAddress) public view returns (uint) {
    return numbers[_myAddress];
  }
}
```

위 _LuckyNumbeR_ 컨트랙트의 _getNum_ 함수를 읽고자하는 _external_ 함수를 위해 인터페이스를 아래와 같이 정의

```solidity
contract NumberInterface {
  function getNum(address _myAddress) public view returns (uint);
}
```

약간 차이가 있으나, 인터페이스를 정의하는 것과 컨트랙트를 정의하는 것은 유사

인터페이스의 경우, 다른 컨트랙트와 상호작용하고자하는 함수 만을 선언할 뿐, 다른 함수나 상태 변수를 정의하지 않음

또, *{} (중괄호)*를 이용하여 함수 몸체를 정의 하지않고, 함수 선언을 *;(세미콜론)*으로 끝마침

```solidity
function getKitty(uint256 _id) external view returns (
    bool isGestating,
    bool isReady,
    uint256 cooldownIndex,
    uint256 nextActionAt,
    uint256 siringWithId,
    uint256 birthTime,
    uint256 matronId,
    uint256 sireId,
    uint256 generation,
    uint256 genes
) {
    Kitty storage kit = kitties[_id];

    // if this variable is 0 then it's not gestating
    isGestating = (kit.siringWithId != 0);
    isReady = (kit.cooldownEndBlock <= block.number);
    cooldownIndex = uint256(kit.cooldownIndex);
    nextActionAt = uint256(kit.cooldownEndBlock);
    siringWithId = uint256(kit.siringWithId);
    birthTime = uint256(kit.birthTime);
    matronId = uint256(kit.matronId);
    sireId = uint256(kit.sireId);
    generation = uint256(kit.generation);
    genes = kit.genes;
}
```

_getKitty_ 함수가 위와 같을때, kittyInterface라는 인터페이스를 정의하고, 인터페이스 내에 _getKitty_ 함수를 선언

```solidity
pragma solidity ^0.4.19;

import "./zombiefactory.sol";

contract KittyInterface{ // 여기에 KittyInterface를 생성한다
    function getKitty(uint256 _id) external view returns (
    bool isGestating,
    bool isReady,
    uint256 cooldownIndex,
    uint256 nextActionAt,
    uint256 siringWithId,
    uint256 birthTime,
    uint256 matronId,
    uint256 sireId,
    uint256 generation,
    uint256 genes
);
}

contract ZombieFeeding is ZombieFactory {
  function feedAndMultiply(uint _zombieId, uint _targetDna) public {
    require(msg.sender == zombieToOwner[_zombieId]);
    Zombie storage myZombie = zombies[_zombieId];
    _targetDna = _targetDna % dnaModulus;
    uint newDna = (myZombie.dna + _targetDna) / 2;
    _createZombie("NoName", newDna);
  }
}
```

## 인터페이스 활용하기

인터페이스를 아래와 같이 우선 정의

```solidity
contract NumberInterface {
  function getNum(address _myAddress) public view returns (uint);
}
```

이후 아래와 같이 컨트랙트에서 인터페이스를 이용

```solidity
contract MyContract {
  address NumberInterfaceAddress = 0xab38...
  // ^ 이더리움상의 FavoriteNumber 컨트랙트 주소이다
  NumberInterface numberContract = NumberInterface(NumberInterfaceAddress)
  // 이제 `numberContract`는 다른 컨트랙트를 가리키고 있다.

  function someFunction() public {
    // 이제 `numberContract`가 가리키고 있는 컨트랙트에서 `getNum` 함수를 호출할 수 있다:
    uint num = numberContract.getNum(msg.sender);
    // ...그리고 여기서 `num`으로 무언가를 할 수 있다
  }
}
```

*ckAddress*라는 변수에 입력된 크립토키티 컨트랙트 주소를 활용하여, *kittyContract*라는 *KittyInterface*를 생성하고, 앞선 예제의 _numberContract_ 선언과 같이 초기화

```solidity
pragma solidity ^0.4.19;

import "./zombiefactory.sol";

contract KittyInterface {
  function getKitty(uint256 _id) external view returns (
    bool isGestating,
    bool isReady,
    uint256 cooldownIndex,
    uint256 nextActionAt,
    uint256 siringWithId,
    uint256 birthTime,
    uint256 matronId,
    uint256 sireId,
    uint256 generation,
    uint256 genes
  );
}

contract ZombieFeeding is ZombieFactory {

  address ckAddress = 0x06012c8cf97BEaD5deAe237070F9587f8E7A266d;
  KittyInterface kittyContract = KittyInterface(ckAddress); // `ckAddress`를 이용하여 여기에 kittyContract를 초기화한다

  function feedAndMultiply(uint _zombieId, uint _targetDna) public {
    require(msg.sender == zombieToOwner[_zombieId]);
    Zombie storage myZombie = zombies[_zombieId];
    _targetDna = _targetDna % dnaModulus;
    uint newDna = (myZombie.dna + _targetDna) / 2;
    _createZombie("NoName", newDna);
  }
}
```

## 다수의 반환값 처리하기

다수의 반환값을 처리하는 예제

```solidity
function multipleReturns() internal returns(uint a, uint b, uint c) {
  return (1, 2, 3);
}

function processMultipleReturns() external {
  uint a;
  uint b;
  uint c;
  // 다음과 같이 다수 값을 할당한다:
  (a, b, c) = multipleReturns();
}

// 혹은 단 하나의 값에만 관심이 있을 경우:
function getLastReturnValue() external {
  uint c;
  // 다른 필드는 빈칸으로 놓기만 하면 된다:
  (,,c) = multipleReturns();
}
```

*feedOnKitty*라는 함수를 생성, _uint_ 타입의 *\_zombieId*와 _\_kittyId_ 인자를 받고 *public*으로 선언

이후 _uint_ 형의 _kittyDna_ 변수를 함수 내부에 선언

그 이후 *\_kittyId*를 *kittyContract.getKitty*에 전달하여 호출하고, *genes*를 *kittyDna*에 저장해야함

이 때, *getKitty*가 다수의 변수를 반환하므로, 우리가 원하는 _genes_ 반환값의 위치를 파악하고 _,_ 수를 적절히 입력 (10번째 변수이므로 9개의 *,*입력)

이후 마지막으로 _feedAndMultiply_ 함수를 호출함

```solidity
pragma solidity ^0.4.19;

import "./zombiefactory.sol";

contract KittyInterface {
  function getKitty(uint256 _id) external view returns (
    bool isGestating,
    bool isReady,
    uint256 cooldownIndex,
    uint256 nextActionAt,
    uint256 siringWithId,
    uint256 birthTime,
    uint256 matronId,
    uint256 sireId,
    uint256 generation,
    uint256 genes
  );
}

contract ZombieFeeding is ZombieFactory {
  address ckAddress = 0x06012c8cf97BEaD5deAe237070F9587f8E7A266d;
  KittyInterface kittyContract = KittyInterface(ckAddress);

  function feedAndMultiply(uint _zombieId, uint _targetDna) public {
    require(msg.sender == zombieToOwner[_zombieId]);
    Zombie storage myZombie = zombies[_zombieId];
    _targetDna = _targetDna % dnaModulus;
    uint newDna = (myZombie.dna + _targetDna) / 2;
    _createZombie("NoName", newDna);
  }

  function feedOnKitty(uint _zombieId, uint _kittyId) public{  // 여기에 함수를 정의
    uint kittyDna;
    (,,,,,,,,,kittyDna) = kittyContract.getKitty(_kittyId);
    feedAndMultiply(_zombieId, kittyDna);
  }
}
```

## 진행 챕터

해당 Chapter는 _Solidity_ 문법을 따로 다루지않고 함수 내용만 추가 (보너스: 키티 유전자)

고양이 유전자와 조합되어 생성된 좀비가 몇개의 독특한 특성을 가져서 고양이 좀비로 보이도록 함

DNA에 몇가지 키티 코드를 추가함

좀비의 외모를 결정하는 16자리의 DNA 중에서 처음 12자리만 이용되므로, 마지막 2자리 숫자를 활용하여 고양이 특성을 만듦

고양이 좀비는 DNA 마지막 2자리로 *99*를 갖는다고 설정 *if*문을 활용하여 좀비가 고양이에서 생성되면 DNA를 *99*로 처리하도록 코딩

*Solidity*에서, *string*간의 동일 여부를 판단하기 위해서는 _keccak256_ 해시 함수를 이용해야함

```solidity
function eatBLT(string sandwich) public {
  // 스트링 간의 동일 여부를 판단하기 위해 keccak256 해시 함수를 이용해야 한다는 것을 기억하자
  if (keccak256(sandwich) == keccak256("BLT")) {
    eat();
  }
}
```

먼저 _feedAndMultiply_ 함수 정의를 변경하여 *\_species*라는 _string_ 타입을 세번째 인자로 전달받도록 함

그 다음, *if*문을 통해 새로운 좀비 DNA 계산 이후에 *\_species*와 _kiity_ 스트링의 _keccak256_ 해시값을 비교하도록 함

*if*문 내에서 DNA 마지막 2자리를 *99*로 대체하도록 함

이 때, _newDna = newDna - newDna % 100 + 99;_ 로직을 이용

설명: newDna가 334455라고 하면 newDna % 100는 55이고, 따라서 newDna - newDna % 100는 334400이다. 마지막으로 여기에 99를 더하면 334499를 얻게 된다.

마지막으로, _feedOnKitty_ 함수 내에서 이루어지는 함수 호출을 변경한다. *feedAndMultiply*가 호출될 때, *"kiity"*를 마지막 인자값으로 전달함

```solidity
pragma solidity ^0.4.19;

import "./zombiefactory.sol";

contract KittyInterface {
  function getKitty(uint256 _id) external view returns (
    bool isGestating,
    bool isReady,
    uint256 cooldownIndex,
    uint256 nextActionAt,
    uint256 siringWithId,
    uint256 birthTime,
    uint256 matronId,
    uint256 sireId,
    uint256 generation,
    uint256 genes
  );
}

contract ZombieFeeding is ZombieFactory {

  address ckAddress = 0x06012c8cf97BEaD5deAe237070F9587f8E7A266d;
  KittyInterface kittyContract = KittyInterface(ckAddress);

  // 여기에 있는 함수 정의를 변경:
  function feedAndMultiply(uint _zombieId, uint _targetDna, string _species) public {
    require(msg.sender == zombieToOwner[_zombieId]);
    Zombie storage myZombie = zombies[_zombieId];
    _targetDna = _targetDna % dnaModulus;
    uint newDna = (myZombie.dna + _targetDna) / 2;
    if (keccak256(_species) == keccak256("kitty")){ // 여기에 if 문 추가
        newDna = newDna - newDna % 100 + 99;
    }
    _createZombie("NoName", newDna);
  }

  function feedOnKitty(uint _zombieId, uint _kittyId) public {
    uint kittyDna;
    (,,,,,,,,,kittyDna) = kittyContract.getKitty(_kittyId);
    // 여기에 있는 함수 호출을 변경:
    feedAndMultiply(_zombieId, kittyDna, "kitty");
  }
}
```

## 진행 챕터

해당 Chapter는 _Solidity_ 문법을 따로 다루지않고 함수 내용만 추가 (마무리하기 Wrapping It Up)

자바스크립트를 활용한 구현 (https://cryptozombies.io/ko/lesson/2/chapter/14)

```javascript
var abi = /* abi generated by the compiler */
var ZombieFeedingContract = web3.eth.contract(abi)
var contractAddress = /* our contract address on Ethereum after deploying */
var ZombieFeeding = ZombieFeedingContract.at(contractAddress)

// 우리 좀비의 ID와 타겟 고양이 ID를 가지고 있다고 가정하면
let zombieId = 1;
let kittyId = 1;

// 크립토키티의 이미지를 얻기 위해 웹 API에 쿼리를 할 필요가 있다.
// 이 정보는 블록체인이 아닌 크립토키티 웹 서버에 저장되어 있다.
// 모든 것이 블록체인에 저장되어 있으면 서버가 다운되거나 크립토키티 API가 바뀌는 것이나
// 크립토키티 회사가 크립토좀비를 싫어해서 고양이 이미지를 로딩하는 걸 막는 등을 걱정할 필요가 없다 ;)
let apiUrl = "https://api.cryptokitties.co/kitties/" + kittyId
$.get(apiUrl, function(data) {
  let imgUrl = data.image_url
  // 이미지를 제시하기 위해 무언가를 한다
})

// 유저가 고양이를 클릭할 때:
$(".kittyImage").click(function(e) {
  // 우리 컨트랙트의 `feedOnKitty` 메소드를 호출한다
  ZombieFeeding.feedOnKitty(zombieId, kittyId)
})

// 우리의 컨트랙트에서 발생 가능한 NewZombie 이벤트에 귀를 기울여서 이벤트 발생 시 이벤트를 제시할 수 있도록 한다:
ZombieFactory.NewZombie(function(error, result) {
  if (error) return
  // 이 함수는 레슨 1에서와 같이 좀비를 제시한다:
  generateZombie(result.zombieId, result.name, result.dna)
})
```

## References

add here

<!-- [Array on mozzila.org](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array){: target="\_blank"} -->

<!-- ![permasecond](/assets/posts/2020-02-21-cmdcolor/permasecond.png) -->
