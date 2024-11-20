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
- 이는 parset가 서브셸에서 시작되기 때문이며, 따라서 출력이 시작 셸에서 보이지 않게 된다. 
- 이를 위한 몇 가지 해결 방법이 있다.

#### 6.8.1.1 임시 파일 사용
- 파이프에서 직접 읽는 대신, 출력을 파일에 저장하고 parset가 그 파일에서 읽도록 한다.

```bash
seq 3 > parallel_input
parset res1,res2,res3 echo ::: parallel_input
echo "$res1"
echo "$res2"
echo "$res3"
rm parallel_input
```

#### 6.8.1.2 프로세스 치환 사용
- 셸이 프로세스 치환을 지원하면(Bash, Zsh, Ksh 모두 지원), 이를 사용할 수 있다.

```bash
parset res echo ::: <(seq 100)
echo "${res[1]}"
echo "${res[99]}"
```

#### 6.8.1.3 FIFO 사용
- 데이터의 양이 많고 GNU Parset가 출력을 생성하기 전에 읽기 시작해야 하는 경우, FIFO를 사용하여 생성할 수 있다.

```bash
mkfifo input_fifo
seq 3 > input_fifo &
parset res1,res2,res3 echo ::: input_fifo
echo "$res1"
echo "$res2"
echo "$res3"
rm input_fifo
```

---
### 6.8.2 env_parset
- env_parset는 parset와 동일한 기능을 수행하지만 env_parallel을 사용한다.
	- 8.7 환경 변수 및 함수 전송 참조
- 따라서 parallel 대신 사용할 경우 별칭, 내보내지 않은 함수 및 내보내지 않은 변수에 접근할 수 있다.

---
# 7. Control the execution
- GNU Parallel은 CPU 코어당 하나의 작업을 병렬로 시작하고 모든 작업이 완료될 때까지 종료되지 않는다. 

## 7.1 동시 작업 수
- 동시 작업 수는 `--jobs` 또는 `-j`로 지정된다.
	- `-N0`는 단일 인수를 읽지만, `sleep 1`을 여러 번 병렬로 실행

```bash
/usr/bin/time parallel -N0 -j64 sleep 1 ::: num128
```

- 64개의 작업을 병렬로 실행하면, 128개의 sleep은 머신 속도에 따라 2-8초가 걸릴 것이다.

- 기본적으로 `--jobs`는 CPU 코어 수와 같다. 
- 따라서 다음과 같이 실행된다.

```bash
/usr/bin/time parallel -N0 sleep 1 ::: num128
```

- 이는 CPU 코어당 2개의 작업을 실행하는 데 걸리는 시간의 두 배가 걸려야 한다.

```bash
/usr/bin/time parallel -N0 --jobs 200% sleep 1 ::: num128
```

- `--jobs 0`은 가능한 한 많은 작업을 병렬로 실행한다.

```bash
/usr/bin/time parallel -N0 --jobs 0 sleep 1 ::: num128
```

- `--jobs`는 작업이 완료될 때마다 다시 읽는 파일에서 값을 읽을 수 있다.

```bash
echo 50% > my_jobs
/usr/bin/time parallel -N0 --jobs my_jobs sleep 1 ::: num128 & sleep 1
echo 0 > my_jobs
wait
```

- GNU Parallel은 시작할 때 `my_jobs`를 읽는다. 
- 이 파일에는 50%가 포함되어 있으므로, GNU Parallel은 코어 수의 50%를 계산하고 그만큼의 작업을 병렬로 시작한다.

- `&` 때문에 GNU Parallel은 백그라운드에서 시작됩니다.

- 1초 후에 `0`이 `my_jobs`에 입력된다. 작업이 완료되면, GNU Parallel은 `my_jobs`를 다시 읽고 가능한 한 많은 작업을 시작한한다.

- CPU 코어 수 대신 CPU 수를 기준으로 비율을 계산할 수도 있다.

```bash
parallel --use-cpus-instead-of-cores -N0 sleep 1 ::: num8
```

---
## 7.2 작업 순서 섞기
- 여러 개의 작업이 있는 경우(예: 여러 입력 소스의 조합으로), 작업의 순서를 섞으면 각기 다른 값을 먼저 실행할 수 있다. 
- 이를 위해 `--shuf`를 사용한다.

```bash
parallel --shuf echo ::: 1 2 3 ::: a b c ::: A B C
```

- 출력: 모든 조합이지만 각 실행마다 다른 순서.

---
## 7.3 상호 작용성 (Interactivity)
- GNU Parallel은 명령을 실행할 때 `--interactive` 플래그를 사용할 수 있도록 요청할 수 있다.

```bash
parallel --interactive echo ::: 1 2 3
```

- 출력:

```bash
echo 1 ?...y
echo 2 ?...n
1
echo 3 ?...y
3
```

- GNU Parallel은 인터랙티브 명령어(예: `emacs`)에 인수를 전달하여 한 번에 파일을 편집할 수 있다.

```bash
parallel --tty emacs ::: file1 file2 file3
```

 - 또는 여러 개의 인수를 한 번에 열어 여러 파일을 열 수 있다.

```bash
parallel -X --tty vi ::: file1 file2 file3
```

---
## 7.4 각 작업에 대한 터미널
- `--tmux`를 사용하면 GNU Parallel이 각 작업 실행에 대해 터미널을 시작할 수 있다.

```bash
seq 10 20 | parallel --tmux 'echo start {}; sleep {}; echo done {}'
```

- 이 명령은 다음과 비슷한 내용을 실행하라고 지시한다.

```bash
tmux -S /tmp/tmsrPr00 attach
```

- 일반 `tmux` 키 조합(CONTROL-b n 또는 CONTROL-b p)을 사용하여 실행 중인 작업의 창 사이를 전환할 수 있다.
- 작업이 완료되면 창을 닫기 전에 10초 동안 일시 정지한다.

- 각 작업을 자신의 창에서 열려면 `--tmuxpane`을 사용
- `--fg`는 즉시 `tmux`에 연결한다.

```bash
parallel --tmuxpane --fg \
'echo start {}; sleep {}; echo done {}' ::: 10 11 12 13 14 15 16 17
```

---
## 7.5 타이밍
- 일부 작업은 시작할 때 많은 I/O를 수행한다. 
- 그래서 GNU Parallel은 새로운 작업을 시작하는 것을 지연시킬 수 있다. 
	- 엄청난 무리의 소동을 피하기 위해
- `--delay X`는 각 시작 전에 최소한 X 초가 경과하도록 보장한다.

```bash
parallel --delay 2.5 echo Starting {}; date ::: 1 2 3
```

- 출력:

```bash
Starting 1
Thu Aug 15 16:24:33 CEST 2013
Starting 2
Thu Aug 15 16:24:35 CEST 2013
Starting 3
Thu Aug 15 16:24:38 CEST 2013
```

- 작업이 특정 시간 이상 걸리는 경우 실패할 가능성이 있는 경우, `--timeout`을 사용하여 중단할 수 있다. 
	- `--timeout`의 정확도는 2초이다. 
	- `--timeout 100000`은 `--timeout 1d3.5h16.6m4s`로 표현할 수 있다.

```bash
parallel --timeout 4.1 sleep {}; echo {} ::: 2 4 6 8
```

- 출력:

```bash
2
4
```

- GNU Parallel은 작업의 중앙값 실행 시간을 계산하고 200% 이상 걸리는 작업을 종료할 수 있다.

```bash
parallel --timeout 200% sleep {}; echo {} ::: 2.1 2.2 3 7 2.3
```

- 출력:

```bash
2.1
2.2
3
2.3
```

- 이는 몇몇 작업이 비정상적으로 실행되어 나머지 작업보다 훨씬 더 오랜 시간이 걸릴 때 유용하다.

---
## 7.6 진행 정보
- 완료된 작업의 실행 시간을 기반으로 GNU Parallel은 총 실행 시간을 추정할 수 있다.

```bash
parallel --eta sleep ::: 1 3 2 2 1 3 3 2 1
```

- 출력:

```bash
Computers / CPU cores / Max jobs to run
1:local / 2 / 2

Computer:jobs running/jobs completed/% of started jobs/
Average seconds to complete
ETA: 2s 0 left 1.11 avg local:0/9/100%/1.1s
```

- GNU Parallel은 `--progress`를 사용하여 진행 정보를 제공할 수 있다.

```bash
parallel --progress sleep ::: 1 3 2 2 1 3 3 2 1
```

- 출력:

```bash
Computers / CPU cores / Max jobs to run
1:local / 2 / 2

Computer:jobs running/jobs completed/% of started jobs/
Average seconds to complete 
local:0/9/100%/1.1S
```

- 진행 표시줄은 `--bar`로 표시할 수 있다.

```bash
parallel --bar sleep ::: 1 3 2 2 1 3 3 2 1
```

- 그래픽 표시줄은 `--bar`와 `zenity`를 사용하여 표시할 수 있다.

```bash
seq 1000 | parallel -j10 --bar '(echo -n {}; sleep 0.1)' \
2> >(zenity --progress --auto-kill --auto-close)
```

---
## 7.7 로그 파일
- 지금까지 완료된 작업의 로그 파일은 `--joblog`를 사용하여 생성할 수 있다.

```bash
parallel --joblog /tmp/log exit ::: 1 2 3 0
cat /tmp/log
```

- 출력:

```bash
Seq Host Starttime    Runtime Send Receive Exitval Signal Command
1 : 1376577364.974 0.008  0 0 1 0 exit 1
2 : 1376577364.982 0.013  0 0 2 0 exit 2
3 : 1376577364.990 0.013  0 0 3 0 exit 3
4 : 1376577365.003 0.003  0 0 0 0 exit 0
```

- 로그에는 작업 순서, 작업이 실행된 호스트, 시작 시간 및 실행 시간, 전송된 데이터 양, 종료 값, 작업을 종료시킨 신호, 마지막으로 실행된 명령이 포함된다.

---
## 7.8 작업 재개
- `--joblog`를 사용하면 GNU Parallel을 중단하고 중단된 지점에서 다시 시작할 수 있다. 
- 완료된 작업의 입력은 변경되지 않는다.

```bash
parallel --joblog /tmp/log exit ::: 1 2 3 0
parallel --resume --joblog /tmp/log exit ::: 1 2 3 0
cat /tmp/log
```

- 출력:

```bash
Seq Host Starttime Runtime Send Receive Exitval Signal Command 
1 : 1376580069.544 0.008 0 0 1 0 exit 1 
2 : 1376580069.552 0.009 0 0 2 0 exit 2 
3 : 1376580069.560 0.012 0 0 3 0 exit 3 
4 : 1376580069.571 0.005 0 0 0 0 exit 0 
5 : 1376580070.028 0.009 0 0 0 0 exit 0 
6 : 1376580070.038 0.007 0 0 0 0 exit 0
```

- 주목: **마지막 두 작업의 시작 시간이 첫 번째 실행과 분명히 다르다**.

- `--resume-failed`와 함께 GNU Parallel은 실패한 작업을 다시 실행한다.

```bash
parallel --resume-failed --joblog /tmp/log exit ::: 1 2 3 0 0 0
cat /tmp/log
```

- 출력:

```bash
Seq Host Starttime Runtime Send Receive Exitval Signal Command
1 : 1376580069.544 0.008 0 0 1 0  exit 1
2 : 1376580069.552 0.009 0 0 2 0  exit 2
3 : 1376580069.560 0.012 0 0 3 0  exit 3
4 : 1376580069.571 0.005 0 0 0 0  exit 0
5 : 1376580070.028 0.009 0 0 0 0  exit 0
6 : 1376580070.038 0.007 0 0 0 0  exit 0
1 : 1376580154.433 0.010 0 0 1 0  exit 1
2 : 1376580154.444 0.022 0 0 2 0  exit 2
3 : 1376580154.466 0.005 0 0 3 0  exit 3
```

- 마지막으로 seq 1 2 3이 반복되었는데, 이는 이들이 0이 아닌 다른 종료 코드를 반환했기 때문이다.

- `--retry-failed`는 `--resume-failed`와 거의 동일하게 작동한다. 
- `--resume-failed`는 명령줄에서 명령을 읽고(joblog의 명령은 무시됨), 
- `--retry-failed`는 명령줄을 무시하고 joblog에 언급된 명령을 다시 실행한다.

```bash
parallel --retry-failed --joblog /tmp/log
cat /tmp/log
```

- 출력:

```bash
Seq Host Starttime Runtime Send Receive Exitval Signal Command
1 : 1376580069.544 0.008 0 0 1 0  exit 1
2 : 1376580069.552 0.009 0 0 2 0  exit 2
3 : 1376580069.560 0.012 0 0 3 0  exit 3
4 : 1376580069.571 0.005 0 0 0 0  exit 0
5 : 1376580070.028 0.009 0 0 0 0  exit 0
6 : 1376580070.038 0.007 0 0 0 0  exit 0
1 : 1376580154.433 0.010 0 0 1 0  exit 1
2 : 1376580154.444 0.022 0 0 2 0  exit 2
3 : 1376580154.466 0.005 0 0 3 0  exit 3
1 : 1376580164.633 0.010 0 0 1 0  exit 1 
2 : 1376580164.644 0.022 0 0 2 0  exit 2
3 : 1376580164.666 0.005 0 0 3 0  exit 3
```

---
## 7.9 종료
### 7.9.1 무조건 종료
- 기본적으로 GNU Parallel은 모든 작업이 완료될 때까지 기다렸다가 종료한다.

- GNU Parallel에 TERM 신호를 보내면, 새로운 작업의 생성을 중단하고 남은 작업이 완료될 때까지 기다린다. 
- 다시 GNU Parallel에 TERM 신호를 보내면, GNU Parallel은 모든 실행 중인 작업을 종료하고 나온다.

### 7.9.2 작업 상태에 따른 종료
- 특정 작업의 경우, 하나의 작업이 실패하고 종료 코드가 0이 아닌 경우 계속 진행할 필요가 없다. 
- GNU Parallel은 `--halt soon,fail=1` 옵션을 사용하여 새로운 작업의 생성을 중단한다.

```bash
parallel -j2 --halt soon,fail=1 echo {} \; exit {} ::: 0 0 1 2 3
```

- 출력:

```bash
0
0
1
parallel: This job failed:
echo 1; exit 1
parallel: Starting no more jobs. Waiting for 1 jobs to finish
2
```

- `--halt now,fail=1` 옵션을 사용하면 실행 중인 작업이 즉시 종료된다.

```bash
parallel -j2 --halt now,fail=1 echo {} \; exit {} ::: 0 0 1 2 3
```

- 출력:

```bash
0
0
1
parallel: This job failed:
echo 1; exit 1
```

---
# 8. Remote execution
- GNU Parallel은 원격 서버에서 작업을 실행할 수 있다.
- 원격 머신과 통신하기 위해 `ssh`를 사용한다.

- 다음에서는 두 개의 서버에 접근할 수 있다고 가정 
	- `$SERVER1`과 `$SERVER2`
```bash
SERVER1=server.example.com
SERVER2=server2.example.net
```

- 따라서 다음을 수행할 수 있어야 한다.
```bash
ssh $SERVER1 echo works
ssh $SERVER2 echo works
```

- 설정은 다음 명령어를 실행하여 할 수 있다.
```bash
ssh-keygen -t dsa; ssh-copy-id $SERVER1; ssh-copy-id $SERVER2
```

- 빈 패스프레이즈(passphrase)를 사용합니다.

---
## 8.1 Sshlogin
- 가장 기본적인 sshlogin은 `-S host/--sshlogin host:`
```bash
parallel --sshlogin $SERVER1 echo running on ::: server1
```

- 출력:
```bash
running on server 1
```

- 다른 사용자 이름을 사용하려면 서버 앞에 `username@`를 추가한다.
```bash
parallel -S username@$SERVER1 echo running on ::: username@server1
```

- 출력:
```bash
running on username@server1
```

- 특별한 sshlogin `:`는 로컬 머신이다.
```bash
parallel -S : echo running on ::: _the_local_machine
```

- 출력:
```bash
running on the_local_machine
```

---
### 8.1.1 SSH 명령어 사용하기
- ssh가 `$PATH`에 없으면 `$SERVER1` 앞에 추가할 수 있다.
```bash
parallel -S '/usr/bin/ssh' $SERVER1 echo custom ::: ssh
```

- 출력:
```bash
custom ssh
```

- ssh 명령어는 `--ssh`로도 지정할 수 있다.


parallel --ssh /usr/bin/ssh -S $SERVER1 echo custom ::: ssh
또는 $PARALLEL_SSH를 설정하여 사용할 수도 있습니다:


export PARALLEL_SSH=/usr/bin/ssh
parallel -S $SERVER1 echo custom ::: ssh
8.1.2 여러 서버
여러 서버는 -S를 여러 번 사용하여 지정할 수 있습니다:


parallel -S $SERVER1 -S $SERVER2 echo ::: running on more hosts
출력(순서는 다를 수 있습니다):


running
on
more
hosts
또는 ,로 구분할 수 있습니다:


parallel -S $SERVER1,$SERVER2 echo ::: running on more hosts