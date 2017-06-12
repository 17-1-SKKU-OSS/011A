---
layout: page
title: Spell_Check
tags: [spell, Jekyll, OSS, SKKU, cafe2]
date: 2017-05-22
comments: false
---

# Caffe2 Spell Checker

caffe2 커뮤니티에 기여하기 위해서 텍스트파일 내의 오타를 찾아 고치는것을 진행하였습니다. Repository 안에는 6천여 개의 파일이 존재하여 하나하나 읽어보고 고치는 것은 불가능했습니다. 그래서 폴더 내의 텍스트 파일을 찾아 오타를 체크하는 프로그램을 제작하게 되었습니다.

또한 이 프로그램은 모든 프로젝트에 적용이 가능합니다.

## Installation

이 프로그램을 사용하기 위해서는 2개의 파이썬 라이브러리를 설치해야합니다.

### binaryornot

``` 
$ easy_install binaryornot
```

Or

```
$ pip install binaryornot
```

### language-checker

```
$ pip install --upgrade language-check
```

파이썬 2.7 버전을 사용하는 경우에는 아래의 명령어도 입력해야 합니다.

```
$ pip install --upgrade 3to2
```

## How to use

### binaryornot

binaryornot은 지정한 파일이 텍스트파일인지 바이너리파일인지 구분하는 함수를 제공하는 라이브러리입니다.

```python
from binaryornot.check import is_binary

is_binary(filepath) # 1:binary file 0:text file
```

### language-checker

language-checker는 텍스트 파일을 읽어 오타가 있는지 체크하는 라이브러리 입니다.

```python
import language_check
tool = language_check.LanguageTool('en-US')

f = open(filepath,'r')
	    
while True:
  line = f.readline()
  if not line: break
  
  matches = tool.check(line)
  for idx in range(len(matches)) :
    print str(matches[idx])

f.close()
```

matches 에는 다음과 같은 형식의 데이터가 들어있습니다.

```
Line 1, column 1, Rule ID: UPPERCASE_SENTENCE_START
Message: This sentence does not start with an uppercase letter
Suggestion: Transfer
transfer the Software. For avoidance of doubt, n...
^^^^^^^^
```

RuleID는 이 오타가 어떤 종류의 오타인지 나타내는 값인데, 실제 위의 소스코드를 실행하면 텍스트 파일에서 필요이상의 오타를 발견하게 되므로 신경쓰지 않아도 되는 RuleID는 예외처리를 했습니다.

RuleID의 목록에 대한 정보는 [이곳](https://community.languagetool.org/rule/list?lang=en)에 있습니다.
[이곳](https://community.languagetool.org/ruleEditor2/index?lang=en)에서는 RuleID를 추가할 수도 있습니다.

## Result

이 프로그램을 caffe2 폴더에 적용시켜서 실행한 결과는 [다음](https://github.com/haracejacob/011A/tree/master/spell_checker/fix/caffe2)과 같습니다.

검출된 오타에서 제외한 RuleID는 다음과 같습니다.
```
['COMMA_PARENTHESIS_WHITESPACE','WHITESPACE_RULE', 'MORFOLOGIK_RULE_EN_US', 'EN_QUOTES' ,'EN_QUOTES[0]', 'EN_QUOTES[1]', 'EN_QUOTES[2]', 'EN_UNPAIRED_BRACKETS']
```

오타 목록을 전부 체크해서 수정하여 Pull-Request를 요쳥하였다.

[Fix a few typos and grammars in comment #719](https://github.com/caffe2/caffe2/pull/719)

> Thanks. These all look good.

라는 답변을 받았고 조만간 Merge가 될 것이라 생각합니다.

