## Output
- Hello world

```rust
fn main() {
	println!("Hello World!");
}
```

- `println!` 은 함수(function)가 아니라 매크로(macro)이다

---
## Cargo
- Rust build system
- Package Manager using when build code ,download dependency and make library

### intructions
- `cargo new`로 새 프로젝트를 생성할 수 있습니다. 
- `cargo build` 명령으로 프로젝트를 빌드할 수 있습니다. 
- `cargo run` 명령어는 한 번에 프로젝트를 빌드하고 실행할 수 있습니다. 
- `cargo check` 명령으로 바이너리를 생성하지 않고 프로젝트의 에러를 체크 할 수 있습니다. 
- 빌드로 만들어진 파일은 작성한 소스 코드와 뒤섞이지 않도록 *target/debug* 디렉터리에 저장됩니다.

---
## Condition
- 조건식에는 bool 타입의 값만 올 수 있다.
- 아래와 같은 식은 잘못되었다.

```rust
fn main(){ 
	let x = 5; 
	if x { 
		println!("number is not zero"); 
	} 
}
```

- 이러면 옳은 식이 된다.

```rust
fn main(){ 
	let x = 5; 
	if x != 0 { 
		println!("number is not zero"); 
	} 
}
```

- 아래처럼 삼항 연산자처럼 사용이 가능하다.

```rust
fn main(){ 
	let condition = true; 
	let x = if condition { 1 } else { 0 }; 
}
```

---
