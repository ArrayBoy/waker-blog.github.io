## Object.values()

> 返回一个对象值的数组。

```javascript
const icecreamColors = {
    chocolate: 'brown',
    vanilla: 'white',
    strawberry: 'red',
}

const colors = Object.values(icecreamColors);
// colors will be equal to ["brown", "white", "red"]
```

## Object.keys()

> 返回一个对象key的数组。

```javascript
const icecreamColors = {
  chocolate: 'brown',
  vanilla: 'white',
  strawberry: 'red',
}

const types = Object.keys(icecreamColors);
// types will be equal to ["chocolate", "vanilla", "strawberry"]
```

## Object.entries()

> 创建一个包含对象的键/值对数组的数组。

```javascript
const weather = {
  rain: 0,
  temperature: 24,
  humidity: 33,
}

const entries = Object.entries(weather);
// entries will be equal to
// [['rain', 0], ['temperature', 24], ['humidity', 33]]
```

## Object spread

> 扩展对象允许为没有突变的对象添加新的属性和值（即创建新对象），也可以将多个对象组合在一起。需要指出的是，传播对象不会嵌套复制。

```javascript
//添加一个新的对象属性和值，而不会改变原始对象。
const spreadableObject = {
  name: 'Robert',
  phone: 'iPhone'
};

const newObject = {
  ...spreadableObject,
  carModel: 'Volkswagen'
}
// newObject will be equal to
// { carModel: 'Volkswagen', name: 'Robert', phone: 'iPhone' }
```

## Object.freeze()

> 防止修改现有的对象属性或向对象添加新的属性和值。 一般认为const会这样做，但是const只允许修改一个对象。

```javascript
//冻结对象以防止更改name属性。
const frozenObject = {
  name: 'Robert'
}

Object.freeze(frozenObject);

frozenObject.name = 'Henry';
// frozenObject will be equal to { name: 'Robert' }
```

## Object.seal()

> 停止将任何新属性添加到对象，但仍允许更改现有属性。

```javascript
//密封对象以防止添加wearsWatch属性。
const sealedObject = {
  name: 'Robert'
}

Object.seal(sealedObject);

sealedObject.name = 'Bob';
sealedObject.wearsWatch = true;
// sealedObject will be equal to { name: 'Bob' }
```

## Object.assign()

> 允许将对象组合在一起。 此方法也可不用，因为可以改为使用对象扩展语法。 与对象扩展运算符一样，Object.assign（）也不会执行深层克隆。

```javascript
//将两个对象组合成一个。
const firstObject = {
  firstName: 'Robert'
}

const secondObject = {
  lastName: 'Cooper'
}

const combinedObject = Object.assign(firstObject, secondObject);
// combinedObject will be equal to { firstName: 'Robert', lastName: 'Cooper' }
```