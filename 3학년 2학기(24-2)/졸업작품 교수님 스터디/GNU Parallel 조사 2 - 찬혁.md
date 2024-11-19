# 6. Control the output
- GNU Parallel은 일반적으로 작업이 완료되면 결과를 출력한다.
## 6.1 Tag output
- 출력은 인수로 접두사를 붙일 수 있다.
```bash
parallel --tag echo foo -{} ::: A B C
```

- 출력 결과(순서는 변동 가능)
```bash
A    foo-A
B    foo-B
C    foo-C
```

- `--tag`는 `--tagstring {}`의 약어이다.
- 다른 문자열을 접두사로 사용하려면 `--tagstring`을 사용한다.
```bash
parallel --tagstring {}-bar echo foo -{} ::: A B C
```

- 출력 결과(순서는 변동 가능)
```bash
A-bar    foo-A
B-bar    foo-B
C-bar    foo-C
```

---
## 6.2 See what is being run
- 무엇이 실행되고 있는지 확인

- `--dryrun`을 사용하여 실행하지 않고도 어떤 명령이 실행될지 확인한다.
```bash
parallel --dryrun echo {} ::: A B C
```

- 출력 결과(순서는 변동 가능)
```bash
echo A
echo B
echo C
```

- 실행하기 전에 명령을 출력하고 싶을 땐 `--verbose`를 사용한다.
```bash
parallel --verbose echo {} ::: A B C
```

- 출력 결과(순서는 변동 가능)
```bash
echo A
echo B
A
echo C
B
C
```

- 이 내용은 반만 맞음. 8.8장에서 더 다룰 것

---
## 6.3 Force same order as input
- 입력과 동일한 순서로 강제 실행

```bash
half_line_print(){
	printf "%s-start\n%s" $1 $1
	sleep $1
	printf "%s\n" -middle
	echo $1-end
}
export -f half_line_print
```

- 숫자 (#)를 인수로 받아들인다. 
- 전체 줄 ‘#-start’와 그 뒤에 반 줄 ‘#’를 출력한다. 
- 그런 다음 # 초 동안 잠자고, ‘-middle’과 ‘#-end’를 출력합니다.

- 인수와 같은 순서로 출력을 강제하려면 `--keep-order/-k`를 사용한다.

```bash
parallel -j2 -k half_line_print ::: 4 2 1
```

- output
```bash
4-start
4-middle
4-end
2-start
2-middle
2-end
1-start
1-middle
1-end
```

---
## 6.4 Output before jobs complete
