# クラス設計 - 全てにつながる設計の基盤
オブジェクト指向設計の基本

## クラス単体で正常に動作する様設計する
- まどろっこしい初期設定をせずとも初めから使える設計にする
- 正しく操作できるメソッドのみを外部に提供する
- 自己防衛責務を全てのクラスが備える

## 成熟したクラスへ成長させる設計術

- インスタンス変数しか持っていないデータクラス
- 引数なしコンストラクタ => 生焼けオブジェクトを生み出しやすい
- 完全コンストラクタ => コンストラクタで不正値を弾く => ガード節
- インスタンス変数、プロパティをfinalで不変にする => readonly propertyが使える
- 値は不変で交換可能であるということ
- プロパティは直接触らせず、公開しているメソッドからのみ変更できる様にする
- システムの使用に必要なメソッドのみを定義する
- プリミティブ型に固執すると意図が異なる値が複数あっても同じint型だからと意図が異なる値を渡してしまうミスを防げない

クラス設計とは、インスタンス変数を不正状態に陥らせないための仕組みづくり

低凝集、高凝集、高凝集なクラスを作ることをカプセル化(意訳)

### 設計パターンの例
- 完全コンストラクタ
- 値オブジェクト
- ストラテジーパターン
- ポリシーパターン
- ファーストクラスコレクション
- スプラウトクラス

