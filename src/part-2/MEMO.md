# 第二章

設計の初歩
どういったことをするのが設計なのかを理解するのが目的

## 省略せずに意図が伝わる名前を設計する

### BAD
```php
$d = 0;
$d = $p1 + $p2;
$d = $d - (($d1 + $d2) / 2);

if ($d < 0) {
    $d = 0;
}
```
### GOOD

```php
$damageAmount = 0;
$damageAmount = $playerArmPower + $playerWeaponPower;
$damageAmount = $damageAmount - (($enemyBodyDefence + $enemyArmorDefence) / 2);

if ($damageAmount < 0) {
    $damageAmount = 0;
}
```

- 意図がわかる変数名にすること

## 変数を使いまわさない、目的ごとの変数を用意する

- 再代入はコードの途中で変数の用途が変わってしまう
- 目的ごとの変数を用意する

```php
$totalPlayerAttackPower = $playerArmPower + $playerWeaponPower;
$totalEnemyDefence = $enemyBodyDefence + $enemyArmorDefence;

$damageAmount = $totalPlayerAttackPower - ($totalEnemyDefence / 2);

if ($damageAmount < 0) {
    $damageAmount = 0;
}
```

## ベタ書きせず意味のあるまとまりでメソッド化

```php
private function sumUpPlayerAttackPower(int $playerBodyPower, int $playerWeaponPower)
{
    return $playerBodyPower + $playerWeaponPower;
}

private function sumUpEnemyDefence(int $enemyBodyDefence, int $enemyArmorDefence)
{
    return $enemyArmorDefence + $enemyBodyDefence;
}

public function estimateDamage(int $totalPlayerAttackPower, int $totalEnemyDefence): int
{
    $damageAmount = $totalPlayerAttackPower - ($totalEnemyDefence / 2);
    
    if ($damageAmount < 0) {
        return 0;
    }
    
    return $damageAmount;
}

$damageAmount = estimateDamage(
    sumUpPlayerAttackPower($playerBodyPower, $playerWeaponPower),
    sumUpEnemyDefence($enemyBodyDefence, $enemyArmorDefence)
);
```

- 意味のあるまとまりでロジックをまとめる => メソッド化する

## 関係し合うデータとロジックをクラスにまとめる
- クラスにすると強く関係するデータとロジックをまとめられる
- 