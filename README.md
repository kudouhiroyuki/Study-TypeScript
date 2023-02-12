```
const result: string = "文字";
const result: number = 0;
const result: boolean = true;
const result: null = null;
const result: undefined = undefined;
const result: number[] = [1, 2, 3];
const result: { id: number; name: string } = { id: 1, name: "名前" };
const result: 'test' = 'test';

function test(name: string): void {}
let testName: string = "名前";
test(testName);

function test(name: string): string { return name }
let testName: string = "名前";
test(testName);

const button1 = <HTMLButtonElement>document.createElement('button');
const button2 = document.createElement('button') as HTMLButtonElement;
const input = <HTMLInputElement>document.createElement('input');
const div = <HTMLDivElement>document.createElement('div');
const a = <HTMLAnchorElement>document.createElement('a');
const p = <HTMLParagraphElement>document.createElement('p');

const onParagraphClick = (event: Event) => {
  if (event.target instanceof HTMLParagraphElement) {}
}
------------------------------------------------------------------------------------------->
■インターセクション型（Intersection）
複数の型をひとつに結合した型
type IntersectionA = {
  id: number
  name: string
}
type IntersectionB = {
  adress: string
}
const result: IntersectionA & IntersectionB = { id: 1, name: "名前", adress: "
h@gmail.com" };

type IntersectionA = {
  id: number
  name?: string
}
type IntersectionB = {
  adress: string
}
const result: IntersectionA & IntersectionB = { id: 1, adress: "
hoge@gmail.com" };
------------------------------------------------------------------------------------------->
■ユニオン型（Union）
与えられた型のうち何れかの型
type Union = number | undefined;
const result: Union = 0

type Union =
  | 400
  | 404;
const result: Union = 404

type Union = (string | number)[];
const result: Union = ["1", 2 , 3]
------------------------------------------------------------------------------------------->
■ジェネリクス型（Generics）
与えられた型を再利用
function generics<T>(item: T): T {
  return item;
}
generics<string>("文字");
generics<number>(0);

function generics<T>(item: T): Array<T> {
  const result: Array<T> = [item];
  return result;
}
let result1: Array<string> = generics<string>("文字");
let result2: Array<number> = generics<number>(0);

interface Generics {
  id: number;
};
function generics<T extends Generics>(item: Generics) {
  return { value: item.id }
};
const result = generics({ id: 0 });

const generics = <T> (value: T) => {
  return value;
}
generics<string>("文字")

const generics = <T>(value: T): T => value;
const result = generics<string>("文字");

const generics = <T extends string>(value: T): T => value;
const result = generics("文字");

class Generics<T> {
  item: T
  constructor (item: T) {
    this.item = item;
  };
};
const result1 = new Generics<string>('文字');
const result2 = new Generics<number>(0);

class Generics<T extends string> {
  item: T
  constructor (item: T) {
    this.item = item;
  };
};
const result = new Generics('文字');

interface Generics<T> {
  value: T;
}
const result1: Generics<string> = { value: "文字" };
const result2: Generics<number> = { value: 0 };

interface Generics<T = string> {
  value: T;
}
const result: Generics<string> = { value: "文字" };

interface Generics<T extends string | number> {
  value: T;
}
const result1: Generics<string> = { value: "文字" };
const result2: Generics<number> = { value: 0 };
------------------------------------------------------------------------------------------->
■パーシャル型 Partial<Type> (ユーティリティ型)
オブジェクト型Tのすべてのプロパティをオプションプロパティにする
type PartialBase = {
  name: string;
  age: string;
  address: string;
};
const result1: Partial<PartialBase> = { name: "名前" };
const result2: Partial<PartialBase> = { name: "名前", age: '30' };
------------------------------------------------------------------------------------------->
■リクワイヤード型 Required<Type> (ユーティリティ型)
Tのすべてのプロパティからオプショナルであることを意味する?を取り除く
type RequiredBase = {
  name?: string;
  age?: string;
  address?: string;
};
const result: Required<RequiredBase> = {
  name: "名前",
  age: "30",
  address: "hoge@gmail.com"
};
------------------------------------------------------------------------------------->
■リードオンリー型 Readonly<Type> (ユーティリティ型)
オブジェクト型Tのプロパティをすべて読み取り専用にする
interface ReadonlyBase {
  name: string
}
const result: Readonly<ReadonlyBase> = {
  name: "",
}
result.name = "名前";

type ReadonlyBase = {
  readonly name: string;
}
const result: ReadonlyBase = { name: "" };
result.name = "名前";

type ReadonlyBase = Readonly<{
  name: string;
  age: string;
  address: string;
}>;
const result: ReadonlyBase = { name: "", age: "", address: "" };
result.name = "名前";

interface ReadonlyBase {
  name: string;
  age: string;
  address: string;
}
function readonly(args: Readonly<ReadonlyBase>) {
  args.name = "名前";
}
readonly({ name: "", age: "", address: "" })

class ReadonlyBase {
  readonly name: string;
  constructor() {
    this.name = "";
  }
}
const result = new ReadonlyBase();
result.name = "名前";

class ReadonlyBase {
  readonly name: string;
  constructor(name: string) {
    this.name = name;
  }
}
const result = new ReadonlyBase("");
result.name = "名前";
------------------------------------------------------------------------------------->
■ピック型 Pick<Type, Keys> (ユーティリティ型)
型TからKeysに指定したキーだけを含むオブジェクト型を返す
interface PickBase {
  name: string;
  age: string;
  address: string;
}
const result: Pick<PickBase, "name" | "age"> = { name: "名前", age: "30" };
------------------------------------------------------------------------------------->
■オミット型 Omit<Type, Keys> (ユーティリティ型)
オブジェクト型TからKeysで指定したプロパティを除いたオブジェクト型を返す
interface OmitBase {
  name: string;
  age: string;
  address: string;
}
const result: Omit<OmitBase, "name" | "age"> = { address: "hoge@gmail.com"
};
------------------------------------------------------------------------------------->
■イクストラクト型 Extract<Type, Union> (ユーティリティ型)
ユニオン型TからUで指定した型だけを抽出した型を返す
type ExtractBase = {
  type: 'a',
  data: number;
} | {
  type: 'b',
  data: string;
};
const result: Extract<ExtractBase, {type: 'a'}> = { type: "a", data: 100 };

type ExtractBase = string | number;
const result: Extract<ExtractBase, number> = 0;

type ExtractBase = "a" | "b" | "c";
const result: Extract<ExtractBase, "a" | "b"> = "a";
-------------------------------------------------------------------------------------
■エクスクルード型 Exclude<Type, Union> (ユーティリティ型)
ユニオン型TからUで指定した型を取り除いたユニオン型を返す
type ExcludeBase = {
  type: 'a',
  data: number;
} | {
  type: 'b',
  data: string;
};
const result: Exclude<ExcludeBase, {type: 'a'}> = { type: "b", data: "100"
};

type ExcludeBase = string | number;
const result: Exclude<ExcludeBase, number> = "名前";

type ExcludeBase = "a" | "b" | "c";
const result: Exclude<ExcludeBase, "a" | "b"> = "c";
```
