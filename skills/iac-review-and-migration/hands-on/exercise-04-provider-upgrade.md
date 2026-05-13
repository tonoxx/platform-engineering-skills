# Exercise 04: Provider バージョンアップ

## シナリオ

チームで使用している AWS Provider が古いバージョンにピン留めされています。セキュリティパッチと新機能のために最新バージョンにアップグレードする必要があります。安全にアップグレードを実施してください。

## 準備

作業ディレクトリに以下の `main.tf` を配置してください。

```hcl
terraform {
  required_version = ">= 1.5"
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "= 4.67.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
}

resource "aws_s3_bucket" "logs" {
  bucket_prefix = "handson-upgrade-"
  acl           = "private"

  versioning {
    enabled = true
  }

  server_side_encryption_configuration {
    rule {
      apply_server_side_encryption_by_default {
        sse_algorithm = "aws:kms"
      }
    }
  }

  tags = {
    Environment = "dev"
    Team        = "platform"
  }
}
```

```bash
terraform init
terraform apply -auto-approve
```

## 演習

1. 現在の Provider バージョンを確認してください
2. `version` 制約を `"~> 5.0"` に変更してください
3. `terraform init -upgrade` を実行してください
4. `terraform plan` を実行し、非推奨警告やエラーを確認してください
5. 検出された問題を修正してください
6. 修正後に `terraform plan` で差分を確認し、安全に apply してください

## ヒント

<details>
<summary>ヒント 1: Provider 4.x → 5.x の主な変更</summary>

AWS Provider 5.x では S3 バケットの設定が個別リソースに分離されました：
- `acl` → `aws_s3_bucket_acl`
- `versioning` → `aws_s3_bucket_versioning`
- `server_side_encryption_configuration` → `aws_s3_bucket_server_side_encryption_configuration`

</details>

<details>
<summary>ヒント 2: エラーへの対処</summary>

Provider 5.x では `acl` 引数は `aws_s3_bucket` から削除されています。別リソースに移行するか、S3 のデフォルト設定（BucketOwnerEnforced）を利用してください。

</details>

## 解答・解説

<details>
<summary>解答を表示</summary>

### Step 1: バージョン確認

```bash
terraform version
terraform providers
# hashicorp/aws v4.67.0
```

### Step 2-3: Provider アップグレード

`main.tf` の version 制約を変更：

```hcl
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
```

```bash
terraform init -upgrade
# Upgrading hashicorp/aws from 4.67.0 to 5.x.x
```

### Step 4: Plan 実行と問題検出

```bash
terraform plan
```

エラーが発生します：
- `acl` 引数は非推奨/削除
- `versioning` ブロックは非推奨
- `server_side_encryption_configuration` ブロックは非推奨

### Step 5: コード修正

```hcl
resource "aws_s3_bucket" "logs" {
  bucket_prefix = "handson-upgrade-"

  tags = {
    Environment = "dev"
    Team        = "platform"
  }
}

resource "aws_s3_bucket_versioning" "logs" {
  bucket = aws_s3_bucket.logs.id
  versioning_configuration {
    status = "Enabled"
  }
}

resource "aws_s3_bucket_server_side_encryption_configuration" "logs" {
  bucket = aws_s3_bucket.logs.id
  rule {
    apply_server_side_encryption_by_default {
      sse_algorithm = "aws:kms"
    }
  }
}
```

### Step 6: Import と Apply

新しいリソースを State に取り込む：

```bash
BUCKET_ID=$(terraform show -json | jq -r '.values.root_module.resources[] | select(.type=="aws_s3_bucket") | .values.id')

terraform import aws_s3_bucket_versioning.logs "$BUCKET_ID"
terraform import aws_s3_bucket_server_side_encryption_configuration.logs "$BUCKET_ID"

terraform plan
# 差分がないことを確認
terraform apply -auto-approve
```

</details>

## クリーンアップ

```bash
terraform destroy -auto-approve
```

## まとめ

- Provider アップグレードは必ず非本番環境で事前テストする
- `terraform init -upgrade` で Provider バイナリを更新する
- AWS Provider 5.x では S3 バケット設定が個別リソースに分離された — これは典型的な breaking change
- アップグレード後の `terraform plan` で全環境の影響を確認する
- Changelog（GitHub Releases）を必ず読み、migration guide に従う
