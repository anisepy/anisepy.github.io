---
title:  "[Git] GitHub 레포 CI 관리하기 "
excerpt: "지속적 통합 관리와 그 관리를 증명하기 위한 codeclimate CI 배지 작업 "

categories:
  - Git
tags:
  - [Git, Github, Commit, CI, CD, codeclimate]

toc: true
toc_sticky: true
 
date: 2022-10-26
last_modified_at: 2022-10-26
---

# GitHub 레포 CI 관리하기

![CICD-Pipeline-e1613664546213](https://user-images.githubusercontent.com/103994044/197958164-9f4ac182-312b-4f64-93dc-a9425d1d9727.png)

### 🦊 CI/CD란?

CI(Continuous Integration)/CD(Continuous Delivery or Deployment)의 기본 개념은 지속적인 통합, 지속적인 서비스 제공, 지속적인 배포 입니다. 

CI를 성공적으로 구현할 경우 애플리케이션에 대한 새로운 코드 변경 사항이 정기적으로 빌드 및 테스트되어 공유 리포지토리에 통합되므로 여러 명의 개발자가 동시에 애플리케이션 개발과 관련된 코드 작업을 할 경우 서로 충돌할 수 있는 문제를 해결할 수 있습니다.

<details>
<summary>CD 설명 보기</summary>
<div markdown="1">

"CD"의 제공(Delivery)과 배포(Deployment)는 파이프라인의 추가 단계에 대한 자동화를 뜻하지만 때로는 얼마나 많은 자동화가 이루어지고 있는지를 설명하기 위해 별도로 사용되기도 합니다.
    
지속적인 *제공*이란 개발자들이 애플리케이션에 적용한 변경 사항이 버그 테스트를 거쳐 리포지토리(예: [GitHub](https://redhatofficial.github.io/#!/main) 또는 컨테이너 레지스트리)에 자동으로 업로드되는 것을 뜻하며, 운영팀은 이 리포지토리에서 애플리케이션을 실시간 프로덕션 환경으로 배포할 수 있습니다. 이는 개발팀과 비즈니스팀 간의 가시성과 커뮤니케이션 부족 문제를 해결해 줍니다. 지속적인 제공은 최소한의 노력으로 새로운 코드를 배포하는 것을 목표로 합니다.
    
지속적인 *배포*(또 다른 의미의 "CD": Continuous Deployment)란 개발자의 변경 사항을 리포지토리에서 고객이 사용 가능한 프로덕션 환경까지 자동으로 릴리스하는 것을 의미합니다. 이는 애플리케이션 제공 속도를 저해하는 수동 프로세스로 인한 운영팀의 프로세스 과부하 문제를 해결합니다. 지속적인 배포는 파이프라인의 다음 단계를 자동화함으로써 지속적인 제공이 가진 장점을 활용합니다.

</div>
</details>

    
[참고 사이트](https://www.redhat.com/ko/topics/devops/what-is-ci-cd)
    

📌 GitHub에 레포에 CI 배지 달기

1. 먼저 레포를 생성해서 로컬에 클론합니다.( 라이센스만 생성)
2. repository settings - Branches - Branch protection rule 로 들어가서
   
    [ ] Require a pull request before merging 체크

    [ ] Require status checks to pass before merging 체크
   - .github/workflows -  lint.yml 파일이 만든 브랜치에서 merge할때
pull request에서 base : main <- compare : 아래 브랜치 일때 lint 테스트 통과 못하면 fail이 뜨고 merge를 막을 수 있음
3. [https://codeclimate.com/](https://codeclimate.com/) 에 들어가서 우측 상단 login - quality - Open source를 클릭합니다.
4. GitHub로 로그인 후 우측 상단에 `Add a repository`에서 생성한 레포를 `Add Repo`합니다.
5. 체크가 완료되면 메뉴에서 Repo Settings - Test coverage 에서 TEST REPORTER ID를 복사해서 메모장에 기록해 두세요
6. [ci - example 레포](https://github.com/anisepy/python-ci-example) 를 클론해서 내용을 받아옵니다.
7. 내용을 생성한 레포에 복붙합니다.
8. vscode에서 해당 내용을 수정합니다.
    - .github/workflows 의 coverage.yml
    
    ```yaml
    env:
      CC_TEST_REPORTER_ID: codeclimate에서 복사해온 [TEST REPORTER ID]를 복붙해주세요
    
    # 만약 TEST REPORTER ID를 보안사항으로 만들고 싶다면
    env:
      CC_TEST_REPORTER_ID: ${{ secrets.CC_TEST_REPORTER_ID }}
    # 이렇게 적어두고 github repo setting - Secrets - Actions - New repository secret 에서 
    # NAME  : CC_TEST_REPORTER_ID / Value : 복사한 TEST REPOTER ID 를 입력하면 된다.
    ```
    
    - .vscode의 launch.json
    
    ```json
    "version": "0.1.0"
    // version이 바뀔때마다 수정해 주세요
    
    "env": {
        "PYTHONPATH": "${workspaceRoot}/레포지토리_이름"
    }
    ```
    
    - coverage.xml
    
    ```xml
    <source>레포의 경로</source>
    
    <!-- example -->
    <source>/Users/krc/Documents/dev/stock_price_downfall_predict</source>
    ```
    
    - main.py
    
    ```python
    ## 원하는 코드들을 lint 에 맞게 적어야 합니다. 
    # 설정한 lint에 따라 코드 작성 기준이 정해집니다.
    # Lint 기준 지정 : 코드-기본설정-설정(cmd + ,) 으로 가서 black, flake8, mypy 등을 체크 
    # 저장시 린트 자동 적용 : cmd + , 에서 lint on save 체크, format on save 체크, settings.json 에서 "editor.codeActionsOnSave": { "source.fixAll.eslint": true } 추가
    # 또는 터미널에서 black [파일 이름] 치면 린트가 되어 맞춰 집니다.
    
    def hello():
        return "Hello World!"
    
    if __name__ == "__main__":  # pragma: no cover
    # # pragma: no cover가 있는 유닛은 린트 체크 제외 대상으로 지정됩니다.
        ret = hello()
        print(ret)
    ```
    
    - main_test.py
    
    ```python
    # main.py의 내용을 test 하는 파일입니다. 
    import unittest
    
    from main import hello # 모듈 명과 불러올 함수
    
    class MainTest(unittest.TestCase):
        def test_hello(self): # 함수 등을 테스트할 함수 이름 지정 
            self.assertEqual(hello(), "Hello World!") # 해당 함수 이름과, return 내용 지정
    
    if __name__ == "__main__":  # pragma: no cover
        unittest.main()
    ```
    
    - README.md
    
    codeclimate login - quality - Open source - 해당 레포 - Repo Settings - Badges 에서 **Maintainability Badge** 와 **Test Coverage Badge**의 Markdown을 복붙해서 배지 코드를 적어놓습니다.
    
    ```markdown
    #### CI Status 예시
    
    [![Maintainability](https://api.codeclimate.com/v1/badges/64f03b8b27952133bfd0/maintainability)](https://codeclimate.com/github/anisepy/stock_price_downfall_predict/maintainability)
    
    [![Test Coverage](https://api.codeclimate.com/v1/badges/64f03b8b27952133bfd0/test_coverage)](https://codeclimate.com/github/anisepy/stock_price_downfall_predict/test_coverage)
    
    ## How to run?
    
    $ python main.py
    
    
    ## How to run test code?
    
    $ python -m unittest discover -p "*_test.py"
    ```
    
    
9. test
    - F1 키 → test → python: 테스트구성 → unittest → .Root directory → *_test.py
10. 커밋
    
    ```bash
    git add .
    
    git commit -a -m “comment”
    
    git checkout -b [브랜치명] : 해당 브랜치를 생성 후 이동
    
    git push --set-upstream origin feature/unittest : 브랜치 없을때 나는 오류, 복붙해서 명령어 치면 브랜치 생성되면서 커밋
    ```
    
    커밋시 action에서 통과되지 않을 시 오류 확인 후 Re-run all jobs 실행 
    
11. Coverage Report, Lint Code Base 모두 통과 시 브랜치 pull request
  
  
  
여기까지입니다. 참고하셔서 모두 깔끔한 레포 code 관리하실 수 있으시길 기원합니다. 