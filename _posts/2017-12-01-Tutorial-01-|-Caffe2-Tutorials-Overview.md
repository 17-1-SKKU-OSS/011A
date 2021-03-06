Translation of Caffe2 documents in Korean, especially tutorials. The first tutorial is about 'Caffe2 Tutorials Overview'. Here is original Documentation about [Caffe2 Tutorials Overview](https://caffe2.ai/docs/tutorials).

# Caffe2 튜토리얼 개요

Caffe2에 대한 관심에 진심으로 감사드립니다. Caffe2가 머신러닝 제품(product) 사용을 위한 고성능 프레임 워크가 되기를 바랍니다. Caffe2는 딥러닝 분야에서, 아이디어를 빠르게 모듈화하고, 프로토타이핑하기 위해 만들어졌습니다. 이 모듈성을 바탕으로 한번 모델을 정의하고 나면, 추가 성능 및 확장성에 관심이 있을 때 최종 제품에서 Python을 사용하지 않고 순수 C++을 사용하여 이러한 모델을 배포 할 수 있습니다. 또한 커뮤니티가 성능이 향상된 고성능 모듈을 개발할 때, 이 모듈을 Caffe2 프로젝트로 쉽게 전환 할 수 있습니다.


## 방향을 정하세요!
1. [미리 준비된 인공신경망 사용하기!](https://caffe2.ai/docs/tutorials.html#null__new-to-deep-learning) (초급)
2. [나만의 신경망 만들기!](https://caffe2.ai/docs/tutorials.html#null__intermediate-tutorials) (중급)
3. [모바일 중심, 딥러닝을 사용하는 앱만들기!](https://caffe2.ai/docs/AI-Camera-demo-android) (고급)

만약 1번을 선택한 경우, 사전에 학습된 모델을 사용하여 데모 프로젝트를 실행하는 방법을 빠르게 보여줍니다.

만약 2번을 선택한 경우, 먼저 인공신경망에 대한 배경지식이 필요합니다. 이미 알고 계신다면 링크를 건너뛰셔도 됩니다. 만약 입문서가 필요하다면 아래에 자료목록이 있습니다. 

만약 3번을 선택한 경우, 링크를 클릭하여 Android 또는 iOS 앱에서 이미지를 분류하는 방법을 알아보시는 것을 권해드립니다. Android Studio나 Xcode는 거의 플러그 앤 플레이(Plug-n-play) 방식이지만, 추후 Caffe2의 C++ 후크와 직접 통합 해야합니다.

어떤 선택을 하든, 각 세션에 대해 튜토리얼을 확인하는 것을 잊지 말아주세요. 당신이 무얼 배울 지 혹시 모르잖아요!

- [초보자 튜토리얼](https://caffe2.ai/docs/tutorials.html#null__beginner-tutorials)
- [Caffe2의 달라진 점](https://caffe2.ai/docs/tutorials.html#null__new-to-caffe2)
- [중급자 튜토리얼](https://caffe2.ai/docs/tutorials.html#null__intermediate-tutorials)
- [상급자 튜토리얼](https://caffe2.ai/docs/tutorials.html#null__advanced-tutorials)



### 딥러닝이 처음인가요?
Michael Nielsen의 [인공신경망과 딥 러닝 (Neural Networks and Deep Learning)](http://neuralnetworksanddeeplearning.com/index.html) 온라인 초안에서 딥러닝에 대한 폭넓은 소개를 제공합니다. 특히 딥러닝을 처음 접한다면 ‘인공신경망 사용’ 챕터와 ‘역전파(backpropagation)의 작동 법’ 챕터가 도움이 될 것입니다. 

회로 및 코드에서의 인공신경망에 대해 알고 싶다면 Andrej Karpathy(Stanford)의 [해커의 인공신경망 가이드]를 확인하는 것을 추천합니다.

### 머신러닝의 전문연구원

CVPR ‘14의 [Vision을 위한 딥러닝 튜토리얼](https://sites.google.com/site/deeplearningcvpr2014/)은 자습서로써 좋은 동반자가 될 것입니다. Caffe 튜토리얼을 이용해 기본적인 프레임워크 사용법을 익히고 나서 CVPR ‘14의 튜토리얼에서 기본적인 아이디어부터 고급연구 방향을 탐색하는 것을 권장합니다.

자습서의 최근 튜토리얼들은 머신러닝과 비전 연구자들을 위한 딥러닝까지 포함합니다.
- [Deep Learning Tutorial](http://www.cs.nyu.edu/~yann/talks/lecun-ranzato-icml2013.pdf): Yann LeCun (NYU, Facebook), Marc’Aurelio Ranzato (Facebook). ICML 2013 tutorial.
- [LISA Deep Learning Tutorial](http://deeplearning.net/tutorial/deeplearning.pdf): LISA Lab, Yoshua Bengio (U. Montréal).


## 튜토리얼과 예제 코드
아래에 제공된 IPython notebook 튜토리얼과 예제 스크립트는 Caffe2 Python 인터페이스에 대해 소개합니다. 일부 자습서는 Caffe 커뮤니티에서 제공했으며 다른 사람들이 Caffe2를 더 다양하게 사용해 볼 수 있도록하는 모든 기여를 환영합니다. IPython 튜토리얼은 각 튜토리얼 제목 아래의 링크를 사용하여 보거나 다운로드 할 수 있습니다. 코드를 직접 보고 스스로 사용 해 보길 원한다면 Github에서 이 ipynb 파일을 직접 볼 수 있습니다. 그러나 Jupyter Notebook에서 실행하고 대화형 기능을 활용하는 것을 권장합니다. [아래의 설치 방법](http://caffe2.ai/docs/tutorials.html#null__tutorials-installation)은 이것을 수행하는 방법을 보여줍니다. 아래 튜토리얼로 바로 들어가고 싶다면 이 부분을 건너뛰십시오.


### 예제 스크립트
[/caffe2/python/examples](https://github.com/caffe2/caffe2/tree/master/caffe2/python/examples)에 있는 예제 스크립트는 Caffe2를 이용해 프로젝트를 시작하기에 아주 훌륭한 자료입니다.

[char_rnn.py](https://github.com/caffe2/caffe2/blob/master/caffe2/python/examples/char_rnn.py): 입력한 텍스트를 샘플링 해서 임의로 유사한 스타일의 텍스트를 생성하는 순환 컨볼루션 인공신경망을 생성합니다. [RNN 및 LSTM 페이지](https://caffe2.ai/docs/RNNs-and-LSTM-networks.html)에 이 스크립트의 사용법에 대한 추가적인 정보가 있습니다.

[lmdb_create_example.py](https://github.com/caffe2/caffe2/blob/master/caffe2/python/examples/lmdb_create_example.py): 임의의 이미지 데이터와 레이블을 포함하는 lmdb 데이터베이스를 생성합니다. 이 데이터베이스는 사용자의 데이터를 import하기 위해 뼈대로 사용될 수 있습니다.

[resnet50_trainer.py](https://github.com/caffe2/caffe2/blob/master/caffe2/python/examples/resnet50_trainer.py): Resnet 50을 위해 병렬 처리 된 multi-GPU 분산 형 트레이너입니다. 예를 들어 imagenet 데이터를 학습시키는 데 사용할 수 있습니다. [동기식 SGD 페이지](https://caffe2.ai/docs/SynchronousSGD.html)에 이 스크립트 사용에 대한 추가 정보가 있습니다.



### 초보자 튜토리얼

[모델과 데이터 셋(Data set) - 초보자](https://caffe2.ai/docs/tutorial-models-and-datasets.html)

Caffe와 딥러닝을 처음 접하고 계신가요? 여기서 시작해 다양한 모델과 데이터 셋에 대해 자세히 알아보세요!

[사전 학습된 모델 로드하기(load)](https://github.com/caffe2/caffe2/blob/master/caffe2/python/tutorials/Loading_Pretrained_Models.ipynb)

Model Zoo의 이점(advantage)을 경험하고, 준비된 모델로 테스트를 해 보세요. 이 튜토리얼에는 약간의 모델들이 바로 적용이 가능하도록 준비되어있으며, 이들이 신경망을 작동시키는 기본적인 단계를 보여줄 것입니다. 그런 다음 이미지를 비롯한 다른 테스트를 입력해 모델이 어떤 방식으로 작동하는 지 볼 수 있습니다.

[이미지 전처리 파이프라인(pipeline)](https://github.com/caffe2/caffe2/blob/master/caffe2/python/tutorials/Image_Pre-Processing_Pipeline.ipynb)

사전 학습된 모델에 입력하거나 테스트 하기 위해 이미지를 어떻게 얻는지 배우세요. 휴대 전화에서 웹캠, 새로운 의료 이미지에 이르기까지 이미지 수집 파이프라인을 고려하는 것 뿐 아니라, 어떤 종류의 이미지 분류에서도 속도와 정확성 모두를 위해 필요한 변환도 고려해야합니다.

- 이미지 크기 조정
- 이미지 범위 조정(rescaling)
- HWC에서 CHW
- RGB에서 BRG로
- Caffe2 적용(ingestion)을 위한 이미지 준비


### Caffe2의 시작

[Caffe에서 Caffe2로의 변환](https://caffe2.ai/docs/caffe-migration.html)

Caffe2에 대해 소개합니다. 기존의 Caffe모델을 Caffe2모델로 변환하는 방법을 알아보세요.

[시작 튜토리얼](https://caffe2.ai/docs/intro-tutorial)

이 후속 튜토리얼에서는 blob, Caffe2 작업 공간 및 텐서(tensors)부터 시작됩니다. net과 연산자, 그리고 간단한 모델을 만들고 실행하는 방법을 다룹니다.

[Caffe2 기초 - Workspaces, Operators, and Nets](https://github.com/caffe2/caffe2/blob/master/caffe2/python/tutorials/Basics.ipynb)

이 IPython 튜토리얼은 Caffe2의 기본적 구성 요소를 소개합니다. 

- 작업 공간(workspace)
- 연산자(Operator)
- Net

[모델 brewing](https://caffe2.ai/docs/brew)

쉽게 모델을 생성하도록 하는 API인 `brew`를 소개하는 튜토리얼입니다. 다음에 대해 배우게 됩니다.

- 연산자 vs helper 함수
- brew 와 arg_scope
- 자신만의 helper함수 만들기 

[Toy Regression - Plotting Lines & Random Data](https://github.com/caffe2/caffe2/blob/master/caffe2/python/tutorials/Toy_Regression.ipynb)

이 튜토리얼은 간단한 선형 회귀(linear regression)로 더 많은 Caffe2 기능을 테마처럼 사용하는 방법을 보여줍니다.

- 모델에 대한 입력을 바탕으로 임의의 샘플 데이터를 생성
- 이 데이터로 네트워크 만들기
- 모델을 자동으로 학습시키기
- 확률적 경사 하강 결과와 인공신경망에서 학습된 변수의 변화 검토



### 중급자 튜토리얼

[MNIST - 필기 인식](https://github.com/caffe2/caffe2/blob/master/caffe2/python/tutorials/MNIST.ipynb)

이 튜토리얼에서는 필기를 식별할 수 있는 작은 컨볼루션 인공신경망(CNN)을 만듭니다. CNN을 학습하고 테스트하는 과정에서 MNIST 데이터 세트의 필기 이미지를 사용합니다. 이것은 CNN 교육에 사용되는 500명의 서로 다른 사람들의 필기체 이미지 60,000 장입니다. 또 다른 10,000개의 테스트 이미지는 교육용 이미지와는 다르며, CNN의 정확성을 테스트하는 데에 사용됩니다.

[본인만의 데이터 셋(Dataset) 만들기](https://github.com/caffe2/caffe2/blob/master/caffe2/python/tutorials/create_your_own_dataset.ipynb)

데이터를 가져오고(importing) 조작하여 Caffe2에서 사용할 수 있도록 만들어보세요. 이 튜토리얼에서는 Iris 데이터셋을 사용합니다.



### 상급자 튜토리얼

[Caffe2로 다중 GPU 학습](https://github.com/caffe2/caffe2/blob/master/caffe2/python/tutorials/Multi-GPU_Training.ipynb)

이 튜토리얼에서는 다중 GPU 학습을 다룹니다. `data_parallel_model`을 사용하여 ResNet-50 모델과 동일하게 ImageNet 데이터베이스의 부분 집합을 빠르게 처리할 수 있는 기본 구조를 제시합니다. 이미지 파이프라인을 효율적으로 처리하고, ResNet 모델을 구축하고, 단일 GPU 상에서 작동하는 Caffe2의 C++연산자들을 살펴 볼 수 있습니다. 또한 `data_parallel_model`에 포함 된 몇 가지 최적화를 제시하고 확장하여 다중 GPU환경에서 실행할 수 있도록 모델을 병렬화하는 방법을 다룹니다.


### 자신의 튜토리얼을 공유해주세요!

훌륭한 튜토리얼 아이디어를 갖고 계신가요? 그렇다면 [이슈](https://github.com/caffe2/caffe2/issues)를 만들어 튜토리얼을 작성하거나 그에 대한 링크를 걸어주세요!

### 연산자

Caffe2에서 계산의 기본 단위 중 하나는 [연산자(operators)](https://caffe2.ai/docs/operators)입니다.

### 자신만의 연산자 만들기

좋은 아이디어입니다! 커스텀 연산자를 만들어 커뮤니티에서 공유하세요! 작성 가이드를 참조해 주세요: [자신만의 연산자 만들기 가이드(Guide for creating your own operators)](https://caffe2.ai/docs/custom-operators)



## 튜토리얼 설치

튜토리얼을 실행하기 위해서 Python2.7, [ipython-notebook](http://jupyter.org/install.html)과 [matplotlib](http://matplotlib.org/users/installing.html)가 필요합니다. 
다음 과정으로 설치가 가능합니다:

### Brew 와 pip을 사용한 MacOSx 

    brew install matplotlib
    pip install ipython notebook
    pip install scikit-image


### Anaconda
Anaconda는 iPython notebook과 함께 설치되므로 matplotlib만 설치하시면 됩니다.

    conda install matplotlib
    conda install scikit-image



### pip

    pip install matplotlib
    pip install ipython notebook
    pip install scikit-image


너무 복잡하다구요? 여기에 설치를 위한 전체적인 리스트가 있습니다. Python, C++, 혹은 시스템 레벨에서 설치하기를 원할 경우, 경우에 따라 `brew install`, `pip install`, 또는 `conda install`을 이용할 수 있습니다. 

flask graphviz hypothesis jupyter leveldb lmdb matplotlib pydot pyyaml requests scikit-image scipy tornado zeromq


대화형 코드 노트북(ipynb files)을 만들고 사용할 수 있는 Jupyter Notebook의 최신 환경설정을 위한 가이드는 [http://jupyter.org](http://jupyter.org/install.html)에서 찾아볼 수 있습니다.

참고: Anaconda Python으로 Caffe2를 이미 성공적으로 설치했다면 멋진 소식입니다! 이미 Jupyter Notebook이 있는 것입니다. 시작은 쉽습니다.


`caffe2/caffe2/python/tutorials` 폴더에 포함 된 셸 스크립트를 실행합니다.


    ./start_ipython_notebook.sh



또는 `jupyter notebook`을 실행 해 브라우저에 로컬 jupyter서버(기본값은 http://localhost:8888/)가 열리면, Caffe2 저장소로 이동하여 `caffe2/caffe2/python/tutorials` 디렉토리에서 찾을 수 있습니다. 이렇게 하면 쉘 스크립트처럼 대화형 기능이 시작됩니다. 이 스크립트에는 PYTHONPATH 환경 변수를 설정하는 추가 기능이 있습니다.
