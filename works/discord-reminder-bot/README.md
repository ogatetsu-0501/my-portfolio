# Discord 連帯責任リマインダー Bot

通知を無視しがちな人でも行動できるよう、  
**軽い社会的プレッシャーを仕組みに組み込んだ Discord リマインダー Bot**です。

スヌーズを繰り返すと、  
サーバーメンバーの中からランダムに選ばれた **連帯責任者** にメンションが飛び、  
「自分だけの問題にしない」状態を作ります。

本 Bot は **現在も実運用中**です。

Discord App Directory  
https://discord.com/discovery/applications/1438173549008519339

---

## 背景

ADHD 傾向のある友人の課題をきっかけに開発しました。

一般的なリマインダーでは、

- 通知を無視しても実害がない  
- スヌーズし続けられる  
- 結果として行動につながらない  

という問題があります。

本 Bot では、  
**スヌーズを重ねるほど他人を巻き込む構造**を取り入れることで、  
行動に移るためのきっかけを作ります。

---

## 特徴と設計思想

### 段階的なプレッシャー設計

- 初期段階：本人のみに通知  
- 一定回数以上スヌーズすると：
  - サーバーメンバーの中から連帯責任者をランダムに選出
  - 毎回固定されないため、特定の人に負担が偏らない

---

### 人間関係を壊さないための配慮

- 巻き込みたくない人は除外指定可能  
- 連帯責任者は固定リストではなく、毎回動的に決定  
- 完了操作は **本人・連帯責任者のどちらでも可能**

完了権限を連帯責任者にも与えているのは、  
本人が反応できない状態で通知が鳴り続ける  
**運用事故を止めるための安全弁**としての設計です。

---

## アーキテクチャ概要

- イベント駆動（常駐プロセスなし）
- 定期トリガーで起動
- Firestore からタスクを取得
- 通知対象のみ処理して即終了

無料枠での永続稼働を前提にしています。

---

## データ設計（Firestore）

タスクは 1 ドキュメントで完結します。

    {
      "task_id": "task_001",
      "guild_id": "guild_xxx",
      "channel_id": "channel_xxx",
      "owner_id": "user_owner",
      "exclude_user_ids": ["user_excluded_1"],
      "snooze_count": 3,
      "is_completed": false,
      "timezone": "Asia/Tokyo",
      "quiet_hours": {
        "start": "23:00",
        "end": "07:00"
      }
    }

1 回の読み取りで、

- 通知するか  
- 誰に通知するか  
- quiet hours 判定  

まで判断できる構造です。

---

## 実装例（中核ロジック）

### 連帯責任者候補の抽出

    def pick_guarantor_candidates(
        guild_member_ids,
        owner_id,
        exclude_user_ids,
    ):
        exclude_set = set(exclude_user_ids)
        return [
            uid for uid in guild_member_ids
            if uid != owner_id and uid not in exclude_set
        ]

---

### スヌーズ回数に応じた通知先決定

    import random

    def pick_mentions(task, guild_member_ids, threshold=3):
        owner_id = task["owner_id"]
        snooze_count = int(task.get("snooze_count", 0))
        exclude_user_ids = task.get("exclude_user_ids", [])

        if snooze_count < threshold:
            return [owner_id]

        candidates = pick_guarantor_candidates(
            guild_member_ids,
            owner_id,
            exclude_user_ids,
        )

        if not candidates:
            return [owner_id]

        return [random.choice(candidates)]

---

### 完了操作の権限制御（本人・連帯責任者）

    def can_complete(task, user_id, guild_member_ids):
        owner_id = task["owner_id"]

        if user_id == owner_id:
            return True

        candidates = pick_guarantor_candidates(
            guild_member_ids,
            owner_id,
            task.get("exclude_user_ids", []),
        )
        return user_id in candidates

---

### quiet hours 判定（翌日またぎ対応）

    def is_quiet_time(now_hhmm, quiet_hours):
        start = quiet_hours["start"]
        end = quiet_hours["end"]

        if start <= end:
            return start <= now_hhmm <= end
        else:
            return now_hhmm >= start or now_hhmm <= end

---

## 使用技術

- Python  
- Discord API  
- Google Cloud Functions  
- Firestore  
- イベント駆動アーキテクチャ  

---

## 想定ユースケース

- 服薬・生活リズム管理  
- 勉強・作業開始のリマインド  
- 小規模グループでのタスク共有  

強制や監視ではなく、  
**行動のきっかけを作る用途**を想定しています。

---

## 設計で意識したこと

- やる気に頼らない  
- 罰ではなく「巻き込み」を使う  
- 責任が特定の人に偏らない  
- 困ったときに止められる逃げ道を用意する  

この Bot は、  
単なる通知ツールではなく  
**人の行動と人間関係のバランスを前提に設計した仕組み**です。

---

## 注意事項

- 本 Bot は医療・治療目的ではありません  
- ハラスメントや強制用途での利用は想定していません
