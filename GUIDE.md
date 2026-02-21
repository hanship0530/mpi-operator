빌드 방법
1. 코드 생성 (CRD 및 클라이언트 재생성)

# 프로젝트 디렉토리로 이동
cd /Users/hanship/Desktop/mpi-operator-0.7.0

# 의존성 정리
make tidy

# CRD 재생성 (types.go의 kubebuilder 마커 기반)
make crd

# 전체 코드 생성 (클라이언트, CRD, SDK 포함)
make generate

# 배포 manifest 재생성
make manifest
2. 빌드 및 테스트

# 포맷팅 및 lint
make fmt
make vet
make lint

# 단위 테스트 실행
make test

# 바이너리 빌드
make mpi-operator.v2

# 빌드된 바이너리 위치
ls -la _output/cmd/bin/mpi-operator.v2
3. Docker 이미지 빌드

# 이미지 빌드
make images

# 또는 특정 태그로 빌드
RELEASE_VERSION=v0.7.0-custom make images
4. Kubernetes에 배포

# kustomize를 사용한 배포
kubectl apply -k manifests/base/

# 또는 단일 manifest로 배포
kubectl apply -f deploy/v2beta1/mpi-operator.yaml
5. 변경 확인

# CRD가 올바르게 등록되었는지 확인
kubectl get crd mpijobsv2.kubeflow.org

# MPIJob 리소스 확인 (새 이름으로)
kubectl get mpijobsv2 -A

# 한번에 삭제
kubectl delete -f deploy/v2beta1/mpi-operator.yaml

# 삭제 시
kubectl delete mpijobsv2 --all --all-namespaces
kubectl delete deployment mpi-operator -n mpi-operator
kubectl delete clusterrolebinding mpi-operator
kubectl delete clusterrole mpi-operator
kubectl delete clusterrole kubeflow-mpijobs-admin
kubectl delete clusterrole kubeflow-mpijobs-edit
kubectl delete clusterrole kubeflow-mpijobs-view
kubectl delete serviceaccount mpi-operator -n mpi-operator
kubectl delete crd mpijobsv2.kubeflow.org
kubectl delete namespace mpi-operator

# 삭제 확인
kubectl get namespace mpi-operator
kubectl get crd mpijobsv2.kubeflow.org
kubectl get clusterrole | grep mpi
kubectl get clusterrolebinding | grep mpi
