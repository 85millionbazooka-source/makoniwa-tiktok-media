# makoniwa-tiktok-media

TikTok に予約投稿する動画を、Metricool に渡すまでの間だけ置く公開の場所。
オーナーの承認（2026-10-12）：この置き場を使うこと、TikTok に載ったのを確かめたら毎回消すこと。1本ごとの承認は要らない。

## 流れ
1. **置く**：作業用のブランチ `claude/project-thread-kamuqb` に `v/<名前>.mp4` を普通のファイルとして commit して push する。`main` には動画を入れない（README だけ）。
2. **raw の URL**：`https://raw.githubusercontent.com/85millionbazooka-source/makoniwa-tiktok-media/claude/project-thread-kamuqb/v/<名前>.mp4`
   - 200 が返ることを `curl -s -o /dev/null -w "%{http_code}" <URL>` で確かめる。
3. **Metricool**：Claude の Metricool コネクタで、この URL を動画として予約する（TikTok はタイトル必須）。公開の投稿はオーナーの OK が出た物だけ。
4. **取り込みの確認**：Metricool の予約に動画が入ったこと、TikTok に載ったことを確かめる。
5. **消す**：ブランチを README だけの commit に戻して強制 push する。
   ```
   git fetch origin claude/project-thread-kamuqb
   git reset --hard <README だけの commit>
   git push --force-with-lease=claude/project-thread-kamuqb:<消す前の commit> origin HEAD:claude/project-thread-kamuqb
   ```
   クラウドのスレッドから止められずに通る（2026-10-13 に試験済み）。

## 大きさの上限
- GitHub：1ファイル 100MB まで（50MB を超えると警告）。
- Metricool の TikTok 動画：MP4 か MOV・500MB まで・3秒〜10分・540px 以上。

## 分かっている事と未確認の事
- raw の URL は転送なしの直リンクで 200、`content-type: application/octet-stream`（2026-10-13 確認）。
- 試し投稿（2026-10-12）で Metricool が取り込めたリリースの添付の URL も octet-stream だった。なので取り込めるはずだが、**raw の URL での取り込みは未確認**。次の本当の投稿で確かめる。
- 強制 push で外しても、古い commit は commit の番号を知っていれば読め、約5分は CDN に残る。宣伝用で公開前提の動画だけを置くので、それでよいと判断した。
- リリース（クラウドからは作成・削除とも 403、PC からは削除が安全判定で止まる）は使わない。古いリリース `trial-001` が残っている（PC かオーナーが消せる）。

## 取り込めなかった時の代わり
1. **GitHub Pages**：このリポジトリで Pages を有効にすると `video/mp4` で配られる（`https://85millionbazooka-source.github.io/makoniwa-tiktok-media/v/<名前>.mp4`）。有効にする設定はオーナーか PC 側で1回だけ。置く・消すの流れは同じ。
2. **リリースの添付**：試し投稿で取り込めた実績がある。作成は PC 側で行い、消すのは PC の安全判定で止まるため、オーナーの一言が要る。
3. **Metricool の画面から手で上げる**：オーナーの手が毎回要るので最後の手段。
