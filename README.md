# PintOS Project 3 · Virtual Memory

KAIST PintOS를 바탕으로 가상 메모리를 구현하는 크래프톤 정글 팀 프로젝트입니다.
SPT, 프레임 관리, 지연 로딩, 프로세스 복사와 파일 매핑을 다룹니다.

> **개발 코드는 [`dev`](https://github.com/Karfton-Jungle-PintOS/PintOS-Project-3/tree/dev)에서 확인해 주세요.**
> `main`의 소스는 초기 버전입니다. `mmap`·`munmap` 작업은 [PR #32](https://github.com/Karfton-Jungle-PintOS/PintOS-Project-3/pull/32)에서 검토 중이며 아직 병합되지 않았습니다.

## 바로가기

| 항목 | 링크 |
| --- | --- |
| 최신 개발 코드 | [dev 브랜치](https://github.com/Karfton-Jungle-PintOS/PintOS-Project-3/tree/dev) |
| 진행 상황·잔여 작업 | [공개 프로젝트 보드](https://github.com/orgs/Karfton-Jungle-PintOS/projects/2) |
| 파일 매핑 작업·검증 결과 | [mmap·munmap PR #32](https://github.com/Karfton-Jungle-PintOS/PintOS-Project-3/pull/32) |
| 환경 설정 안내 | [초기 개발 환경 가이드 원문](docs/development-environment.md) |
| 과제 명세 | [KAIST PintOS · Virtual Memory](https://casys-kaist.github.io/pintos-kaist/project3/introduction.html) |

## 구현 영역

| 영역 | 주요 내용 | 확인 위치 |
| --- | --- | --- |
| SPT·Frame Table | 페이지 정보와 물리 프레임 관리 | [dev/vm](https://github.com/Karfton-Jungle-PintOS/PintOS-Project-3/tree/dev/pintos/vm) |
| Lazy Loading·Fork | 첫 접근 시 페이지 적재, 자식 프로세스의 페이지 복사 | [dev/vm.c](https://github.com/Karfton-Jungle-PintOS/PintOS-Project-3/blob/dev/pintos/vm/vm.c) |
| mmap·munmap | 파일 매핑·해제, 변경된 내용의 파일 반영 | [PR #32](https://github.com/Karfton-Jungle-PintOS/PintOS-Project-3/pull/32) |
| Swap·자원 정리 | 페이지 교체와 스왑, 종료 시 자원 회수 | [프로젝트 보드](https://github.com/orgs/Karfton-Jungle-PintOS/projects/2) |

구현과 검증은 진행 중이며, `mmap`·`swap` 관련 잔여 작업이 있습니다.
아래는 **PR #32의 커밋 `29d9f1e`**를 실행한 결과입니다. `main`·`dev`의 검증 결과와 구분합니다.

| 검증 범위 | PASS | FAIL |
| --- | ---: | ---: |
| 전체 141개 | 100 | 41 |
| 그중 mmap 25개 | 21 | 4 |

실패 항목과 제한 사항은 [PR #32](https://github.com/Karfton-Jungle-PintOS/PintOS-Project-3/pull/32)의 검증 기록을 확인해 주세요.

## 개발 환경

- Docker Desktop과 VS Code의 Dev Containers 확장이 필요합니다.
- 저장소의 `.devcontainer`는 Ubuntu 22.04, x86-64, GCC, QEMU 환경을 구성합니다.
- 개발 코드를 받으려면 `dev` 브랜치를 사용합니다.

```bash
git clone --branch dev https://github.com/Karfton-Jungle-PintOS/PintOS-Project-3.git
cd PintOS-Project-3
```

VS Code에서 이 폴더를 열고 **Dev Containers: Reopen in Container**를 실행합니다.
이후 명령은 컨테이너의 **Bash 터미널**, 저장소 최상위 폴더에서 시작합니다.

## 빌드와 테스트

```bash
source pintos/activate
cd pintos/vm
make
make check
```

`make`와 `make check`는 `pintos/vm/build`에서 실행됩니다.
전체 결과는 `pintos/vm/build/results`에 기록됩니다.

개별 테스트는 빌드 후 다음과 같이 실행할 수 있습니다.

```bash
cd build
make tests/vm/mmap-kernel.result
```

테스트 기록에는 실행한 커밋, 명령, 통과·실패 결과를 함께 남깁니다.
`dev`를 받은 경우 아직 병합되지 않은 PR #32의 변경은 포함되지 않습니다.

## 개발 및 PR 절차

1. 자신의 fork에서 최신 원본 `dev`를 기준으로 작업 브랜치를 만듭니다.
2. 변경 목적에 맞게 `feat/`, `fix/`, `docs/` 접두사를 사용합니다.
3. 빌드와 관련 테스트를 확인하고 결과 및 남은 제한 사항을 PR에 적습니다.
4. fork에서 **이 저장소의 `dev`**를 대상으로 PR을 엽니다.
5. PR 작성자 외 **2명의 승인**을 받은 뒤 병합합니다.

`main`과 `dev`에는 PR을 통해 변경을 반영하며 관리자 우회는 허용하지 않습니다.
검토 후 새 변경을 올리면 승인을 다시 받아야 합니다.
PR은 [저장소 템플릿](.github/pull_request_template.md)에 맞춰 작성합니다.
