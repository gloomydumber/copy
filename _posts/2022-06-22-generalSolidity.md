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

위 _LuckyNumber_ 컨트랙트의 _getNum_ 함수를 읽고자하는 _external_ 함수를 위해 인터페이스를 아래와 같이 정의

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

## 컨트랙트의 불변성

컨트랙트에 코드를 배포하고나면 컨트랙트는 불변(Immutable)임

이에, DApp에서 컨트랙트 주소를 바꿀 수 있도록 외부 의존성 처리를 해두는 것이 좋음

즉, 어떤 컨트랙트 주소를 다루어야 한다면, *address contractAddress = 0x...*과 같으로 컨트랙트 내부에 직접 주소를 대입해서 선언하는 것보다,

컨트랙트 주소가 변할 가능성을 열어두고 변수로 처리

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

  // 1. 이 줄을 지우게:
  address ckAddress = 0x06012c8cf97BEaD5deAe237070F9587f8E7A266d;
  // 2. 여기서 대입을 빼고 그냥 선언으로 바꾸게:
  KittyInterface kittyContract = KittyInterface(ckAddress);

  // 3. 여기 setKittyContractAddress 메소드를 추가하게

  function feedAndMultiply(uint _zombieId, uint _targetDna, string _species) public {
    require(msg.sender == zombieToOwner[_zombieId]);
    Zombie storage myZombie = zombies[_zombieId];
    _targetDna = _targetDna % dnaModulus;
    uint newDna = (myZombie.dna + _targetDna) / 2;
    if (keccak256(_species) == keccak256("kitty")) {
      newDna = newDna - newDna % 100 + 99;
    }
    _createZombie("NoName", newDna);
  }

  function feedOnKitty(uint _zombieId, uint _kittyId) public {
    uint kittyDna;
    (,,,,,,,,,kittyDna) = kittyContract.getKitty(_kittyId);
    feedAndMultiply(_zombieId, kittyDna, "kitty");
  }

}
```

위의 코드에서,

*ckAddress*를 지우고, *kittyContract*를 어떤 것도 대입하지 않고 변수 선언만 하도록 함

*setKittyContractAddress*라는 이름의 함수를 생성하고, _address_ 타입의 *\_address*를 인자로 받고 *external*로 선언

함수 내에서 *kittyContract*에 \*KittyInterface(\_address)를 대입하는 코드 작성

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
  KittyInterface kittyContract;

  function setKittyContractAddress(address _address) external {
    kittyContract = KittyInterface(_address);
  }

  function feedAndMultiply(uint _zombieId, uint _targetDna, string _species) public {
    require(msg.sender == zombieToOwner[_zombieId]);
    Zombie storage myZombie = zombies[_zombieId];
    _targetDna = _targetDna % dnaModulus;
    uint newDna = (myZombie.dna + _targetDna) / 2;
    if (keccak256(_species) == keccak256("kitty")) {
      newDna = newDna - newDna % 100 + 99;
    }
    _createZombie("NoName", newDna);
  }

  function feedOnKitty(uint _zombieId, uint _kittyId) public {
    uint kittyDna;
    (,,,,,,,,,kittyDna) = kittyContract.getKitty(_kittyId);
    feedAndMultiply(_zombieId, kittyDna, "kitty");
  }
}
```

## 소유 가능한 컨트랙트

앞선 예제에서 *setKittyContractAddress*는 *external*로 선언됨

즉, 누구든 이 함수를 호출할 수 있다는 의미

따라서, 컨트랙트를 소유 가능하게 만들어서 특별한 권리를 가지게 해서 소유자만이 호출 가능하도록 해야함

아래는 _OpenZepplin_ 솔리디티 라이브러리에서 가져온 _Ownable_ 컨트랙트임

```solidity
/**
 * @title Ownable
 * @dev The Ownable contract has an owner address, and provides basic authorization control
 * functions, this simplifies the implementation of "user permissions".
 */
contract Ownable {
  address public owner;
  event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

  /**
   * @dev The Ownable constructor sets the original `owner` of the contract to the sender
   * account.
   */
  function Ownable() public {
    owner = msg.sender;
  }

  /**
   * @dev Throws if called by any account other than the owner.
   */
  modifier onlyOwner() {
    require(msg.sender == owner);
    _;
  }

  /**
   * @dev Allows the current owner to transfer control of the contract to a newOwner.
   * @param newOwner The address to transfer ownership to.
   */
  function transferOwnership(address newOwner) public onlyOwner {
    require(newOwner != address(0));
    OwnershipTransferred(owner, newOwner);
    owner = newOwner;
  }
}
```

여기서 *Solidity*의 몇가지 새로운 문법 및 요소를 알 수 있음

_생성자 (Constructor)_ : *function Ownable()*은 컨트랙트와 동일한 이름을 가진 함수로서 생성자임

생성자는 생략할 수 있음, 생성자 함수는 컨트랙트가 생성될 때 딱 한번만 실행 됨

_함수 제어자(Function Modifier)_ : *modifier onlyOwner()*는 제어자로서, 다른 함수들에 대한 접근을 제어하기 위해 사용되는 일종의 유사 함수임

보통 함수 실행 전의 요구사항 충족 여부를 확인하는 데에 사용 *onlyOwner*의 경우, 오직 컨트랙트의 소유자만 해당 함수를 실행할 수 있도록 접근을 제한

_indexed_ 키워드 : 이후에 다시 알아보고 지금은 무시

위의 _Ownable_ 컨트랙트는 다음과 같은 것들을 수행

1. 컨트랙트가 생성되면 컨트랙트의 생성자가 *owner*에 *msg.sender(컨트랙트를 배포한 사람)*를 대입

2. 특정한 함수들에 대해서 오직 *소유자*만 접근할 수 있도록 제한 가능한 _onlyOwner_ 제어자를 추가함

3. 새로운 *소유자*에게 해당 컨트랙트의 소유권을 옮길 수 있도록 함

*onlyOwner*는 컨트랙트에서 흔히 쓰이는 것 중 하나라, 대부분의 솔리디티 DApp들은 _Ownable_ 컨트랙트를 복사/붙여넣기 하면서 시작됨

이후, 첫 컨트랙트는 이 컨트랙트를 상속해서 만듦

아래 코드로 실습, _Ownable_ 컨트랙트를 활용하여 접근 제한

우리 코드가 *ownable.sol*의 내용을 *import*하게 하고, _ZombieFactory_ 컨트랙트가 _Ownable_ 컨트랙트를 상속 하도록 수정

```solidity
pragma solidity ^0.4.19;

import "./ownable.sol"; // 1. 여기서 import하게

contract ZombieFactory is Ownable { // 2. 상속을 추가하게
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
        randDna = randDna - randDna % 100;
        _createZombie(_name, randDna);
    }
}
```

## onlyOwner 함수 제어자

_ZombieFactory_ 컨트랙트가 _Ownable_ 컨트랙트를 상속하고 있으니, 이제 _onlyOwner_ 함수 제어자를 _ZombieFeeding_ 에서도 사용할 수 있음

```solidity
ZombieFeeding is ZombieFactory
ZombieFactory is Ownable
```

상속의 구조가 위와 같기 때문임

함수 제어자는 함수처럼 보이지만, _function_ 키워드가 아니라 _modifier_ 키워드를 사용

*modifer*를 함수 호출하듯이 호출하는 것은 불가능

대신에 함수 정의부 끝에 붙여서 해당 함수의 작동 방식을 제어함

```solidity
/**
 * @dev Throws if called by any account other than the owner.
 */
modifier onlyOwner() {
  require(msg.sender == owner);
  _;
}
```

위의 제어자를 아래와 같은 방식으로 사용

```solidity
contract MyContract is Ownable {
  event LaughManiacally(string laughter);

  // 아래 `onlyOwner`의 사용 방법을 잘 보게:
  function likeABoss() external onlyOwner {
    LaughManiacally("Muahahahaha");
  }
}
```

_likeABoss_ 함수를 호출하면, _onlyOwner_ 제어자 부분의 코드가 먼저 실행됨

그리고 *onlyOwner*의 _\_;_ 부분을 _likeABoss_ 함수로 되돌아가 해당 코드를 실행하게 됨

*\_;*는 *modifier*가 붙은 원본 함수를 실행시키는 위치로 이해하면 됨

아래의 실습에서 _onlyOwner_ 제어자를 *setKittyContractAddress*에 추가해봄

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
  KittyInterface kittyContract;

  // 이 함수를 수정하게:
  function setKittyContractAddress(address _address) external onlyOwner{
    kittyContract = KittyInterface(_address);
  }

  function feedAndMultiply(uint _zombieId, uint _targetDna, string _species) public {
    require(msg.sender == zombieToOwner[_zombieId]);
    Zombie storage myZombie = zombies[_zombieId];
    _targetDna = _targetDna % dnaModulus;
    uint newDna = (myZombie.dna + _targetDna) / 2;
    if (keccak256(_species) == keccak256("kitty")) {
      newDna = newDna - newDna % 100 + 99;
    }
    _createZombie("NoName", newDna);
  }

  function feedOnKitty(uint _zombieId, uint _kittyId) public {
    uint kittyDna;
    (,,,,,,,,,kittyDna) = kittyContract.getKitty(_kittyId);
    feedAndMultiply(_zombieId, kittyDna, "kitty");
  }
}
```

## 가스(Gas)

누군가가 무한 반복문을 써서 네트워크를 방해하거나, 자원 소모가 큰 연산을 써서 네트워크 자원을 모두 사용하지 못하도록 만드는 것을 방지하기위해 도입

가스를 아끼기 위한 구조체 압축이 필요함

한편, *uint*에는 _uint8_, _uint16_, _uint32_ 등의 하위 타입이 있으나, *uint*의 크기에 관계없이 256비트의 저장 공간을 할당하기 때문에 저장 공간 측면에서는 의미가 없음

즉, _uint(uint256)_ 대신에 *uint8*을 쓴다고해서 가스 소모를 줄이는데 영향이 없음

그러나 예외가 있는데, 구조차 안에서 여러개의 _uint_ 변수를 사용할 경우, 이 때는 더 작은 크기의 *uint*를 사용하는 것이 저장 공간을 실질적으로 줄임

따라서 구조체 안에서의 변수 할당은 가스 비용에 영향을 끼침

또, *uint c; uint32 a; uint32 b;*가 *uint32 a; uintc; uint32 b;*로 구성된 구조체 보다 가스를 덜 소모함

_uint32_ 형태의 변수 선언 순서가 영향을 끼침

```solidity
struct NormalStruct {
  uint a;
  uint b;
  uint c;
}

struct MiniMe {
  uint32 a;
  uint32 b;
  uint c;
}

// `mini`는 구조체 압축을 했기 때문에 `normal`보다 가스를 조금 사용할 것이네.
NormalStruct normal = NormalStruct(10, 20, 30);
MiniMe mini = MiniMe(10, 20, 30);
```

실습에서는, Zombie 구조체에 2개의 속성인 _level(uint32)_, *readyTime(uint32)*을 더 추가함

```solidity
pragma solidity ^0.4.19;

import "./ownable.sol";

contract ZombieFactory is Ownable {
    event NewZombie(uint zombieId, string name, uint dna);

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    struct Zombie {
        string name;
        uint dna;
        uint32 level; // 여기 새 데이터를 입력하게
        uint32 readyTime;
    }

    Zombie[] public zombies;

    mapping (uint => address) public zombieToOwner;
    mapping (address => uint) ownerZombieCount;

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
        randDna = randDna - randDna % 100;
        _createZombie(_name, randDna);
    }
}
```

## 시간 단위

솔리디티는 _now_, _seconds_, _minutes_, _hours_, _days_, _weeks_, _years_ 같은 시간 단위를 제공함

*now*는 현재시간을 32비트 유닉스 타임스탬프로 제공 (다만, 타입은 *uint256*형으로 제공함. 필요한 경우 *uint32*로 형 변환을 해야함)

다른 시간 단위들은 그에 해당하는 초 단위의 _uint_ 숫자로 변환됨

가령, *1 hours*는 *3600*의 *uint*형으로 변환됨

```solidity
uint lastUpdated;

// `lastUpdated`를 `now`로 설정
function updateTimestamp() public {
  lastUpdated = now;
}

// 마지막으로 `updateTimestamp`가 호출된 뒤 5분이 지났으면 `true`를, 5분이 아직 지나지 않았으면 `false`를 반환
function fiveMinutesHavePassed() public view returns (bool) {
  return (now >= (lastUpdated + 5 minutes));
}
```

실습에서는, 좀비들이 공격하거나 먹이를 먹은 후 1일이 지나야만 다시 공격하거나 먹이를 먹을 수 있는 _cooldown_ 기능을 위해 시간 단위 변수들을 활용

우선 *uint*형의 *cooldownTime*이라는 변수를 선언하고, *1 days*를 할당

이후, _Zombie_ 구조체에 *level*과 *readyTime*을 추가한 것을 고려하여, _\_createZombie()_ 함수를 업데이트함

즉, *zombie.push*에 2개의 인수를 추가 (_1 (level)_, _uint32(now + cooldownTime) (readyTime)_))

```solidity
pragma solidity ^0.4.19;

import "./ownable.sol";

contract ZombieFactory is Ownable {
    event NewZombie(uint zombieId, string name, uint dna);

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;
    uint cooldownTime = 1 days; // 1. `cooldownTime`을 여기에 정의하게

    struct Zombie {
        string name;
        uint dna;
        uint32 level;
        uint32 readyTime;
    }

    Zombie[] public zombies;

    mapping (uint => address) public zombieToOwner;
    mapping (address => uint) ownerZombieCount;

    function _createZombie(string _name, uint _dna) internal {
        // 2. 아래 줄을 업데이트하게:
        uint id = zombies.push(Zombie(_name, _dna, 1, uint32(now + cooldownTime))) - 1;
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
        randDna = randDna - randDna % 100;
        _createZombie(_name, randDna);
    }
}
```

## 진행 챕터

해당 Chapter는 _Solidity_ 문법을 따로 다루지않고 함수 내용만 추가 (좀비 재사용 대기 시간)

*feedAndMultiPly*를 다음과 같이 수정할 것임

1. 먹이를 먹으면 좀비가 재사용 대기에 들어가고,

2. 좀비는 재사용 대기 시간이 지날 때까지 고양이들을 먹을 수 없음

먼저, 우리가 좀비의 *readyTime*을 설정하고 확인할 수 있도록 하는 헬퍼 함수를 정의 할 것임

한편, 구조체를 인수로 전달할 수 있음

_private_ 또는 _internal_ 함수에 인수로서 구조체의 storage 포인터를 전달할 수 있음

```solidity
function _doStuff(Zombie storage _zombie) internal {
  // _zombie로 할 수 있는 것들을 처리
}
```

실습은 우선, _triggerCooldown_ 함수를 정의, _Zombie storage_ 포인터 타입인 *\_zombie*를 인수로 받고, *internal*임

함수의 내용으로, _\_zombie.readyTime_ 을 *uint32(noew + cooldownTime)*으로 설정

다음으로, _\_isReady_ 라는 함수를 정의, _\_zombie_ 라는 이름의 _Zombie storage_ 타입을 인수로 받고, *internal view*이며 *bool*을 리턴

함수의 내용으로, *(\_zombie.readyTime <= now)*를 리턴하고 이 값은 _true_ 혹은 *false*일 것임

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
  KittyInterface kittyContract;

  function setKittyContractAddress(address _address) external onlyOwner {
    kittyContract = KittyInterface(_address);
  }

  function _triggerCooldown(Zombie storage _zombie) internal { // 1. `_triggerCooldown` 함수를 여기에 정의하게
    _zombie.readyTime = uint32(now + cooldownTime);
  }

  function _isReady(Zombie storage _zombie) internal view returns (bool){ // 2. `_isReady` 함수를 여기에 정의하게
    return (_zombie.readyTime <= now);
  }

  function feedAndMultiply(uint _zombieId, uint _targetDna, string _species) public {
    require(msg.sender == zombieToOwner[_zombieId]);
    Zombie storage myZombie = zombies[_zombieId];
    _targetDna = _targetDna % dnaModulus;
    uint newDna = (myZombie.dna + _targetDna) / 2;
    if (keccak256(_species) == keccak256("kitty")) {
      newDna = newDna - newDna % 100 + 99;
    }
    _createZombie("NoName", newDna);
  }

  function feedOnKitty(uint _zombieId, uint _kittyId) public {
    uint kittyDna;
    (,,,,,,,,,kittyDna) = kittyContract.getKitty(_kittyId);
    feedAndMultiply(_zombieId, kittyDna, "kitty");
  }
}
```

## Public 함수 & 보안

보안을 점검하는 좋은 방법은, 코딩한 모든 *public*과 _external_ 함수를 검사하고, 사용자들이 그 함수들을 남용할 수 있는 방법이 있는지 고민해보는 것임

_onlyOwner_ 같은 제어자를 갖지 않는 이상, 어떤 사용자든 이 함수들을 호출할 수 있음을 주의

실습에서는, _feedAndMultiply_ 함수의 경우 유저가 마음대로, 인자인 *\_targetDna*나 *\_species*의 값을 정해서 전달할 수 있음

이 함수를 고려해볼때, 오직 *feedOnKitty()*에 의해서만 호출이 될 필요가 있음

따라서 함수를 *internal*로 처리하는 것이 옳은 방법임

우선, *feedAndMultiply*를 *public*에서 *internal*로 수정

이후, *feedAndMultiply*가 *cooldownTime*을 고려하도록 코딩해야함

먼저, *myZombie*를 찾은후에, *\_isReady()*를 확인하는 _require_ 문을 추가하고, 거기에 *myZombie*를 전달

이렇게 하면 좀비의 재사용 대기 시간이 끝난 다음에만 함수 호출이 가능

함수의 끝에서 *\_triggerCooldown(myZombie)*로 함수 호출하여, 먹이를 먹는 것이 좀비의 재사용 대기시간을 만들도록 함

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
  KittyInterface kittyContract;

  function setKittyContractAddress(address _address) external onlyOwner {
    kittyContract = KittyInterface(_address);
  }

  function _triggerCooldown(Zombie storage _zombie) internal {
    _zombie.readyTime = uint32(now + cooldownTime);
  }

  function _isReady(Zombie storage _zombie) internal view returns (bool) {
      return (_zombie.readyTime <= now);
  }

  // 1. 이 함수를 internal로 만들게
  function feedAndMultiply(uint _zombieId, uint _targetDna, string _species) internal {
    require(msg.sender == zombieToOwner[_zombieId]);
    Zombie storage myZombie = zombies[_zombieId];
    require(_isReady(myZombie)); // 2. 여기에 `_isReady`를 확인하는 부분을 추가하게
    _targetDna = _targetDna % dnaModulus;
    uint newDna = (myZombie.dna + _targetDna) / 2;
    if (keccak256(_species) == keccak256("kitty")) {
      newDna = newDna - newDna % 100 + 99;
    }
    _createZombie("NoName", newDna);
    _triggerCooldown(myZombie); // 3. `_triggerCooldown`을 호출하게
  }

  function feedOnKitty(uint _zombieId, uint _kittyId) public {
    uint kittyDna;
    (,,,,,,,,,kittyDna) = kittyContract.getKitty(_kittyId);
    feedAndMultiply(_zombieId, kittyDna, "kitty");
  }
}
```

## 함수 제어자의 또 다른 특징

함수 제어자는 인수 또한 받을 수 있음

```solidity
// 사용자의 나이를 저장하기 위한 매핑
mapping (uint => uint) public age;

// 사용자가 특정 나이 이상인지 확인하는 제어자
modifier olderThan(uint _age, uint _userId) {
  require (age[_userId] >= _age);
  _;
}

// 차를 운전하기 위햐서는 16살 이상이어야 하네(적어도 미국에서는).
// `olderThan` 제어자를 인수와 함께 호출하려면 이렇게 하면 되네:
function driveCar(uint _userId) public olderThan(16, _userId) {
  // 필요한 함수 내용들
}
```

실습에서는, 좀비의 특정 레벨에 도달할 때 특정 능력을 부여하는 용도로 함수 제어자의 인수 활용

우선 _zombiehelper.sol_ 을 생성

_ZombieHelper_ 컨트랙트에서, _aboveLevel_ 이라는 _modifier_ 생성

이 *modifer(제어자)*는 _\_level(uint)_, _\_zombieId(uint)_ 두 개의 인수를 받음

함수 내용에서는 *zombies[_zombieId].level*이 _\_level_ 이상인지 확실하게 확인

_modifier_ 사용 시 _\_;_ 위치를 꼭 고려

```solidity
pragma solidity ^0.4.19;

import "./zombiefeeding.sol";

contract ZombieHelper is ZombieFeeding {

  modifier aboveLevel(uint _level, uint _zombieId){ // 여기서 시작하게
    require(zombies[_zombieId].level >= _level);
    _;
  }
}
```

## 진행 챕터

해당 Chapter는 _Solidity_ 문법을 따로 다루지않고 함수 내용만 추가 (좀비 제어자)

_aboveLevel_ 제어자를 활용하여 다음 기능을 부여

레벨 2 이상인 좀비인 경우, 사용자들은 그 좀비의 이름을 바꿀 수 있도록 함

레벨 20 이상인 좀비인 경우, 사용자들은 그 좀비에게 임의의 DNA를 줄 수 있음

다음은 예제 코드임

```solidity
// 사용자의 나이를 저장하기 위한 매핑
mapping (uint => uint) public age;

// 사용자가 특정 나이 이상인지 확인하는 제어자
modifier olderThan(uint _age, uint _userId) {
  require (age[_userId] >= _age);
  _;
}

// 차를 운전하기 위햐서는 16살 이상이어야 하네(적어도 미국에서는).
function driveCar(uint _userId) public olderThan(16, _userId) {
  // 필요한 함수 내용들
}
```

실습에서는, _changeName_ 이라는 함수를 만드는데, _\_zombieId(uint)_, *\_newName(string)*을 인자로 받고 *external*이며 _aboveLevel_ 제어자를 가짐

제어자에서, *\_level*에는 _2_ 값을 전달해야하고, _\_zombieId_ 또한 전달 되어야 함

함수의 내용에서는, *require*문을 활용하여 *msg.sender*가 *zombieToOwner[_zombieId]*와 같은지 검증

이후, *zombies[_zombieId].name*에 *\_newName*을 대입해야함

_changeName_ 함수 아래에, *changeDna*라는 또다른 함수를 *external*로 만듦

*changeName*과 거의 유사하나, 두 번째 인수를 *\_newDna(uint)*로 받고, *aboveLevel*의 _\_level_ 매개 변수에 *20*을 전달해야함

이후, 좀비의 *dna*를 *\_newDna*로 설정

```solidity
pragma solidity ^0.4.19;

import "./zombiefeeding.sol";

contract ZombieHelper is ZombieFeeding {
  modifier aboveLevel(uint _level, uint _zombieId) {
    require(zombies[_zombieId].level >= _level);
    _;
  }

  function changeName(uint _zombieId, string _newName) external aboveLevel(2, _zombieId){ // 여기서 시작하게
    require(msg.sender == zombieToOwner[_zombieId]);
    zombies[_zombieId].name = _newName;
  }

  function changeDna(uint _zombieId, uint _newDna) external aboveLevel(20, _zombieId){
    require(msg.sender == zombieToOwner[_zombieId]);
    zombies[_zombieId].dna = _newDna;
  }
}
```

## View 함수를 사용해 가스 절약하기

_view_ 함수는 사용자에 의해 외부에서 호출되었을 때 가스를 전혀 소모하지 않는다

이는 _view_ 함수가 블록체인 상에서 데이터를 읽기만 하고, 어떤 것도 수정하지 않기 때문이다

즉, 함수에 *view*를 부여하는 것은, *web3.js*에 "이 함수는 실행할 때 로컬 이더리움 노드에 질의만 날리면 되지, 트랜잭션을 만들 필요는 없다"라는 얘기를 하는 것과 같다

참고로, _view_ 함수가 동일 컨트랙트 내에 있는, _view_ 함수가 아닌 다른 함수에서 내부적으로 호출될 경우에는 가스를 소모할 것임

즉, _view_ 함수는 외부에서 호출됐을 때에만 무료이다

실습에서, 사용자의 전체 좀비를 볼 수 있는 메소드인 *getZombiesByOwner*를 만들어 본다

*getZombiesByOwner*라는 이름의 함수를 만들되, _address_ 타입의 *\_owner*를 인수로 받고 _external view_ 함수로 구성하여 호출 시 가스를 쓸 필요없도록 한다

반환값은 _uint[]_ 를 반환하도록한다

```solidity
pragma solidity ^0.4.19;

import "./zombiefeeding.sol";

contract ZombieHelper is ZombieFeeding {
  modifier aboveLevel(uint _level, uint _zombieId) {
    require(zombies[_zombieId].level >= _level);
    _;
  }

  function changeName(uint _zombieId, string _newName) external aboveLevel(2, _zombieId) {
    require(msg.sender == zombieToOwner[_zombieId]);
    zombies[_zombieId].name = _newName;
  }

  function changeDna(uint _zombieId, uint _newDna) external aboveLevel(20, _zombieId) {
    require(msg.sender == zombieToOwner[_zombieId]);
    zombies[_zombieId].dna = _newDna;
  }

  function getZombiesByOwner(address _owner) external view returns (uint[]){ // 자네의 함수를 여기에 만들게

  }

}
```

## Storage는 비싸다

비용을 최소화하기 위해서, 진짜 필요한 경우가 아니면 storage에 데이터를 쓰지 않는 것이 좋음

이를 위해, 때때로는 겉보기에 비효율적으로 보이는 프로그래밍 구성을 할 필요가 있음

예를들면, 어떤 배열에서 내용을 찾기 위해, 단순히 변수에 저장하는 것 대신 함수가 호출될 때마다 배열을 memory에서 만드는 것과 같은 경우다

이에 storage를 사용하지 않고 메모리에 배열을 선언하는 법을 알아본다

이를 위해 단순히 배열에 _memory_ 키워드를 사용해준다

단, 메모리 배열은 고정된 크기의 인수와 함께 생성되어야함

예제의 경우 다음과 같다

```solidity
function getArray() external pure returns(uint[]) {
  // 메모리에 길이 3의 새로운 배열을 생성한다.
  uint[] memory values = new uint[](3);
  // 여기에 특정한 값들을 넣는다.
  values.push(1);
  values.push(2);
  values.push(3);
  // 해당 배열을 반환한다.
  return values;
}
```

실습의 경우, 우선, *result*라는 이름의 _uint[] memory_ 변수를 선언한다

해당 변수에 _uint_ 배열을 대입하고, 배열의 길이는 이 *\_owner*가 소유한 좀비의 개수여야 함

이는 *mapping*을 통해 찾을 수 있는데, *ownerZombieCount[_owner]*임

또 반환은 _result_ 값으로 함

```solidity
pragma solidity ^0.4.19;

import "./zombiefeeding.sol";

contract ZombieHelper is ZombieFeeding {
  modifier aboveLevel(uint _level, uint _zombieId) {
    require(zombies[_zombieId].level >= _level);
    _;
  }

  function changeName(uint _zombieId, string _newName) external aboveLevel(2, _zombieId) {
    require(msg.sender == zombieToOwner[_zombieId]);
    zombies[_zombieId].name = _newName;
  }

  function changeDna(uint _zombieId, uint _newDna) external aboveLevel(20, _zombieId) {
    require(msg.sender == zombieToOwner[_zombieId]);
    zombies[_zombieId].dna = _newDna;
  }

  function getZombiesByOwner(address _owner) external view returns(uint[]) {
    uint[] memory result = new uint[](ownerZombieCount[_owner]); // 여기서 시작하게
    return result;
  }
}
```

## For 반복문

이전의 실습에서와 같이, _ownerToZombies_ 배열에다가 소유한 좀비를 하나씩 저장해둔다면,

좀비의 소유권이 바뀔 때에 배열의 인덱스 하나하나를 감소시켜야하는데, 이는 굉장히 비효율적인 가스 소모를 일으킬 수 있다

이에 *view*함수로 _getZombiesByOwner_ 함수에서 _for_ 반복문을 통해 좀비 배열의 모든 요소를 접근한 후, 그것으 활용하여 *tranfer*를 시도하면

_view_ 함수이기 때문에, *storage*를 사용하지 않기 때문에 가스 비용 소모가 없을 것이다

이에 _for_ 반복문 사용법을 알아본다. 아래는 에시이다

```solidity
function getEvens() pure external returns(uint[]) {
  uint[] memory evens = new uint[](5);
  // 새로운 배열의 인덱스를 추적하는 변수
  uint counter = 0;
  // for 반복문에서 1부터 10까지 반복함
  for (uint i = 1; i <= 10; i++) {
    // `i`가 짝수라면...
    if (i % 2 == 0) {
      // 배열에 i를 추가함
      evens[counter] = i;
      // `evens`의 다음 빈 인덱스 값으로 counter를 증가시킴
      counter++;
    }
  }
  return evens; // [2, 4, 6, 8, 10]
}
```

실습에서는 _uint_ 형의 *counter*라는 이름의 변수를 선언하고 *0*을 대입한다

_result_ 배열에서 인덱스를 추적하기 위해 _counter_ 변수를 사용한다

*uint i = 0*에서 시작해서 _i < zombies.length_ 까지 증가하는 _for_ 반복문을 선언한다

_for_ 반복문 안에서, *zombieToOwner[i]*가 *\_owner*와 같은지 확인하는 _if_ 문을 만든다

_if_ 문 안에서, *result[counter]*에 *i*를 대입해서 _result_ 배열에 좀비의 ID를 추가한다

또, *counter*를 1 증가시킨다

이 경우, *\_owner*가 소유한 모든 좀비를 가스 소모없이 반환하는 함수가 완성된다

```solidity
pragma solidity ^0.4.19;

import "./zombiefeeding.sol";

contract ZombieHelper is ZombieFeeding {
  modifier aboveLevel(uint _level, uint _zombieId) {
    require(zombies[_zombieId].level >= _level);
    _;
  }

  function changeName(uint _zombieId, string _newName) external aboveLevel(2, _zombieId) {
    require(msg.sender == zombieToOwner[_zombieId]);
    zombies[_zombieId].name = _newName;
  }

  function changeDna(uint _zombieId, uint _newDna) external aboveLevel(20, _zombieId) {
    require(msg.sender == zombieToOwner[_zombieId]);
    zombies[_zombieId].dna = _newDna;
  }

  function getZombiesByOwner(address _owner) external view returns(uint[]) {
    uint[] memory result = new uint[](ownerZombieCount[_owner]);
    uint counter = 0; // 여기서 시작하게
    for (uint i = 0; i < zombies.length; i++){
      if(zombieToOwner[i] == _owner){
        result[counter] = i;
        counter++;
      }
    }
    return result;
  }
}
```

## Payable

지금까지 함수가 언제, 어디서 호출 될 수 있는지 제어하는 접근 제어자(visibility modifier)를 다뤄보았음

*private*은 컨트랙트 내부의 다른 함수들에 의해서만 호출될 수 있음을 의미

*internal*은 *prvate*과 비슷하지만, 해당 컨트랙트를 상속하는 컨트랙트에서도 호출할 수 있음을 의미

*external*은 오직 컨트랙트 외부에서만 호출 가능함

*public*은 컨트랙트 내외부 모두에서, 어디서든 호출 가능함

또한 상태 제어자(state modifier)에 관해서도 배웠음

*view*는 해당 함수를 실행해도 어떤 데이터를 저장하거나 변경하지 않음을 알려줌

*pure*는 해당 함수가 어떤 데이터도 저장하거나 변경하지 않을 뿐만아니라, 읽는 것 조차 하지않음을 알려줌, 순수 연산하는 함수임을 표현한다고 이해

*제어자*에 대해서도 배웠음. _onlyOwner_, *aboveLevel*과 같은 것들임

*제어자*에는 _payable_ 또한 존재함

_payable_ 제어자는 컨트랙트를 통해 ETH를 지불하는 기능을 구현할 때 사용함

다음은 예시 코드

```solidity
contract OnlineStore {
  function buySomething() external payable {
    // 함수 실행에 0.001이더가 보내졌는지 확실히 하기 위해 확인:
    require(msg.value == 0.001 ether);
    // 보내졌다면, 함수를 호출한 자에게 디지털 아이템을 전달하기 위한 내용 구성:
    transferThing(msg.sender);
  }
}
```

이 에제에서, *msg.value*는 컨트랙트로 이더가 얼마나 보내졌는지 확인하는 방법임

단위는 기본적으로 *ehter*임

위의 예제 컨트랙트를 *web3.js*에서는 아래와 같이 호출 했을 것임

```javascript
// `OnlineStore`는 자네의 이더리움 상의 컨트랙트를 가리킨다고 가정하네:
OnlineStore.buySomething({
  from: web3.eth.defaultAccount,
  value: web3.utils.toWei(0.001),
});
```

_value_ 필드에 *ether*를 얼마나 보낼지 설정

만약, 함수에 _payable_ 제어자가 없는 데 이더를 보내려고 한다면, 함수에서 트랜잭션을 거부할 것임

실습의 경우, _uint_ 타입의 _levelUpFee_ 변수를 정의하고, *0.001 ether*를 할당

_levelUp_ 이라는 함수를 생성하되, _uint_ 타입의 *\_zombieId*라는 변수를 받고, *external*이면서 _payable_ 이어야 함

이 함수는 먼저, *msg.value*가 *levelUpFee*와 같은지 *require*로 확인해야함

그 후, *level*을 증가시킴 : _zobmies[_zombieId].level++_

```solidity
pragma solidity ^0.4.19;

import "./zombiefeeding.sol";

contract ZombieHelper is ZombieFeeding {
  uint levelUpFee = 0.001 ether; // 1. 여기에 levelUpFee를 정의하게

  modifier aboveLevel(uint _level, uint _zombieId) {
    require(zombies[_zombieId].level >= _level);
    _;
  }

  function levelUp(uint _zombieId) external payable{  // 2. 여기에 levelUp 함수를 삽입하게
    require(msg.value == levelUpFee);
    zombies[_zombieId].level++;
  }

  function changeName(uint _zombieId, string _newName) external aboveLevel(2, _zombieId) {
    require(msg.sender == zombieToOwner[_zombieId]);
    zombies[_zombieId].name = _newName;
  }

  function changeDna(uint _zombieId, uint _newDna) external aboveLevel(20, _zombieId) {
    require(msg.sender == zombieToOwner[_zombieId]);
    zombies[_zombieId].dna = _newDna;
  }

  function getZombiesByOwner(address _owner) external view returns(uint[]) {
    uint[] memory result = new uint[](ownerZombieCount[_owner]);
    uint counter = 0;
    for (uint i = 0; i < zombies.length; i++) {
      if (zombieToOwner[i] == _owner) {
        result[counter] = i;
        counter++;
      }
    }
    return result;
  }
}
```

## 출금

컨트랙트에 출금 기능을 하는 함수를 만들지 않으면, 이더가 해당 컨트랙트에 갇히게 됨

다음의 예시 처럼, 컨트랙트에서 이더를 인출하는 함수를 작성할 수 있음

(해당 예시는 _Owanble_ 컨트랙트를 Import 하여, *owner*와 *onlyOwner*를 사용하고 있음을 참고)

```solidity
contract GetPaid is Ownable {
  function withdraw() external onlyOwner {
    owner.transfer(this.balance);
  }
}
```

해당 예시 처럼, _transfer_ 함수를 이용하여 특정 이더리움 주소에 송금이 가능함

그리고, *this.balance*는 컨트랙트에 저장돼있는 전체 잔액을 반환함

만약, 누군가 한 아이템에 대해 초과 지불을 했다면, 이더를 *msg.sender*로 환불해주는 기능 또한 다음과 같이 만들 수 있음

```solidity
uint itemFee = 0.001 ether;
msg.sender.transfer(msg.value - itemFee);
```

실습에서는, 먼저, _withdraw_ 함수를 생성함

위의 예제의 _GetPaid_ 컨트랙트의 구성과 동일하도록 코딩

이더의 가격은 계속 변하므로, *levelUpFee*를 고정적으로 코딩하지말고, 설정할 수 있도록 함수를 만드는 것이 좋은 설계일 것임

*setLevelUpFee*라는 이름의 *uint*형 *\_fee*라는 이름의 인자를 받고, *external*이며 _onlyOwner_ 제어자를 사용하는 함수를 생성함

이 함수는 *levelUpFee*를 *\_fee*로 설정해야함

```solidity
pragma solidity ^0.4.19;

import "./zombiefeeding.sol";

contract ZombieHelper is ZombieFeeding {
  uint levelUpFee = 0.001 ether;

  modifier aboveLevel(uint _level, uint _zombieId) {
    require(zombies[_zombieId].level >= _level);
    _;
  }

  function withdraw() external onlyOwner { // 1. 여기에 withdraw 함수를 생성하게
    owner.transfer(this.balance);
  }

  function setLevelUpFee(uint _fee) external onlyOwner {  // 2. 여기에 setLevelUpFee를 생성하게
    levelUpFee = _fee;
  }

  function levelUp(uint _zombieId) external payable {
    require(msg.value == levelUpFee);
    zombies[_zombieId].level++;
  }

  function changeName(uint _zombieId, string _newName) external aboveLevel(2, _zombieId) {
    require(msg.sender == zombieToOwner[_zombieId]);
    zombies[_zombieId].name = _newName;
  }

  function changeDna(uint _zombieId, uint _newDna) external aboveLevel(20, _zombieId) {
    require(msg.sender == zombieToOwner[_zombieId]);
    zombies[_zombieId].dna = _newDna;
  }

  function getZombiesByOwner(address _owner) external view returns(uint[]) {
    uint[] memory result = new uint[](ownerZombieCount[_owner]);
    uint counter = 0;
    for (uint i = 0; i < zombies.length; i++) {
      if (zombieToOwner[i] == _owner) {
        result[counter] = i;
        counter++;
      }
    }
    return result;
  }
}
```

## 진행 챕터

해당 Chapter는 _Solidity_ 문법을 따로 다루지않고 함수 내용만 추가 (좀비 전투)

좀비 전투를 위한 기능을 추가할 것임

_zombieattack.sol_ 파일을 생성하고, _^0.4.19_ 솔리디티 버전을 선언

*zombiehelper.sol*을 *import*함

*ZombieHelper*를 상속하는 *ZombieBattle*이라는 이름의 새 *contract*를 선언

```solidity
pragma solidity ^0.4.19;

import "./zombiehelper.sol";

contract ZombieBattle is ZombieHelper{

}
```

## 난수(Random Numbers)

게임 등에서 일정 수준의 무작위성를 요구하는 때가 있음

*Solidity*에서는 _keccak256_ 해시함수를 통해 난수를 생성할 수 있음

다음은 예제 코드

```solidity
// Generate a random number between 1 and 100:
uint randNonce = 0;
uint random = uint(keccak256(now, msg.sender, randNonce)) % 100;
randNonce++;
uint random2 = uint(keccak256(now, msg.sender, randNonce)) % 100;
```

%100 을 써서, 마지막 2자리 숫자만 받도록 함

다만, *nonce*는 딱 한번만 사용되어야함

그러나, 이더리움의 트랜잭션 처리 구조 상, 이 방법은 공격에 취약함

_random >= 50_ 이면 돈이 두배가 되고, _random < 50_ 이면 돈을 다 잃는 컨트랙트가 있다고 가정하면,

본인의 노드에서 이를 실행했을 때, 50 이상 일때만 트랜잭션을 전파하면 되기 때문임

따라서, 이더리움에서 안전하게 난수를 만드는 방법은 굉장히 어려운 문제임

https://ethereum.stackexchange.com/questions/191/how-can-i-securely-generate-a-random-number-in-my-smart-contract

결국 실습에서는 경제적 보상을 다루는 난수가 아닌, 순전히 공부나 재미용의 난수 생성을 시도해 볼것임

우선, 컨트랙트에 *randNonce*라는 이름의 _uint_ 타입의 변수를 *0*을 할당하여 선언

이후, _randMod_ (random-modulus)라는 이름의 함수를 생성, 함수는 _uint_ 타입의 *\_modulus*라는 인자를 받고, *internal*이며 _uint_ 타입을 반환함

해당 함수는 먼저 _randNonnce_ 값을 증가시킴

이후, 한줄의 코드로 _now_, _msg.sender_, *randNonce*의 _keccak256_ 해시 값을 계산하고 *uint*로 변환함 그리고 그 값을 *% \_modulus*를 한 후 _return_

```solidity
import "./zombiehelper.sol";

contract ZombieBattle is ZombieHelper {
  uint randNonce = 0; // 여기서 시작하게
  function randMod(uint _modulus) internal returns(uint) {
    randNonce++;
    return uint(keccak256(now, msg.sender, randNonce)) % _modulus;
  }
}
```

## 진행 챕터

해당 Chapter는 _Solidity_ 문법을 따로 다루지않고 함수 내용만 추가 (좀비 싸움)

좀비 전투는 다음과 같이 진행될 것임

좀비 중 하나를 고르고, 상대방의 좀비를 공격 대상으로 선택함

공격하는 쪽의 좀비라면, 70%의 승리 확률를 가지고, 방어하는 쪽의 좀비는 30% 쪽의 승리 확률을 가짐

공격하는 쪽, 방어하는 쪽 양측 좀비들은 전투 결과에 따라 증가하는 *winCount*와 *lossCount*를 가질 것임

공격하는 쪽의 좀비가 이기면, 좀비의 레벨이 오르고 새로운 좀비가 생성됨

공격하는 쪽의 좀비가 패배하면, *lossCount*가 증가하는 일 말고는 아무일도 일어나지 않음

좀비가 이기든 지든, 공격하는 쪽 좀비의 재사용 대기시간이 활성화됨

구현할 내용이 많기 때문에 여러 챕터에 나누어 구현할 것임

현재 챕터에서는, 컨트랙트에 *attackVictoryProbability*라는 이름의 _uint_ 변수를 추가하고, *70*을 할당

_attack_ 이라는 이름의 함수를 생성하고, *\_zombieId(uint)*와 _targetId(uint)_ 두 개의 매개변수를 받아오는 _external_ 함수로 설정

```solidity
import "./zombiehelper.sol";

contract ZombieBattle is ZombieHelper {
  uint randNonce = 0;
  uint attackVictoryProbability = 70; // 여기에 attackVictoryProbability를 만들게

  function randMod(uint _modulus) internal returns(uint) {
    randNonce++;
    return uint(keccak256(now, msg.sender, randNonce)) % _modulus;
  }

  function attack(uint _zombieId, uint _targetId) external{ // 여기에 새로운 함수를 만들게

  }
}
```

## 공통 로직 구조 개선하기(Refactoring)

_attack_ 함수를 *zombieId*의 소유자만이 실행할 수 있도록 해야함

이전, _changeName()_, _changeDna()_, _feedMultiply()_ 함수에서 우리는 다음과 같은 방식을 사용함

```solidity
require(msg.sender == zombieToOwner[_zombieId]);
```

_attack_ 함수에도 이와 똑같은 내용을 적용할 필요가 있음

그런데, 이렇게 계속 반복되는 경우 *modifer*로 지정해주는 것이 좋음

실습의 경우, *zombiefeeding.sol*을 열어서 리팩토링함

우선, *modifer*를 *ownerOf*라는 이름으로 생성하고, *\_zombieId(uint)*를 인수로 받음

제어자 내용으로 *msg.sender*와 *zombieToOwner[_zombieId]*가 같은지 *require*문을 통해 확인함

이후, _feedAndMultiply_ 함수 정의 부분을 _ownerOf_ 제어자를 사용하도록 변경

이제 *modifier*를 사용하게 되었으니, _require(msg.sender == zombieToOwner[_zombieId]);_ 부분을 삭제

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

  KittyInterface kittyContract;

  modifier ownerOf(uint _zombieId){  // 1. 여기에 제어자를 생성하게
    require(msg.sender == zombieToOwner[_zombieId]);
    _;
  }

  function setKittyContractAddress(address _address) external onlyOwner {
    kittyContract = KittyInterface(_address);
  }

  function _triggerCooldown(Zombie storage _zombie) internal {
    _zombie.readyTime = uint32(now + cooldownTime);
  }

  function _isReady(Zombie storage _zombie) internal view returns (bool) {
      return (_zombie.readyTime <= now);
  }

  // 2. 함수 정의 부분에 제어자를 추가하게:
  function feedAndMultiply(uint _zombieId, uint _targetDna, string _species) internal ownerOf(_zombieId) {
    // 3. 이 줄을 지우게 (주석처리함)
    // require(msg.sender == zombieToOwner[_zombieId]);
    Zombie storage myZombie = zombies[_zombieId];
    require(_isReady(myZombie));
    _targetDna = _targetDna % dnaModulus;
    uint newDna = (myZombie.dna + _targetDna) / 2;
    if (keccak256(_species) == keccak256("kitty")) {
      newDna = newDna - newDna % 100 + 99;
    }
    _createZombie("NoName", newDna);
    _triggerCooldown(myZombie);
  }

  function feedOnKitty(uint _zombieId, uint _kittyId) public {
    uint kittyDna;
    (,,,,,,,,,kittyDna) = kittyContract.getKitty(_kittyId);
    feedAndMultiply(_zombieId, kittyDna, "kitty");
  }
}
```

## 구조 더 개선하기

*zombiehelper.sol*에 우리의 새로운 *modifer*인 *ownerOf*를 적용할 부분의 두 군데 더 있으므로 적용한다

우선, *changeName()*에 *ownerOf*를 적용한다

또, *changeDna()*에 *ownerOf*를 적용한다

기존의 _require_ 문은 제거 또는 주석처리

```solidity
pragma solidity ^0.4.19;

import "./zombiefeeding.sol";

contract ZombieHelper is ZombieFeeding {
  uint levelUpFee = 0.001 ether;

  modifier aboveLevel(uint _level, uint _zombieId) {
    require(zombies[_zombieId].level >= _level);
    _;
  }

  function withdraw() external onlyOwner {
    owner.transfer(this.balance);
  }

  function setLevelUpFee(uint _fee) external onlyOwner {
    levelUpFee = _fee;
  }

  // 1. 이 함수를 `ownerOf`를 사용하도록 변경하게:
  function changeName(uint _zombieId, string _newName) external aboveLevel(2, _zombieId) ownerOf(_zombieId) {
    // require(msg.sender == zombieToOwner[_zombieId]);
    zombies[_zombieId].name = _newName;
  }

  // 2. 이 함수에도 똑같이 적용하게:
  function changeDna(uint _zombieId, uint _newDna) external aboveLevel(20, _zombieId) ownerOf(_zombieId) {
    // require(msg.sender == zombieToOwner[_zombieId]);
    zombies[_zombieId].dna = _newDna;
  }

  function getZombiesByOwner(address _owner) external view returns(uint[]) {
    uint[] memory result = new uint[](ownerZombieCount[_owner]);
    uint counter = 0;
    for (uint i = 0; i < zombies.length; i++) {
      if (zombieToOwner[i] == _owner) {
        result[counter] = i;
        counter++;
      }
    }
    return result;
  }
}
```

## 진행 챕터

해당 Chapter는 _Solidity_ 문법을 따로 다루지않고 함수 내용만 추가 (공격으로 돌아가자!)

다시 *zombieattack.sol*을 편집한다

이제 _onwerOf_ 제어자도 사용할 수 있으니, _attack_ 함수를 계속 정의하도록 한다

우선, 함수 호출자가 *\_zombieId*를 소유하고 있는지 확인하기 위해 _attack_ 함수에 _ownerOf_ 제어자를 추가한다

우리가 함수에서 해야할 것은 두 좀비의 _stroage_ 포인터를 얻어서 그것을 활용하는 것임

*Zombie storage*를 *myZombie*라는 이름으로 선언하고, 여기에 *zombies[_zombieId]*를 대입

*Zombie storage*를 *enemyZombie*라는 이름으로 선언하고, 여기에 *zombies[_targetId]*를 대입

전투의 결과를 결정하기 위해 0과 99 사이의 난수를 사용할 것임

*uint*형의 _rand_ 변수를 선언하고, 여기에 _randMod_ 함수에 *100*을 인수로 사용한 값을 대입

```solidity
import "./zombiehelper.sol";

contract ZombieBattle is ZombieHelper {
  uint randNonce = 0;
  uint attackVictoryProbability = 70;

  function randMod(uint _modulus) internal returns(uint) {
    randNonce++;
    return uint(keccak256(now, msg.sender, randNonce)) % _modulus;
  }

  // 1. 여기에 제어자를 추가하게
  function attack(uint _zombieId, uint _targetId) external ownerOf(_zombieId) {
    Zombie storage myZombie = zombies[_zombieId]; // 2. 여기서 함수 정의를 시작하게
    Zombie storage enemyZombie = zombies[_targetId];
    uint rand = randMod(100);
  }
}
```

## 진행 챕터

해당 Chapter는 _Solidity_ 문법을 따로 다루지않고 함수 내용만 추가 (좀비 승리와 패배)

좀비 싸움에서 얼마나 많이 이기고 졌는지의 기록을 통해 좀비 순위표를 생성할 수 있음

다양한 방법이 있지만, _Zombie_ 구조체에 *winCount*와 _lossCount_ 상태를 만들어서 저장하도록함

*zombiefactory.sol*의 _Zombie_ 구조체 정의에서 이 상태 속성을 추가해보도록 함

실습에서, _Zombie_ 구조체가 _uint16_ 타입의 *winCount*와 *lossCount*를 추가하도록 함

참고로, 구조체 안에서는 *uint*를 압축할 수 있으므로, 되도록 작은 수의 타입을 사용하는 것이 좋음

*uint8*의 경우 2^8 = 256이므로 너무 작을 것이므로 *uint16*으로 설정함

이는 2^16 = 65536이므로, 한 사용자가 매일 179년 동안 이기거나 패배해야하는 수치로 비교적 안전함

_Zombie_ 구조체가 변경되었으니, _\_createZombie()_ 함수 또한 수정되어야함

새로운 좀비가 0승 0패로 생성되도록 수정처리

```solidity
pragma solidity ^0.4.19;

import "./ownable.sol";

contract ZombieFactory is Ownable {

    event NewZombie(uint zombieId, string name, uint dna);

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;
    uint cooldownTime = 1 days;

    struct Zombie {
      string name;
      uint dna;
      uint32 level;
      uint32 readyTime;
      uint16 winCount; // 1. 여기에 새로운 속성을 추가하게
      uint16 lossCount;
    }

    Zombie[] public zombies;

    mapping (uint => address) public zombieToOwner;
    mapping (address => uint) ownerZombieCount;

    function _createZombie(string _name, uint _dna) internal {
        // 2. 여기서 새로운 좀비의 생성을 수정하게:
        uint id = zombies.push(Zombie(_name, _dna, 1, uint32(now + cooldownTime), 0, 0)) - 1;
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
        randDna = randDna - randDna % 100;
        _createZombie(_name, randDna);
    }
}
```

## 진행 챕터

해당 Chapter는 _Solidity_ 문법을 따로 다루지않고 함수 내용만 추가 (좀비 승리)

좀비 승리 시의 컨트랙트를 구현해보자

우선 *rand*가 *attackVictoryProbability*와 같거나 더 작은지 확인하는 _if_ 문을 작성

만약 이 조건이 참이라면, 다음과 같은 코드 진행 :

*myZombie*의 *winCount*를 증가시킴

*myZombie*의 *level*을 증가시킴

*enemyZombie*의 *lossCount*를 증가시킴

_feedAndMultiply_ 함수를 실행시키되, 3번째 인자의 *\_species*는 *"zombie"*라는 문자열을 전달

```solidity
import "./zombiehelper.sol";

contract ZombieBattle is ZombieHelper {
  uint randNonce = 0;
  uint attackVictoryProbability = 70;

  function randMod(uint _modulus) internal returns(uint) {
    randNonce++;
    return uint(keccak256(now, msg.sender, randNonce)) % _modulus;
  }

  function attack(uint _zombieId, uint _targetId) external ownerOf(_zombieId) {
    Zombie storage myZombie = zombies[_zombieId];
    Zombie storage enemyZombie = zombies[_targetId];
    uint rand = randMod(100);
    if(rand <= attackVictoryProbability){ // 여기서 시작하게
      myZombie.winCount++;
      myZombie.level++;
      enemyZombie.lossCount++;
      feedAndMultiply(_zombieId, enemyZombie.dna, "zombie");
    }
  }
}
```

## 진행 챕터

해당 Chapter는 _Solidity_ 문법을 따로 다루지않고 함수 내용만 추가 (좀비 패배)

좀비 패배 시의 컨트랙트를 구현해보자

이를 위해서는 _else_ 문을 활용해야함

아래는 _else_ 문의 예시

```solidity
if (zombieCoins[msg.sender] > 100000000) {
  // 엄청난 부자다!!!
} else {
  // 더 많은 좀비 코인이 필요해...
}
```

실습에서, *else*문을 추가하여, 우리의 좀비가 질 때 다음과 같이 구현함 :

*myZombie*의 *lossCount*를 증가시킴

*enemyZombie*의 *winCount*를 증가시킴

_else_ 문 밖에서, *myZombie*에 대해 _\_triggerCooldown_ 함수를 실행하여 하루에 한번만 공격 시도가 가능하도록 함

```solidity
import "./zombiehelper.sol";

contract ZombieBattle is ZombieHelper {
  uint randNonce = 0;
  uint attackVictoryProbability = 70;

  function randMod(uint _modulus) internal returns(uint) {
    randNonce++;
    return uint(keccak256(now, msg.sender, randNonce)) % _modulus;
  }

  function attack(uint _zombieId, uint _targetId) external ownerOf(_zombieId) {
    Zombie storage myZombie = zombies[_zombieId];
    Zombie storage enemyZombie = zombies[_targetId];
    uint rand = randMod(100);
    if (rand <= attackVictoryProbability) {
      myZombie.winCount++;
      myZombie.level++;
      enemyZombie.lossCount++;
      feedAndMultiply(_zombieId, enemyZombie.dna, "zombie");
    } else { // 여기서 시작하게
      myZombie.lossCount++;
      enemyZombie.winCount++;
    }
    _triggerCooldown(myZombie);
  }
}
```

## 이더리움 상의 토큰

이더리움 상에서 *토큰*은 기본적으로 몇몇 공통 규약을 따르는 스마트 컨트랙트이다

즉, 다른 모든 토큰 컨트랙트가 사용하는 표준 함수 집합을 구현한 것이 *토큰*이다

대표적인 *토큰*으로 *ERC20*이 있다

*ERC20*의 표준 함수 집합으로는 *transfer(address \_to, uint256 \_value)*나 _balanceOf(address \_owner)_ 같은 함수들이 있다

내부적으로 스마트 컨트랙트는 보통 *mapping(address => uint256) balances*와 같은 매핑을 가지고 있다

이는 각각의 주소에 잔액이 얼마나 있는지 기록한 것이다

_ERC20_ *토큰*이 똑같은 이름의 동일한 함수 집합을 공유하기 때문에, 이러한 형식을 지닌 다른 DApp에서도 호환이 가능하다

우리 실습에서는 좀비를 다루는데, 이는 대체불가능한 토큰으로 설계하는 것이 옳다

이에 _ERC20_ *토큰*이 아닌 *ERC721 토큰*으로 설계한다

*ERC721 표준*을 사용하면 우리의 컨트랙트에서 거래/판매나 경매나 중계 로직을 직접 구현하지 않아도 된다

우리가 *ERC721 표준*을 따르기만 하면, 누군가 *ERC 721 표준*에 맞게 거래가 가능한 DApp을 구현한 곳에서 이용이 될 수 있다

실습에서는, 우선 *ZombieOwnership*이라는 컨트랙트를 새로 코딩한다

먼저, *zombieownership.sol*을 생성하고, _pragma_ 버전을 표기해준다

*zombieattack.sol*을 *import*하고, *ZombieOnwership*이라는 새로운 컨트랙트를 선언하고 *ZombieAttack*을 상속한다

```solidity
pragma solidity ^0.4.19; // 여기서 시작하게

import "./zombieattack.sol";

contract ZombieOwnership is ZombieAttack {

}
```

## ERC721 표준, 다중 상속

아래는 _ERC721 표준_ 이다

```solidity
contract ERC721 {
  event Transfer(address indexed _from, address indexed _to, uint256 _tokenId);
  event Approval(address indexed _owner, address indexed _approved, uint256 _tokenId);

  function balanceOf(address _owner) public view returns (uint256 _balance);
  function ownerOf(uint256 _tokenId) public view returns (address _owner);
  function transfer(address _to, uint256 _tokenId) public;
  function approve(address _to, uint256 _tokenId) public;
  function takeOwnership(uint256 _tokenId) public;
}
```

토큰 컨트랙트를 구현할 때, 처음 해야 할 일은 _표준_ 인터페이스를 따로 복사하여 저장하고, *import "./erc721.sol"*과 같이 임포트하는 것이다

그리고 해당 컨트랙트를 상속하는 우리의 컨트랙트를 만들고, 각각의 함수를 오버라이딩 하여 정의한다

그런데 실습에서, *ZombieOwnership*은 이미 *ZombieAttack*을 상속하고 있다

이 때, 어떻게 하면 _ERC721_ 또한 상속하게 할 수 있을까?

*Solidity*에서는 다음과 같이 다중 상속을 허용한다

```solidity
contract SatoshiNakamoto is NickSzabo, HalFinney {
  // 오 이런, 이 세계의 비밀이 밝혀졌군!
}
```

실습에서는 우선, _erc721.sol_ 파일을 _zombieownership.sol_ 파일에서 임포트한다

이후, *ZombieOwnership*이 *ZombieAttack*과 _ERC721_ 모두 상속하는 것으로 선언한다

```solidity
pragma solidity ^0.4.19;

import "./zombieattack.sol";
import "./erc721.sol"; // 여기서 import 하게.

// 여기서 ERC721 상속을 선언하게.
contract ZombieOwnership is ZombieAttack, ERC721 {

}
```

## balanceOf & ownerOf

*balanceOf*는 단순히 *address*를 인자로 받아, 해당 *address*가 토큰을 얼마나 가지고 있는지 반환함

```solidity
 function balanceOf(address _owner) public view returns (uint256 _balance);
```

*ownerOf*는 토큰 ID를 인자로 받아, 해당 토큰 ID를 소유하고 있는 사람의 *address*를 반환함

이 정보를 저장하고 있는 *mapping*을 구현해 놓았다면, 그것을 쉽게 활용할 수 있음

```solidity
  function ownerOf(uint256 _tokenId) public view returns (address _owner);
```

실습에서는, *\_owner*가 가진 좀비의 수를 반환하도록 *balanceOf*를 구현하고,

ID가 *\_tokenId*인 좀비를 가진 주소를 반환하도록 *ownerOf*를 구현한다

```solidity
pragma solidity ^0.4.19;

import "./zombieattack.sol";
import "./erc721.sol";

contract ZombieOwnership is ZombieAttack, ERC721 {

  function balanceOf(address _owner) public view returns (uint256 _balance) {
    return ownerZombieCount[_owner]; // 1. 여기서 `_owner`가 가진 좀비의 수를 반환하게.
  }

  function ownerOf(uint256 _tokenId) public view returns (address _owner) {
    return zombieToOwner[_tokenId]; // 2. 여기서 `_tokenId`의 소유자를 반환하게.
  }

  function transfer(address _to, uint256 _tokenId) public {

  }

  function approve(address _to, uint256 _tokenId) public {

  }

  function takeOwnership(uint256 _tokenId) public {

  }
}
```

## 리팩토링

실습에서, 기존에 _ownerOf_ 라는 *modifer*가 있는데도 불구하고, *ownerOf*라는 이름의 함수를 또 정의함

그렇다면 *ZombieOwnership*의 *ownerOf*라는 함수 이름을 다른 것으로 바꾸어야 할까?

아니다. _ERC721_ 표준을 따르는 컨트랙트들은 모두 *ownerOf*라는 이름의 함수를 지니고 있을 것이다

이에 우리가 함수 이름을 다른 것으로 변경한다면 표준을 위배하는 것이고, 다른 DApp들과 호환되지 않을 것이다

이에, 함수가 아닌 *modifier*인 *ownerOf*의 이름을 바꾸는것이 옳은 선택이다

실습에서는, *zombiefeeding.sol*에서 _modifier_ 이름을 *ownerOf*에서 *onlyOwnerOf*로 바꾼다

먼저, 제어자를 정의 하는 이름을 *onlyOwnerOf*로 바꾸고, 이 제어자를 사용하는 _feedAndMultiply_ 함수에서도 제어자 이름을 변경해서 사용한다

(_zombiehelper.sol_, _zombieattack.sol_ 에서도 변경해준다)

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

  KittyInterface kittyContract;
  // 1. 제어자의 이름을 `onlyOwnerOf`로 바꾸게.
  modifier onlyOwnerOf(uint _zombieId) {
    require(msg.sender == zombieToOwner[_zombieId]);
    _;
  }

  function setKittyContractAddress(address _address) external onlyOwner {
    kittyContract = KittyInterface(_address);
  }

  function _triggerCooldown(Zombie storage _zombie) internal {
    _zombie.readyTime = uint32(now + cooldownTime);
  }

  function _isReady(Zombie storage _zombie) internal view returns (bool) {
      return (_zombie.readyTime <= now);
  }

  // 2. 여기서도 제어자의 이름을 바꾸게.
  function feedAndMultiply(uint _zombieId, uint _targetDna, string _species) internal onlyOwnerOf(_zombieId) {
    Zombie storage myZombie = zombies[_zombieId];
    require(_isReady(myZombie));
    _targetDna = _targetDna % dnaModulus;
    uint newDna = (myZombie.dna + _targetDna) / 2;
    if (keccak256(_species) == keccak256("kitty")) {
      newDna = newDna - newDna % 100 + 99;
    }
    _createZombie("NoName", newDna);
    _triggerCooldown(myZombie);
  }

  function feedOnKitty(uint _zombieId, uint _kittyId) public {
    uint kittyDna;
    (,,,,,,,,,kittyDna) = kittyContract.getKitty(_kittyId);
    feedAndMultiply(_zombieId, kittyDna, "kitty");
  }
}
```

## ERC721 전송 로직

*ERC721*에서는 토큰을 전송할 때 2개의 다른 방식이 있음

```solidity
function transfer(address _to, uint256 _tokenId) public;
function approve(address _to, uint256 _tokenId) public;
function takeOwnership(uint256 _tokenId) public;
```

우선, 토큰의 소유자가 전송 상대의 *address*와 전송하고자 하는 *\_tokenId*와 함께 _transfer_ 함수를 호출 하는 방법이 있고,

당므으로는 토큰의 소유자가 마찬가지로 전송 상대의 *address*와, 전송하고자 하는 *\_tokenId*를 가지고 *approve*를 호출함

이를 통해, 컨트랙트에 누가 해당 토큰을 가질 수 있도록 *허가*받았는지 저장함

(주로, *mapping(uint256 => address)*를 통함)

이후 누군가가 *takeOwnership*을 호출하면, 해당 컨트랙트는 이 _msg.sender_ 가 소유자로 부터 토큰을 받을 수 있게 허가를 받았는지 확인하고,

허가를 받은 상대면 해당 토큰을 그에게 전송함

결과적으로, *transfer*와 _takeOwnership_ 모두 전송 로직 자체는 동일하지만 순서는 반대임

(전자는 토큰을 보내는 사람이 함수를 호출하고, 후자는 토큰을 받는 사람이 함수를 호출함)

그래서, 이 두 로직에서 공통된 프라이빗 함수 *\_transfer*를 만들어서 추상화 하는 것이 중복을 막아 좋은 코딩을 할 수 있음

실습에서는, 먼저 *\_transfer*에 대한 로직을 정의할 것임

우선 *\_transfer*라는 이름의 함수를 정의하고, _address \_from_, _address \_to_, 그리고 _uint256 \_tokenId_ 세 개의 인수를 받고, _private_ 함수여야 함

이후 소유자가 바뀌면 바뀔 2개의 매핑을 쓸것임

_ownerZombieCount_ 매핑(한 소유자가 얼마나 많은 좀비를 가지고 있는지 기록)과 _zombieToOwner_ 매핑(어떤 좀비를 누가 가지고 있는지 기록)임

이 함수에서 처음 해야 할 일은 바로 좀비를 받는 사람(_address \_to_)의 *ownerZombieCount*를 증가시키는 것임

다음으로, 좀비를 보내는 사람(_address \_from_)의 *ownerZombieCount*를 감소시켜야 함

또, 이 *\_tokenId*에 해당하는 _zombieToOwner_ 매핑 값이 *\_to*를 가르키도록 변경함

*ERC721 스펙*에는 _Transfer_ 이벤트가 포함되어 있음

이 함수의 마지막 줄에서 적절한 정보와 함께 _Transfer_ 이벤트를 실행해야함

이를 위해 *erc721.sol*을 참고하여 작성

```solidity
pragma solidity ^0.4.19;

import "./zombieattack.sol";
import "./erc721.sol";

contract ZombieOwnership is ZombieAttack, ERC721 {

  function balanceOf(address _owner) public view returns (uint256 _balance) {
    return ownerZombieCount[_owner];
  }

  function ownerOf(uint256 _tokenId) public view returns (address _owner) {
    return zombieToOwner[_tokenId];
  }

  function _transfer(address _from, address _to, uint256 _tokenId) private{ // 여기에 _transfer()를 정의하게.
  ownerZombieCount[_to]++;
  ownerZombieCount[_from]--;
  zombieToOwner[_tokenId] = _to;
  Transfer(_from, _to, _tokenId);
  }

  function transfer(address _to, uint256 _tokenId) public {

  }

  function approve(address _to, uint256 _tokenId) public {

  }

  function takeOwnership(uint256 _tokenId) public {

  }
}
```

## ERC721 전송 (이어서)

이제 퍼블릭 _transfer_ 함수를 구현한다

어려운 부분은 이미 구현한 _\_transfer_ 함수가 다 처리 했기 때문에 쉬울 것이다

먼저, 해당 토큰의 소유자만 전송할 수 있도록 해야 하므로, _onlyOwnerOf_ 제어자를 활용한다

_transfer_ 내부에서 *\_transfer*를 호출 하는 것으로 함수를 끝마치되, _address \_from_ 인수에, *msg.sender*를 전달하는 것을 참고한다

```solidity
pragma solidity ^0.4.19;

import "./zombieattack.sol";
import "./erc721.sol";

contract ZombieOwnership is ZombieAttack, ERC721 {
  function balanceOf(address _owner) public view returns (uint256 _balance) {
    return ownerZombieCount[_owner];
  }

  function ownerOf(uint256 _tokenId) public view returns (address _owner) {
    return zombieToOwner[_tokenId];
  }

  function _transfer(address _from, address _to, uint256 _tokenId) private {
    ownerZombieCount[_to]++;
    ownerZombieCount[_from]--;
    zombieToOwner[_tokenId] = _to;
    Transfer(_from, _to, _tokenId);
  }

  // 1. 여기에 제어자를 추가하게.
  function transfer(address _to, uint256 _tokenId) public onlyOwnerOf(_tokenId) {
    _transfer(msg.sender, _to, _tokenId); // 2. 여기서 함수를 정의하게.
  }

  function approve(address _to, uint256 _tokenId) public {

  }

  function takeOwnership(uint256 _tokenId) public {

  }
}
```

## ERC721: Approve

_approve_ / *takeOwnership*을 사용하는 전송은 2 단계로 나뉜다

1. 소유자가 새로운 소유자에게 *address*와 보내고 싶은 토큰의 *\_tokenId*를 이용하여 *approve*를 호출한다

2. 새로운 소유자가 *\_tokenId*를 사용하여 _takeOwnership_ 함수를 호출하면, 컨트랙트는 *approve*가 이미 됐는지 확인하고 토큰을 전송한다

2번의 함수 호출이 발생하게 되는데, 함수 호출 사이에 누가 무엇에 대해 승인이 되었는지 저장할 매핑 등의 데이터 구조가 필요하다

실습에서는, _zombieApprovals_ 매핑을 먼저 정의하는데, *uint*를 *address*로 연결하는 매핑이다

이 매핑으로, 누군가 *\_tokenId*로 *takeOwnership*을 호출하면, 이 매핑을 써서 누가 그 토큰을 가지도록 승인받았는지 확인할 수 있다

_approve_ 함수에서, 오직 소유자만이 *approve*를 실행 할 수 있도록 해야 하므로 _onlyOwnerOf_ 제어자를 추가한다

함수의 내용에서는 *zombieApprovals*의 _\_tokenId_ 요소를 *\_to*와 같게 해야 한다

마지막으로, _ERC721_ 표준에는 _Approval_ 이벤트 또한 존재한다

_erc721.sol_ 에서 인수를 확인하고 이벤트 발생 처리하고, 특히 *msg.sender*를 *\_owner*에 전달해준다

```solidity
pragma solidity ^0.4.19;

import "./zombieattack.sol";
import "./erc721.sol";

contract ZombieOwnership is ZombieAttack, ERC721 {
  mapping (uint => address) zombieApprovals; // 1. 여기에 mapping을 정의하게.

  function balanceOf(address _owner) public view returns (uint256 _balance) {
    return ownerZombieCount[_owner];
  }

  function ownerOf(uint256 _tokenId) public view returns (address _owner) {
    return zombieToOwner[_tokenId];
  }

  function _transfer(address _from, address _to, uint256 _tokenId) private {
    ownerZombieCount[_to]++;
    ownerZombieCount[_from]--;
    zombieToOwner[_tokenId] = _to;
    Transfer(_from, _to, _tokenId);
  }

  function transfer(address _to, uint256 _tokenId) public onlyOwnerOf(_tokenId) {
    _transfer(msg.sender, _to, _tokenId);
  }

  // 2. 여기에 함수 제어자를 추가하게.
  function approve(address _to, uint256 _tokenId) public onlyOwnerOf(_tokenId) {
    zombieApprovals[_tokenId] = _to; // 3. 여기서 함수를 정의하게.
    Approval(msg.sender, _to, _tokenId);
  }

  function takeOwnership(uint256 _tokenId) public {

  }
}
```

## ERC721: takeOwnership

_takeOwnership_ 함수에서는, *msg.sender*가 이 토큰을 지닐 수 있게 이미 승인되었는지 확인하고, 승인이 되었다면 *\_transfer*를 호출하도록 코딩한다

실습에서는 먼저, _require_ 문을 써서 *zombieApprovals*의 _\_tokenId_ 요소가 *msg.sender*와 같은지 확인한다

*\_transfer*를 호출하기 위해, 토큰을 소유한 사람의 주소를 알 필요가 있는데, 이를 _ownerOf_ 함수를 통해 알 수 있다

이에, _address_ 변수를 _owner_ 라는 이름으로 선언하고, 여기에 _ownerOf(\_tokenId)_ 와 같이 대입한다

마지막으로, *\_transfer*에 필요한 인자들을 전달하고 호출하되, *\_to*에 *msg.sender*를 사용하는 것을 상기한다

```solidity
pragma solidity ^0.4.19;

import "./zombieattack.sol";
import "./erc721.sol";

contract ZombieOwnership is ZombieAttack, ERC721 {
  mapping (uint => address) zombieApprovals;

  function balanceOf(address _owner) public view returns (uint256 _balance) {
    return ownerZombieCount[_owner];
  }

  function ownerOf(uint256 _tokenId) public view returns (address _owner) {
    return zombieToOwner[_tokenId];
  }

  function _transfer(address _from, address _to, uint256 _tokenId) private {
    ownerZombieCount[_to]++;
    ownerZombieCount[_from]--;
    zombieToOwner[_tokenId] = _to;
    Transfer(_from, _to, _tokenId);
  }

  function transfer(address _to, uint256 _tokenId) public onlyOwnerOf(_tokenId) {
    _transfer(msg.sender, _to, _tokenId);
  }

  function approve(address _to, uint256 _tokenId) public onlyOwnerOf(_tokenId) {
    zombieApprovals[_tokenId] = _to;
    Approval(msg.sender, _to, _tokenId);
  }

  function takeOwnership(uint256 _tokenId) public {
    require(zombieApprovals[_tokenId] == msg.sender); // 여기서 시작하게.
    address owner = ownerOf(_tokenId);
    _transfer(owner, msg.sender, _tokenId);
  }
}
```

## 오버플로우 막기

(더 깊은 *ERC721 표준*에 관한 컨트랙트를 보려면 _OpenZeppelin ERC721_ 컨트랙트 등을 참고할것)

8비트 데이터를 저장 할 수 있는 _uint8_ 변수 하나에 저장 될 수 있는 가장 큰 수 는 이진수로 11111111(십진수로는 2^8 - 1 = 255)이다

```solidity
uint8 number = 255;
number++;
```

이 경우, _number_ 변수에 저장 된 값은 이진수로 *00000000*으로 되돌아간다

이것을 *오버플로우*라 한다

이러한 *오버플로우*나 *언더플로우*를 막기 위해서 *OpenZeppelin*에서 제공하는 *SafeMath*라는 라이브러리를 사용할 수 있다

*Solidity*에서 *라이브러리*는 특별한 종류의 컨트랙트로, 기본(native) 데이터 타입에 함수를 붙일 때도 유용하게 사용된다

_SafeMath_ 라이브러리를 사용 할 때에는 *using SafeMath for uint256*과 같은 구문을 사용한다

_SafeMath_ 라이브러리는 4개의 함수(_add_, _sub_, _mul_, _div_)를 가지고 있다

```solidity
using SafeMath for uint256;

uint256 a = 5;
uint256 b = a.add(3); // 5 + 3 = 8
uint256 c = a.mul(2); // 5 * 2 = 10
```

실습에서는, _SafeMath_ 라이브러리를 프로젝트에 추가해본다

먼저, *safemath.sol*을 *zombiefactory.sol*에 임포트하고, _using SafeMath for uint256;_ 을 선언한다

```solidity
pragma solidity ^0.4.19;

import "./ownable.sol";
import "./safemath.sol"; // 1. 여기서 import 하게.

contract ZombieFactory is Ownable {
  using SafeMath for uint256; // 2. 여기에 using safemath를 선언하게.

  event NewZombie(uint zombieId, string name, uint dna);

  uint dnaDigits = 16;
  uint dnaModulus = 10 ** dnaDigits;
  uint cooldownTime = 1 days;

  struct Zombie {
    string name;
    uint dna;
    uint32 level;
    uint32 readyTime;
    uint16 winCount;
    uint16 lossCount;
  }

  Zombie[] public zombies;

  mapping (uint => address) public zombieToOwner;
  mapping (address => uint) ownerZombieCount;

  function _createZombie(string _name, uint _dna) internal {
    uint id = zombies.push(Zombie(_name, _dna, 1, uint32(now + cooldownTime), 0, 0)) - 1;
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
    randDna = randDna - randDna % 100;
    _createZombie(_name, randDna);
  }
}
```

## SafeMath 파트 2

이하는 _SafeMath_ 내부의 코드이다

```solidity
library SafeMath {

  function mul(uint256 a, uint256 b) internal pure returns (uint256) {
    if (a == 0) {
      return 0;
    }
    uint256 c = a * b;
    assert(c / a == b);
    return c;
  }

  function div(uint256 a, uint256 b) internal pure returns (uint256) {
    // assert(b > 0); // Solidity automatically throws when dividing by 0
    uint256 c = a / b;
    // assert(a == b * c + a % b); // There is no case in which this doesn't hold
    return c;
  }

  function sub(uint256 a, uint256 b) internal pure returns (uint256) {
    assert(b <= a);
    return a - b;
  }

  function add(uint256 a, uint256 b) internal pure returns (uint256) {
    uint256 c = a + b;
    assert(c >= a);
    return c;
  }
}
```

먼저 _library_ 키워드가 사용된 것을 알 수 있다

_library_ 키워드는, *contract*와 비슷하지만 조금 다른 점이 있는데, _using_ 키워드를 사용할 수 있도록 해주는 것이 그것이다

```solidity
using SafeMath for uint;
// 우리는 이제 이 메소드들을 아무 uint에서나 쓸 수 있네.
uint test = 2;
test = test.mul(3); // test는 이제 6이 되네
test = test.add(5); // test는 이제 11이 되네
```

윗윗 코드에서, *mul*과 _add_ 함수는 2개의 인수를 필요로 하지만, *using SafeMath for uint*를 선언 할 때,

우리가 함수를 적용하는 *uint(test)*는 첫 번째 인수로 자동으로 전달 됨

*SafeMath*의 역할을 알기 위해, _add_ 함수의 내용을 통해 알아보고자 한다

```solidity
function add(uint256 a, uint256 b) internal pure returns (uint256) {
  uint256 c = a + b;
  assert(c >= a);
  return c;
}
```

기본적으로 *add*는 그저 2개의 *uint*를 _+_ 처럼 더한다

그런데, _assert_ 구문을 써서 그 합이 *a*보다 크도록 보장한다

즉, 저 _assert_ 구문이 오버플로우를 막아주는 것이다

*assert*는 조건을 만족하지 않으면 에러를 발생시킨다는 점에서 *require*와 비슷하다

*assert*와 *require*의 차이점은, *require*는 함수 실행이 실패하면 남은 가스를 사용자에게 되돌려주지만,

*assert*는 되돌려 주지 않는다

이 사실만으로는 *assert*가 아닌 *require*를 사용하고 싶을것이다

따라서 *assert*는 이와 같은 _uint_ 오버플로우 처럼 코드가 심각하게 잘못 실행 될 때 사용하는 구문이다

즉, *SafeMath*의 _add_, _sub_, _mul_, *div*는 간단히 4가지 기본 사칙연산을 수행하지만, 오버플로우나 언더플로우가 발생하면 에러를 발생시키는 역할이다

실습에서는, 우리의 컨트랙트에 *SafeMath*를 도입하는 것이다

즉, _+_, _-_, _\*_, */*를 사용하는 곳은 _add_, _sub_, _mul_, *div*로 교체한다

예를 들어,

```solidity
myUint++;
```

와 같은 코드를,

```solidity
myUint = myUint.add(1);
```

과 같이 처리한다

이에, _ZombieOwnership_ 컨트랙트에서 수학 연산한 곳 2곳을 찾아 _SafeMath_ 메소드로 처리한다

```solidity
pragma solidity ^0.4.19;

import "./zombieattack.sol";
import "./erc721.sol";
import "./safemath.sol";

contract ZombieOwnership is ZombieAttack, ERC721 {
  using SafeMath for uint256;

  mapping (uint => address) zombieApprovals;

  function balanceOf(address _owner) public view returns (uint256 _balance) {
    return ownerZombieCount[_owner];
  }

  function ownerOf(uint256 _tokenId) public view returns (address _owner) {
    return zombieToOwner[_tokenId];
  }

  function _transfer(address _from, address _to, uint256 _tokenId) private {
    // 1. SafeMath의 `add`로 교체하게.
    ownerZombieCount[_to] = ownerZombieCount[_to].add(1);
    // 2. SafeMath의 `sub`로 교체하게.
    ownerZombieCount[_from] = ownerZombieCount[_from].sub(1);
    zombieToOwner[_tokenId] = _to;
    Transfer(_from, _to, _tokenId);
  }

  function transfer(address _to, uint256 _tokenId) public onlyOwnerOf(_tokenId) {
    _transfer(msg.sender, _to, _tokenId);
  }

  function approve(address _to, uint256 _tokenId) public onlyOwnerOf(_tokenId) {
    zombieApprovals[_tokenId] = _to;
    Approval(msg.sender, _to, _tokenId);
  }

  function takeOwnership(uint256 _tokenId) public {
    require(zombieApprovals[_tokenId] == msg.sender);
    address owner = ownerOf(_tokenId);
    _transfer(owner, msg.sender, _tokenId);
  }
}
```

## SafeMath 파트 3

_ZombieAttack_ 컨트랙트의 아래와 같은 코드에도 오버플로우 문제가 존재한다

```solidity
myZombie.winCount++;
myZombie.level++;
enemyZombie.lossCount++;
```

그런데, *winCount*와 *lossCount*는 *uint16*이고, *level*은 *uint32*이다

이런 인수들에 *SafeMath*의 _add_ 메소드를 사용하면, 이 타입들을 *uint256*으로 변경해버리기 때문에, 오버플로우를 막지 못한다

```solidity
function add(uint256 a, uint256 b) internal pure returns (uint256) {
  uint256 c = a + b;
  assert(c >= a);
  return c;
}

// 만약 `uint8`에 `.add`를 호출한다면, 타입이 `uint256`로 변환되네.
// 그러니 2^8에서 오버플로우가 발생하지 않을 것이네. 256은 `uint256`에서 유효한 숫자이기 때문이지.
```

따라서, *uint16*과 *uint32*에서도 오버플로우 / 언더플로우를 막기 위해서 2개의 라이브러리를 더 만들어야 한다는 것을 의미한다

이를 *SafeMath16*과 *SafeMath32*라고 정의한다

실습에서는, *SafeMath32*를 *uint32*에 쓴다는 것을 선언

또, *SafeMath16*을 *uint16*에 쓴다는 것을 선언

*ZombieFactory*에 *SafeMath*를 써야할 곳을 수정

```solidity
pragma solidity ^0.4.19;

import "./ownable.sol";
import "./safemath.sol";

contract ZombieFactory is Ownable {
  using SafeMath for uint256;
  using SafeMath32 for uint32; // 1. using SafeMath32 for uint32를 선언하게.
  using SafeMath16 for uint16; // 2. using SafeMath16 for uint16를 선언하게.

  event NewZombie(uint zombieId, string name, uint dna);

  uint dnaDigits = 16;
  uint dnaModulus = 10 ** dnaDigits;
  uint cooldownTime = 1 days;

  struct Zombie {
    string name;
    uint dna;
    uint32 level;
    uint32 readyTime;
    uint16 winCount;
    uint16 lossCount;
  }

  Zombie[] public zombies;

  mapping (uint => address) public zombieToOwner;
  mapping (address => uint) ownerZombieCount;

  function _createZombie(string _name, uint _dna) internal {
    // 참고: 우리는 Year 2038 문제를 막지 않기로 하겠네... 그러니 readyTime에서 오버플로우를 걱정할 필요는 없네.
    // 우리 앱은 2038년에는 좀 꼬이겠지 ;)
    uint id = zombies.push(Zombie(_name, _dna, 1, uint32(now + cooldownTime), 0, 0)) - 1;
    zombieToOwner[id] = msg.sender;
    // 3. 여기에 SafeMath의 `add`를 사용하게:
    ownerZombieCount[msg.sender] = ownerZombieCount[msg.sender].add(1);
    NewZombie(id, _name, _dna);
  }

  function _generateRandomDna(string _str) private view returns (uint) {
    uint rand = uint(keccak256(_str));
    return rand % dnaModulus;
  }

  function createRandomZombie(string _name) public {
    require(ownerZombieCount[msg.sender] == 0);
    uint randDna = _generateRandomDna(_name);
    randDna = randDna - randDna % 100;
    _createZombie(_name, randDna);
  }
}
```

## SafeMath 파트 4

이제 *ZOmbieAttacK*에서 _++_ 증가 부분을 _SafeMath_ 메소드로 구성한다

주석부분으로 처리해놓아서, 그 부분만 수정하면된다

```solidity
pragma solidity ^0.4.19;

import "./zombiehelper.sol";

contract ZombieAttack is ZombieHelper {
  uint randNonce = 0;
  uint attackVictoryProbability = 70;

  function randMod(uint _modulus) internal returns(uint) {
    // 여기 하나 있네!
    randNonce = randNonce.add(1);
    return uint(keccak256(now, msg.sender, randNonce)) % _modulus;
  }

  function attack(uint _zombieId, uint _targetId) external onlyOwnerOf(_zombieId) {
    Zombie storage myZombie = zombies[_zombieId];
    Zombie storage enemyZombie = zombies[_targetId];
    uint rand = randMod(100);
    if (rand <= attackVictoryProbability) {
      // 여기 세 개 더 있군!
      myZombie.winCount = myZombie.winCount.add(1);
      myZombie.level = myZombie.level.add(1);
      enemyZombie.lossCount = enemyZombie.lossCount.add(1);
      feedAndMultiply(_zombieId, enemyZombie.dna, "zombie");
    } else {
      // ...그리고 2개 더!
      myZombie.lossCount = myZombie.lossCount.add(1);
      enemyZombie.winCount = enemyZombie.winCount.add(1);
      _triggerCooldown(myZombie);
    }
  }
}
```

## 주석(Comment)

```solidity
// 주석은 // 를 사용하여 사용

/*
혹은 이렇게 여러줄 주석 사용
This is a multi-lined comment. I'd like to thank all of you
 who have taken your time to try this programming course.
 I know it's free to all of you, and it will stay free
 forever, but we still put our heart and soul into making
 this as good as it can be.
*/
```

_Solidity_ 커뮤니티에서 주석을 위해 표준으로 쓰이는 형식은 *natspec*이다

https://docs.soliditylang.org/en/develop/natspec-format.html

```solidity
/// @title 기본적인 산수를 위한 컨트랙트
/// @author H4XF13LD MORRIS 💯💯😎💯💯
/// @notice 지금은 곱하기 함수만 추가한다.
contract Math {
  /// @notice 2개의 숫자를 곱한다.
  /// @param x 첫 번쨰 uint.
  /// @param y 두 번째 uint.
  /// @return z (x * y) 곱의 값
  /// @dev 이 함수는 현재 오버플로우를 확인하지 않는다.
  function multiply(uint x, uint y) returns (uint z) {
    // 이것은 일반적인 주석으로, natspec에 포함되지 않는다.
    z = x * y;
  }
}
```

*@title*과 *@author*는 말 그대로 타이틀과 저작자이다

*@notice*는 사용자에게 컨트랙트 / 함수가 무엇인지 설명한다

*@dev*는 개발자에게 추가적인 상세 정보를 설명하기 위해 사용한다

*@param*과 *@return*은 함수에서 어떤 매개 변수와 반환값을 가지는지 설명한다

모든 함수에 대해 이러한 모든 태그들을 쓸 필요는 없다

다만, _@dev_ 정도는 작성하여, 함수가 어떤것을 하는지 설명하는 것이 좋다

실습에서는, *@title*과 *@author*와 *@dev*를 작성해본다

*ZombieOwnership*에 _natspec_ 태그의 주석을 작성한다

*@title*에는 좀비 소유권 전송을 관리하는 컨트랙트라고 작성한다

*@author*에는 이름을 쓴다

*@dev*에는 OpenZepplin의 ERC721 표준 초안 구현을 따른다고 설명한다

## References

[natspec for Solidity Comment](https://docs.soliditylang.org/en/develop/natspec-format.html){: target="\_blank"}

<!-- [Array on mozzila.org](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array){: target="\_blank"} -->

<!-- ![permasecond](/assets/posts/2020-02-21-cmdcolor/permasecond.png) -->
