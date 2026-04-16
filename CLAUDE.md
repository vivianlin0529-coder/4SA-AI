# CLAUDE.md — 全域性 Claude Code 設定

## 硬體環境（DGX Spark）

- **GPU**：NVIDIA GB10 Blackwell
- **記憶體**：128GB UMA 架構（CPU 與 GPU 共享，非獨立 VRAM）
- **CPU**：arm64 架構，20 核心（非 x86）
- **硬碟**：4TB SSD
- **SWAP**：64GB

> 由於 UMA 架構，**無法使用 `nvidia-smi` 查看記憶體使用量**，請改用 `free -h`、`htop` 等一般記憶體工具。

## 軟體環境

- **作業系統**：Ubuntu Linux 24.04（arm64 移植版）
- **CUDA**：13.0
- **系統驅動程式**：580（若有更新，請手動修正此檔案）

---

## 開發規範

### 套件與環境管理

| 情境 | 工具 |
|------|------|
| 系統服務 | 預設使用 `docker` / `podman`，儘量不原生安裝 |
| 軟體安裝 | 優先使用 `brew`（Linuxbrew），權限足夠時可用 `apt-get` |
| Python 環境 | 一律使用 `uv` 建立虛擬環境，**嚴禁直接使用 `pip` 安裝** |
| 執行 Python 腳本 | 優先考慮 `uvx`，不可行時才安裝套件 |
| Node.js | 一律透過 `nvm` 安裝，套件用 `npm`，執行時使用 `npx` |

**禁止使用原生 Python 環境（system Python）。**

### 專案管理

- 所有軟體專案一律 push 到 **GitHub**（預設為 public repo，repo name 以專案資料夾名稱為準）
- 每個專案必須依標準格式撰寫 `README.md`

### 語言與安全

- 所有對話及產出內容一律使用**繁體中文台灣用語**
- **嚴禁使用簡體字**，或繁體字但中國大陸用語
- **嚴禁在任何對話中直接貼出密碼、金鑰、Access Token、API Key 等機密資料**
- 機密資料存放於環境變數，或專案根目錄下的 `.env` 檔案，需要時讀取之

### 獲得最新資料

- 任何與 API、函式、程式庫相關的內容，**務必先上網搜尋再動手**
- 優先使用 **Context7**（`use context7`）取得最新文件
- 上網搜尋須根據以下五個維度進行校對查證：
  - **人**：相關開發者或社群
  - **事**：任務目標
  - **時**：今天的日期（請查詢當天日期）
  - **地**：中華民國台灣台北市
  - **物**：專案目標與技術棧

### 測試優先

- 所有專案必須進行測試，並保證測試覆蓋率
- 善用工具（browser use、computer use），務必進行 **end-to-end 測試**
- 深入理解 User Story，善加利用測試用例設計方法
- **單元測試**與**功能測試**必須完成

---

## CLI 工具使用規範

- **最高優先序**：使用內建 CLI 工具；能用 CLI 完成的工作，一定要用 CLI
- 開發前務必先確認所在資料夾：執行 `pwd`
- 需要存取更上層目錄時，**必須先詢問並獲得同意**
- **任何情況下嚴禁執行 `rm -rf /`**

### 可用 CLI 工具清單

| 分類 | 工具 |
|------|------|
| 套件管理 | `brew`（Linuxbrew）、`apt`（arm64）、`uv`（Python 虛擬環境）、`npm`（via nvm）、`snap` |
| 開發工具 | `git`、`gh`（GitHub CLI）、`node`（via nvm）、`python3`、`java`（OpenJDK）、`gcc`/`g++`、`cmake`、`make`、`hf`（HuggingFace CLI） |
| GPU / CUDA | `nvcc`（`nvidia-smi` 無法查看 UMA 記憶體，勿用於此目的） |
| 容器 | `docker`、`podman` |
| 多媒體與實用工具 | `ffmpeg`、`curl`、`wget`、`jq`、`yq`（via brew）、`rg`（ripgrep）、`fzf`、`eza`（via brew）、`htop`、`vim`、`tmux` |
