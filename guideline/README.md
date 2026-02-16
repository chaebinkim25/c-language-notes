# SEI CERT C Coding Standard - Unofficial Guideline (Korean)

> **⚠️ Disclaimer (면책 조항):**
>
> [SEI CERT C Coding Standard](https://wiki.sei.cmu.edu/confluence/display/c/SEI+CERT+C+Coding+Standard)의 내용을 학습 및 연구 목적으로 요약하고 정리한 비공식(Unofficial) 가이드이다.
>
> 이 프로젝트는 Carnegie Mellon University(CMU)나 Software Engineering Institute(SEI)와 공식적인 제휴 관계가 없으며, 원본 표준의 내용을 대체할 수 없습니다. 정확한 원문과 최신 업데이트는 반드시 [공식 위키](https://wiki.sei.cmu.edu/confluence/display/c/SEI+CERT+C+Coding+Standard)를 참조하시기 바랍니다.

## 📖 프로젝트 소개 (Introduction)

C 언어는 강력하지만, 메모리 관리와 미정의 동작(Undefined Behavior) 등 보안 취약점을 유발할 수 있는 요소가 많습니다.

이 프로젝트는 **SEI CERT C Coding Standard (2016 Edition 및 최신 위키)**를 기반으로, C 프로그래머가 안전하고 신뢰할 수 있는 코드를 작성하도록 돕기 위해 핵심 규칙(Rules)과 권고(Recommendations)를 알기 쉽게 정리한 한국어 가이드입니다.

### 🎯 목표 (Goals)
- 방대한 CERT C 표준을 핵심 위주로 요약 (Summary)
- 난해한 규격을 이해하기 쉬운 한국어 설명으로 풀이 (Translation & Explanation)
- 최신 컴파일러 환경(C99/C11/C23)에 맞춘 예제 코드 제공
- `Rules`(필수)와 `Recommendations`(권고)의 명확한 구분

## 🗂️ 목차 (Table of Contents)

이 가이드는 SEI CERT의 공식 분류 체계를 따릅니다.

1.  **[PRE] Preprocessor (전처리기)**
    - 매크로 사용 시 주의사항 및 안전한 헤더 파일 관리
2.  **[DCL] Declarations and Initialization (선언과 초기화)**
    - 변수 스코프 최소화, 상수 올바른 사용법
3.  **[EXP] Expressions (표현식)**
    - 연산자 우선순위, 평가 순서, 부작용(Side effects) 방지
4.  **[INT] Integers (정수)**
    - 정수 오버플로우/언더플로우 방지, 부호 있는/없는 정수 혼용 주의
5.  **[FLP] Floating Point (부동소수점)**
    - 정밀도 문제 및 비교 연산 주의사항
6.  **[ARR] Arrays (배열)**
    - 배열 인덱스 초과(Buffer Overflow) 방지
7.  **[STR] Characters and Strings (문자열)**
    - 문자열 터미널(NULL) 처리 및 안전한 문자열 함수 사용
8.  **[MEM] Memory Management (메모리 관리)**
    - 동적 할당, 해제(free), 메모리 누수 방지, Dangling Pointer
9.  **[FIO] Input/Output (입출력)**
    - 파일 입출력 검증 및 포맷 문자열 취약점 방지
10. **[ENV] Environment (환경)**
    - 프로그램 실행 환경 및 시스템 호출 보안
11. **[SIG] Signals (시그널)**
    - 비동기 시그널 처리 안전성
12. **[ERR] Error Handling (에러 처리)**
    - 반환값 검사 및 예외 상황 처리
13. **[MSC] Miscellaneous (기타)**
    - 기타 보안 권고 사항

## 📚 참고 자료 (References)

이 가이드는 다음의 공식 자료들을 바탕으로 작성되었습니다.

* **Official Wiki:** [SEI CERT C Coding Standard Wiki](https://wiki.sei.cmu.edu/confluence/display/c/SEI+CERT+C+Coding+Standard)
* **PDF:** SEI CERT C Coding Standard 2016 v1.0 (Rules Only)

## ⚖️ 라이선스 (License & Attribution)

이 프로젝트의 요약 및 번역 내용은 **MIT License**를 따릅니다.
단, 본 프로젝트가 기반으로 하는 원본 데이터(Original Source Material)에 대한 저작권은 **Carnegie Mellon University**에 있습니다.

자세한 내용은 [LICENSE.md](./LICENSE.md) 파일을 확인해 주십시오.

### Copyright Notice for Original Material
> This material is based upon work funded and supported by the Department of Defense under Contract No. FA8702-15-D-0002 with Carnegie Mellon University for the operation of the Software Engineering Institute, a federally funded research and development center.
>
> **Copyright 2016 Carnegie Mellon University.**
>
> NO WARRANTY. THIS CARNEGIE MELLON UNIVERSITY AND SOFTWARE ENGINEERING INSTITUTE MATERIAL IS FURNISHED ON AN "AS-IS" BASIS.
