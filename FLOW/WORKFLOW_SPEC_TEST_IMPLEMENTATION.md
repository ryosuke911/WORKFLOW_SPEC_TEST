# 仕様化テスト実装ワークフロー

## 概要
recorded_behaviors.yamlに記録された動作を、実際のソースコードを参照しながら正確に再現するテストコードを実装するためのワークフロー定義です。

## ワークフロー全体図
```mermaid
graph TD
    %% スタイル定義
    classDef input fill:#0277bd,stroke:#b3e5fc,color:#fff
    classDef process fill:#424242,stroke:#90caf9,color:#fff
    classDef output fill:#2e7d32,stroke:#a5d6a7,color:#fff
    classDef review fill:#6a1b9a,stroke:#e1bee7,color:#fff

    Input1[recorded_behaviors.yaml]:::input
    Input2[ソースコード]:::input
    Input3[code_analysis.yaml]:::input
    
    Input1 --> Phase1[実装計画策定]:::process
    Input2 --> Phase1
    Input3 --> Phase1
    Phase1 --> Plan[spec_test_implementation_plan.yaml]:::output
    
    Plan --> Phase2[テスト実装ループ]:::process
    Input1 --> Phase2
    Input2 --> Phase2
    Input3 --> Phase2
    
    Phase2 --> Tests[仕様化テストコード]:::output
    Phase2 --> Report[実装レポート]:::output
```

## 1. 実装計画策定フェーズ

### フェーズの流れ
```mermaid
sequenceDiagram
    participant AI as Cursor Agent
    participant RB as recorded_behaviors.yaml
    participant SC as Source
    participant Plan as spec_test_implementation_plan.yaml

    Note over AI: フェーズ開始

    rect rgba(65, 105, 225, 0.2)
        Note over AI: 1. 動作記録分析
        AI->>RB: 動作記録の読み込み
        RB-->>AI: 動作パターン
        Note over AI: パターン分類
    end

    rect rgba(60, 179, 113, 0.2)
        Note over AI: 2. コード解析
        AI->>SC: コード構造の確認
        SC-->>AI: 実装詳細
        Note over AI: 要件整理
    end

    rect rgba(218, 112, 214, 0.2)
        Note over AI: 3. 計画策定
        AI->>Plan: 計画の出力
        Plan-->>AI: 出力確認
        AI->>RB: 整合性確認
        RB-->>AI: 確認完了
    end

    Note over AI: フェーズ完了
```

### 概要
recorded_behaviors.yamlとcode_analysis.yamlから、テスト実装の計画を策定します。

### 入力
- recorded_behaviors.yaml（動作記録）
- ソースコード
- code_analysis.yaml（実装コードの解析結果）


### 1. 動作記録の分析
1. recorded_behaviors.yamlの読み込みと解析
   - 各コントローラーの動作を整理
   - 入力値と期待される結果を抽出
   - データベース変更の記録を確認

2. 動作パターンの分類
   - 正常系と異常系の識別
   - 依存関係の特定
   - 優先順位の決定

### 2. 技術要件の確認
1. code_analysis.yamlの解析
   - 実装の技術的制約を確認
   - 必要なコンポーネントを特定
   - インターフェースの要件を把握

2. セキュリティ要件の確認
   - 認証・認可の要件
   - バージョン管理の仕様
   - データ検証ルール

### 3. 実装計画の策定
1. 出力ファイルの作成
   ```bash
   # 出力先の確認
   target_file="/FLOW/output/output/spec_test_implementation_plan.yaml"
   ```

2. 計画の構造
   ```yaml
   targets:
     - id: string           # 機能ID
       priority: number     # 優先順位
       source_file: string  # 実装対象ファイル
       test_cases:
         - name: string     # テストケース名
           type: string     # テストタイプ
           priority: string # 優先度
           dependencies: [] # 依存関係
           expected_behavior:
             input: {}      # 入力値
             result: {}     # 期待される結果
   ```

3. 検証項目
   - [ ] recorded_behaviors.yamlの全動作が網羅されているか
   - [ ] 依存関係が正しく設定されているか
   - [ ] 優先順位が適切か
   - [ ] セキュリティ要件が満たされているか

### 4. 品質チェック
1. 形式検証
   - YAMLの文法チェック
   - 必須フィールドの存在確認
   - 参照整合性の確認

2. 内容検証
   - recorded_behaviorsとの整合性確認
   - テストケースの網羅性確認
   - 依存関係の循環参照チェック

## 成果物
- /FLOW/output/output/spec_test_implementation_plan.yaml

## 完了条件
1. spec_test_implementation_plan.yamlが正しい場所に生成されていること
2. recorded_behaviors.yamlの全動作が計画に反映されていること
3. 全てのテストケースに具体的な期待動作が記述されていること
4. セキュリティと性能の要件が明示されていること

## エラー処理
1. 出力ディレクトリが存在しない場合
   - ディレクトリを作成
   - 作成できない場合はエラーを報告

2. 整合性エラーの検出時
   - エラー内容を報告
   - 修正案を提示

## 注意事項
- 出力ファイルは必ず指定されたディレクトリに配置すること
- 動作記録との整合性を最優先すること
- セキュリティ要件を明示的に記述すること
- パフォーマンス最適化の方針を含めること

## 2. テスト実装ループフェーズ

### 概要
実装計画（spec_test_implementation_plan.yaml）に定義された各テストケースを順次実装していきます。
以下のループを、すべてのテストケースが実装完了するまで繰り返します。

```mermaid
graph TB
    %% スタイル定義
    classDef process fill:#424242,stroke:#90caf9,color:#fff
    classDef decision fill:#ff5722,stroke:#ffccbc,color:#fff
    classDef input fill:#0277bd,stroke:#b3e5fc,color:#fff
    
    %% メインフロー
    Start[次のテストケース選択] --> P1[1.動作記録の解析]:::process
    P1 --> P2[2.実装コードの詳細理解]:::process
    P2 --> P3[3.テストコード実装]:::process
    P3 --> P4[4.テスト実行と検証]:::process
    P4 --> Decision{全テストケース<br>完了?}:::decision
    Decision -->|No| Start
    Decision -->|Yes| End[実装完了]

    %% 入力ファイルの関連付け（左側に配置）
    RecordedBehaviors[recorded_behaviors.yaml]:::input --> |参照| P1
    SourceCode[実装コード]:::input --> |参照| P2

    %% レイアウト調整
    subgraph inputs [入力ファイル]
        RecordedBehaviors
        SourceCode
    end

    %% 位置調整
    subgraph main [メインフロー]
        direction TB
        Start
        P1
        P2
        P3
        P4
        Decision
        End
    end

    %% スタイル設定
    style inputs fill:none,stroke:none
    style main fill:none,stroke:none
```

### ループの各ステップ詳細

1. **次のテストケース選択**
   - spec_test_implementation_plan.yamlから未実装のテストケースを優先順位に従って選択
   ```yaml
   # 例：次に実装するテストケースの選択
   current_target:
     id: "todo_policy_update"
     priority: 3
     source_file: "app/Policies/TodoPolicy.php"
     required_data:
       - "test_user"
       - "sample_todo"
     dependencies:
       - "todo_service_get_todos"
     implementation_notes: "タスク更新ポリシーの検証"
   ```

2. **動作記録の解析**

   a. **動作記録の特定**
      - recorded_behaviors.yamlから対象テストケースの動作記録を抽出
      - 動作記録の構造を理解
      ```yaml
      # 例：TodoController.updateの動作記録
      TodoController.update:
        test_cases:
          basic:
            description: "基本的なタスク更新の動作を記録"
            execution:
              steps:
                - action: "タスク更新"
                  path: "/todos/3"
                  method: "PUT"
                  input:
                    title: "更新後のタスク"
                    description: "これは更新後のタスクです"
      ```

   b. **テストポイントの抽出**
      - 入力データの特定
      - 期待される出力の特定
      - 状態変化の特定
      - エッジケースの特定

3. **実装コードの詳細理解**

   a. **関連ファイルの特定**
      - 主要な実装ファイル
      - 関連するモデル
      - 依存するサービス
      - ポリシーやバリデーション

   b. **コードの構造解析**
      - メソッドのシグネチャ
      - 処理フロー
      - データの流れ
      - エラーハンドリング
      ```php
      // 例：TodoPolicyの実装
      public function update(User $user, Todo $todo)
      {
          return $user->id === $todo->user_id;
      }
      ```

   c. **テスト要件の整理**
      - 必要なテストデータ
      - テストケースの分類
      - 検証ポイント

4. **テストコード実装**

   a. **テストクラスの構造化**
      - Unit層とFeature層の適切な分離
      - テストデータのセットアップ
      ```php
      class TodoPolicyTest extends TestCase
      {
          use RefreshDatabase;

          private TodoPolicy $policy;
          private User $user;
          private Todo $todo;

          protected function setUp(): void
          {
              parent::setUp();
              // テストの準備
          }
      }
      ```

   b. **テストケースの実装**
      - 動作記録に基づくテストケース
      - エッジケースのテスト
      - バリデーションのテスト
      ```php
      /**
       * @test
       * @group todo_policy_update
       */
      public function update_allows_todo_owner()
      {
          $result = $this->policy->update($this->user, $this->todo);
          $this->assertTrue($result);
      }
      ```

   c. **アサーションの実装**
      - データベースの状態検証
      - レスポンスの検証
      - 副作用の検証
      ```php
      // データベースの検証
      $this->assertDatabaseHas('todos', [
          'id' => $this->todo->id,
          'title' => '更新後のタスク'
      ]);

      // レスポンスの検証
      $response->assertStatus(302);
      $response->assertRedirect('/todos');
      ```

5. **テスト実行と検証**

   a. **テストの実行**
      ```bash
      php artisan test --group todo_policy_update
      ```

   b. **結果の検証**
      - すべてのテストが成功することを確認
      - カバレッジの確認
      - エッジケースの網羅確認

   c. **実装状況の更新**
      ```yaml
      # implementation_status.yaml
      status:
        completed:
          - id: "todo_policy_update"
            test_files:
              - "tests/Unit/Policies/TodoPolicyTest.php"
              - "tests/Feature/Todo/TodoUpdatePolicyTest.php"
            status: "completed"
            completed_at: "2024-02-15 14:30:00"
      ```

### テスト実装のベストプラクティス

1. **動作記録の忠実な再現**
   - recorded_behaviors.yamlの記録を正確に再現
   - テストデータは動作記録に基づいて作成
   - 検証項目は動作記録から漏れなく抽出

2. **テストの階層化**
   - Unit層：個別のコンポーネントのテスト
     - サービスクラスの機能テスト
     - ポリシーの認可ロジックテスト
     - バリデーションルールのテスト

   - Feature層：統合的な機能テスト
     - HTTPリクエスト/レスポンスのテスト
     - データベーストランザクションのテスト
     - 認証/認可の統合テスト

3. **テストデータの管理**
   - Factory/Seederの適切な使用
   - テストケース間のデータ独立性確保
   - 現実的なテストデータの作成

4. **テストの可読性**
   - 明確なテストケース名
   - テストの意図を示すコメント
   - グループタグによる分類
   ```php
   /**
    * @test
    * @group todo_policy_update
    * @description タスクの所有者が更新できることを確認
    */
   ```

5. **エラーケースの網羅**
   - バリデーションエラー
   - 認可エラー
   - 不正なデータ
   - エッジケース

### ループの終了条件
- spec_test_implementation_plan.yamlに定義されたすべてのテストケースが実装完了
- 各テストケースが動作記録と一致することを確認済み

### 出力（ループ完了後）
1. 実装された仕様化テストコード一式
2. 実装完了レポート
```yaml
# test_implementation_report.yaml
implementation_report:
  status: "completed"
  total_test_cases: 3
  implemented_test_cases: 3
  test_files:
    - path: "tests/Feature/TodoCreationTest.php"
      cases: 1
    - path: "tests/Feature/TodoWithTagsTest.php"
      cases: 1
    - path: "tests/Feature/TodoDeletionTest.php"
      cases: 1
  execution_results:
    total_assertions: 15
    passed: 15
    failed: 0
```

## 実装時の重要な注意点

1. **動作記録の忠実な再現**
   - recorded_behaviors.yamlの記録をそのまま再現することを最優先
   - 「より良い実装」への改善は目的としない

2. **実際の依存関係の維持**
   - モックやスタブは使用しない
   - 実際のデータベースや外部サービスとの連携をそのまま使用

3. **テストデータの準備**
   - 記録時と同じ状態を再現できるようにデータを準備
   - シーダーやマイグレーションを適切に活用

4. **コメントとドキュメント**
   - 参照している動作記録を明確に記載
   - テストの意図と前提条件を明記