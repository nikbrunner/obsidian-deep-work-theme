---
title: User  Defined Type Guards
created: 21.09.23 — Thursday
lang: en 
---

# User  Defined Type Guards

#dev/lang/typescript 


[if statement - Accessing different properties in a typescript union type - Stack Overflow](https://stackoverflow.com/questions/43496154/accessing-different-properties-in-a-typescript-union-type)

## Example
```ts
const hasAlteosShopUrl = ( 
	value: BikePrintAdvertisement | AlteosBikePrintAdvertisement
): value is AlteosBikePrintAdvertisement => Object.prototype.hasOwnProperty.call(value, “shopUrl”);
```

### `isObjectFromType()`
```ts
interface Counter {
  one: string;
  two: string;
  three: string;
}

const obj: Counter = {
  one: "one",
  two: "two",
  three: "three",
};

// isType
const isType = <T>(val: any, validateFn: (val: any) => boolean): val is T => {
  return validateFn(val);
};

const isCounter = isType<Counter>(obj, (obj) =>
  Object.prototype.hasOwnProperty.call(obj, "one")
);

console.log(isCounter);

// numver
const num = "jo";
const isNum = isType<number>(num, (num) => typeof num === "number");
console.log(isNum);

// isObjectFromType()
const isObjectFromType = <O>(obj: any, verficationProp: string): obj is O => {
  return Object.prototype.hasOwnProperty.call(obj, verficationProp);
};

const isCounterAlt = isObjectFromType<Counter>(obj, "two");

console.log(isCounterAlt);

```

## `isEnumValue()`
### Description

### Example
```ts
// Definition
export function isEnumValue<T extends string | number, Enum extends string | number>(
  enumVariable: { [key in T]: Enum }
) {
  const enumValues = Object.values(enumVariable);
  return (value: string | number): value is Enum => enumValues.includes(value);
}

// Usage
export enum Currency {
  "EUR" = "EUR",
  "CHF" = "CHF"
}

const checkCurrency = (currency: any): Currency => {
  if (!isEnumValue(Currency)(currency)) {
    throw new Error(`Currency "${currency}" is not a member of the Currency enum!`);
  } else {
    return currency;
  }
};

// with the usage of the function not only gets validated that the
// Although req.query.currency on runtime could be anything,
// query is part of the enum, it also returns the correct type after it
const currency = req.query.currency && checkCurrency(req.query.currency);  
```
