# Exercise 05: State Surgery — モジュールリファクタリング

## シナリオ

フラットに配置された Terraform リソースをモジュール構成にリファクタリングする必要があります。リソースを再作成せずに（destroy/create なし）、`terraform state mv` を使って State 内のリソースアドレスを移動してください。

## 準備

### Step 1: フラット構成でリソースを作成

作業ディレクトリに以下の `main.tf` を配置してください。

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

resource "aws_vpc" "app" {
  cidr_block = "10.50.0.0/16"
  tags       = { Name = "handson-surgery-vpc" }
}

resource "aws_subnet" "app_public" {
  vpc_id     = aws_vpc.app.id
  cidr_block = "10.50.1.0/24"
  tags       = { Name = "handson-surgery-public" }
}

resource "aws_subnet" "app_private" {
  vpc_id     = aws_vpc.app.id
  cidr_block = "10.50.2.0/24"
  tags       = { Name = "handson-surgery-private" }
}
```

```bash
terraform init
terraform apply -auto-approve
```

### Step 2: State のバックアップ

```bash
# 必ずバックアップを取る
terraform state pull > terraform.tfstate.backup
```

## 演習

リソースを以下のモジュール構成にリファクタリングしてください。**リソースの destroy/create は発生させないこと。**

目標構成：

```
.
├── main.tf           # provider + module 呼び出し
└── modules/
    └── networking/
        ├── main.tf   # VPC + Subnets
        ├── variables.tf
        └── outputs.tf
```

1. `modules/networking/` ディレクトリとファイルを作成してください
2. ルートの `main.tf` を module 呼び出しに書き換えてください
3. `terraform state mv` で各リソースを module 配下に移動してください
4. `terraform plan` で差分なし（No changes）を確認してください

## ヒント

<details>
<summary>ヒント 1: state mv の書式</summary>

```bash
terraform state mv 'aws_vpc.app' 'module.networking.aws_vpc.app'
```

State 内のリソースアドレスを旧パスから新パスに移動します。

</details>

<details>
<summary>ヒント 2: module の構成</summary>

`modules/networking/variables.tf` には VPC の CIDR ブロックを変数として定義します。ルートの `main.tf` から module を呼び出す際にこの変数を渡します。

</details>

## 解答・解説

<details>
<summary>解答を表示</summary>

### Step 1: Module ファイルの作成

```bash
mkdir -p modules/networking
```

`modules/networking/variables.tf`:

```hcl
variable "vpc_cidr" {
  type = string
}

variable "public_subnet_cidr" {
  type = string
}

variable "private_subnet_cidr" {
  type = string
}
```

`modules/networking/main.tf`:

```hcl
resource "aws_vpc" "app" {
  cidr_block = var.vpc_cidr
  tags       = { Name = "handson-surgery-vpc" }
}

resource "aws_subnet" "app_public" {
  vpc_id     = aws_vpc.app.id
  cidr_block = var.public_subnet_cidr
  tags       = { Name = "handson-surgery-public" }
}

resource "aws_subnet" "app_private" {
  vpc_id     = aws_vpc.app.id
  cidr_block = var.private_subnet_cidr
  tags       = { Name = "handson-surgery-private" }
}
```

`modules/networking/outputs.tf`:

```hcl
output "vpc_id" {
  value = aws_vpc.app.id
}
```

### Step 2: ルート main.tf を書き換え

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

module "networking" {
  source              = "./modules/networking"
  vpc_cidr            = "10.50.0.0/16"
  public_subnet_cidr  = "10.50.1.0/24"
  private_subnet_cidr = "10.50.2.0/24"
}
```

### Step 3: State の移動

```bash
terraform state mv 'aws_vpc.app' 'module.networking.aws_vpc.app'
terraform state mv 'aws_subnet.app_public' 'module.networking.aws_subnet.app_public'
terraform state mv 'aws_subnet.app_private' 'module.networking.aws_subnet.app_private'
```

### Step 4: 検証

```bash
terraform init  # module の初期化
terraform plan
# No changes. Your infrastructure matches the configuration.
```

`No changes` が表示されれば、リソースの再作成なしにリファクタリング成功です。

</details>

## クリーンアップ

```bash
terraform destroy -auto-approve
rm -rf modules/
```

## まとめ

- `terraform state mv` でリソースの再作成なしにアドレスを変更できる
- State 操作前のバックアップ (`terraform state pull`) は必須
- 操作後に `terraform plan` で `No changes` を確認するまで完了とみなさない
- Terraform 1.1+ では `moved` ブロックを config に記述することで state mv をコード化できる
