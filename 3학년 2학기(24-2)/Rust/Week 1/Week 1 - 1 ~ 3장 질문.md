## 1. return value

```Rust
fn main() {
	let mut counter = 0;
	let result = loop {
		counter += 1;
		if counter == 10 {
			break counter * 2;
		}
	}
	println!("result : {result}");
}
```

- `;` return 시 사용하는 이유...?
- 안 해도 동일한 결과가 나온다.
- 아래의 코드 참고

```rust
fn main() {
	let mut counter = 0;
	let result = loop {
		counter += 1;
		if counter == 10 {
			break counter * 2
		}
	}
	println!("result : {result}");
}
```

---
## 2. `#[allow(dead_code)]`
- 이게 뭔지 잘 모르겠습니다

---
## 3. 세미콜론 반환값 문제
```rust
fn main() {
	let x = (let y = 6);
}
```

- 이거 컴파일 오류가 난다.
- let은 반환값이 없다?

```rust
fn main() {
	let y = {
		let x = 3;
		x + 1 // 세미콜론이 없다. 세미콜론 붙이면 반환값이 없어져서 y 할당이 안 된다.
	}
}
```

- y는 4가 된다.

---
