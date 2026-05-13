# Kubernetes Troubleshooting — Hands-on Exercises

Kubernetes クラスター上で意図的に障害を再現し、診断から復旧までを実践する演習集です。

## 前提条件

- EKS または OpenShift クラスターへのアクセス（kubectl 接続済み）
- Namespace の作成・削除権限
- 演習用の専用 Namespace を推奨（`kubectl create namespace k8s-handson`）

## 演習一覧

| # | 演習 | 対象障害 | 所要時間 |
|---|------|---------|---------|
| 01 | [CrashLoopBackOff の診断と修復](exercise-01-crashloopbackoff.md) | OOMKilled / メモリ制限 | 20分 |
| 02 | [ImagePullBackOff の診断と修復](exercise-02-imagepullbackoff.md) | レジストリ認証 / イメージ名 | 15分 |
| 03 | [NetworkPolicy による通信遮断](exercise-03-networkpolicy-isolation.md) | ラベル不一致 / 名前空間分離 | 25分 |
| 04 | [PVC Pending の診断と修復](exercise-04-pvc-pending.md) | StorageClass / AccessMode | 20分 |
| 05 | [ResourceQuota 枯渇](exercise-05-resource-quota-exhaustion.md) | CPU/メモリ Quota / LimitRange | 25分 |

## 進め方

1. 各演習は独立しているので、順不同で取り組めます
2. 「シナリオ」を読み、提供されるマニフェストを apply して障害を再現します
3. 「演習」セクションの指示に従い、自力で診断・修復を試みてください
4. 「解答・解説」は診断が完了してから確認してください
5. 最後に `templates/troubleshooting_runbook.md` テンプレートで所見を記録してください

## クリーンアップ

全演習完了後：

```bash
kubectl delete namespace k8s-handson
```
