### 1. 저장소 개설

GitHub 저장소를 개설합니다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6b9a8b8c-e7d5-4d16-bd89-6078b84b696c/Untitled.png)

---

### 2. GitHub Actions 코드 작성

1. GitHub Actions을 사용해서 **파이썬 3.8 ~ 3.10 버전 별로** 출력 결과를 저장하십시오.
2. upload-artifact을 사용해서 출력된 결과물을 아티팩트로 저장해주세요.
Ref : https://github.com/actions/upload-artifact

**GitHub Actions 설정**

저장소의 메인 디렉토리에 .github/workflows 라는 새 디렉토리를 만든 후 이동

```bash
mkdir -p .github/workflows
```

**YAML형식의 워크플로우 파일 생성**

```bash
vim workflowPy.yml 

=======================================================================================

# 워크플로우 이름 
name: Python_Version

# Push, Pull_request 이벤트가 나올때 발동한다.
on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest

    # matrix strategy를 사용하여 여러 Python 버전을 작업한다.
    strategy:
      matrix:
        python-version: ["3.8", "3.9", "3.10"]

    steps:
      # 저장소의 코드를 체크아웃
      - uses: actions/checkout@v3

      # 각 Python 버전을 설정
      - name: Set up Python ${{ matrix.python-version }}
        uses: actions/setup-python@v2
        with:
          python-version: ${{ matrix.python-version }}

      # Python 버전을 출력하고 그 결과를 version.txt 파일에 저장
      - name: Display Python version
        run: python -c "import sys; print(sys.version)" > version.txt

      # version.txt 파일을 아티팩트에 업로드
      - name: Upload Python version as an artifact
        uses: actions/upload-artifact@v2
        with:
          # 아티팩트의 이름을 정의
          name: python-version-${{ matrix.python-version }}
          # 아티팩트로 업로드할 파일의 경로를 지정
          path: version.txt
```

`push`를 한 뒤 `Action` 탭에 들어가 `Workflow RUN`을 해준다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0c1ee65a-3b66-4a20-86b6-a6de809a8831/Untitled.png)

방금 만든 `Workflow`에 들어가준다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/86361a66-247a-4894-8f48-2936bc27c5eb/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/223ff0dc-208f-4cb2-a986-c0c32b475cdb/Untitled.png)

**잘 저장이 되었다.**
