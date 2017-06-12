---
layout: page
title: Tutorial
tags: [tutorial, Jekyll, OSS, SKKU, cafe2]
date: 2017-05-22
comments: false
---


# 개요

Caffe2를 저희 팀의 프로젝트로 삼게 되면서, 라이브러리를 직접 사용해 보기 위해 기본적으로 제공하는 Tutorial을 보고 설치 및 실행을 시도해보았습니다.하지만 간단한 Tutorial조차 너무나 많은 파일의 설치를 요구하고 있었고, 추가적으로 설정해 주어야 하는 사항 또한 넘쳐났습니다.

![Imgur](http://i.imgur.com/Cf7sPxZ.png)

단순한 Caffe2 기본 설정 및 실행에 며칠의 시간을 쏟아부었는데도 끝내 실행을 시키지 못하자, 저는 이러한 부분이 조금 더 개선이 되었으면 하는 생각이 들었습니다. 많은 사람들이 프로그램을 처음 입문할 때 콘솔창에 Hello World를 쓰듯, Caffe2에도 오직 Hello World만을 표시할 수 있는 Compact한 Tutorial이 필요했습니다.

![Imgur](http://i.imgur.com/7FT9gItl.png)

특히 Caffe2는 Python과 C++ 모두를 지원하는데도 불구하고, 예제 자체가 Python에 치중되어 있는 경향이 있었습니다. 따라서 저는 이 부족한 C++ 부분에 집중을 해서, 제가 필요했던 'C++를 이용한 가장 단순한 형태의 Caffe2 튜토리얼' 을 만들기로 하였습니다.


# 1. C++ 설정 시작

Caffe2에서 기본적으로 제공하는 Installation Guide에서 요구하는 Dependency는 다음과 같았습니다. [보러가기](https://caffe2.ai/docs/getting-started.html?platform=windows&configuration=compile)

 - Video Drivers
 - NVIDIA CUDA/cuDNN
 - Python 2.7.6 to Python 2.7.13
 - C++ compiler such as VS 2017
 - CMake
 - protobuf

그러나 위의 것을 다 설치하고 나서도 기존의 C++ 예제가 무겁고 잘 실행되지도 않아 프로젝트를 진행하는 데 어려움을 겪었습니다. 제가 원하는 것은 오직 C++만을 위한 가장 간단한 실행 예제였기 때문에, 무작정 VS와 Caffe2만 다운받은 뒤에 실행해보기로 하였습니다.

이미 존재하는 일부 C++ 예제를 뜯어보았을 때, 프로그램에서 **<blob.h>**라는 헤더 파일을 사용하는 것을 알 수 있었습니다. 따라서 일단 다음과 같은 소스 코드를 정상적으로 작동시키기로 마음먹었습니다.

>#include <blob.h>
>
>int main()
>{   
>    printf("Hello Caffe2!");
>    return 0;
>}

다운로드한 Caffe2 파일을 찾아보자, core라는 하위 폴더 에서 c와 관련된 모든 헤더/소스파일이 나오는 것을 확인할 수 있었습니다. 찾고 있던 **blob.h** 또한 관련된 폴더 안에서 찾을 수 있었습니다. 따라서 VS의 Project Property 부분을 만져, including directory로 Caffe2 (root directory) 와 core를 포함시키자 기존의 에러가 사라지고 새로운 에러가 뜬 것을 확인할 수 있었습니다. **blob.h**를 살펴보니 Caffe2의 폴더에서 헤더 파일 **caffe2.pb.h**을 불러오고 있었는데, 해당 헤더 파일이 존재하지 않는다는 에러였습니다. 구글에 검색해 보니 같은 문제로 많은 사람들이 어려움을 겪고 있는 것을 확인할 수 있었습니다.


# 2. Protocol Buffers 세팅

이 부분을 추가로 알아보니, 위에서 Caffe2의 Dependency 중 하나로 표시되었던 protobuf가 Google이 만든 오픈소스 프로그램인 Protocol Buffer라는 것을 알게 되었습니다. 주어진 하나의 Data Format을 사용자 입맛에 맞게 JSON, C++ Class 등 다양한 형태로 변환할 수 있게 해주는 Language-neutral한 프로그램입니다. 다시 말해서, 우리가 이를 C++에 걸맞게 사용하기 위해서는 protobuf를 이용해 변환을 해 주어야 한다는 뜻입니다. 따라서 protobuf를 설치한 뒤, 위에서 필요했던 **caffe2.pb.h** 파일을 생성하기 위해 원본 데이터인 **caffe2.proto**를 변환, 필요로 했던 **caffe2.pb.h**와 **caffe2.pb.cc** 파일을 생성해냈습니다. 그러자 마침내 에러 갯수가 **단 하나**로 줄어들었습니다.

이렇게 뜬 에러는 소스코드 자체에서도 protobuf를 사용하고 있기 때문에 발생하는 에러였습니다. including directory로 protobuf의 src 폴더를 포함시켜주자 에러가 사라지고, 콘솔 화면에서 Hello Caffe2가 뜬 것을 확인할 수 있었습니다.

결과적으로 프로그램을 실행하는 데 있어 필요한 Dependency와 Setting을 다음과 같이 간소화하는 데 성공하였습니다.

 - Caffe2 설치
 - C++ Compiler 설치
 - Protobuff 설치
 - Including Directory에 Caffe2, Protobuff 추가
 - Caffe2에 있는 proto 파일을 Protobuff로 변환


# 3. Tutorial 파일 작성 후 pull-request

이렇게 진행한 결과를 남들과 공유하고자, md 파일을 이용해 간단한 튜토리얼을 작성하였습니다. [보러가기](https://github.com/17-1-SKKU-OSS/011A/blob/master/cplusplus_tutorial.md)

이를 원래 프로젝트에도 반영하고자 Tutorial과 관련된 문서들이 어디 있는지 찾아보았더니, gh-pages라는 하위 branch를 두어 이 곳에서 모든 문서 처리를 하고 있는 것을 알았습니다. 이러한 branch 형식이 생소했기 때문에, 처음에는 어디에 올려서 pull-request를 해야 할 지 감이 잘 오지 않았습니다.

계속 이것저것 시도해 본 결과 Caffe2 프로젝트를 fork해 올 때 모든 branch를 가져온다는 것을 뒤늦게 알게 되었고, 저희 팀의 local project에서도 gh-pages branch가 존재한다는 것을 알 게 되었습니다. 이 곳에 만들었던 tutorial 파일을 다시 업로드하고, Caffe2에서 document에 요구하는 사항을 참고하여 약간의 수정을 더했습니다.

이후 Caffe2의 gh-pages branch에 직접 pull-request를 날려보았습니다. 공식적인 답변은 없는 상황이지만, 이렇게 처음부터 끝까지 OSS에 직접 기여하려는 노력을 시도해보니 참 의미가 있었습니다. 
