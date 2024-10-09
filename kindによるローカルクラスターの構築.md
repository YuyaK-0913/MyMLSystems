# kindによるローカルクラスターの構築

## 概要
技術検証などを行うためにkubernetesクラスターをクイックに構築したいことが割とある。

docker+kindを使えば、ローカルPCの環境にkubernetesクラスターの構築が容易に可能なのでおすすめです。（ローカルクラスターの構築は、minikubeやk3sなどいくつか方法があるが、kindを使うのが1番簡単な気がする）

## 前提
- wslとDockerがインストール済みであること。

## 0. Dockerのインストール
一応Dockerのインストールも書いておく。
- 公式ドキュメントの手順でOK。
https://docs.docker.com/engine/install/ubuntu/

- Docker DesktopでもOK。

## 1. kubectlのインストール
公式ドキュメント
https://kind.sigs.k8s.io/

```
$ curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
$ sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
$ kubectl version --client
```

## 2. kindのインストール
```
$ curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-amd64
$ chmod +x ./kind
$ sudo mv ./kind /usr/local/bin/kind
$ kind version
```

## 3. kubernetesクラスターの構築
- 下記のコマンドでシングルノードクラスターが構築可能。
（簡単な検証目的であればこれで十分）
  ```
  kind create cluster
  ```

- kindでは、ノードのDockerイメージを指定できるので、下記のようにイメージを指定することも可能。
  ```
  kind create cluster --image=kindest/node:v1.29.0
  ```

## 4. kubernetesクラスターとの接続確認
```
kubectl cluster-info --context kind-kind

# 実行結果
Kubernetes control plane is running at https://127.0.0.1:40743
CoreDNS is running at https://127.0.0.1:40743/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```
