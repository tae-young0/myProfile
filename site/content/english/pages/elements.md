---
title: "Elements"
# meta title
meta_title: ""
# meta description
description: "This is meta description"
# save as draft
draft: false
---

{{< toc >}}
### 📌 대표 활동 {##activity}s

{{< accordion "Linux C 기반 계정 관리 시스템 개발" >}}

- **수행 기간:** 2026년 9월 ~
- **진행 상태:** **진행 중 (In progress)**
- Lorem ipsum dolor sit amet consectetur

{{< /accordion >}}

{{< accordion "How can I adjust Horizontal centering" >}}

- Lorem ipsum dolor sit amet consectetur adipisicing elit.
- Lorem ipsum dolor sit amet consectetur adipisicing elit.
- Lorem ipsum dolor sit amet consectetur

{{< /accordion >}}

{{< accordion "Should you use Negative margin?" >}}

- Lorem ipsum dolor sit amet consectetur adipisicing elit.
- Lorem ipsum dolor sit amet consectetur adipisicing elit.
- Lorem ipsum dolor sit amet consectetur

{{< /accordion >}}


---

 * **활동 내용:**
 Ubuntu Linux 환경에서 C 언어와 바이너리 파일 입출력(File I/O)을 활용하여 사용자 계정 생성, 조회, 수정 기능을 갖춘 프로그램(`ac_create.c`, `ac_query.c`, `ac_update.c`)을 직접 설계하고 구현하고 있습니다.
---
 * **Purpose**
  * **해결하려 한 문제:** 기존 텍스트 기반 파일 입출력 방식의 한계로 인해 데이터 처리 속도 저하 및 예기치 않은 데이터 손상 및 인코딩 오류
  * **달성하려 한 목표:** Ubuntu Linux 환경에서 C 언어와 바이너리 파일 입출력(File I/O) 방식을 활용해 데이터 처리 효율성을 높이고, `struct account` 구조체 설계 및 파일 포인터 제어를 통해 사용자 계정 생성(`ac_create.c`), 조회(`ac_query.c`), 수정(`ac_update.c`)이 안전하게 이루어지는 구조적 시스템을 구축
---
 * **본인 역할 및 기여 범위**
 * **수행 형태:** **개인 수행 과제 (Individual Project)**
 * **기여:**
  1. **구조체 및 파일 구조 설계:** 계정 정보(ID, 이름, 잔액 등)를 체계적으로 관리하기 위한 `struct account` 바이너리 레코드 구조를 직접 설계
  2. **핵심 모듈 구현:** 계정 생성(`ac_create.c`), 데이터 조회(`ac_query.c`), 정보 수정(`ac_update.c`) 등 핵심 기능 모듈 전체를 C 언어로 직접 작성
  3. **파일 포인터 제어 및 예외 처리:** `fseek`과 `fread`/`fwrite`를 활용해 지정된 위치의 레코드를 즉시 탐색·수정하는 로직을 구축하였으며, 잘못된 입력이나 파일 접근 오류에 대한 예외 처리 로직을 전담 구현

*  **수행 과정과 선택 이유**
  1. **요구사항 분석 및 설계:** 사용자 계정 데이터 구조체(`struct account`) 정의 및 기능별 모듈 분리 설계
  2. **모듈 구현 및 예외 처리:** `ac_create.c`, `ac_query.c`, `ac_update.c` C 소스 파일 구현 및 파일 입출력 예외 로직 작성
  3. **디버깅 및 버전 관리:** Ubuntu Linux 환경에서 GCC 빌드, 실행 테스트, Git/GitHub을 이용한 최종 버전 푸시 및 문서화
  4. **바이너리 파일 입출력(C 언어) 선택 이유:** 
  텍스트 기반(CSV, TXT) 처리 방식은 레코드 길이가 가변적이어서 특정 계정을 탐색하거나 수정할 때 전체 파일을 순차 검색해야 하는 성능적 한계가 있었습니다. 반면 바이너리 파일 구조와 C 언의 `fseek` 포인터 제어를 활용하면 고정된 구조체 크기만큼 직접 이동(Random Access)이 가능하여 데이터 탐색·수정 속도를 획기적으로 향상시킬 수 있기 때문에 이 기술 방식을 선택했습니다.

---

### 산출물

구현된 소스 코드 `ac_create.c` 

```c { linenos=true }
#include <stdio.h>
#include <stdlib.h> 

struct account {
      int id;
      char name[20];
      double balance;
  };

int main(void) {
      FILE *fp = fopen("accounts.dat", "ab");
      struct account acc = {1001, "TaeYoung", 50000.0};
      fwrite(&acc, sizeof(struct account), 1, fp);
      fclose(fp);
      printf("Account record created successfully.\n");
      return 0;
  }
```

```mermaidflowchart TD
    바이너리 파일 입출력과 구조체를 활용하여 신규 사용자 계정 레코드(1001, TaeYoung, 50000.0)를 데이터 파일(accounts.dat)에 안전하게 생성하는 모듈을 구현한 것입니다.
```

###  학습 결과 및 향후 개선 방향

* **학습 결과 (배운 점):**
  Linux 환경에서 C 언어 구조체 기반 바이너리 파일 포인터(`fseek`, `fread`, `fwrite`)를 직접 제어해 보며, 메모리와 실제 파일 시스템 간의 하부 데이터 처리 메커니즘을 깊이 있게 이해할 수 있었습니다. 또한, 명령줄 인수를 활용해 프로그램을 기능별로 모듈화하고 예외 처리를 정밀하게 설계하는 실무적 구현 역량을 익혔습니다.
* **한계점 및 향후 개선 방향:**
  현재 시스템은 단일 로컬 바이너리 파일 구조로 작동하여 사용자 수가 급증할 경우 데이터 검색 효율이 저하되고 다중 사용자 접근 시 동시성 제어(Concurrency Control)가 어렵다는 한계가 있습니다. 이를 개선하기 위해 향후 **B-Tree 인덱싱 알고리즘**을 도입해 탐색 성능을 $O(1)$ 수준으로 최적화하고, SQLite나 PostgreSQL 같은 **관계형 데이터베이스(RDBMS)**와의 연동 및 파일 트랜잭션 잠금(Locking) 기술을 적용하여 데이터 안정성과 동시 처리 성능을 향상시킬 계획입니다.

---


# Heading 1

## Heading 2

### Heading 3

#### Heading 4

##### Heading 5

###### Heading 6

---

### Emphasis

The emphasis, aka italics, with _asterisks_ or _underscores_.

Strong emphasis, aka bold, with **asterisks** or **underscores**.

The combined emphasis with **asterisks and _underscores_**.

Strike through uses two tildes. ~~Scratch this.~~

---

### Button

{{< button label="Button" link="/" style="solid" >}}

---

### Link

[I'm an inline-style link](https://www.google.com)

[I'm an inline-style link with title](https://www.google.com "Google's Homepage")

[I'm a relative reference to a repository file](../blob/master/LICENSE)

URLs and URLs in angle brackets will automatically get turned into links.
<http://www.example.com> or <http://www.example.com> and sometimes
example.com (but not on Github, for example).

Some text to show that the reference links can follow later.

---

### Paragraph

Lorem ipsum dolor sit amet consectetur adipisicing elit. Quam nihil enim maxime corporis cumque totam aliquid nam sint inventore optio modi neque laborum officiis necessitatibus, facilis placeat pariatur! Voluptatem, sed harum pariatur adipisci voluptates voluptatum cumque, porro sint minima similique magni perferendis fuga! Optio vel ipsum excepturi tempore reiciendis id quidem? Vel in, doloribus debitis nesciunt fugit sequi magnam accusantium modi neque quis, vitae velit, pariatur harum autem a! Velit impedit atque maiores animi possimus asperiores natus repellendus excepturi sint architecto eligendi non, omnis nihil. Facilis, doloremque illum. Fugit optio laborum minus debitis natus illo perspiciatis corporis voluptatum rerum laboriosam.

---

### Ordered List

1. List item
2. List item
3. List item
4. List item
5. List item

---

### Unordered List

- List item
- List item
- List item
- List item
- List item

---

### Notice

{{< notice "note" >}}
This is a simple note.
{{< /notice >}}

{{< notice "quote" >}}
This is a simple quote.
{{< /notice >}}

{{< notice "tip" >}}
This is a simple tip.
{{< /notice >}}

{{< notice "info" >}}
This is a simple info.
{{< /notice >}}

{{< notice "warning" >}}
This is a simple warning.
{{< /notice >}}

---
### 🎯 역량과 적용 사례 {#competencies}

{{< tabs >}}

{{< tab "Communication" >}}

#### 글로벌 소통 역량

프랑스어 전공 수업을 수강하며 외국인 교수님과 꾸준히 대화를 나누고 작성한 글과 발표에 대해 구체적인 피드백을 주고받는 소통 방식을 연습했습니다. 이 과정에서 언어적 장벽을 넘어 복잡한 생각을 상대방의 눈높이에 맞춰 명확하게 전달하고 경청하는 태도를 몸소 익혔습니다.

{{< /tab >}}

{{< tab "Technical" >}}

#### 개발 프로세스 및 시스템 기술 이해도(C, C++, Python, Linux)

Ubuntu Linux 기반에서 구조체와 바이너리 파일 입출력을 활용한 계정 관리 시스템 프로그램을 직접 구현하며 시스템 하부의 데이터 처리 방식을 깊이 이해했습니다. 
또한 VS Code와 Git/GitHub를 활용해 빌드부터 버전 관리까지의 실무적 개발 프로세스를 체득했습니다.

{{< /tab >}}

{{< tab "Insight" >}}

#### 데이터 기반 문제 해결

데이터분석준전문가(ADsP) 자격을 취득하는 과정에서 정량적 지표를 다루는 방법론을 배우고, 감이나 단순 추측이 아닌 객관적 데이터에 기반해 문제를 바라보는 시각을 확립했습니다.
{{< /tab >}}
{{< /tabs >}}

---

### Accordions

{{< accordion "Why should you need to do this?" >}}

- Lorem ipsum dolor sit amet consectetur adipisicing elit.
- Lorem ipsum dolor sit amet consectetur adipisicing elit.
- Lorem ipsum dolor sit amet consectetur

{{< /accordion >}}

{{< accordion "How can I adjust Horizontal centering" >}}

- Lorem ipsum dolor sit amet consectetur adipisicing elit.
- Lorem ipsum dolor sit amet consectetur adipisicing elit.
- Lorem ipsum dolor sit amet consectetur

{{< /accordion >}}

{{< accordion "Should you use Negative margin?" >}}

- Lorem ipsum dolor sit amet consectetur adipisicing elit.
- Lorem ipsum dolor sit amet consectetur adipisicing elit.
- Lorem ipsum dolor sit amet consectetur

{{< /accordion >}}


---

### Blockquote

> 바이너리 파일 입출력과 구조체를 활용하여 신규 사용자 계정 레코드(1001, TaeYoung, 50000.0)를 데이터 파일(accounts.dat)에 안전하게 생성하는 모듈을 구현한 것입니다.

---
### p29 학습 결과 및 향후 개선 방향

* **학습 결과 (배운 점):**
  Linux 환경에서 C 언어 구조체 기반 바이너리 파일 포인터(`fseek`, `fread`, `fwrite`)를 직접 제어해 보며, 메모리와 실제 파일 시스템 간의 하부 데이터 처리 메커니즘을 깊이 있게 이해할 수 있었습니다. 또한, 명령줄 인수를 활용해 프로그램을 기능별로 모듈화하고 예외 처리를 정밀하게 설계하는 실무적 구현 역량을 익혔습니다.
* **한계점 및 향후 개선 방향:**
  현재 시스템은 단일 로컬 바이너리 파일 구조로 작동하여 사용자 수가 급증할 경우 데이터 검색 효율이 저하되고 다중 사용자 접근 시 동시성 제어(Concurrency Control)가 어렵다는 한계가 있습니다. 이를 개선하기 위해 향후 **B-Tree 인덱싱 알고리즘**을 도입해 탐색 성능을 $O(1)$ 수준으로 최적화하고, SQLite나 PostgreSQL 같은 **관계형 데이터베이스(RDBMS)**와의 연동 및 파일 트랜잭션 잠금(Locking) 기술을 적용하여 데이터 안정성과 동시 처리 성능을 향상시킬 계획입니다.

---

### Tables

| Tables        |      Are      |  Cool |
| ------------- | :-----------: | ----: |
| col 3 is      | right-aligned | $1600 |
| col 2 is      |   centered    |   $12 |
| zebra stripes |   are neat    |    $1 |

---

### Image

{{< image src="images/image-placeholder.png" caption="" alt="alter-text" height="" width="" position="center" command="fill" option="q100" class="img-fluid" title="image title"  webp="false" >}}

---

### Gallery

{{< gallery dir="images/gallery" class="" height="400" width="400" webp="true" command="Fit" option="" zoomable="true" >}}

---

### Slider

{{< slider dir="images/gallery" class="max-w-[600px] ml-0" height="400" width="400" webp="true" command="Fit" option="" zoomable="true" >}}

---


