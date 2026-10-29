## Phần 1: Cơ bản

### 1.1. `any` và `unknown` khác nhau thế nào? Khi nào nên dùng `unknown`?

Trả lời:
any : không kiểm tra kiểu dữ liệu, có thể thêm bất kỳ thuộc tính nào mà không cần kiểm tra 
unknown : bắt buộc phải kiểm tra trước khi sử dụng
dùng unknown khi mình chưa biết kiểu của dữ liệu như kiểu JSON, lỗi trong catch
### 1.2. `interface` và `type` khác nhau ở đâu? Em sẽ chọn cái nào và vì sao?

Trả lời:
interface : mô tả cấu trúc đối tượng , hỗ trợ extends và khai báo thêm 
type : linh hoạt hơn, có thể định nghĩa union, tuple và kết hợp các kiểu bằng &
em sẽ chọn interface khi định nghĩa user,product hoặc API còn type khi gặp kiểu dữ liệu phức tạp hơn
### 1.3. `null` và `undefined` khác nhau thế nào? Tuỳ chọn `strictNullChecks` có tác dụng gì?

Trả lời:
undefined: Biến hoặc thuộc tính chưa được gán giá trị
null: Giá trị rỗng được gán có chủ đích
strictNullChecks là tùy chọn trong TypeScript để kiểm tra chặt chẽ null với undefined
### 1.4. Optional chaining `?.` và nullish coalescing `??` dùng để làm gì? Cho ví dụ.

Trả lời:
?.: Truy cập thuộc tính an toàn khi đối tượng có thể là null hoặc undefined
??: Cung cấp giá trị mặc định khi giá trị bên trái là null hoặc undefined
ví dụ :
```ts
interface User {
  name: string;
  address?: {
    city: string;
  };
}
const user: User = {
  name: "Quang"
};
console.log(user.address?.city); nó sẽ ra undefined
const city = user.address?.city ?? "Hà Nội";
console.log(city);
```
## Phần 2: Hệ thống kiểu (so sánh với Java)

### 2.1. Structural typing là gì? Cho ví dụ hai kiểu khác tên nhưng vẫn gán được cho nhau. Java xử lý trường hợp này thế nào?

Trả lời:
Structural typing là cơ chế kiểm tra kiểu dữ liệu dựa trên cấu trúc, thuộc tính của đối tượng
Ví dụ :
```ts
interface User {
  id: number;
  name: string;
}
interface Customer {
  id: number;
  name: string;
}
const customer: Customer = {
  id: 1,
  name: "Quang"
};
const user: User = customer;
```
Java thì không cho phép gắn dù kiểu giống nhau muốn gắn được thì cần dùng extends,implements và xem quan hệ có hợp lý không
### 2.2. Union type là gì? Với một biến kiểu `string | number`, làm sao biết lúc chạy nó đang là kiểu nào?

Trả lời:
Union type cho phép một biến có thể thuộc một trong nhiều kiểu dữ liệu
Lúc đang chạy thì sẽ dùng typeof để kiểm tra kiểu dữ liệu của biến

### 2.3. Discriminated union là gì? Viết một ví dụ mô tả trạng thái đơn hàng.

Trả lời:
Discriminated union là union type mà các kiểu thành viên có chung một thuộc tính dùng để phân biệt
Ví dụ :
```ts
type Order =
  | {
      status: "pending";
      orderId: number;
    }
  | {
      status: "shipping";
      orderId: number;
      trackingCode: string;
    }
  | {
      status: "completed";
      orderId: number;
      deliveredAt: Date;
    };
function handleOrder(order: Order) {
  switch (order.status) {
    case "pending":
      console.log("Đơn đang chờ");
      break;
    case "shipping":
      console.log(order.trackingCode);
      break;
    case "completed":
      console.log(order.deliveredAt);
      break;
  }
}
```
### 2.4. Vì sao trong TypeScript hiện đại, nhiều người dùng union of literals (ví dụ `'S' | 'M' | 'L'`) thay cho `enum`?

Trả lời: vì Union of literals đơn giản, gọn và không tạo thêm object JavaScript ở runtime

### 2.5. Kiểu dữ liệu của TypeScript có tồn tại lúc chạy (runtime) không? Điều này khác Java thế nào và ảnh hưởng gì khi viết code?

Trả lời: Không tồn tại ở runtime . Trong java thì thông tin của class vẫn tồn tại trong runtime cho phép kiểm tra bằng instanceof.
Ảnh hưởng vì TypeScript không kiểm tra kiểu tại runtime nên dữ liệu nhận từ API có thể sai cấu trúc dù đã khai báo interface.

## Phần 3: Generic và utility type

### 3.1. Viết một hàm generic `first<T>(arr: T[])` trả về phần tử đầu tiên của mảng. Kiểu trả về nên là gì?

Trả lời:
```ts
function first<T>(arr: T[]): T | undefined {
  return arr[0];
}
console.log(first<number>([1, 2, 3])); 
console.log(first<string>(["A", "B"]));
console.log(first([]));
```
Kiểu trả về nên là T | undefined
### 3.2. Cú pháp `<T extends ...>` dùng để làm gì? Cho ví dụ.

Trả lời: extends trong generic dùng để giới hạn kiểu dữ liệu được truyền vào, yêu cầu kiểu đó phải đáp ứng một cấu trúc
Ví dụ : 
```ts
interface HasId {
  id: number;
}
function getId<T extends HasId>(obj: T): number {
  return obj.id;
}
const user = {
  id: 1,
  name: "Quang"
};
console.log(getId(user));
```
### 3.3. Khi viết API cập nhật sản phẩm (chỉ gửi lên những trường cần sửa), em sẽ định nghĩa kiểu dữ liệu đầu vào thế nào? Dùng utility type nào?

Trả lời: 
Em sẽ định nghĩa kiểu dữ liệu đầu vào cho tất cả các thuộc tính của sản phẩm đều là optional
Em sẽ sử dụng Partial<T> để biến tất cả thuộc tính của một kiểu thành optional.
## Phần 4: Bất đồng bộ

### 4.1. `async/await` hoạt động thế nào? JavaScript chạy một luồng thì làm sao xử lý được nhiều request cùng lúc?

Trả lời:
async: Khai báo hàm bất đồng bộ, luôn trả về Promise.
await: Chờ Promise hoàn thành trong phạm vi hàm async.
JavaScript thường chạy code trên một main thread, nhưng có thể xử lý nhiều tác vụ I/O nhờ Event Loop và các API bất đồng bộ của môi trường runtime.
Khi gặp await, hàm async tạm dừng, nhưng không chặn toàn bộ JavaScript thread. Những tác vụ khác vẫn có thể được xử lý.
### 4.2. Nếu quên `await` khi gọi một hàm `async` thì chuyện gì xảy ra? Cho ví dụ.

Trả lời:
Khi quên await, ta nhận về một Promise thay vì giá trị đã được giải quyết
Ví dụ : 
```ts
async function getNumber(): Promise<number> {
  return 100;
}
const result = getNumber();
console.log(result);
sửa 
async function main() {
  const result = await getNumber();
  console.log(result);
}
main();
```
### 4.3. Xử lý lỗi trong hàm `async` như thế nào?

Trả lời: xử lý lỗi e sử dụng try/catch kết hợp await

## Phần 5: Thực hành

### 5.1. Khai báo kiểu props cho một component React `ProductList` nhận vào danh sách sản phẩm và một hàm `onSelect` được gọi khi chọn một sản phẩm.

Trả lời:
```ts
interface Product {
  id: number;
  name: string;
  price: number;
}
interface ProductListProps {
  products: Product[];
  onSelect: (product: Product) => void;
}
function ProductList({
  products,
  onSelect
}: ProductListProps) {
  return (
    <div>
      {products.map((product) => (
        <div key={product.id}>
          <h3>{product.name}</h3>
          <p>{product.price} VND</p>

          <button onClick={() => onSelect(product)}>
            Chọn sản phẩm
          </button>
        </div>
      ))}
    </div>
  );
}
```
### 5.2. Khai báo kiểu cho body của request `POST /products` trong Express, và viết handler sử dụng kiểu đó.

Trả lời:
```ts
import express, { Request, Response } from "express";
const app = express();
app.use(express.json());
interface CreateProductDto {
  name: string;
  price: number;
  description?: string;
}
app.post(
  "/products",
  (
    req: Request<{}, {}, CreateProductDto>,
    res: Response
  ) => {
    const { name, price, description } = req.body;
    if (
      typeof name !== "string" ||
      name.trim() === "" ||
      typeof price !== "number" ||
      !Number.isFinite(price) ||
      price < 0 ||
      (description !== undefined &&
        typeof description !== "string")
    ) {
      res.status(400).json({
        message: "Dữ liệu sản phẩm không hợp lệ"
      });
      return;
    }
    const product = {
      id: 1,
      name,
      price,
      description
    };
    res.status(201).json(product);
  }
);
```
### 5.3. Đoạn code dưới đây có lỗi gì? Sửa lại cho đúng.

```ts
interface Product {
  id: number;
  name: string;
  price?: number;
}

function getTotal(products: Product[]): number {
  return products.reduce((sum, p) => sum + p.price, 0); // sai ở đây vì trên khai báo  price?: number có thể bị undefined không thể cộng trực tiếp khi bật strictNullChecks
}

async function loadProducts(): Product[] { // sai ở đây vì hàm có async luôn trả về Promise, không thể khai báo kiểu trả về là Product[].
  const res = await fetch('/api/products');
  return res.json();
}
```

Trả lời:
Sửa lại 
```ts
 interface Product {
  id: number;
  name: string;
  price?: number;
}
function getTotal(products: Product[]): number {
  return products.reduce(
    (sum, p) => sum + (p.price ?? 0),
    0
  );
}
async function loadProducts(): Promise<Product[]> {
  const res = await fetch('/api/products');
  return res.json();
}
```
