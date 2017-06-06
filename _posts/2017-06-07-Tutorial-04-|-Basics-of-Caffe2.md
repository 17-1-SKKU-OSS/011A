Translation of Caffe2 documents in Korean, especially tutorials. The fourth tutorial is about 'Basics of Caffe2 - Workspaces, Operators, and Nets'. Here is original Documentation about [Basics of Caffe2 - Workspaces, Operators, and Nets](https://caffe2.ai/docs/tutorial-basics-of-caffe2.html).



# Caffe2 기초 - Workspaces, Operators, and Nets

이 튜토리얼에서는 Caffe2의 기본적인 구성요소에 대해 소개하도록 하겠습니다.

- Workspaces(작업 공간)
- Operators(연산자)
- Nets

이 챕터를 시작하기에 앞서 [Intro Tutorial](https://caffe2.ai/docs/intro-tutorial)을 복습하셔도 좋습니다.

[Browse the Tutorial](https://github.com/caffe2/caffe2/blob/master/caffe2/python/tutorials/Basics.ipynb)

이 튜토리얼에서는 Caffe2의 기본이라고 할 수 있는 연산자와 net을 작성하는 방법을 비롯한 기본 개념을 살펴보겠습니다.

먼저, caffe2를 import합니다. `core`와 `workspace`는 가장 많이 쓰게 될 2가지가 될 것입니다. caffe2에 의해 만들어진 프로토콜 버퍼를 조종하고 싶다면, `caffe2.proto`에서 `caffe2_pb2`도 함께 import합니다. 


    # 기본적인 파이썬 라이브러리도 몇 개 import해줍니다. 
    from matplotlib import pyplot
    import numpy as np
    import time
    
    # 아래의 droid들이 지금 찾는 것들입니다. 
    from caffe2.python import core, workspace
    from caffe2.proto import caffe2_pb2
    # Let's show all plots inline.
    %matplotlib inline


caffe2가 GPU를 지원하지 않는다는 경고 메시지를 받을 수도 있습니다. CPU 전용 빌드를 실행시키고 있다는 의미입니다. CPU만으로도 아무 문제없이 실행할 수 있고, 문제를 일으키지 않으니 걱정하지 마세요!


## Workspaces

먼저 모든 데이터가 모이는 workspace부터 살펴봅시다.

만약 Matlab에 익숙하다면, workspace는 만들고 저장하는 blob들로 구성되어있습니다. 지금은 blob을 numpy의 ndarray와 유사하지만 연속적인 n차원의 Tensor로 간주 할 것입니다. 아래서 blob은 모든 유형의 C++ 객체를 저장 할 수 있는 유형화된 포인터지만, Tensor는 blob에 저장된 가장 일반적인 유형이라는 것을 보여줄 것입니다. 먼저 인터페이스부터 보여드리겠습니다. 

`Blobs()`는 workspace에 존재하는 모든 blob들을 출력합니다. `HasBlob()` 쿼리는 workspace에 blob이 존재하는지 여부를 알려줍니다. 지금은 아무것도 갖고있지 않습니다. 


    print("Current blobs in the workspace: {}".format(workspace.Blobs()))
    print("Workspace has blob 'X'? {}".format(workspace.HasBlob("X")))



`FeedBlob()`을 사용해 blob을 workspace로 보낼 수 있습니다. 


    X = np.random.randn(2, 3).astype(np.float32)
    print("Generated X from numpy:\n{}".format(X))
    workspace.FeedBlob("X", X)


    Generated X from numpy:
    [[-0.56927377 -1.28052795 -0.95808828]
     [-0.44225693 -0.0620895  -0.50509363]]


이제 workspace에 어떤 blob이 있는지 확인 해 봅시다. 


    print("Current blobs in the workspace: {}".format(workspace.Blobs()))
    print("Workspace has blob 'X'? {}".format(workspace.HasBlob("X")))
    print("Fetched X:\n{}".format(workspace.FetchBlob("X")))



    Current blobs in the workspace: [u'X']
    Workspace has blob 'X'? True
    Fetched X:
    [[-0.56927377 -1.28052795 -0.95808828]
     [-0.44225693 -0.0620895  -0.50509363]]


저 배열들이 동일한 것인지 확인 해 봅시다.


    np.testing.assert_array_equal(X, workspace.FetchBlob("X"))



만약, 존재하지 않는 blob에 접근을 시도한다면 에러가 나타날 것입니다. 


    try:
        workspace.FetchBlob("invincible_pink_unicorn")
    except RuntimeError as err:
        print(err)



    [enforce fail at pybind_state.cc:441] gWorkspace->HasBlob(name).


즉시 사용하지 못할 수도 있는 것이 있습니다: 파이썬에서, 각각 다른 이름을 갖고 있는 여러 개의 workspace를 갖고 작업을 전환할 수 있습니다. 다른 workspace에 존재하는 blob들은 서로에게 독립적입니다. `CurrentWorkspace` 명령어를 이용하여 현재 workspace를 조회할 수 있습니다. 이름(gutentag)을 이용해 작업 공간을 전환하고, 만약 존재하지 않는다면 생성해봅시다.


    print("Current workspace: {}".format(workspace.CurrentWorkspace()))
    print("Current blobs in the workspace: {}".format(workspace.Blobs()))

    # workspace를 전환합니다. 두 번째 인자 "True"는 workspace가 없을 때 생성하는 것을 의미합니다.
    workspace.SwitchWorkspace("gutentag", True)

    # 현재 workspace를 출력 해봅시다. 아직 workspace에 아무것도 없는 것을 잊지 마세요.
    print("Current workspace: {}".format(workspace.CurrentWorkspace()))
    print("Current blobs in the workspace: {}".format(workspace.Blobs()))



    Current workspace: default
    Current blobs in the workspace: ['X']
    Current workspace: gutentag
    Current blobs in the workspace: []


기본 workspace로 다시 돌아가 봅시다.


    workspace.SwitchWorkspace("default")
    print("Current workspace: {}".format(workspace.CurrentWorkspace()))
    print("Current blobs in the workspace: {}".format(workspace.Blobs()))



    Current workspace: default
    Current blobs in the workspace: ['X']


마지막으로, `ResetWorkspace()`는 현재 workspace의 모든 내용을 삭제합니다. 


    workspace.ResetWorkspace()




## Operators
Caffe2의 연산자는 일종의 함수라고 할 수 있습니다. C++쪽에서는 모두 공통 인터페이스에서 파생되어 유형별로 등록되므로 runtime 중에 다른 연산자를 호출 할 수 있습니다. 연산자의 인터페이스는 `caffe2/proto/caffe2.proto`에 정의되어있습니다. 기본적으로, 그것은 여러 개의 입력을 받아 여러 개의 출력을 만들어냅니다.

Caffe2 Python에서 "연산자 만들기"라고 말하면 아무것도 실행되지 않습니다. 연산자의 특징을 지정하는 프로토콜 버퍼를 생성 할 뿐이죠. 나중에 실행을 위해 C ++ 백엔드로 전송됩니다. protobuf에 익숙하지 않다면 구조화 된 데이터를 위한 json같은 직렬화 도구라고 생각하면 좋습니다. 프로토콜 버퍼에 대한 자세한 내용은 [여기](https://developers.google.com/protocol-buffers/)를 참조하십시오.

실제 예제를 살펴봅시다.


    # 연산자 생성
    op = core.CreateOperator(
        "Relu", # 실행시키고자하는 연산자의 타입
        ["X"], # 이름을 이용해 input blob을 listing
        ["Y"], # 이름을 이용해 output blob을 listing
    )
    # 끝!


앞서 언급했듯, 생성된 연산자라는 것은 실질적으로 protobuf 객체입니다. 내용을 살펴봅시다.


    print("Type of the created op is: {}".format(type(op)))
    print("Content:\n")
    print(str(op))



    Type of the created op is: <class 'caffe2.proto.caffe2_pb2.OperatorDef'>
    Content:
    
    input: "X"
    output: "Y"
    name: ""
    type: "Relu"


됐습니다, 연산자를 실행시켜봅시다. 먼저, input X를 workspace로 가져옵니다. 그리고 나서, 연산자를 실행시키는 가장 쉬운 방법은 `workspace.RunOperatorOnce(operator)` 명령어 입니다. 


    workspace.FeedBlob("X", np.random.randn(2, 3).astype(np.float32))
    workspace.RunOperatorOnce(op)

실행 이후에, 연산자가 제대로 일을 수행하는지 살펴봅시다. 여기서는 연산자가 신경망([Relu])을 활성화하는 역할을 하는 함수입니다. 


    print("Current blobs in the workspace: {}\n".format(workspace.Blobs()))
    print("X:\n{}\n".format(workspace.FetchBlob("X")))
    print("Y:\n{}\n".format(workspace.FetchBlob("Y")))
    print("Expected:\n{}\n".format(np.maximum(workspace.FetchBlob("X"), 0)))



    Current blobs in the workspace: ['X', 'Y']
    
    X:
    [[ 1.03125858  1.0038228   0.0066975 ]
     [ 1.33142471  1.80271244 -0.54222912]]
    
    Y:
    [[ 1.03125858  1.0038228   0.0066975 ]
     [ 1.33142471  1.80271244  0.        ]]
    
    Expected:
    [[ 1.03125858  1.0038228   0.0066975 ]
     [ 1.33142471  1.80271244  0.        ]]


이 예에서 Y와 출력이 일치하는 것을 기대 한 것이라면, 이 연산자는 제대로 작동하고 있는 것입니다. 

연산자는 필요하다면 선택적 인수(arguments) 또한 사용합니다. 연산자와 인수는 키-값 쌍으로 지정됩니다. 텐서를 만들어 Gaussian 확률 변수로 채우는 간단한 예제를 살펴보겠습니다.


    op = core.CreateOperator(
        "GaussianFill",
        [], # GaussianFill은 다른 인자(파라미터)를 필요로하지 않습니다.
        ["Z"],
        shape=[100, 100], # int들을 리스팅해서 인수를 설정합니다.
        mean=1.0,  # float형식으로 소수점 아래 한자리를 갖는 mean
        std=1.0, # float형식으로 소수점 아래 한자리를 갖는 std
    )
    print("Content of op:\n")
    print(str(op))



    Content of op:
    
    output: "Z"
    name: ""
    type: "GaussianFill"
    arg {
      name: "std"
      f: 1.0
    }
    arg {
      name: "shape"
      ints: 100
      ints: 100
    }
    arg {
      name: "mean"
      f: 1.0
    }


그것을 실행하고 의도한 대로 값이 나오는지 봅시다.


    workspace.RunOperatorOnce(op)
    temp = workspace.FetchBlob("Z")
    pyplot.hist(temp.flatten(), bins=50)
    pyplot.title("Distribution of Z")



    <matplotlib.text.Text at 0x7f2bd2d51710>


종 모양의 곡선이 보여지면 제대로 작동 된 것입니다!

![](https://caffe2.ai/static/images/tutorial-basic-distribution.png)

## Nets

net은 기본적으로 계산 그래프입니다. Net은 일련의 명령으로 작성된 프로그램처럼, 여러 개의 연산자로 구성됩니다. 한 번 살펴봅시다.

우리가 net에 대해 말할 때, BlobReference에 대해서도 함께 말하고 있는 셈입니다. BlobReference는 문자열 주위를 감싸서 연산자를 쉽게 연결시킬 수 있도록 하는 객체입니다. 

다음의 파이썬 math와 기본적으로 같은 역할을 하는 network를 만들어봅시다. 


    X = np.random.randn(2, 3)
    W = np.random.randn(5, 3)
    b = np.ones(5)
    Y = X * W^T + b



차근차근 과정을 보여드리겠습니다. Caffe2의 `core.Net`은 NetDef 프로토콜 버퍼의 주변을 감싸는 class입니다.

network를 생성하면, 그 기저의 프로토콜 버퍼는 네트워크 이름을 제외하고 기본적으로 비어있습니다. net을 만들고 proto 내용을 보여주게 만들어 봅시다.


    net = core.Net("my_first_net")
    print("Current network proto:\n\n{}".format(net.Proto()))



    Current network proto:
    
    name: "my_first_net"


X라고 불리는 blob을 만들고, GaussianFill을 이용해서 랜덤 데이터로 채웁니다. 


    X = net.GaussianFill([], ["X"], mean=0.0, std=1.0, shape=[2, 3], run_once=0)
    print("New network proto:\n\n{}".format(net.Proto()))


    New network proto:
    
    name: "my_first_net"
    op {
      output: "X"
      name: ""
      type: "GaussianFill"
      arg {
        name: "std"
        f: 1.0
      }
      arg {
        name: "run_once"
        i: 0
      }
      arg {
        name: "shape"
        ints: 2
        ints: 3
      }
      arg {
        name: "mean"
        f: 0.0
      }
    }


앞선 `core.CreateOperator` 호출과 약간의 차이점을 관찰 할 수 있을 것입니다. 기본적으로, net이 있다면, 연산자를 바로 생성하고 동시에 파이썬 트릭을 사용하여 net에 추가시킬 수 있습니다 : SomeOp가 연산자의 문자열 타입으로 등록된 상태에서 `net.SomeOp`을 호출하면 이것은 곧바로 다음으로 번역시킵니다.



    op = core.CreateOperator("SomeOp", ...)
    net.Proto().op.append(op)




또한, X가 무엇인지 궁금할 수 있습니다. X는 기본적으로 2가지를 기록하는 `BlobReference`입니다.

- 이름이 무엇인지. str(X)를 이용해서 이름에 접근 할 수 있습니다.
- 그것으로부터 어떤 net이 생성되는지. 내부 변수 `_from_net`에 의해서 기록되는데 별로 필요하지는 않을 것입니다.

유효성을 검사해봅시다. 또 한 가지 기억해야 할 것은 우리는 아직 아무것도 실행하지 않았다는 것입니다. 따라서 X는 symbol일 뿐 아무것도 갖고 있지 않습니다. 지금 당장 숫자 값을 얻는 것을 기대하지는 마세요 :)


    print("Type of X is: {}".format(type(X)))
    print("The blob name is: {}".format(str(X)))



    Type of X is: <class 'caffe2.python.core.BlobReference'>
    The blob name is: X


계속해서 W와 b를 생성합니다.



    W = net.GaussianFill([], ["W"], mean=0.0, std=1.0, shape=[5, 3], run_once=0)
    b = net.ConstantFill([], ["b"], shape=[5,], value=1.0, run_once=0)


이제, 간단한 코드가 하나 생겼습니다. BlobReference 객체는 생성된 net이 무엇인지 알고 있기 때문에, net에서 연산자를 만들 수 있을 뿐만 아니라 BlobReference으로부터 연산자를 만들 수도 있습니다. 이런 식으로 FC연산자를 만들어봅시다.


    Y = X.FC([W, b], ["Y"])


`X.FC(...)`는 `X`를 해당 연산자의 첫 번째 input으로 삽입 해 `net.FC`의 간단한 대리자 역할을 합니다. 따라서 위에서 한 작업은 다음의 것과 동일합니다. 

    Y = net.FC([X, W, b], ["Y"])

현재 network를 살펴봅시다.


    print("Current network proto:\n\n{}".format(net.Proto()))



    Current network proto:
    
    name: "my_first_net"
    op {
      output: "X"
      name: ""
      type: "GaussianFill"
      arg {
        name: "std"
        f: 1.0
      }
      arg {
        name: "run_once"
        i: 0
      }
      arg {
        name: "shape"
        ints: 2
        ints: 3
      }
      arg {
        name: "mean"
        f: 0.0
      }
    }
    op {
      output: "W"
      name: ""
      type: "GaussianFill"
      arg {
        name: "std"
        f: 1.0
      }
      arg {
        name: "run_once"
        i: 0
      }
      arg {
        name: "shape"
        ints: 5
        ints: 3
      }
      arg {
        name: "mean"
        f: 0.0
      }
    }
    op {
      output: "b"
      name: ""
      type: "ConstantFill"
      arg {
        name: "run_once"
        i: 0
      }
      arg {
        name: "shape"
        ints: 5
      }
      arg {
        name: "value"
        f: 1.0
      }
    }
    op {
      input: "X"
      input: "W"
      input: "b"
      output: "Y"
      name: ""
      type: "FC"
    }



너무 장황하죠? 그래프로 시각화 해봅시다. Caffe2는 아주 기본적인 기능을 제공하는 시각화 도구를 제공합니다. ipython으로 그것을 보여 드리겠습니다. 


    from caffe2.python import net_drawer
    from IPython import display
    graph = net_drawer.GetPydotGraph(net, rankdir="LR")
    display.Image(graph.create_png(), width=800)

![](https://caffe2.ai/static/images/tutorial-basics-net-graph.png)

우리는 `Net`을 정의했지만 아직 아무것도 실행되지는 않았습니다. 위의 net은 네트워크의 정의를 갖고 있는 protobuf라는 것을 기억하세요. 실제로 네트워크를 실행하기 원할 때, 다음의 일들이 일어납니다.

- protobuf에서 C++ net 객체를 인스턴스화합니다.
- 인스턴스화 된 net의 Run () 함수를 호출합니다.

우리가 무언가를 하기 전에 `ResetWorkspace()` 명령어를 이용해 모든 workspace의 앞선 변수들을 clear해야 합니다.

파이썬을 이용해서 net을 실행시키는 방법은 두 가지가 있습니다. 우리는 아래 예제 중에 첫 번째 방법을 수행 할 것입니다. 

1. 인스턴스화시켜 실행시킨 후에 바로 파괴하는 `workspace.RunNetOnce()`을 사용합니다.
2. 조금 더 복잡하고 2가지 단계가 필요합니다: 

   (a) workspace가 소유한 C++ net 객체를 만들기 위해 `workspace.CreateNet()`를 호출합니다. 

   (b) network 이름을 전달해서 `workspace.RunNet()`을 사용합니다.



    ```
    workspace.ResetWorkspace()
    print("Current blobs in the workspace: {}".format(workspace.Blobs()))
    workspace.RunNetOnce(net)
    print("Blobs in the workspace after execution: {}".format(workspace.Blobs()))
    # blobs의 내용을 dump합시다.
    for name in workspace.Blobs():
        print("{}:\n{}".format(name, workspace.FetchBlob(name)))
    ```

    ```
    Current blobs in the workspace: []
    Blobs in the workspace after execution: ['W', 'X', 'Y', 'b']
    W:
    [[-0.96537346  0.42591459  0.66788739]
     [-0.47695673  2.25724339 -0.10370601]
     [-0.20327474 -3.07469416  0.47715324]
     [-1.62159526  0.73711687 -1.42365313]
     [ 0.60718107 -0.50448036 -1.17132831]]
    X:
    [[-0.99601173 -0.61438894  0.10042733]
     [ 0.23359862  0.15135486  0.77555442]]
    Y:
    [[ 1.76692021  0.07781416  3.13944149  2.01927781  0.58755434]
     [ 1.35693741  1.14979863  0.85720366 -0.37135673  0.15705228]]
    b:
    [ 1.  1.  1.  1.  1.]
    ```

자, 이번에는 두 번째 방법을 이용해 net을 생성하고 실행시켜봅시다. `ResetWorkspace()`를 사용하여 변수를 지우고, 전에 생성 해 둔 workspace의 net 객체인 `CreateNet(net_object)`를 이용해 net을 만듭니다. 이후 `RunNet(net_name)`에 이름을 전달하여 net을 실행합니다.


    workspace.ResetWorkspace()
    print("Current blobs in the workspace: {}".format(workspace.Blobs()))
    workspace.CreateNet(net)
    workspace.RunNet(net.Proto().name)
    print("Blobs in the workspace after execution: {}".format(workspace.Blobs()))
    for name in workspace.Blobs():
        print("{}:\n{}".format(name, workspace.FetchBlob(name)))



    Current blobs in the workspace: []
    Blobs in the workspace after execution: ['W', 'X', 'Y', 'b']
    W:
    [[-0.29295802  0.02897477 -1.25667715]
     [-1.82299471  0.92877913  0.33613944]
     [-0.64382178 -0.68545657 -0.44015241]
     [ 1.10232282  1.38060772 -2.29121733]
     [-0.55766547  1.97437167  0.39324901]]
    X:
    [[-0.47522315 -0.40166432  0.7179445 ]
     [-0.8363331  -0.82451206  1.54286408]]
    Y:
    [[ 0.22535783  1.73460138  1.2652775  -1.72335696  0.7543118 ]
     [-0.71776152  2.27745867  1.42452145 -4.59527397  0.4452306 ]]
    b:
    [ 1.  1.  1.  1.  1.]


`RunNetOnce`와 `RunNet`은 몇 가지 차이점이 있는데, 아마 주요 차이점은 연산속도일 것입니다. `RunNetOnce`는 protobuf를 직렬화하여 Python과 C 사이를 통과하고 네트워크를 인스턴스화하는 것까지 포함하기 때문에 실행하는 데 시간이 오래 걸릴 수 있습니다. 이 경우를 전반적으로 살펴보겠습니다.


    # `%timeit`의 마법이 C++에서 제대로 작동하지 않는 것 같아서 loop를 이용하겠습니다. 
    start = time.time()
    for i in range(1000):
        workspace.RunNetOnce(net)
    end = time.time()
    print('Run time per RunNetOnce: {}'.format((end - start) / 1000))
    
    start = time.time()
    workspace.CreateNet(net)
    for i in range(1000):
    workspace.RunNet(net.Proto().name)
    end = time.time()
    print('Run time per RunNet: {}'.format((end - start) / 1000))



    Run time per RunNetOnce: 0.000364284992218
    Run time per RunNet: 4.42600250244e-06

파이썬에서 Caffe2를 사용하고 싶다면 위의 몇 가지 주요 구성 요소를 활용하는 것만으로도 좋을 것입니다. 더 많은 것이 필요하다고 느껴지면, 튜토리얼에 추가 할 계획입니다. 이제부터, 튜토리얼의 나머지 부분을 확인 해보세요!
