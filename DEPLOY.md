# 部署与维护指南

基于 [Quartz](https://quartz.jzhao.xyz/)（v4.5.2）搭建的个人博客"半熟"，内容来自 Obsidian 笔记库 `knowledge/cs`。

---

## 一、首次部署到 GitHub Pages

### 1. 建一个空的 GitHub 仓库

去 GitHub 新建一个仓库（比如叫 `Nan-notes`），**不要**勾选 "Add a README file"，保持空仓库。

### 2. 打开终端，把这个文件夹推上去

```bash
cd "/Users/nanwang1/Desktop/knowledge/blog"

# 如果提示 "Another git process seems to be running" / lock 文件报错，先清一下：
rm -f .git/*.lock .git/refs/heads/*.lock

git add -A
git commit -m "docs: add deployment guide"
git branch -M main
git remote add origin https://github.com/你的用户名/你的仓库名.git
git push -u origin main
```

推送时会要求登录 GitHub（用户名 + Personal Access Token，不是密码；没有 token 的话去 GitHub 头像 → Settings → Developer settings → Personal access tokens 生成一个）。

### 3. 开启 GitHub Pages

进仓库的 **Settings → Pages**，"Build and deployment" 的 Source 选 **GitHub Actions**（不是 "Deploy from a branch"）。保存后，仓库的 **Actions** 标签页会自动开始跑 `Deploy Quartz site to GitHub Pages` 这个工作流，跑完（一两分钟）网站就上线了。

网址是：`https://你的用户名.github.io/你的仓库名/`

### 4. 把真实网址写回配置

打开 `quartz.config.ts`，把这一行：

```ts
baseUrl: "REPLACE_ME.github.io/REPLACE_ME",
```

改成你实际的地址，例如：

```ts
baseUrl: "yourname.github.io/Nan-notes",
```

改完提交推送一次（`git add -A && git commit -m "update baseUrl" && git push`），这个字段只影响 RSS/分享链接等细节，不改也不影响网站本身能不能访问。

---

## 二、以后怎么更新笔记

**方式一（推荐，简单）：** 每次想更新博客时，把 Obsidian 库（`knowledge/cs`）里改动过的 `.md` 文件和图片，重新复制/覆盖到本项目的 `content/` 对应目录下，然后：

```bash
cd "/Users/nanwang1/Desktop/knowledge/blog"
git add -A
git commit -m "更新笔记"
git push
```

推送后 GitHub Actions 会自动重新构建部署，几分钟后网站更新。

**方式二（进阶）：** 把 `content/` 换成指向 Obsidian 库的符号链接（symlink），这样 Obsidian 里改完不用手动复制。如果想用这种方式，可以随时让我帮你设置。

**没有放进来的内容：** `cs61a_practice`（作业/lab/project 解答代码）出于学术诚信考虑没有发布，如果以后想加入，告诉我。

---

## 三、加入 VSCode 里的笔记

VSCode 那部分笔记还没导入。把文件/文件夹路径告诉我，我可以帮你整理格式并放进 `content/` 对应目录。

---

## 四、本地预览（可选）

如果想在推送前先在自己电脑上看效果：

```bash
cd "/Users/nanwang1/Desktop/knowledge/blog"
npm install
npx quartz build --serve
```

然后浏览器打开 `http://localhost:8080`。

---

## 五、常用小改动

- **改站点标题/语言/主题颜色**：`quartz.config.ts`
- **改首页文字**：`content/index.md`
- **加新笔记**：直接把 `.md` 文件放进 `content/` 对应文件夹，文件里的 Obsidian 双向链接 `[[...]]`、图片、标签都会自动识别
