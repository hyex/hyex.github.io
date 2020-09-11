---
layout: post
title:  "[DB]] MongoDB & Mongoose"
date:   2020-09-11 23:51:00 +0900
categories: unclassified
---

## Contents
- [MongoDB](#mongodb)
- [Install](#install)
- [CRUD](#crud)
- [Mongoose](#mongoose)


## MongoDB

Document-Oriented NoSQL Database.
- Opensource
- **Engine** C++
- **Schema-less** 고정 스키마가 존재하지 않다. 같은 collection 내에 있더라도 다른 schema, 즉 각자의 고유한 field를 가질 수 있다. 동적 스키마라고도 한다.
- JOIN 기능이 없지만 CRUD Query가 고속으로 동작한다. JOIN 대신에 스키마를 디자인할 때 document에 최대한 많은 데이터를 포함시킨다.
- Scalability가 우수하며 Sharding이 가능

> **NoSQL** Not Only SQL, 기존의 RDBMS의 한계를 극복하기 위한, 유연한 비관계형 데이터베이스이다.   

### 용어 정리

| Terms | desc | in RDMS | 
|:------:|--------------|------|
|Database|Collection의 물리적인 컨테이너|Database|
|Collection|Document의 집합으로 구성|table|
|Document|JSON obejct 형태의 key-value의 쌍|record, row|
|Field||column|

### SQL 

| Method | MongoDB | RDMS | 
|:------:|--------------|------|
|insert|db.users.insert({...})|insert into users(...) values(...)|
|Select|db.users.find({...})|select * from users where ...|
|Update|db.users.update({...}, {...})|update users set ... where ...|
|Delete|db.users.remove({...})|delete from users where ...|


## Install

https://poiemaweb.com/mongdb-basics#42-mac

> Shell 사용을 안하고 있어서 생략한다.  

## CRUD

Create, Read, Update, Delete을 의미한다.

https://poiemaweb.com/mongdb-basics-shell-crud#create

> Shell 사용을 안하고 있어서 생략한다.   

## Mongoose 

Node.js와 MongoDB를 위한 ODM(Object Data Mapping) Library.


### Setting

1. Install
```js
npm init
npm install express body-parser mongoose dotenv
```

2. Connection 
환경변수 관리를 위해 위에서 설치한  `dotenv`(환경설정을 위한 패키지)을 이용한다.
`.env` 파일 생성 후 mongoose에 접속하기 위한 계정과 비밀번호를 환경변수로 저장한다.
```js
# MongoDB URI & User/Password  
MONGO_URI=mongodb://localhost/<db-name>  
# mlab의 경우  
# MONGO_URI=mongodb://<userid>:<password>@<database>:<port>/<db-name>
```

`app.js`에 다음과 같이 추가한다.
```js
// ENV
require('dotenv').config();

// Node.js의 native Promise 사용  
// mongoose가 지원하는 MPromise가 deprecated되어 ES6의 promise를 사용
mongoose.Promise  =  global.Promise;

// CONNECT TO MONGODB SERVER 
mongoose.connect(process.env.MONGO_URI,  {  useMongoClient:  true  })  .then(()  =>  console.log('Successfully connected to mongodb'))  
.catch(e  =>  console.error(e));
```

### Schema & Model

`models/todo.js` 파일을 생성하고 아래 내용을 작성한다.

```js
// models/todo.js
const  mongoose  =  require('mongoose');

// Define Schemes  
const  todoSchema  =  new  mongoose.Schema({  
	todoid:  {  type:  Number,  required:  true,  unique:  true  },
	content:  {  type:  String,  required:  true  },  
	completed:  {  type:  String,  default:  false  }  
}, {  
	timestamps:  true  
});

// Create Model & Export  
module.exports  =  mongoose.model('Todo',  todoSchema);
```

#### Schema
document의 Data 구조, 즉 필드 타입에 대한 정보를 JSON 형태로 정의한 것이다. MongoDB는 Schema-less이기 때문에 장점도 있지만 어떤 필드가 어떤 데이터 타입인지 알기 어려운 단점이 있다. 이러한 문제를 보완하기 위해 사용하는 것이 Schema이다. 테이블이라고 생각할 수 있지만, 그보단 틀이라고 생각하면 될 것같다.
Schema는 Model 생성 시 인자로 전달된 후, 더이상 사용하지 않는다.

#### Schema가 지원하는 데이터 타입
- String
- Number
- Boolean
- Buffer
- Date
- Array
- Schema.types.ObjectId
- Schema.types.Mixed

> document 생성 시 자동으로 추가되는 `_id` 는 document 내에서 유일함을 보장하는 12 byte binary data, primary-key이다. 명시적으로 정의하지 않아도 `insert()`나  `save()` 메소드 호출 시 자동으로 추가된다.

#### Model
도큐먼트를 생성하는 역할을 담당한다.
Schema를 이용해 Model을 생성한다. 보통 대문자로 시작하며, 단수를 사용한다. 실제 collection 이름은 복수형으로 자동 변환되어 사용된다. 

```js
const Todo = mongoose.model('Todo', todoSchema)
```

model은 생성자이므로 instance를 생성할 수 있는데 이것은 개별 document를 나타낸다. 

```js
const  todo  =  new  Todo({ 
	todoid:  1,  
	content:  'MongoDB',  
	completed:  false,  
	...  
});
```

> Front가 따로 있는 경우, 당연히 request에 담겨온다.  


### Statics model methods
모델의 메소드.
특정 document가 존재하지 않거나 필요가 없는 경우에 유용하다.

```js
const  Todo  =  mongoose.model('Todo',  todoSchema);

Todo.find({})  
	.then(todo  =>  console.log(todo))  
	.catch(err  =>  console.log(err))
```


### Document instance methods
(모델이 생성한 인스턴스인) 도큐먼트의 메소드.

```js
var  Todo  =  mongoose.model('Todo',  todoSchema);

// model instance (= document)  
var  todo  =  new  Todo({  
	todoid:  1,  
	content:  'MongoDB',  
	completed:  false  
});

// Document instance method  
todo.save  
	.then(()  =>  console.log('Saved successfully'));
```

