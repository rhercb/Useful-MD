```ts
// Pēc iekavām ir atgriežamā vērtība
function addNumber(a: number, b: number): number {
    return a + b;
}
```

```ts
function testMe(a: string, testFn: () => void) { // Void nozimē ka nekas netiek atgriezts
    testFn?.(); // Optional, ja nu gadījumā funckijas nav, nebūs jāraksta if
}
```

```ts
type MutationFunction = (v: number) => number // izveidojam savu funkcijas tipu, lai to varētu izmantot
function testMe(a: string, testFn: MutationFunction) {
    testFn?.();
}
```

```ts
// Var arī atgriezt funckiju
function adderFn = (v: number): (v:number) => number {
    return (v:number) => number + v
}
```

```ts
// safe 'any' = unknown type, pēctam ir jādefinē šie parametri kā vērtība, arg1 as number/string etc
// and overloading
funcion test = (arg1: unknown, arg2: unknown): { x: number, y: number } {
    let cord = {
        x: 0,
        y: 0
    };

    coord = {
      x: arg1 as number,
      y: arg2 as number,
    };

    return cord
}
```

```ts
// Generics
// Genereic is <> before open brackets
// T tiks nomainits ar jebko kas tur būs
function simpleState<T>(
  initial: T
): [() => T, (v: T) => void] {
  let str: T = initial;
  return [
    () => str,
    (v: T) => {
      str = v;
    },
  ];
}

const [st1setter, st1getter] = simpleState(1) // Visur kur ir izmntots T, type tagad būs number

const [st2setter, st2getter] = simpleState(null)
st2setter('str') // Nevar izmantot, jo innitial ir NULL
// Tapēc vajar overridot T
const [st2setter, st2getter] = simpleState<string | null>(null)
st2setter('str') // Tagad darbojās

// Interfacam arī var izmantot generics
interface Rank<RankItem> {
    item: RankItem,
    rank: (v: RankItem) => number
}
```

```ts
// keyof
// KeyType has to be one of the keys in DataType
function pluck<DataType, KeyType extends keyof DataType>(
    items: dataType[],
    key: KeyType
): DataType[KeyType][] {
    return items.map(item => item[key])
}

const dogs = [
    { name: 'Dog', age: 12 },
    { name: 'Dog 23', age: 22 }
]

console.log(pluck(dogs, "age")) // Nūs hint ka var izmantot age vai name atslēgas
```

```ts
// Event map

interface Base {
    time: number;
    user: string;
}

interface EventMap {
    addToCart: Base & { quantity: number, productID: number } // ar & apvieno
}
```

```ts
// Utility Types
interface myUser {
    name: string;
    id: string;
    email?: string;
    phone?: string;
}

type MyUserOptionals = Partial<MyUser>; // PArtial uztaisa visus MyUser laukus optional

type RequiredMyUser = Required<MyUser> // Visi lauki būs obligāti

type JustEmailAndName = Pick<MyUser, 'email' | 'name'> // Paņem tos laukus no MyUser kurus mums vajag ar Pick

// pirmais ir id type, otrs ir output
const mapNyId = (users: MyUser[]): Record<string, MyUser> => {
    return user.reduce((a, v) => {
        return {
            ...a,
            [v.id]: v,
        }
    }, {})
}

// Omit removes ierakstus pēc key
const mapNyId = (users: MyUser[]): Omit<MyUser, 'id'>> => {
    return user.reduce((a, v) => {
        comnst {  id, ..other } = v;
        return {
            ...a,
            [id]: other,
        }
    }, {})
}
```

```ts
// Readonly
interface Cat {
    readonly name: string;
    breed: string
}

type ReadOnlyCat = Readonly<Cat>

// Realy const array
const reallyConst = [1,2,3] as const; // Neļaus pārrakstīs vērtības
```

```ts
// Generics in classes

// K = key, T = type
interface Database<T, K> {
  get(id: K): T;
  set(id: K, value: T): void;
}

interface Persistable {
  saveToString(): string;
  restoreFromString(storedState: string): void;
}

type DBKeyType = string | number | symbol;

class InMemoryDatabase<T, K extends DBKeyType> implements Database<T, K> {
  protected db: Record<K, T> = {} as Record<K, T>;
  get(id: K): T {
    return this.db[id];
  }
  set(id: K, value: T): void {
    this.db[id] = value;
  }
}

class PersistentMemoryDB<T, K extends DBKeyType>
  extends InMemoryDatabase<T, K>
  implements Persistable {
  saveToString(): string {
    return JSON.stringify(this.db);
  }
  restoreFromString(storedState: string): void {
    this.db = JSON.parse(storedState);
  }
}

const myDB = new PersistentMemoryDB<number, string>();
```

```ts
// We can use ternary operators
type FetchPokemon<T> = T extends undefined ? Promise<PokemonResult[]> : void
```