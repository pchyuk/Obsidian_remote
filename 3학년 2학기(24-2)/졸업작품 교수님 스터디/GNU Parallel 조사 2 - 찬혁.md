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

- `--keep-order` `--line-buffer`를 사용하면 GNU Parallel은 첫 번째 작업의 출력을 완료할 때까지 계속해서 해당 작업의 라인을 출력한 후, 두 번째 작업이 실행되는 동안 계속해서 그 작업의 라인을 출력한다. 
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
2-a
2-b
4-a
4-b
```

### 6.4.1 Buffer on disk
- 디스크의 버퍼

- GNU Parallel은 출력 결과를 임시 파일에 버퍼링한다. 
- 프로그램의 출력이 여유 디스크 공간보다 많으면, 디스크는 `--group` 또는 `--line-buffer` `--keep-order`를 사용할 때 채워진다. 
- 이는 `--line-buffer`를 `--keep-order` 없이 사용할 때 적용되지 않으며, `--ungroup`(버퍼링하지 않음)에서도 적용되지 않는다.

---
## 6.5 Save output into files
- 파일로 출력 저장

- GNU Parallel은 각 작업의 출력을 파일에 저장할 수 있다.
```bash
parallel --files echo ::: A B C
```

- 출력은 다음과 비슷하다.
```bash
/tmp/pAh6uWQcg.par
/tmp/opjhZCzAX4.par
/tmp/wOAT_Rph2o.par
```

- 기본적으로 GNU Parallel은 `/tmp`에 파일로 출력을 캐시한다. 
- 이는 `$TMPDIR` 또는 `--tmpdir`을 설정하여 변경할 수 있다.

```bash
parallel --tmpdir /var/tmp --files echo ::: A B C
```

- 출력은 다음과 비슷하다.
```bash
/var/tmp/N_vk7phQRc.par
/var/tmp/7ZAcf3WZ.par
/var/tmp/Liuka_2L.par
```

- 또는
```bash
TMPDIR=/var/tmp parallel --files echo ::: A B C
```

- 출력: 위와 동일

- 출력 결과는 `--results`를 사용하여 구조화된 방식으로 저장할 수 있다.
```bash
parallel --results outfile echo ::: A B C
```

- 출력:
```bash
A
B
C
```

---
## 6.6 Save to CSV/TSV
- CSV/TSV로 저장하기

- 많은 프로그램이 쉼표로 구분된 값/탭으로 구분된 값 파일을 지원한다. 
- GNU Parallel도 마찬가지. 
- `--results` 인자가 .csv 또는 .tsv로 끝나면 출력은 CSV/TSV 파일이 된다.

```bash
parallel --results my.csv echo ::: A B ::: C D
```

---
## 6.7 SQL 데이터베이스에 저장하기
- GNU Parallel은 SQL 데이터베이스에 저장할 수 있다.
- GNU Parallel을 테이블에 지정하면, 각 변수와 출력이 각자의 열에 함께 배치된다.

### 6.7.1 CSV를 SQL 데이터베이스로 사용하기
- 가장 간단한 방법은 CSV 파일을 저장 테이블로 사용하는 것이다.
```bash
parallel --sqlandworker csv:///%%2Ftmp%2Flog.csv \
seq ::: 10 ::: 12 13 14
cat /tmp/log.csv
```

---
## 6.8 셸 변수에 출력 저장
- GNU Parset는 GNU Parallel의 출력 결과를 셸 변수로 설정한다. 
- GNU Parset에는 하나의 중요한 제한 사항이 있다.
	- **파이프의 일부가 될 수 없다.** 
	- 특히, 이는 표준 입력(stdin)에서 읽거나 다른 프로그램으로 출력을 파이프할 수 없음을 의미한다.

- GNU Parset는 셸 함수이다. 
- 다음 명령어를 실행하여 활성화한다.
```bash
env_parallel --install
```

- 그 후 새 셸을 시작한다.

- Parset는 bash, dash, ash, sh, ksh, zsh에서 지원됩니다.

- parset를 사용하려면, 목적 변수(destination value)를 일반 GNU Parallel 옵션과 명령어 앞에 배치한다.

```bash
parset myvar1,myvar2 -j2 echo ::: a b
echo $myvar1
echo $myvar2
```

- 출력
```bash
a
b
```

- 단일 변수만 제공하면 배열로 처리된다.
```bash
parset myarray seq {} 5 ::: 1 2 3
echo "${myarray[1]}"
```

- 출력
```bash
2
3
4
5
```

- 명령어를 실행할 수 있는 배열:
```bash
cmd=("echo '<<Joe \"double space\" cartoon>>'" "pwd")
parset data -j2 ::: "${cmd[@]}"
echo "${data[0]}"
echo "${data[1]}"
```

- 출력
```bash
<<Joe "double space" cartoon>>
[current dir]
```

---
### 6.8.1 파이프에서 읽지 않기
- GNU Parset는 파이프에서 읽을 수 없다. 
- 이는 parset가 서브셸에서 시작되기 때문이며, 
- 따라서 출력이 시작 셸에서 보이지 않게 된다. 이를 위한 몇 가지 해결 방법이 있습니다.

6.8.1.1 임시 파일 사용

파이프에서 직접 읽는 대신, 출력을 파일에 저장하고 parset가 그 파일에서 읽도록 합니다:

bash


seq 3 > parallel_input
parset res1,res2,res3 echo ::: parallel_input
echo "$res1"
echo "$res2"
echo "$res3"
rm parallel_input
6.8.1.2 프로세스 치환 사용

셸이 프로세스 치환을 지원하면(Bash, Zsh, Ksh 모두 지원), 이를 사용할 수 있습니다:

bash


parset res echo ::: <(seq 100)
echo "${res[1]}"
echo "${res[99]}"
6.8.1.3 FIFO 사용

데이터의 양이 많고 GNU Parset가 출력을 생성하기 전에 읽기 시작해야 하는 경우, FIFO를 사용하여 생성할 수 있습니다.