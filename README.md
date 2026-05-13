# Platform Engineering Skills

Claude Code 用のカスタムスキル集です。クラウドネイティブなプラットフォームエンジニアリングに特化した知識ベースを提供します。

## Skills

### Platform Infrastructure

#### [cloud-native-platform-engineering](skills/cloud-native-platform-engineering/SKILL.md)
クラウドネイティブプラットフォームの設計・構築・運用ガイド (Day 0/1/2)。
対象: OpenShift, AWS, Kubernetes, Backstage, Terraform, Crossplane, Sigstore, SPIFFE/SPIRE

#### [kubernetes-troubleshooting](skills/kubernetes-troubleshooting/SKILL.md)
Kubernetes の障害診断・解決ワークフロー。Pod ライフサイクル障害、ネットワーク/DNS 問題、ストレージ・リソース枯渇。

#### [iac-review-and-migration](skills/iac-review-and-migration/SKILL.md)
IaC (Terraform/Crossplane/Pulumi) のコードレビュー、バージョンアップ、State 管理のベストプラクティス。

### Observability & Reliability

#### [observability-driven-development](skills/observability-driven-development/SKILL.md)
アプリケーションコードへの OpenTelemetry 計装パターン、SLI 設計、構造化ログ規約。

#### [incident-command](skills/incident-command/SKILL.md)
オンコール設計、Incident Commander ワークフロー、エスカレーション基準、ポストモーテム運営。

### Developer Experience

#### [api-design-and-contract-testing](skills/api-design-and-contract-testing/SKILL.md)
OpenAPI / gRPC スキーマ設計、Pact による Consumer-Driven Contract Testing、API バージョニング戦略。

#### [inner-source-developer-portal](skills/inner-source-developer-portal/SKILL.md)
Backstage プラグイン開発、TechDocs、Software Catalog Scorecard による成熟度評価。

### Organization & Strategy

#### [finops-cost-engineering](skills/finops-cost-engineering/SKILL.md)
クラウドコスト分析、Kubernetes コスト配賦、Reserved/Spot 最適化、FinOps ガバナンス。

#### [platform-adoption-playbook](skills/platform-adoption-playbook/SKILL.md)
プラットフォーム採用戦略、DORA メトリクス計測、Team Topologies の適用。

### Meta

#### [research-driven-skill-builder](skills/research-driven-skill-builder/SKILL.md)
MECE 原則に基づくリサーチ駆動のスキル設計・作成ワークフロー。

## ディレクトリ構成

```
skills/
├── cloud-native-platform-engineering/
│   ├── SKILL.md
│   ├── references/    (3 files)
│   └── templates/     (3 files)
├── kubernetes-troubleshooting/
│   ├── SKILL.md
│   ├── references/    (3 files)
│   └── templates/     (1 file)
├── iac-review-and-migration/
│   ├── SKILL.md
│   ├── references/    (3 files)
│   └── templates/     (1 file)
├── observability-driven-development/
│   ├── SKILL.md
│   └── references/    (3 files)
├── incident-command/
│   ├── SKILL.md
│   ├── references/    (3 files)
│   └── templates/     (2 files)
├── api-design-and-contract-testing/
│   ├── SKILL.md
│   └── references/    (3 files)
├── inner-source-developer-portal/
│   ├── SKILL.md
│   ├── references/    (3 files)
│   └── templates/     (1 file)
├── finops-cost-engineering/
│   ├── SKILL.md
│   ├── references/    (3 files)
│   └── templates/     (1 file)
├── platform-adoption-playbook/
│   ├── SKILL.md
│   ├── references/    (3 files)
│   └── templates/     (1 file)
└── research-driven-skill-builder/
    ├── SKILL.md
    └── references/    (1 file)
```

## License

MIT — See [LICENSE](LICENSE) for details.
