# Git 與工程協作

## 為什麼要學這個

寫 code 不是一個人寫一輩子的事。會 Git 與工程協作流程，比多會一個套件重要 10 倍。

入職前如果 Git 還只會 `git add . && git commit -m "update"`，這份文件請務必看完。

---

# 第一部分：Git 基礎

## 必須會的指令

```bash
# 看狀態
git status
git log --oneline --graph --decorate -20
git diff                  # 工作區 vs staged
git diff --staged         # staged vs 上一個 commit

# 開分支
git switch -c feature/login-api
git switch main
git switch -                # 回到上一個分支

# 加檔 / commit
git add path/to/file       # 不要習慣 git add .
git add -p                 # 互動式選擇 hunk（強烈推薦）
git commit -m "feat: add login api"

# 同步遠端
git fetch
git pull --rebase origin main
git push -u origin feature/login-api

# 修錯
git restore path/to/file           # 丟掉工作區變更
git restore --staged path/to/file  # 退出 staging
git commit --amend                 # 修上一個 commit（還沒 push 前）
```

## 必須懂的觀念

- **working directory / staging area / repository**：三層的差別
- **branch 只是指標**：分支不是資料夾，是個指向 commit 的 ref
- **rebase vs merge**：兩種把分支合在一起的方式，差別與時機
- **fast-forward vs merge commit**：什麼時候會產生 merge commit
- **HEAD、origin/main、main 的差異**

## 不要做的事

- `git add .` 把整個 repo 加進去 — 容易誤 commit `.env` 與 build artifact
- 在 main 上直接 commit — 永遠開 feature branch
- `git push --force` — 會把別人的 commit 蓋掉。要用也用 `--force-with-lease`
- `git pull` 不加 `--rebase` 又愛產生 merge commit
- commit message 寫 `update`、`fix bug`、`test`、`wip`

---

# 第二部分：Branch 策略

Stranity 採類 GitHub Flow：

- `main`：永遠可部署
- `feature/xxx`：開新功能
- `fix/xxx`：修 bug
- `refactor/xxx`：重構，不改行為
- `chore/xxx`：建置 / 設定 / 文件

範例：

- `feature/login-api`
- `feature/strategy-pagination`
- `fix/jwt-expiry-bug`
- `refactor/strategy-service`
- `chore/upgrade-gorm-v2`

## 規則

- 每個 feature / fix 一條分支
- 分支生命週期短：開出來 → 1 至 5 天內 merge → 刪掉
- 分支基於最新的 `main`：開分支前先 `git pull --rebase origin main`

---

# 第三部分：Commit Message 規範

採 Conventional Commits 簡化版。

## 格式

```
<type>: <短描述>

<可選的詳細說明，跨多行>
<說「為什麼」比說「做了什麼」重要>
```

## type 對照表

| type | 用途 |
|------|------|
| `feat` | 新功能 |
| `fix` | bug 修正 |
| `refactor` | 不改行為的重構 |
| `docs` | 只改文件 |
| `test` | 加 / 修測試 |
| `chore` | 建置、設定、依賴升級 |
| `perf` | 效能改善 |
| `style` | 排版、空白、格式（不改邏輯） |

## 好範例

```
feat: add JWT refresh token rotation

Previous implementation reused the same refresh token after refresh,
making token theft replayable. This change issues a new refresh
token on every refresh and invalidates the previous one.
```

```
fix: prevent N+1 query on strategy list

Preload Symbol relation in repository.ListStrategies so listing
1000 strategies makes 1 query instead of 1001.
```

```
refactor: extract strategy validation into service layer
```

## 壞範例

```
update                        ❌ 什麼都沒說
fix bug                       ❌ 什麼 bug？
WIP                           ❌ 不該 push 上來
asdfasdf                      ❌ 認真寫
add code                      ❌ 廢話
1                             ❌ 拜託
```

## 一個 commit 一件事

- 不要一個 commit 同時改 5 個無關功能
- 不要把「改完 A、發現 B 也壞了、順手修 B、又動到 C 的格式」全包在一個 commit
- 不確定怎麼拆？用 `git add -p` 互動式選 hunk

---

# 第四部分：Pull Request 流程

## 標準流程

1. 從最新的 main 開分支
   ```bash
   git switch main && git pull --rebase origin main
   git switch -c feature/strategy-export
   ```
2. 寫 code，分次 commit
3. 推上去
   ```bash
   git push -u origin feature/strategy-export
   ```
4. 開 PR，填好描述（見下方）
5. 等 CI 通過、reviewer 看完
6. 根據 review 改、push 新 commit（**不要 force push 在 review 中**）
7. approve 後 squash 或 rebase merge

## PR 描述範本

```markdown
## 修改內容
- 加上 GET /api/v1/strategies/export 匯出 CSV
- 新增 export_logs 表記錄誰在何時匯出哪些資料

## 修改原因
PM Issue #123：客服需要批次匯出策略資料給審計

## 測試方式
1. `make seed-strategies`
2. `curl -H "Authorization: Bearer $TOKEN" http://localhost:8080/api/v1/strategies/export -o out.csv`
3. 打開 out.csv 確認欄位、UTF-8 BOM 正確

## Screenshot（前端）
（貼圖）

## Checklist
- [x] 加 unit test
- [x] 跑過 lint / format
- [x] 本機測試通過
- [ ] 需要 migration？無
- [ ] 影響其他模組？只動 strategy 模組
```

## PR 大小

- 理想：**< 400 行變更**
- 上限：**< 800 行**
- 超過就拆 — reviewer 看不完，bug 也藏在裡面

## PR 標題

用和 commit message 同樣的格式：

```
feat: add strategy CSV export endpoint
fix: handle nil pointer in user middleware
refactor: split strategy service into read/write
```

---

# 第五部分：Code Review

## 被 review 的態度

- review 是合作不是攻擊
- 不同意可以討論，但要說「我為什麼這樣寫」，不是「我就喜歡這樣」
- reviewer 沒抓到的問題不代表沒有，最終是 author 負責
- 同樣的回饋被講第二次：自己整理一份個人 lessons learned

## 你 review 別人的態度（入職第二、三月後會輪到）

- 看到問題用「建議」「也許」「考慮」這種語氣，不要命令
- 區分必要修改與個人偏好：
  - **`must`**：bug、安全問題、違反約定
  - **`should`**：可維護性、清晰度
  - **`nit`** / **`optional`**：個人偏好、可省略
- 講「為什麼」，不只講「怎麼改」
- 讚美值得讚美的 — 不只挑毛病

## 常見 review 重點

- 命名是否清楚
- 錯誤是否被吞掉
- 邊界條件（空陣列、nil、極限值）
- 並發安全（goroutine、race）
- N+1 query
- 沒包 transaction 的多步驟寫入
- 前端 effect 依賴漏掉
- 沒加 loading / error 處理
- 沒考慮 401 / 403 的使用者流程

---

# 第六部分：解決 conflict

## 不要慌

conflict 不是 bug，是兩個人改了同一段。處理流程：

```bash
git switch main && git pull --rebase origin main
git switch feature/my-branch
git rebase main
# 出現 conflict
# 開 editor 看 <<<<<<< ... ======= ... >>>>>>> 區塊
# 決定保留哪一邊（或合併）
# 確認 file 沒問題
git add path/to/file
git rebase --continue
# 全部 file 都處理完就會結束
git push --force-with-lease
```

## 不確定怎麼處理？

- 先用 `git stash` 保留現在工作
- 跟原 commit 的作者確認他的意圖
- **絕對不要** 隨便挑一邊保留就 commit — conflict 多半代表兩邊都有變更需求

---

# 第七部分：Linux 基礎

每天會用到：

```bash
# 檔案
ls -la
cd path
pwd
cp src dst
mv src dst
rm file               # rm -rf 慎用
mkdir -p path/to/dir

# 找東西
find . -name "*.go"
grep -rn "TODO" .
rg "TODO"             # ripgrep，更快

# 看檔
cat file
less file             # q 退出
head -20 file
tail -f file          # 看 log 即時更新

# 看程式
ps aux | grep go
lsof -i :8080         # 哪個 process 用 8080
kill <pid>
kill -9 <pid>         # 強制（少用）

# 壓縮
tar -czf out.tar.gz dir/
tar -xzf out.tar.gz

# 權限
chmod +x script.sh
chown user:group file
```

## 不可不知

- **絕對路徑 vs 相對路徑**
- **環境變數**：`echo $PATH`、`export FOO=bar`
- **pipe 與 redirect**：`cmd1 | cmd2`、`cmd > file.log`、`cmd 2>&1`
- **背景執行**：`cmd &`、`Ctrl+Z` 暫停、`bg` / `fg`
- **vim 基本**：能在 server 上改設定檔就好（`i` 進編輯、`Esc` 退出、`:wq` 存檔離開、`:q!` 不存離開）

---

# 第八部分：Docker 基礎

## 為什麼必懂

Stranity 所有服務都跑在 Docker。本機開發、staging、production 都是。

## 必須會

```bash
# image
docker pull postgres:16
docker images
docker rmi <image>

# container
docker run -d --name pg -p 5432:5432 -e POSTGRES_PASSWORD=pass postgres:16
docker ps
docker ps -a              # 含已停止
docker logs <container>
docker logs -f <container>
docker exec -it <container> bash
docker stop <container>
docker rm <container>

# 清理
docker system df          # 看磁碟用量
docker system prune       # 清沒用的（小心）
```

## Docker Compose

```bash
docker compose up -d            # 起全部
docker compose up postgres      # 只起特定 service
docker compose logs -f api
docker compose down             # 停全部
docker compose down -v          # 連 volume 一起砍（資料會掉）
docker compose ps
docker compose exec api bash
```

## Dockerfile 概念

至少要看得懂並能改：

- `FROM`、`WORKDIR`、`COPY`、`RUN`、`CMD`、`ENV`、`EXPOSE`
- multi-stage build：第一階段 build、第二階段只放執行檔（image 變小）

---

# 第九部分：日常工作節奏

入職後典型一天：

1. **早上**：拉最新 `main`、看 PR 通知、回 review 留言
2. **接任務**：理解需求 → 在 issue / Notion 寫下你的理解 → 確認後再動
3. **動工**：開 branch、寫 code、本機跑通
4. **頻繁 commit**：寫一段、跑通一段、commit 一段
5. **每天至少一次 push**：避免機器壞掉資料全失
6. **完成**：跑 lint / test / 自我 code review → 開 PR
7. **下班前**：留可以接手的狀態（commit 完、push 完、有筆記）

---

# 入職前 self check

- [ ] 能不查資料開分支、切換、commit、push
- [ ] 能用 `git add -p` 互動式選 hunk
- [ ] 能解 conflict 不慌
- [ ] commit message 不會出現 `update` / `fix bug`
- [ ] 看得懂 `docker compose up -d` 在做什麼
- [ ] 知道為什麼 `.env` 不該 commit、`.gitignore` 怎麼寫
