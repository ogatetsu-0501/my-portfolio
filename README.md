# MY-PORTFOLIO

業務改善ツール、自動化スクリプト、Bot、シミュレーターなど、  
**実務課題・個人課題を解決するために開発したプロジェクトのポートフォリオ**です。

「現場で本当に使えるか」「非エンジニアでも運用できるか」を重視し、  
小規模でも **設計・安全性・継続運用** まで考えた実装を行っています。

---

## ポートフォリオの構成

```text
MY-PORTFOLIO
├─ docs/
│  ├─ about.md        # 自己紹介・価値観
│  └─ skills.md       # 技術スタック
│
├─ works/             # 各プロジェクト
│  ├─ accounting-migration/
│  ├─ attendance-extension/
│  ├─ avatar-bazzar-assistant/
│  ├─ cardgame-simulator/
│  ├─ discord-reminder-bot/
│  ├─ game-growth-simulator/
│  ├─ image-recognition-tool/
│  └─ vba-product-tool/
│
└─ README.md          # このファイル

## プロジェクト一覧

### 業務改善・自動化

- **会計システム移行自動化ツール**  
  Python / Selenium  
  → 大量仕訳データを安全に自動移行

- **勤怠入力補助 Chrome 拡張**  
  JavaScript / Chrome API  
  → 入力漏れ防止・入力効率向上

- **画像認識＋自動操作ツール**  
  Python / OpenCV / pytesseract  
  → 画面認識による検査・確認作業の自動化

- **Excel VBA 商品・シリンダーデータ管理ツール**  
  Excel / VBA  
  → 直入力文化からの脱却、長期運用を意識した設計

---

### Bot・クラウド・データ活用

- **Discord 連帯責任リマインダー Bot**  
  Python / Firestore / GCP  
  → 行動促進を目的としたUX設計

- **ゲーム内マーケット分析・可視化ツール**  
  Node.js / GCP / JavaScript  
  → データ収集・分析・可視化までの一貫実装

---

### シミュレーター・ロジック重視ツール

- **ゲーム育成シミュレーター（静的攻略ツール）**  
  HTML / JavaScript  
  → 長期依存・確率計算を扱う軽量Webツール

- **LoveCard（カードゲームシミュレーター）**  
  Python / Pygame  
  → ゲーム進行自動化＋確率検証＋自己更新設計

---

## 設計方針・特徴

- **非エンジニア利用を前提にしたUI・配布設計**
- **安全性・再現性を重視した自動化**
- **小規模でも壊れにくい構造**
- **実データ・実運用を通じた改善**

「とりあえず動く」ではなく、  
**現場で使い続けられること**を重視しています。

---

## 詳細について

- 各プロジェクトの詳細は `works/各フォルダ/README.md` を参照してください。
- 自己紹介・スキルセットは `docs/` 配下にまとめています。
