<!-- code-to-doc-findings
{
  "schemaVersion": 1,
  "scope": "受注CSV取り込み（バッチ入口 BatchMain から OrderImportService まで。画面は対象外）",
  "sourceCommit": "cdfd92928ba32b6f1878768c5dce1d4bd0b4d480"
}
-->
# Findings: 受注CSV取り込み

Agents add rows with Decision `pending`. Only people fill Decision (`adopted` or `rejected`), Reason and Decided by.

| ID | Claim | Evidence | Type | Decision | Reason | Decided by |
|---|---|---|---|---|---|---|
| F-001 | バッチの入口は、環境変数のパスワードで認証し、認証できなかった場合は終了コード 2 で終わる。 | [[source:wholesale-batch/src/main/java/jp/co/tsubame/wholesale/batch/BatchMain.java#L35-L50]] | fact | pending | | |
| F-002 | `import-orders` コマンドは、BATCH 権限に加えて SALES または MANAGER の権限を要求する。 | [[source:wholesale-batch/src/main/java/jp/co/tsubame/wholesale/batch/BatchOrchestrator.java#L37-L38]] | fact | pending | | |
| F-003 | 同じ external_key が連続しない複数のグループに現れた場合、そのすべてのグループが `orderImport.noncontiguous` で拒否される。 | [[source:wholesale-batch/src/main/java/jp/co/tsubame/wholesale/batch/BatchOrchestrator.java#L68-L80]] | fact | pending | | |
| F-004 | 同じ external_key が登録済みで、内容のハッシュも同じ場合、新しい注文は作らず、元の注文を返す（`orderImport.replayed`）。 | [[source:wholesale-batch/src/main/java/jp/co/tsubame/wholesale/batch/service/OrderImportService.java#L36-L37]] [[source:wholesale-batch/src/main/java/jp/co/tsubame/wholesale/batch/service/OrderImportService.java#L82-L88]] | fact | pending | | |
| F-005 | 注文日と希望納品日に使える日付の書式が何か、`Dates.parse` を読んでいないためわからない。次に `Dates.parse` の実装（wholesale-core の common 配下）を読む。 | [[source:wholesale-batch/src/main/java/jp/co/tsubame/wholesale/batch/OrderGroup.java#L90-L91]] | unknown | pending | | |
| F-006 | CSV の文字コード、BOM、引用符つき改行の扱いがわからない。次に `CsvReader` と `InputSnapshot` を読む。 | — | unknown | pending | | |
