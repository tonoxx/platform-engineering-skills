# IaC Review & Migration — Hands-on Exercises

Terraform を使った Infrastructure as Code のレビュー、State 操作、ドリフト検出を実践する演習集です。

## 前提条件

- AWS アカウント（演習用のリソース作成権限）
- Terraform CLI 1.5 以上
- AWS CLI（認証済み）
- S3 バケット + DynamoDB テーブル（State backend 用、演習05で必要）

## 注意事項

- 演習ではAWS 上に実リソースを作成します。**演習完了後は必ずクリーンアップしてください**
- コストを最小限に抑えるため、リソースは最小構成を使用しています
- 本番アカウントではなく、検証用アカウントで実施することを推奨します

## 演習一覧

| # | 演習 | 対象スキル | 所要時間 |
|---|------|---------|---------|
| 01 | [State Import](exercise-01-state-import.md) | 手動作成リソースの Terraform 取り込み | 20分 |
| 02 | [Blast Radius レビュー](exercise-02-blast-radius-review.md) | PR レビューとチェックリスト記入 | 20分 |
| 03 | [Drift 検出と修復](exercise-03-drift-detection.md) | 手動変更の検出と判断 | 25分 |
| 04 | [Provider バージョンアップ](exercise-04-provider-upgrade.md) | AWS Provider の更新と影響確認 | 20分 |
| 05 | [State Surgery (state mv)](exercise-05-state-surgery.md) | モジュールリファクタリング | 30分 |

## 進め方

1. 各演習は独立しているので、順不同で取り組めます（ただし Exercise 01 → 03 は連続して取り組むと理解が深まります）
2. 「シナリオ」を読み、提供される HCL コードを使って環境を構築します
3. 「演習」セクションの指示に従い、自力で操作・判断を行ってください
4. 「解答・解説」は操作が完了してから確認してください
5. IaC レビューチェックリスト (`templates/iac_review_checklist.md`) を活用してください

## クリーンアップ

各演習のクリーンアップ手順に従うか、演習ディレクトリで以下を実行：

```bash
terraform destroy -auto-approve
```
