# MCP Create Server

動的MCPサーバー管理サービスは、Model Context Protocol (MCP) サーバーを動的に作成、実行、管理するためのツールです。このサービス自体がMCPサーバーとして機能し、他のMCPサーバーを子プロセスとして起動・管理することで、フレキシブルなMCPエコシステムを実現します。

## 主要機能

- MCPサーバーコードの動的な作成と実行
- 複数のプログラミング言語（TypeScript、JavaScript、Python）のサポート
- 子MCPサーバーのツール実行
- サーバーコードの更新と再起動
- 不要なサーバーの削除

## インストール方法

```bash
# リポジトリをクローン
git clone https://example.com/mcp-create.git
cd mcp-create

# 依存関係インストール
npm install

# ビルド
npm run build

# 実行
npm start
```

## Docker での実行

```bash
# Docker イメージのビルド
docker build -t mcp-create .

# Docker コンテナの実行
docker run -it mcp-create
```

## Claude Desktop との連携

Claude Desktop 構成ファイル (`claude_desktop_config.json`) に以下を追加:

```json
{
  "mcpServers": {
    "mcp-create": {
      "command": "docker",
      "args": ["run", "-i", "--rm", "mcp-create"]
    }
  }
}
```

## 提供ツール一覧

| ツール名 | 説明 | 入力パラメータ | 出力 |
|---------|-----|--------------|-----|

| create-server-from-template | テンプレートからMCPサーバーを作成 | language: string | { serverId: string, message: string } |
| execute-tool | サーバー上のツールを実行 | serverId: string<br>toolName: string<br>args: object | ツールの実行結果 |
| get-server-tools | サーバーのツール一覧を取得 | serverId: string | { tools: ToolDefinition[] } |

| delete-server | サーバーを削除 | serverId: string | { success: boolean, message: string } |
| list-servers | 実行中のサーバー一覧を取得 | なし | { servers: string[] } |

## 使用例

### 新規サーバー作成


### ツール実行

```json
{
  "name": "execute-tool",
  "arguments": {
    "serverId": "ba7c9a4f-6ba8-4cad-8ec8-a41a08c19fac",
    "toolName": "echo",
    "args": {
      "message": "Hello, dynamic MCP server!"
    }
  }
}
```

## 技術仕様

- Node.js 18以上
- TypeScript
- 依存パッケージ:
  - @modelcontextprotocol/sdk: MCPクライアント・サーバー実装
  - child_process (Node.js組込み): 子プロセス管理
  - fs/promises (Node.js組込み): ファイル操作
  - uuid: 一意なサーバーID生成

## セキュリティ考慮事項

- **コード実行の制限:** 任意のコードを実行するため、サンドボックス化を検討する
- **リソース制限:** メモリ、CPU使用量、ファイル数などに上限を設ける
- **プロセス監視:** ゾンビプロセスや暴走プロセスの監視と強制終了
- **パス検証:** ファイルパスに対する適切な検証を行い、ディレクトリトラバーサル攻撃を防止

## ライセンス

MIT
