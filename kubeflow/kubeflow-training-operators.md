---
description: MPIJob Operator, TFJob Operator, PyTorchJob Operator
---

# Kubeflow Training Operators

Kubeflow Training Operators에는 MPIJob Operator, TFJob Operator, PytorchJob Operator가 있는데, Kubernetes 상에서 ML모델을 학습할 때 분산학습 기능을 제공해 학습에 드는 시간을 줄일 수 있다.&#x20;

Training Operators를 이해하려면, Kubernetes Operator 개념을 알아야하며 [\[Kubernetes Operator\]](https://ubiradio.gitbook.io/dev/kubernetes/kubernetes-operator) 글을 참고하자.

Kubeflow에서 제공하는 Training Operator에 대해 알아보자.

### MPIJob Operator

MPIJob Operator 이용해 Kubernetes 상에서 Horovod 기반으로 All Reduce 분산학습을 쉽게 실행할 수 있다.&#x20;

MPIJob Operator가 TFJob Operator, PytorchJob Operator 와 다른 점은 Tensorflow, Keras, Pytorch, MXnet 등 다양한 딥러닝 프레임워크로 개발한 코드를 조금만 수정해서 분산학습을 실행할 수 있다는 점이다.

MPIJob Operator Component는 역할에 따라 Launcher, Worker로 구성되며, MPIJob CRD의 spec 하위 필드에 정의할 수 있다.

MPIJob Operator를 이해하려면, 먼저 Horovod 기반 All Reduce 분산학습 방식을 이해해야 하며, [\[Distributed Training\]](https://ubiradio.gitbook.io/dev/kubeflow/distributed-training) 글을 참고하자.

#### MPIJob 실행 절차

MPIJob Operator는 MPIJob CR의 변경 사항이 발생하면, MPIJob Controller가 다음 절차에 따라 MPIJob을 실행한다.

1\) ConfigMap을 생성한다.\
ConfigMap에는 hostfile과 mpirun에서 사용할 kubexec.sh 쉘 스크립트를 포함하고 있다. ConfigMap에서 hostfile 항목에는 Worker StatefulSet 파드 목록을 정의하고 kubexec.sh 스크립트 항목에는 /etc/hosts 파일에 호스트 네임을 등록할 스크립트를 정의한다.\
2\) Role, RoleBinding, Service Account 를 생성한다.\
3\) Worker StatefulSet 파드를 생성한다.\
4\) Worker StatefulSet 파드가 준비될 때까지 기다린다.\
5\) Launcher Job을 생성하고, mpirun 명령어를 원격으로 실행하는데에 필요한 환경변수를 설정하고 mpirun 명령어를 실행한다. Launcher Job 파드를 초기화할 때 kubectl을 복사한 후, kubexec.sh을 각 Worker StatefulSet 파드에 실행한다.\
6\). Launcher Job이 완료되면, Worker StatefulSet에 replica를 0으로 설정한다. 예를 들어, workerReplicas 2로 설정되었다면 0으로 변경한다.

![출처: https://github.com/kubeflow/community/blob/master/proposals/mpi-operator-proposal.md](<../.gitbook/assets/image (6).png>)

### TFJob Operator

TFJob Operator는 Kubernetes 상에서Tensorflow 분산학습 Job을 실행할 수 있는 Kubeflow Operator로 분산학습을 수행할 클러스터 환경을 쉽게 구축하고 클러스터 내에서 분산학습을 실행할 수 있다.\
\
다음은 TFJob CR을 이용한 TensorFlow 분산학습 실행 과정이다.

![](https://cdn-images-1.medium.com/max/1200/0\*HS3N7Yd1ylKBiaMi.png)

1\) 사용자는 tfjobs.kubeflow.org CRD를 참고하여 TFJob CR을 작성한다.\
2\) kubectl 명령어를 사용해 작성한 TFJob CR을 kube-apiserver에 등록하면, TFJob Operator가 TFJob CR 명세를 읽어 분산학습을 수행할 클러스터를 구축한 후, 준비가 되면 분산학습을 실행한다.

### PyTorchJob Operator

PytorchJob Operator는 Kubernetes 상에서PyTorch 분산학습 Job을 실행할 수는 있는 Kubeflow Operator 이고, 학습 과정은 TFJob 과 유사하며 다음과 같다.

![](https://cdn-images-1.medium.com/max/1200/0\*lDwuQhjZb9PDW\_SN.png)

### 참고자료

[https://github.com/kubeflow/](https://github.com/kubeflow/)\
[https://www.kubeflow.org/docs/components/training/](https://www.kubeflow.org/docs/components/training/)
