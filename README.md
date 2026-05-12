# Platform Engineering Skills

Claude Code 用のカスタムスキル集です。クラウドネイティブなプラットフォームエンジニアリングに特化した知識ベースを提供します。

## Skills

### [cloud-native-platform-engineering](skills/cloud-native-platform-engineering/SKILL.md)

Platform Engineer (PfE) 向けのクラウドネイティブプラットフォーム設計・構築・運用ガイドです。

- **Day 0**: IDP 設計、DevContainer / EaaS、MCP 統合、Golden Path
- **Day 1**: CI/CD (Tekton / GitHub Actions)、GitOps (Argo CD)、IaC、DevSecOps
- **Day 2**: SRE (SLO/SLI)、Observability (OpenTelemetry)、FinOps、AIOps

対象技術: OpenShift, AWS (EKS/ECR/AMP), Kubernetes, Backstage, Terraform, Crossplane, Sigstore, SPIFFE/SPIRE

### [research-driven-skill-builder](skills/research-driven-skill-builder/SKILL.md)

リサーチ駆動で高品質なスキルを設計・作成するためのワークフローガイドです。

- MECE (Mutually Exclusive, Collectively Exhaustive) 原則に基づく構造設計
- 並列リサーチ -> MECE 分類 -> 実装 -> 検証のステップ
- スキル構造パターン (Lifecycle / Layered / Persona / Capability)

## 使い方

Claude Code のスキルとしてインストールして使用します。

```bash
claude skill install /path/to/skills/<skill-name>
```

## ディレクトリ構成

```
skills/
├── cloud-native-platform-engineering/
│   ├── SKILL.md                 # メインスキル定義 (ルーター)
│   ├── references/
│   │   ├── environment-as-a-service.md
│   │   ├── release-engineering-and-security.md
│   │   └── site-reliability-and-operations.md
│   └── templates/
│       ├── architecture_decision_record.md
│       ├── postmortem.md
│       └── slo_document.md
└── research-driven-skill-builder/
    ├── SKILL.md                 # メインスキル定義
    └── references/
        └── mece-design-patterns.md
```

## License

See individual skill files for license information.
