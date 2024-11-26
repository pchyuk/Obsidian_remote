# 9. Pipe mode
- GNU Parallel은 명령어 템플릿에 값을 넣는 대신, 표준 입력(stdin)을 명령어로 파이프를 통해 전달할 수 있다.  
- `--pipe` 기능은 GNU Parallel을 다른 모드로 전환한다. 즉, 표준 입력 데이터를 명령어 실행을 위한 인수로 처리하는 대신, 데이터를 명령어의 표준 입력(stdin)으로 전달한다.

- 일반적인 상황은 다음과 같다.
```bash
command_A | command_B | command_C
```

- 여기서 `command_B`가 느릴 경우, 이를 병렬로 실행하여 속도를 높이고자 한다.  
	- 3장에서 제공된 테스트 파일이 필요

## 9.1 Block size
- 기본적으로 GNU Parallel은 `command_B`의 인스턴스를 시작하고, 1MB의 블록을 읽은 다음, 가장 가까운 레코드를 찾아 해당 청크(chunk)를 해당 인스턴스에 전달한다. 
- 그런 다음 다른 인스턴스를 시작하고, 또 다른 블록을 읽어 가장 가까운 레코드를 찾아 두 번째 인스턴스에 해당 청크를 전달한다.

- 예제
```bash
cat numi000000 | parallel --pipe wc
```