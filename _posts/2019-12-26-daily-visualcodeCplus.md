---
title: "[MAC] VSCode에 C,C++ 사용하기"
categories:
  - Daily
read_time: false
tags:
  - Daily
comments:
  - true
toc: true
toc_sticky: true
---

## 들어가기전

기존에 알고리즘 문제를 풀 때 C++을 사용하였는데, 맥북으로 노트북을 바꾸면서 코딩을 하던 비쥬얼 스튜디오를 더이상 사용할 수 없게 되었다.

공식문서를 보면 맥에 비쥬얼 스튜디오가 있긴 하지만, C,C++ 언어를 지원하지 않는다고 되어있다.

그래서 VSCode를 통해 C,C++을 사용하는 방법을 알아보도록 하겠다.

내용 정리를 위한 포스팅이기 때문에 틀린 부분이 있을 수 있습니다. 댓글로 말씀해주시면 감사드리겠습니다.

## 사전 작업

먼저 맥에 아래 두개가 설치 되어있어야 합니다.

* [g++](https://zetawiki.com/wiki/GCC,_gcc,_g%2B%2B)
* [lldb](https://ko.wikipedia.org/wiki/LLDB)

아래 입력을 통해 확인해볼 수 있다.

```bash
$ g++ -v
```

lldb는 그냥 터미널에 쳐보면 알 수 있다.

## Extensions 설치

VSCode 마켓으로 이동하여 C++을 입력하면, 아래 그림과 같은 애가 나옵니다.

![](/assets/img/daily/extensions.png)

설치하고 VSCode를 재실행하면 됩니다.

## 파일 생성 및 프로그래밍

먼저 비쥬얼코드에서 새로운 폴더를 하나 만들어 줍니다.

그리고, hello.cpp 파일을 하나 생성합니다.

![](/assets/img/daily/filename.png)

이후, command + shift + b 명령어를 주면, tasks.json으로 접근할 수 있게 됩니다.

```json
{
// tasks.json 형식에 대한 설명서는 
    // https://go.microsoft.com/fwlink/?LinkId=733558의 내용을 참조하세요.
    "version": "2.0.0",
    "tasks": [
        {
            "type": "shell",
            "label": "cpp build active file",
            "command": "/usr/bin/cpp",
            "args": [
                "-g",
                "${file}",
                "-o",
                "${fileDirname}/${fileBasenameNoExtension}.out"
            ],
            "options": {
                "cwd": "/usr/bin"
            },
            "problemMatcher": [
                "$gcc"
            ],
            "group": "build"
        },
        {
            "type": "shell",
            "label": "g++ build active file",
            "command": "g++",
            "args": [
                "-g",
                "${file}",
                "-std=c++11",
                "-o",
                "${fileDirname}/${fileBasenameNoExtension}.out"
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            },
            "problemMatcher": [
                "$gcc"
            ]
        },
        {
            "label": "exec",
            "type": "shell",
            "command": "${fileDirname}/${fileBasenameNoExtension}.out",
            "group": {
                "kind": "build",
                "isDefault": true
            },
            "problemMatcher": [
                "$gcc"
            ]
        }
    ]
}
```

내용을 위와 같이 바꿔줍니다.

그 후, 다시 ctrl + shift + b 를 누르면, 

![](/assets/img/daily/exec.png)

이와 같이 나옵니다.

먼저 g++ 로 빌드를 하고, 그 후, exec 을 누르면 실행이 되는 것을 알 수 있습니다.

![](/assets/img/daily/run.png)

## 참고

[MinJun Kweon의 블로그](https://minz.dev/mac-visual-studio-code-c-c++-build/)

[VSCode C/C++ 코딩 초기세팅(Mac)](https://ldgeao99.tistory.com/203)


