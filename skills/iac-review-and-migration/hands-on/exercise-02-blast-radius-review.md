# Exercise 02: Blast Radius レビュー — PR レビューとチェックリスト記入

## シナリオ

チームメンバーが RDS インスタンスの設定変更を含む Terraform PR を出しました。あなたはレビュアーとして、この変更の Blast Radius を評価し、レビューチェックリストを完成させてください。

## 準備

この演習はドキュメントベースです。以下の Terraform diff を PR として読み解いてください。

### 変更前 (current)

```hcl
resource "aws_db_instance" "main" {
  identifier           = "payments-db"
  engine               = "postgres"
  engine_version       = "14.9"
  instance_class       = "db.t3.medium"
  allocated_storage    = 100
  storage_encrypted    = true
  multi_az             = true
  skip_final_snapshot  = false
  deletion_protection  = true

  db_name  = "payments"
  username = "admin"
  password = var.db_password

  vpc_security_group_ids = [aws_security_group.db.id]
  db_subnet_group_name   = aws_db_subnet_group.main.name

  tags = {
    Environment = "production"
    Team        = "payments"
  }
}

resource "aws_security_group" "db" {
  name_prefix = "payments-db-"
  vpc_id      = var.vpc_id

  ingress {
    from_port       = 5432
    to_port         = 5432
    protocol        = "tcp"
    security_groups = [var.app_sg_id]
  }
}
```

### 変更後 (proposed)

```hcl
resource "aws_db_instance" "main" {
  identifier           = "payments-db"
  engine               = "postgres"
  engine_version       = "15.4"
  instance_class       = "db.r6g.large"
  allocated_storage    = 200
  storage_encrypted    = true
  multi_az             = true
  skip_final_snapshot  = false
  deletion_protection  = true

  db_name  = "payments"
  username = "admin"
  password = var.db_password

  vpc_security_group_ids = [aws_security_group.db.id]
  db_subnet_group_name   = aws_db_subnet_group.main.name

  apply_immediately = true

  tags = {
    Environment = "production"
    Team        = "payments"
  }
}

resource "aws_security_group" "db" {
  name_prefix = "payments-db-"
  vpc_id      = var.vpc_id

  ingress {
    from_port       = 5432
    to_port         = 5432
    protocol        = "tcp"
    security_groups = [var.app_sg_id]
  }

  ingress {
    from_port   = 5432
    to_port     = 5432
    protocol    = "tcp"
    cidr_blocks = ["10.0.0.0/8"]
  }
}
```

## 演習

`templates/iac_review_checklist.md` テンプレートをコピーし、以下の観点で記入してください。

1. **Blast Radius**: 何が変更され、どのリソースが影響を受けますか？
2. **破壊的変更**: `engine_version` の変更は in-place 更新ですか、それとも replace が必要ですか？
3. **セキュリティ**: Security Group の変更にリスクはありますか？
4. **コスト**: インスタンスクラスとストレージ変更のコスト影響は？
5. **リスク**: `apply_immediately = true` の意味と本番環境でのリスクは？
6. **判定**: この PR を Approve / Request Changes のどちらにしますか？理由は？

## ヒント

<details>
<summary>ヒント 1: engine_version 変更の影響</summary>

PostgreSQL のメジャーバージョン変更（14 → 15）は RDS ではインスタンスの再起動が必要です。`apply_immediately = true` が設定されているため、apply 時に即座にダウンタイムが発生します。

</details>

<details>
<summary>ヒント 2: Security Group の変更</summary>

新しい ingress ルール `cidr_blocks = ["10.0.0.0/8"]` は RFC 1918 プライベートアドレス全体からのアクセスを許可します。これは意図的でしょうか？最小権限の原則に反していないか確認してください。

</details>

## 解答・解説

<details>
<summary>解答を表示</summary>

### Blast Radius 評価

| 項目 | 変更内容 | リスク |
|------|---------|-------|
| engine_version | 14.9 → 15.4 (メジャーアップグレード) | **高** — ダウンタイム発生、互換性問題の可能性 |
| instance_class | db.t3.medium → db.r6g.large | **中** — 再起動、コスト増 |
| allocated_storage | 100 → 200 GB | **低** — オンライン拡張可能 |
| apply_immediately | 未設定 → true | **高** — メンテナンスウィンドウを待たず即時適用 |
| Security Group | アプリ SG のみ → 10.0.0.0/8 追加 | **高** — 広範なネットワークアクセス許可 |

### レビュー判定: **Request Changes**

理由：
1. **apply_immediately + メジャーバージョンアップ**: 本番で即座にダウンタイムが発生する。メンテナンスウィンドウで実施するか、Blue/Green デプロイを検討すべき
2. **Security Group の CIDR**: `10.0.0.0/8` は広すぎる。アクセス元を具体的なサブネット CIDR に絞るべき
3. **テスト不足**: メジャーバージョンアップの動作確認を staging で先に行うべき
4. **コスト影響**: db.t3.medium → db.r6g.large + ストレージ倍増で月額コストが約3倍（要見積もり）

</details>

## まとめ

- Blast Radius 評価は変更ごとにリスクレベルを判定する
- `apply_immediately = true` は本番環境で特に注意が必要
- Security Group の CIDR 変更はセキュリティレビューの最重要チェックポイント
- メジャーバージョンアップは staging での事前検証を必須とする
