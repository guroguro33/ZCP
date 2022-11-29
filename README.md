# ZCP

## 概要
- Pod：コンテナ群を束ねたもの
- Node：Podを束ねたもの（実態は仮想サーバ）
- MasterNode：WorkerNodeを管理するもの
- kubernetesクラスタ：1つ以上のMasterNodeと複数のWorkerNodeをまとめたもの

## ZCPとは
- CaaS マネージドKubernetesサービス
- クラウド運用者によって完全に管理されたLinuxコンテナの実行基盤であるKubernetesクラスタを提供
  - セルフサービス
  - スケーラブル
  - シングルテナント…他の開発者から完全に独立したクラスタを提供できる
- ZCPを使ってサービスを開発・運用するには、まずアプリケーションをコンテナ化する

### セルフヒーリング
- 障害の発生したノードを自動的に削除し、新たにノードを作成し、クラスタにジョインさせる

### ローリングアップデート
- ノードの作成と削除を繰り返してクラスタを更新する仕組み
- ZCPはクラスタ自体のアップデートのこと

## bpctlとは
- ZCPではKubernetesクラスタの管理に、Blackpearl（ブラックパール）システムを使用
- bpctlは、Blackpearlを介して、ZCPで払い出されたクラスタの操作を接続を行うためのCLIツール

## ログイン
1. bpctlの初回セットアップ（ZCPへの認証設定）
2. 接続先Blackpearlサーバを設定
3. Blackpearlのコンテキストの設定、向き先を切り替え
4. ZCPへログイン

## Kubernetes

### リソース
- kubernetesオブジェクト（Pod、Serviceなど）のこと
- k8sではリソースを作成・登録することでコンテナの実行などを行う
- 主なリソース
  - Pod, ReplicaSet, Deployment
  - NameSpace
  - Service, Ingress
- リソースは、マニフェストファイルで定義する（yamlやjson）

### namespace
- namespaceを使って１つのkubernetesクラスタを仮想的に分割することができる
- デフォルトで下記が用意されている
  - kube-system
  - kube-public
  - default

#### コマンド
```
// namespaceの作成
kubectl create namespace $USER // 自分の名前のnamespaceを作成

// namespaceの一覧を取得
kubectl get namespace
```

### コンテキスト
- 接続先のクラスタ情報とユーザの認証情報、namespaceをセットにしたもの
- コンテキストを切り替えることで、操作するクラスタを変更できる

```
// コンテキストの設定
kubectl config set-context $(kubectl config current-context) --namespace=$USER

// コンテキストの確認
kubectl config get-contextx
```

### Pod
- 操作コマンド
```
// podをデプロイ
kubectl create -f hello-pod.yaml

// podの一覧表示
kubectl get pods

// podの詳細表示
kubectl describe pod hello-pod

// ログ確認（最新）
kubectl logs hello-pod -f

// podの削除
kubectl delete pod hello-pod
```

### Deployment

#### Deployment
- ReplicaSetを生成、管理しローリングアップデートやロールバックといったデプロイ管理を行います。

#### ReplicaSet
- 同じ仕様のPodのレプリカ数を管理します。

#### Pod
- デプロイの最小単位
- 1つ以上のコンテナをまとめたもの

#### コマンド例
```
// デプロイ
kubectl apply -f deployment.yaml

// 確認
kubectl get deployment java-sample-spring-ga
kubectl get pods,deployment,rs // rsはReplicaSetのこと

// 詳細確認（Pod）
kubectl describe pods --selector=app=java-sample-spring-ga // ラベル名でフィルタリング
```

### Ingress
- L7ロードバランサー
- バーチャルホストやパスによる振り分けを実現

### Service
- L4ロードバランサー
- Podに対するアクセスを分散させる
- ラベルとセレクタという仕組みを使って振り分け先のPodを決定する
```
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    # ロードバランスする先のPodラベルを複数設定できる
    app: MyApp
  ports:
  - protocol: TCP
    port: 80
    targetPort: 9376
```

<img width="1050" alt="スクリーンショット 2022-11-29 13 57 26" src="https://user-images.githubusercontent.com/48667277/204442611-a1480d8b-4e1a-457c-b8b7-f0969ce6987e.png">
