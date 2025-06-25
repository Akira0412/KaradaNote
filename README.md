# からだノート (KaradaNote)

Health Connect の歩数データを多角的に可視化し、AI コーチで運動習慣を促進するAndroidアプリです。

## 🏗️ アーキテクチャ

- **React Native 0.79** + **TypeScript**
- **Expo Bare Workflow** (Android専用)
- **Health Connect** 連携 (歩数データ取得)
- **SQLite** ローカルデータベース
- **Google OAuth** 認証
- **OpenAI GPT-4o** AIコーチ機能

## 📱 画面構成

1. **Login** - Google OAuth ログイン
2. **Dashboard** - メトリクス表示 + グラフ
3. **Coach** - AIコーチとのチャット
4. **Settings** - プロフィール設定

## 🔧 開発環境セットアップ

### 1. 必要な環境

- Node.js 18+
- Android Studio
- Android SDK (API 29-34)
- EAS CLI

### 2. インストール

```bash
# 依存関係のインストール
npm install

# Android プロジェクトのビルド準備
npm run prebuild

# Dev Client のビルド (初回のみ)
eas build --profile development --platform android
```

### 3. 設定が必要な項目

#### Google OAuth 設定
`app/services/auth.ts` で以下を設定:
```typescript
webClientId: 'YOUR_WEB_CLIENT_ID.apps.googleusercontent.com',
iosClientId: 'YOUR_IOS_CLIENT_ID.apps.googleusercontent.com',
```

#### OpenAI API キー
AIコーチ機能を使用するには OpenAI API キーが必要です。

### 4. 開発サーバー起動

```bash
# Metro bundler 起動
npm start

# Android エミュレータで実行
npm run android
```

## 📊 データ構造

### Users テーブル
```sql
CREATE TABLE users (
  id TEXT PRIMARY KEY,
  googleUid TEXT UNIQUE NOT NULL,
  weightKg REAL,
  strideM REAL DEFAULT 0.7,
  createdAt TEXT NOT NULL
);
```

### StepRecords テーブル
```sql
CREATE TABLE step_records (
  date TEXT PRIMARY KEY,
  steps INTEGER NOT NULL,
  distanceM REAL NOT NULL,
  kcal REAL NOT NULL,
  fatKg REAL NOT NULL,
  userId TEXT NOT NULL
);
```

## 🔢 換算ロジック

- **距離**: `steps × stride (default: 0.7m)`
- **カロリー**: `steps × 0.05 kcal`
- **脂肪燃焼**: `kcal ÷ 7700 kg`

## 🚀 ビルド & リリース

### Development Build
```bash
eas build --profile development --platform android
```

### Production Build
```bash
eas build --profile production --platform android
```

### OTA Update
```bash
eas update --branch production
```

## 📋 コマンド一覧

| コマンド | 説明 |
|---------|------|
| `npm start` | Dev server 起動 |
| `npm run android` | Android で実行 |
| `npm run prebuild` | Native プロジェクト生成 |
| `npm run typecheck` | TypeScript チェック |
| `npm run lint` | ESLint 実行 |

## 🔐 権限

### Android 権限
- `android.permission.health.READ_STEPS` - Health Connect 歩数読み取り
- `android.permission.INTERNET` - ネットワーク通信
- `android.permission.ACCESS_NETWORK_STATE` - ネットワーク状態確認

## 🛠️ 技術スタック詳細

### Core
- React Native 0.79
- Expo SDK 53
- TypeScript 5.3

### State Management
- Zustand 4.4 (状態管理)
- React Query 5.17 (サーバー状態)

### UI/UX
- React Native Paper 5.12 (Material Design)
- NativeWind 2.0 (Tailwind CSS)
- Victory Native 36.8 (グラフ)

### Native Modules
- react-native-health-connect 1.7 (Health Connect)
- react-native-sqlite-storage 6.0 (SQLite)
- @react-native-google-signin/google-signin 13.1 (Google Auth)

### Security
- expo-secure-store 13.0 (トークン暗号化保存)

## 📞 サポート

開発に関する質問や問題報告は、プロジェクトのIssueページまでお願いします。

---

© 2025 KaradaNote - Health Connect 対応歩数可視化アプリ