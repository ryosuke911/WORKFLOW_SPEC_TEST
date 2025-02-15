## SpecStory

## ドキュメント作成のためのYAMLとmermaidの活用 (2025/2/9 14:25:38)

_**User**_

4.のドキュメント作成のところに、必要であればmarkdownファイルの中にYAMLやmermaidも適用してOKと追記。カーソルエージェントに指示が伝わりやすい形式を使用すること。是もファイル自体はmarkdownだよ。

---

_**Assistant**_

.cursorrules ファイルを更新して、ドキュメント作成セクションを拡張します。

````text:.cursorrules
// ... existing code ...

4. ドキュメント作成
   ```markdown
   # ワークフロー定義書

   ## 概要
   [ワークフローの目的と適用範囲]

   ## メタデータ
   ```yaml
   version: 1.0.0
   last_updated: YYYY-MM-DD
   author: Cursor Agent
   status: draft|review|approved
   ```

   ## ワークフロー図
   ```mermaid
   flowchart TD
       [フローチャート定義]
   ```

   ## ステップ定義
   1. [ステップ1の詳細手順]
      ```yaml
      input:
        - 入力項目1
        - 入力項目2
      output:
        - 出力項目1
        - 出力項目2
      criteria:
        - 判断基準1
        - 判断基準2
      ```
   2. [ステップ2の詳細手順]
   ...

   ## 成果物テンプレート
   [各ステップの成果物テンプレート]

   ## 品質基準
   - [評価項目と判断基準]
   ```

// ... existing code ...
````

このように、YAMLとmermaidを追加することで、以下の利点があります：

1. YAMLによるメタデータの構造化
   - バージョン管理が容易になる
   - ステータス管理が明確になる
   - 各ステップの入出力と判断基準を構造化できる

2. mermaidによるフロー図の視覚化
   - ワークフローの流れを視覚的に理解しやすくなる
   - 分岐条件や並行処理を明確に表現できる

3. Cursorエージェントの理解向上
   - 構造化されたデータにより、エージェントが処理しやすくなる
   - 明確な形式により、誤解を防ぐことができる

---

_**User**_

このファイルをフィードバック

---

_**Assistant**_

現在の.cursorrules ファイルの内容を確認しました。以下のフィードバックと提案をさせていただきます：

1. メタデータとワークフロー図の追加は良い改善点です：
   - YAMLによる構造化されたメタデータ
   - mermaidによる視覚的なワークフロー表現

2. ただし、以下の点で改善の余地があります：

````text:.cursorrules
4. ドキュメント作成
   ```markdown
   // ... existing code ...

   ## ステップ定義
   1. [ステップ1の詳細手順]
      ```yaml
      step_id: STEP_001
      name: ステップ1
      description: |
        ステップの詳細な説明
      input_artifacts:
        - name: 入力項目1
          type: document|code|test
          required: true
      output_artifacts:
        - name: 出力項目1
          type: document|code|test
      validation:
        - criteria: 判断基準1
          type: automated|manual
      ```
   2. [ステップ2の詳細手順]
   ...
   ```

   ## 品質基準
   ```yaml
   quality_checks:
     - category: コード品質
       criteria:
         - name: テストカバレッジ
           threshold: "80%"
     - category: ドキュメント品質
       criteria:
         - name: 完全性
           checklist:
             - 要件の網羅性
             - エッジケースの考慮
   ```
````

この変更により：

1. 各ステップの定義がより構造化され、Cursorエージェントが理解しやすくなります
2. 品質基準も構造化され、自動チェックが容易になります
3. 入出力の依存関係が明確になります
4. バリデーション基準が具体的になります

これらの変更を検討されてはいかがでしょうか？

---

_**User**_

Cursorエージェントに指示を与えるためのワークフローを作成するという指示は今で十分伝わってるかな

---

_**Assistant**_

はい、現在の.cursorrules ファイルの構造は、Cursorエージェントへの指示を与えるワークフローとして十分な基本構造を持っています。ただし、さらに明確にするために、以下の点を追加することを提案します：

````text:.cursorrules
// ... existing code ...

## 重要な注意事項

- ワークフローは再利用可能な形で設計してください
- 各ステップは明確な判断基準を持つようにしてください
- 異なるプロジェクトタイプへの適用可能性を考慮してください
- **具体的な実装例ではなく、汎用的な手順を定義**してください
+ - Cursorエージェントへの指示は具体的なアクションとして記述してください
+ - 各ステップの成功/失敗条件を明確に定義してください
+ - エージェントが自動で判断できる基準と人間の介入が必要な判断を区別してください

## ワークフロー構成要素

## 必須コンポーネント
- ステップ定義
- 判断基準
- 成果物テンプレート
- 品質チェックリスト
+ - エージェント実行可能なアクション定義
+ - エラーハンドリング手順
+ - 成功/失敗の判定基準

// ... existing code ...
````

これらの追加により：

1. エージェントの役割がより明確になります
2. 自動化可能な部分と人間の判断が必要な部分が区別されます
3. エラー時の対応が明確になります
4. 各ステップの完了判定がより具体的になります

このような追加によって、Cursorエージェントがより確実にワークフローを実行できるようになると考えます。

---

_**User**_

仕様化テスト計画の前に、依存関係の分析が必要と思う。だから @WORKFLOW_SPEC_TEST_DI_ANALYZE.md を最初のフェーズに追加

---

_**Assistant**_

はい、その通りです。WORKFLOW_SPEC_TEST_MAIN.mdを更新して、依存関係分析フェーズを最初のフェーズとして追加しましょう。

````markdown:WORKFLOW_SPEC_TEST_MAIN.md
# 仕様化テストメインワークフロー

## 概要
依存性注入リファクタリングの安全性を確保するための仕様化テスト作成ワークフロー

## 前提条件
- リファクタリング対象のコードが特定されていること

## フェーズ
```mermaid
graph TD
    Start[開始]
    Analyze[依存関係分析]
    Plan[仕様化テスト計画]
    Impl[仕様化テスト実装]
    Exec[仕様化テスト実行]
    Eval[仕様化テスト評価]
    DI[依存性注入リファクタリング]
    End[終了]

    Start --> Analyze
    Analyze --> Plan
    Plan --> Impl
    Impl --> Exec
    Exec --> Eval
    Eval -->|OK| DI
    Eval -->|NG| Plan
```

## 0. 依存関係分析フェーズ
- 目的：リファクタリング対象コードの依存関係を分析し、テスト戦略を決定する
- サブワークフロー：WORKFLOW_SPEC_TEST_DI_ANALYZE.md
- 主な分析項目：
  - 依存関係グラフの構築
  - 結合度・凝集度の計算
  - コード特性の分析
  - レイヤー分析
- 出力：
  - 依存関係分析レポート
  - リファクタリング候補の提案
  - テスト要件の初期評価

// ... existing phases ...
````

この変更により：

1. 依存関係分析が最初のフェーズとして明確に位置づけられます
2. 分析結果に基づいて、より効果的なテスト計画を立てることができます
3. リファクタリングの影響範囲が事前に把握できます
4. テストの優先順位付けがより適切に行えます

---

_**User**_

0じゃなくて1からにして

---

_**Assistant**_

はい、フェーズ番号を修正します。

````markdown:WORKFLOW_SPEC_TEST_MAIN.md
# 仕様化テストメインワークフロー

// ... existing code ...

## 1. 依存関係分析フェーズ
- 目的：リファクタリング対象コードの依存関係を分析し、テスト戦略を決定する
- サブワークフロー：WORKFLOW_SPEC_TEST_DI_ANALYZE.md
- 主な分析項目：
  - 依存関係グラフの構築
  - 結合度・凝集度の計算
  - コード特性の分析
  - レイヤー分析
- 出力：
  - 依存関係分析レポート
  - リファクタリング候補の提案
  - テスト要件の初期評価

## 2. 仕様化テスト計画フェーズ
// ... existing code ...

## 3. 仕様化テスト実装フェーズ
// ... existing code ...

## 4. 仕様化テスト実行フェーズ
// ... existing code ...

## 5. 仕様化テスト評価フェーズ
// ... existing code ...
````

フェーズ番号を1から始めて、順番に番号を振り直しました。これにより、より自然な番号付けになりました。

---

_**User**_

2. 仕様化テスト計画フェーズを動作記録実施フェーズに変更。 @WORKFLOW_SPEC_TEST_PLAN.md の動作記録フェーズまで。最終出力はecorded_behaviors.yamlになる。4. 実装計画策定フェーズは　@WORKFLOW_SPEC_TEST_IMPL.md の1. テスト構造の決定よりも前に移動する。変に書き換えたりせず、移動だけして

---

_**Assistant**_

はい、フェーズの再構成を行います。

````markdown:WORKFLOW_SPEC_TEST_MAIN.md
# 仕様化テストフェーズ定義

## フェーズ概要
```mermaid
graph TD
    P1[依存関係分析]
    P2[動作記録実施]
    P3[実装計画策定]
    P4[仕様化テスト実装]
    P5[仕様化テスト実行]

    P1 --> P2
    P2 --> P3
    P3 --> P4
    P4 --> P5
```

## 1. 依存関係分析フェーズ
**実行ワークフロー**: `WORKFLOW_SPEC_TEST_DI_ANALYZE.md`
// ... existing code ...

## 2. 動作記録実施フェーズ
**実行ワークフロー**: `WORKFLOW_SPEC_TEST_PLAN.md`（0. 観点情報補完フェーズから3. 動作記録フェーズまで）

### 目的
- 既存コードの動作観察計画の立案
- 動作記録の実施
- 実際の動作の記録

### 入力
- 依存関係分析結果（`FLOW/output/dependency_analysis.yaml`）

### 出力
- 動作記録（`FLOW/output/recorded_behaviors.yaml`）

## 3. 実装計画策定フェーズ
**実行ワークフロー**: `WORKFLOW_SPEC_TEST_PLAN.md`（4. 実装計画策定フェーズ）

### 目的
- テスト実装の全体方針策定
- テストの優先順位付け
- テストデータ要件の定義

### 入力
- 依存関係分析結果（`FLOW/output/dependency_analysis.yaml`）
- 動作記録（`FLOW/output/recorded_behaviors.yaml`）

### 出力
- テスト実装計画（`FLOW/output/spec_test_implementation_plan.yaml`）

## 4. 仕様化テスト実装フェーズ
**実行ワークフロー**: `WORKFLOW_SPEC_TEST_IMPL.md`

// ... existing code ...

## 5. 仕様化テスト実行フェーズ
**実行ワークフロー**: `WORKFLOW_SPEC_TEST_EXEC.md`

// ... existing code ...
````

これにより、フェーズの順序が以下のように整理されました：
1. 依存関係分析
2. 動作記録実施（旧：仕様化テスト計画の一部）
3. 実装計画策定（旧：仕様化テスト計画の一部）
4. 仕様化テスト実装
5. 仕様化テスト実行

---

_**User**_

仕様化テスト実装の中に実装計画策定を入れる。全部でフェーズは4つね。依存関係分析フェーズ→動作記録フェーズ→仕様化テスト実装フェーズ→仕様化テスト実行フェーズにして

---

_**Assistant**_



はい、フェーズを4つに整理し直します。

````markdown:WORKFLOW_SPEC_TEST_MAIN.md
# 仕様化テストフェーズ定義

## フェーズ概要
```mermaid
graph TD
    P1[依存関係分析]
    P2[動作記録]
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

## 2. 動作記録フェーズ
**実行ワークフロー**: `WORKFLOW_SPEC_TEST_PLAN.md`（観点情報補完から動作記録まで）

### 目的
- 既存コードの動作観察計画の立案
- 動作記録の実施
- 実際の動作の記録

### 入力
- 依存関係分析結果（`FLOW/output/dependency_analysis.yaml`）

### 出力
- 動作記録（`FLOW/output/recorded_behaviors.yaml`）

## 3. 仕様化テスト実装フェーズ
**実行ワークフロー**: `WORKFLOW_SPEC_TEST_IMPL.md`

### 目的
- テスト実装計画の策定
- テストコードの実装
- 実装レポートの生成

### 入力
- 依存関係分析結果（`FLOW/output/dependency_analysis.yaml`）
- 動作記録（`FLOW/output/recorded_behaviors.yaml`）

### 出力
- テストコード（`tests/Feature/**/*Test.php`）
- 実装レポート（`FLOW/output/test_implementation_report.yaml`）

## 4. 仕様化テスト実行フェーズ
**実行ワークフロー**: `WORKFLOW_SPEC_TEST_EXEC.md`

### 目的
- 実装したテストの実行と結果の収集
- 実装レポートとの整合性確認
- カバレッジ情報の収集と分析

### 入力
- テストコード（`tests/Feature/**/*Test.php`）
- 実装レポート（`FLOW/output/test_implementation_report.yaml`）

### 出力
- テスト実行結果（`FLOW/output/test_results.yaml`）
- カバレッジレポート（`FLOW/output/coverage_report.html`）

## フェーズ間のファイル依存関係
```mermaid
graph TD
    DY[dependency.yaml] --> DA[dependency_analysis.yaml]
    DA --> RB[recorded_behaviors.yaml]
    DA --> TC[Test Code]
    RB --> TC
    TC --> TR[test_results.yaml]
```

## 注意事項
1. 各フェーズは独立したワークフローファイルで管理
2. フェーズ間の入出力ファイルは厳密に定義
3. 評価フェーズでNGの場合は必要なフェーズに戻る
4. 全フェーズでレビューポイントを設定
````

---



---

