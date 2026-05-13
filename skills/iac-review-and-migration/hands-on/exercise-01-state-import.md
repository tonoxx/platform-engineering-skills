# Exercise 01: State Import — 手動作成リソースの Terraform 取り込み

## シナリオ

運用チームが緊急対応で AWS CLI を使って S3 バケットを手動作成しました。このバケットを Terraform 管理下に取り込み、今後は IaC で管理できるようにしてください。

## 準備

### Step 1: 手動でリソースを作成

```bash
# 一意のバケット名を生成
BUCKET_NAME="handson-import-$(date +%s)"
echo "Bucket name: $BUCKET_NAME"

# S3 バケットを手動作成
aws s3api create-bucket \
  --bucket "$BUCKET_NAME" \
  --region us-east-1

# タグを付与
aws s3api put-bucket-tagging \
  --bucket "$BUCKET_NAME" \
  --tagging 'TagSet=[{Key=Environment,Value=dev},{Key=Team,Value=platform}]'
```

### Step 2: Terraform プロジェクトを作成

作業ディレクトリを作成し、以下の `main.tf` を配置してください。

```hcl
terraform {
  required_version = ">= 1.5"
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
}

# TODO: この resource ブロックを完成させてください
resource "aws_s3_bucket" "imported" {
  bucket = "REPLACE_WITH_BUCKET_NAME"
}
```

```bash
terraform init
```

## 演習

1. `main.tf` の `bucket` 属性を実際のバケット名に修正してください
2. `terraform import` コマンドで既存バケットを State に取り込んでください
3. `terraform plan` を実行し、差分がないことを確認してください
4. 差分がある場合、`main.tf` を修正して差分を解消してください
5. タグの設定も Terraform 管理下に含めてください

## ヒント

<details>
<summary>ヒント 1: import コマンドの形式</summary>

```bash
terraform import aws_s3_bucket.imported <BUCKET_NAME>
```

</details>

<details>
<summary>ヒント 2: plan で差分が出る場合</summary>

import 後に `terraform plan` で差分が出るのは、Terraform config に記述されていない属性が実リソースに存在するためです。`terraform show` で State 内の属性を確認し、config に反映してください。

</details>

## 解答・解説

<details>
<summary>解答を表示</summary>

### 手順

```bash
# 1. バケット名を修正（main.tf を編集）
# bucket = "handson-import-1234567890"

# 2. import 実行
terraform import aws_s3_bucket.imported "$BUCKET_NAME"
# aws_s3_bucket.imported: Importing from ID "handson-import-1234567890"...
# aws_s3_bucket.imported: Import prepared!

# 3. State の確認
terraform show

# 4. plan で差分確認
terraform plan
```

plan でタグの差分が出る場合、`aws_s3_bucket_tagging` リソースを追加：

```hcl
resource "aws_s3_bucket" "imported" {
  bucket = "handson-import-1234567890"
}

resource "aws_s3_bucket_tagging" "imported" {
  bucket = aws_s3_bucket.imported.id

  tag {
    key   = "Environment"
    value = "dev"
  }

  tag {
    key   = "Team"
    value = "platform"
  }
}
```

タグリソースも import：

```bash
terraform import aws_s3_bucket_tagging.imported "$BUCKET_NAME"
terraform plan
# No changes. Your infrastructure matches the configuration.
```

</details>

## クリーンアップ

```bash
terraform destroy -auto-approve
```

## まとめ

- `terraform import` は State にリソースを追加するが、config は自動生成されない
- import 後に `terraform plan` で差分がないことを必ず確認する
- AWS の最新 provider ではリソースが細分化されている（S3 バケット本体、タグ、ACL 等が別リソース）
- `terraform show` で State 内の属性を確認し、config に反映する
