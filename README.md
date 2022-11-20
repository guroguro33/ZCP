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
