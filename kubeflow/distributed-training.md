---
description: Horovod, TensorFlow Distributed Training, PyTorch Distributed Training
---

# Distributed Training

### Distributed Training Goals

분산학습 프레임워크는 **기존의 ML모델 학습 코드를 조금만 바꾸어도 분산학습을 지원하는 것을 목표**로 한다. **대규모 ML모델도 학습 가능**해야 하며(Model Parallelism) **ML모델 학습 시간도 줄일 수 있어야 한다**(Data Parallelism).\


#### **Model Parallelism**

![](https://cdn-images-1.medium.com/max/1200/0\*n5D\_xZRh8NijbfS5.png)

#### **Data Parallelism**

![](https://cdn-images-1.medium.com/max/1200/0\*LRpnyGivbsU-mc8d.png)

### Distributed Training Mechanism

#### **Synchronous training**

동기식 학습은 **전체 데이터를 쪼개서 Worker들이 Gradient를 계산**하고, 계산한 Gradient를 집계한 후 새로운 ML모델을 생성한다. 생성한 ML모델을 Worker에 전송하고 Gradient를 계산하는 과정을 반복하여 학습을 수행한다.

#### **Asynchronous training**

비동식 학습은 **Worker들이 각각 전체 데이터를 사용해 Gradient를 계산**하고, 각각 비동기적으로 Gradient를 업데이트하는 방식이다. 일반적으로 동기식 학습은 all-reduce 방식으로 구현하고, 비동기식 학습은 Parameter Server 방식을 사용한다.

![](<../.gitbook/assets/image (7).png>)

### Parameter Server Training

Worker가 데이터를 학습하여 Gradient를 계산한 후, Parameter Server로 전송하고 평균을 계산해 다시 Worker로 전송하는 방식이다. **Worker는 대역폭 전체를 사용하지 않지만, Parameter Server는 대역폭 병목현상이 발생**할 수 있다. Parameter Server를 여러 개 둘 경우, 네트워크 상호 연결이 복잡해 질 수도 있다.

![](https://cdn-images-1.medium.com/max/1200/0\*guKgaLQgTAqi2Wg\_.png)

### All Reduce-based Distributed Training

**Worker가 서로 Gradient를 주고 받으면서 Reducing하는 방식으로 동작**한다. Local ReduceScatter, Remote AllReduce, Local Gather 순으로 Reducing 을 진행한다.

![](https://cdn-images-1.medium.com/max/1200/1\*Xr0k-gO6EYcxCi7k7GDK1g.png)

![](https://cdn-images-1.medium.com/max/1200/1\*Cnjkv7Bw0V6rdq2-rzeDmA.png)

### TensorFlow Distributed Training

TensorFlow 클러스터 내에서 가질 수 있는 역할은 Chief, PS, Worker, Evaluator 중 하나이며, PS 역할은 Parameter Server Training에서만 사용한다.\
\
Chief는 **모델 체크포인트**을 작업을 수행한다. \
PS는 **모델 파라미터 서버** 역할을 수행한다. \
Worker는 **Gradient 구하는 역할**을 하며, \
Chief 설정을 하지 않았다면 0번 Worker가 Chief가 된다. \
Evaluator는 **평가 지표 계산**하는 역할을 한다**.**

![](<../.gitbook/assets/image (38).png>)

TensorFlow 분산환경 클러스터 구성 TensorFlow 는 `tf.distribute.Strategy` 패키지를 사용하여 분산학습을 수행할 수 있다. 분산학습 을 수행할 클러스터 환경 구성은 `tf.train.ClusterSpec`에 직접 설정하거나, `TF_CONFIG` 환경 변수를에 설정할 수 있다.

#### `tf.train.ClusterSpec`

```python
cluster = tf.train.ClusterSpec({"worker": ["worker0.example.com:2222",
                                           "worker1.example.com:2222",
                                           "worker2.example.com:2222"],
                                "ps": ["ps0.example.com:2222",
                                       "ps1.example.com:2222"]})
```

#### `TF_CONFIG`  Environment variable

`TF_CONFIG` 환경변수는 JSON 포맷으로 , Cluster를 구성할 Host와 역할을 지정할 수 있다.

#### `tf.distribute.Strategy` 분산 패키지

&#x20;`tf.distribute.Strategy` 는 기존 ML모델 학습 코드를 조금만 수정해도 분산 학습이 가능하며, Multi-GPU 인지, Multi-Node 인지에 따라 분산학습 방법이 구분된다.

`MirroredStrategy` \
장비 하나에서 다중GPU(Multi-GPU)를 이용한 동기식 분산학습 방법이다.

`MultiWorkerMirroredStrategy` \
Multi-Worker 를 이용한 동기식 분산학습으로 각 Worker는 Multi-GPU를 사용할 수 있다. Multi-Worker들 사이에는 All Reduce방식을 사용한다.

`ParameterServerStrategy`\
Parameter Server 방식의 비동기식 분산학습을 제공한다.

### PyTorch Distributed Training

PyTorch는 `torch.distributed` 패키지에서 분산학습을 제공하며, Parameter Server 분산 방식으로 `DataParallel`과 All Reduce 분산학습 방식으로 `DistributedDataParallel`을 지원한다.

#### DataParallel (DP)

**DP는 Master-Worker 구조**로 Master는 Cluster Coordinator역할을 수행하고 Worker는 데이터를 학습한다.

Master는 모델 Weight를 복제하고 Worker에 Broadcast한다.\
Master는 학습데이터를 쪼개서 각각 Worker에 전송한다.\
Worker에서 Gradient 을 계산한 후, 각 GPU에서 Local Gradient를 수집한다.\
Master는 수집한 Gradient를 집계하고 모델 업데이트를 수행한다.

![](https://cdn-images-1.medium.com/max/1200/0\*eFLzUZThBOf2U-uJ.png)

#### DistributedDataParallel (DDP)

**DDP는 All Reduce 방식**으로 Worker만으로 분산학습을 수행한다.

![](https://cdn-images-1.medium.com/max/1200/0\*QGPx-EUN3LRQf1dG.png)

#### &#x20;**PyTorch DDP Example**

{% code title="https://pytorch.org/docs/stable/notes/ddp.html" %}
```python

# 프로세스가 서로 통신할 수 있도록 초기화를 수행한다.
torch.distributed.init_process_group(backend='mpi')

# 초기화가 완료되면, 각 프로세스에 GPU 장치를 매핑한다.
local_rank = int(os.environ['LOCAL_RANK'])
device = torch.device("cuda:{}".format(local_rank))
torch.cuda.set_device(local_rank)

# 데이터셋을 분산하기 위해 DistributedSampler를 생성한다.
torch.utils.data.distributed.DistributedSampler(trainset)

# 학습 혹은 평가에서 사용할 데이터를 Dataloaders에서 가져온다.
train_loader = torch.utils.data.DataLoader(trainset,
                        batch_size=batch_size,
                        shuffle=(train_sample is None),
                        num_workers=workers,
                        pin_memory=False,
                        sampler=train_sampler)
                        
# 분산학습 방식을 정의하고 모델을 GPU 장치와 매핑한다.
model = Net().to(device)
Distributor = nn.parallel.DistributedDataParallel
model = Distributor(model)

# GPU 장치에 데이터를 매핑한다.
for data, target in train_loader:
   data, target = data.to(device), target.to(device)
```
{% endcode %}

#### `PyTorch`  Environment variable

`WORLD_SIZE`는 클러스터 총 노드 수이고, \
RANK는 각 노드의 고유 식별자이다. RANK 0 \~WORLD\_SIZE — 1까지 인덱스를 사용하며, 각 노드에 인덱스를 부여한다. RANK가 0이면 Master이다. `MASTER_ADDR`, `MASTER_PORT` 는 Master 정보로 모든 노드에 설정한다.

### Horovod

Horovod는 Tensorflow, Keras, PyTorch, MXNet 에서 MPI기반으로 **Multi-GPU를 활용하여 Distributed Training을 지원하는 프레임워크**이고, Parameter Server 분산학습 방식의 네트워크 대역폭 병목 현상을 개선하기 위해 고안된 **All Reduce 분산학습 방식**을 사용한다.

Horovod를 활용하면, 기존 학습코드에 적은 코드를 추가하여 손 쉽게 Distributed Training을 구현할 수 있다.

#### Horovod에 사용하는 용어

Horovod를 이해하기 위해서는 다음과 같은 MPI와 관련된 몇가지 용어를 먼저 알아야 한다. MPI (Message Passing Interface)는 병렬화 기술로 여러대의 CPU/GPU를 병렬화할 때 사용한다.

size 전체 프로세스 개수\
slots 프로세스 개수 (processing unit으로 보통 Worker당 Process 개수를 정의함)\
rank 클러스터에서 `0 ~ size-1` 사이의 고유한 프로세스 ID\
local\_rank 하나의 호스트에서 고유한 프로세스 ID

다음은 각각 GPU장치가 2개씩 장착된 호스트 2대를 가진 시스템 환경에서 GPU 장치를 어떻게 구별하는 보여준다.

![](<../.gitbook/assets/image (9).png>)

#### Horovod 기반 분산학습 예제

`hvd.init()` 실행한다.\
`hvd.local_rank()` 별로 사용할 GPU를 지정한다. \
Learning rate를 Worker 개수에 따라 조정한다. \
기존 Optimizer를 Horovod Optimzer 로 확장한다. \
rank 0 초기 상태를 다른 rank와과 동기화하기 위 hook를 설정한다. \
rank 0 만 체크포인트 하도록 설정한다.

{% code title="https://github.com/kubeflow/mpi-operator/blob/master/examples/horovod/tensorflow_mnist.py" %}
```python
import tensorflow as tf
import horovod.tensorflow as hvd

# Initialize Horovod
# 호로보드를 초기화 한다.
hvd.init()

# 사용할 GPU를 매핑한다.
config = tf.ConfigProto()
config.gpu_options.allow_growth = True
config.gpu_options.visible_device_list = str(hvd.local_rank())

# 모델 코드를 작성한다.
loss = ...
opt = tf.train.AdamOptimizer(lr=0.01 * hvd.size())

# 기존 모델 옵티마이저를 호로보드 분산 옵티마이저로 확장한다.
opt = hvd.DistributedOptimizer(opt)

# rank 0 초기 상태를 다른 rank와 동기화하기 위해 hook를 설정한다.
hooks = [hvd.BroadcastGlobalVariablesHook(0)]

# rank 0 를 체크포인트 한다.
ckpt_dir = "/tmp/train_logs" if hvd.rank() == 0 else None
```
{% endcode %}
