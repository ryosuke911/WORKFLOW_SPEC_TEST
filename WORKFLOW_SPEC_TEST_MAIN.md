# 仕様化テストフェーズ定義

## フェーズ概要
```mermaid
graph TD
    P1[依存関係分析]
    P2[仕様化テスト計画]
    P3[仕様化テスト実装]
    P4[仕様化テスト実行]

    P1 --> P2
    P2 --> P3
    P3 --> P4
```

## 1. 依存関係分析フェーズ
**実行ワークフロー**: `WORKFLOW_SPEC_TEST_DI_ANALYZE.md`

### 目的
- リファクタリング対象コードの依存関係を分析
- テスト戦略決定のための情報収集
- 依存関係の構造化と可視化

### 入力
- 依存関係YAML（`dependency.yaml`）

### 出力
- 依存関係分析結果（`FLOW/output/dependency_analysis.yaml`）

## 2. 仕様化テスト計画フェーズ
**実行ワークフロー**: `WORKFLOW_SPEC_TEST_PLAN.md`

### 目的
- 既存コードの動作観察計画の立案
- 動作記録の実施
- テスト実装計画の策定

### 入力
- 依存関係分析結果（`FLOW/output/dependency_analysis.yaml`）

### 出力
- 動作記録（`FLOW/output/recorded_behaviors.yaml`）
- テスト実装計画（`FLOW/output/spec_test_implementation_plan.yaml`）

## 3. 仕様化テスト実装フェーズ
**実行ワークフロー**: `WORKFLOW_SPEC_TEST_IMPL.md`

### 目的
実装計画と動作記録に基づいて、仕様化テストコードを実装します。

### 入力
- 動作記録（`FLOW/output/recorded_behaviors.yaml`）
- テスト実装計画（`FLOW/output/spec_test_implementation_plan.yaml`）

### 処理内容
1. テスト構造の決定
   - 実装計画の `implementation_strategy.test_structure` に基づくテストクラスの配置
   - 優先順位の反映

2. テストケースの実装
   - 実装計画の `priorities.*.test_cases` に基づくテストメソッドの生成
   - 動作記録の参照と反映

3. テストコードの生成
   - 正常系、エラーケース、境界値ケースの実装
   - テストメソッドの命名規則の適用
   - コメントとアサーションの整備

### 出力
- テストコード（`tests/Feature/**/*Test.php`）
- 実装レポート（`FLOW/output/test_implementation_report.yaml`）
  - 実装の網羅性
  - テストケースの追跡可能性
  - コード品質の検証結果
  - 実装の進捗状況

## 4. 仕様化テスト実行フェーズ
**実行ワークフロー**: `WORKFLOW_SPEC_TEST_EXEC.md`

### 目的
- 実装したテストの実行と結果の収集
- 実装レポートとの整合性確認
- カバレッジ情報の収集と分析

### 入力
- テストコード（`tests/Feature/**/*Test.php`）
- 実装レポート（`FLOW/output/test_implementation_report.yaml`）

### 処理内容
1. テストの実行
   - PHPUnitによるテストの実行
   - テスト結果の収集
   - コードカバレッジの計測

2. 実装レポートとの整合性確認
   - テストケースの実行結果と実装計画の照合
   - 実行順序と優先度の確認

3. 結果の集計とレポート生成
   - テスト実行結果の構造化
   - カバレッジ情報の集計

### 出力
- テスト実行結果（`FLOW/output/test_results.yaml`）
- カバレッジレポート（`FLOW/output/coverage_report.html`）
  ```yaml
  evaluation:
    coverage_status: "PASS|FAIL"
    test_quality: "PASS|FAIL"
    improvements_needed: [...]
    next_action: "PROCEED|RETRY"
  ```

## フェーズ間のファイル依存関係
```mermaid
graph TD
    DY[dependency.yaml] --> DA[dependency_analysis.yaml]
    DA --> OD[observation_design.yaml]
    DA --> IP[spec_test_implementation_plan.yaml]
    OD --> RB[recorded_behaviors.yaml]
    RB --> IP
    IP --> TC[Test Code]
    TC --> TR[test_results.yaml]
```

## 注意事項
1. 各フェーズは独立したワークフローファイルで管理
2. フェーズ間の入出力ファイルは厳密に定義
3. 評価フェーズでNGの場合は計画フェーズに戻る
4. 全フェーズでレビューポイントを設定 