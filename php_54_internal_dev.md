# PHP 5.4 社内開発環境ドキュメント

## ✅ 現在の開発環境

- **PHPバージョン**：5.4.x
- **Webサーバー**：Apache 2.2系
- **DBサーバー**：MariaDB 10.4（2025年時点）
- **フレームワーク**：Ethna（社内独自カスタマイズ済み）
- **OS**：CentOS 7 または Amazon Linux 2（Docker対応中）
- **その他**：
  - memcacheクラスターを使用
  - PEARやMDB2によるDBアクセス
  - session/cookie管理はPHP標準機能を使用

---

## ⚠️ 制約事項

- PHP 5.4のため、`short array syntax`（`[]`）非対応
- `traits`, `generator`, `password_hash()` など未サポート
- Composerは使っていない／導入が困難
- クラスオートローディングは独自ロジック（`require_once`ベース）
- mbstring や iconv を多用
- テストコードは SimpleTest を採用（一部手動）

---

## 💪 よくある実装例

### DBアクセス

```php
$sql = "SELECT * FROM users WHERE user_id = ?";
$sth = $db->prepare($sql);
$sth->execute(array($user_id));
```

### 配列の書き方（長形式）

```php
$data = array(
  'name' => '田中',
  'age' => 30
);
```

---

## 🧱 開発ルール（抜粗）

- 文字コード：UTF-8（BOMなし）
- 改行コード：LF
- ファイル末尾に改行を入れる
- エラーハンドリング：基本は `try-catch` ではなく戻り値判定

---

## 📦 将来的な移行案（参考）

| 現在の構成           | 移行先候補                    |
| --------------- | ------------------------ |
| PHP 5.4 + Ethna | PHP 8.x + Laravel        |
| PEAR MDB2       | PDO または Eloquent ORM     |
| 独自テンプレート        | Blade テンプレート             |
| 手動環境構築          | Docker + CI/CD           |
| 単体テストなし         | PHPUnit + GitHub Actions |

---

## 📝 備考

- 社内ポリシーにより外部通信制限あり
- 移行は段階的（Blue/Green デプロイやマイグレーションテスト推奨）

