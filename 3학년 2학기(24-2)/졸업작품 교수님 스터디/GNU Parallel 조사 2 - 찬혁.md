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
- 그런 다음 # 초 동안 잠자고, ‘-middle’과 ‘#-end’를 출력한다다.

- 인수와 같은 순서로 출력을 강제하려면 `--keep-order/-k`를 사용한다.

```bash
parallel -j2 -k half_line_print ::: 4 2 1
```

- 출력
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
- 작업 완료 전에 출력하기

- GNU Parallel은 명령이 완료될 때까지 출력을 연기한다.

```bash
parallel -j2 half_line_print ::: 4 2 1
```

- 출력
```bash
2-start
2-middle
2-end
1-start
1-middle
1-end
4-start
4-middle
4-end
```

- 이는 `--group`이 기본값이기 때문이다. 
- 즉시 출력을 받으려면 `--ungroup/-u`를 사용해라.

```bash
parallel -j2 --ungroup half_line_print ::: 4 2 1
```

- 출력
```bash
4-start
42-start
2-middle
2-end
1-start
1-middle
1-end
-middle
4-end
```

- `--ungroup`은 빠르지만 `--tag`를 비활성화하여 한 작업의 반 줄이 다른 작업의 반 줄과 섞일 수 있다. 
	- 두 번째 줄에서 '4-middle'이 '2-start'와 섞인 경우

- 이를 피하려면 `--linebuffer`를 사용하여 전체 줄만 출력하도록 합니다:

```bash
parallel -j2 --linebuffer half_line_print ::: 4 2 1
```

- 출력
```bash
4-start
2-start
2-middle
2-end
1-start
1-middle
1-end
4-middle
4-end
```

- `--keep-order` 또는 `--line-buffer`를 사용하면 GNU Parallel은 첫 번째 작업의 출력을 완료할 때까지 계속해서 해당 작업의 라인을 출력한 후, 두 번째 작업이 실행되는 동안 계속해서 그 작업의 라인을 출력한다. 
- 전체 라인을 버퍼링하지만, **서로 다른 작업의 출력은 섞이지 않는다.**

- 비교:
```bash
parallel -j4 'echo {}-a;sleep {};echo {}-b' ::: 1 3 2 4
```

- 출력:
```bash
1-a
1-b
2-a
2-b
3-a
3-b
4-a
4-b
```


```bash
parallel -j4 --line-buffer 'echo {}-a;sleep {};echo {}-b' ::: 1 3 2 4
```

- 출력:
```bash
2-a
3-a
1-a
4-a
1-b
2-b
3-b
4-b
```


```bash
parallel -j4 -k --line-buffer 'echo {}-a;sleep {};echo {}-b' ::: 1 3 2 4
```

- 출력:
```bash
1-a
1-b
3-a
3-b
2-b
4-a
4-b
```