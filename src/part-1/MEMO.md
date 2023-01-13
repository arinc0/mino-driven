# 第一章
悪しき構造の弊害を知覚できる様になることが目的。

## 意味不明な命名

### BAD
```php
class MemoryStateManager {
    private int $intValue01;

    public function changeIntValue01(int $changeValue): void
    {
        $this->intValue01 -= $changeValue;

        if ($this->intValue01 < 0) {
            $this->intValue01 = 0;
            update_state_02_flag();
        }
    }
}
```
### GOOD

```php
// メモリ割り当てるってことなのかな？(適当)
class MemoryStateManager {
    private int $memoryCapacity01;
    private int $memoryCapacity02;

    public function allocate(int $allocateCapacity): void
    {
        $this->memoryCapacity01 -= $allocateCapacity;

        if ($this->memoryCapacity01 < 0) {
            $this->memoryCapacity01 = 0;
            update_state_02_flag();
        }
    }
}
```



- 技術駆動命名
- 連番命名

## 理解を困難にする条件分岐のネスト

### BAD
```php
// 生存しているか判定
if (0 < $member->hitPoint()) {
    // 行動可能かを判定
    if ($member->canAct()) {
        // 魔法力が残存しているかを判定
        if ($magic->costMagicPoint() <= $member->magicPoint()) {
            $member->consumeMagicPoint($magic->costMagicPoint());
            $member->chant($magic);
        }
    }
}
```

### GOOD 
```php
// 生存しているか判定
if (0 > $member->hitPoint()) {
    // 死んでる
}

// 行動可能かを判定
if ($member->cannotAct()) {
    // 行動できない
}

// 魔法力が残存しているかを判定
if ($magic->costMagicPoint() <= $member->magicPoint()) {
    $member->consumeMagicPoint($magic->costMagicPoint());
    $member->chant($magic);
}

```
- 何重にもネストしたロジック

## さまざまな悪魔を招きやすいデータクラス

```php
// 契約金額
class ContractAmount {
    // 税込金額
    public int $amountIncludingTax;
    
    // 消費税率
    public float $salesTaxRate;
}

// 契約を管理するクラス
class ContractManager {
    public ContractAmount $contractAmount;
    
    // 税込金額を計算する
    public function calculateAmountIncludingTax(int $amountExcludingTax, float $salesTaxRate): int
    {
        $multiplier = 1.0 + $salesTaxRate;
        $amountIncludingTax = $amountExcludingTax * $multiplier;
        
        // 切り捨て
        return floor($amountIncludingTax);
    }
    
    // 契約締結する
    public function conclude(int $amountExcludingTax): void
    {
        // 省略
        $amountIncludingTax = $this->calculateAmountIncludingTax($amountExcludingTax, $salesTaxRate);
        /** @var ContractAmount $contractAmount */
        $contractAmount new ContractAmount();
        $contractAmount->amountIncludingTax = $amountIncludingTax;
        $contractAmount->salesTaxRate = $salesTaxRate;
        // 省略
    }
}
```

データを保持するクラスと、データを使って計算するロジックが離れている => 低凝集

### 低凝集によって引き起こされる弊害
- 重複コード
- 修正もれ
- 可読性低下
- 未初期化状態 => アンチパターン 生焼けオブジェクト
- 不正値の混入

↓

開発生産性の低下

## 悪魔退治の基本
悪しき構造の弊害を知ることでなんとかしないといけないという意志が生まれる
OOPの基本んであるクラスを適切に設計すること