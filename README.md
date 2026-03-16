# homepage

Davinci 的个人项目主页。

这是一个静态单页网站，当前包含：

- 首页主视觉
- GitHub 跳转入口
- 下滑切换的子项目展示区

## Project Integration Format

后续每个子项目独立维护、独立部署，`homepage` 只通过统一的项目数据格式和封面图接入。

推荐目录规范：

- 项目源码目录：`/home/admin/projects/<slug>/`
- 项目部署目录：`/home/admin/projects/<slug>/`
- 线上访问路径：`/projects/<slug>/`

主页项目数据示例见：

`site/projects.json`

当前首页项目展示区已经改成数据驱动，展示界面只读取：

- `site/projects.json`
- `site/assets/projects/` 下的封面图

建议每个项目至少提供以下字段：

- `slug`：项目唯一标识，用于部署路径
- `name`：项目名称
- `subtitle`：项目副标题
- `description`：主页展示用简短说明
- `tags`：项目标签数组
- `cover`：主页默认展示图，建议使用 `homepage` 自己维护的封面路径
- `ratio`：展示图比例，当前支持 `wide`、`standard`、`tall`
- `gallery`：项目展示图数组
- `url`：项目线上访问地址
- `repo`：项目 GitHub 仓库地址
- `status`：项目状态，例如 `draft` 或 `online`

当前首页只渲染 `status: "online"` 的项目。

推荐图片资源规范：

- `cover` 建议放在 `homepage/site/assets/projects/`
- `gallery` 可以指向项目自己的资源路径
- 推荐封面命名：`<slug>-cover.<ext>`
- 推荐展示图命名：
  - `<slug>-cover.<ext>`
  - `<slug>-detail-1.<ext>`
  - `<slug>-detail-2.<ext>`
- 推荐展示图比例：
  - `wide`：适合横向网页截图
  - `standard`：适合常规截图
  - `tall`：适合竖版页面或海报
- 格式优先 `png`、`jpg` 或 `webp`

详细图片规范：

- `cover`
  - 用于 homepage 首页项目卡主图
  - 建议由 homepage 自己维护，不直接依赖项目目录里的图片
  - 推荐尺寸：
    - `wide`：`1600x900`、`1920x1080`、`2400x1200`
    - `standard`：`1200x900`、`1600x1200`
    - `tall`：`1200x1500`、`1440x1800`
- `wide`
  - 推荐比例：`16:9` 到 `2.1:1`
  - 适合横向网页截图、首页首屏、控制台类界面
- `standard`
  - 推荐比例：`4:3`
  - 适合普通页面截图、说明页、信息页
- `tall`
  - 推荐比例：`4:5`
  - 适合竖向页面、海报、移动端界面
- 文件大小
  - 单张封面图建议控制在 `3MB` 内
  - 优先保证清晰，再压缩到合理体积
- 图片内容
  - 尽量保留页面主体，不要截太碎
  - 封面图优先展示最能代表项目的一屏
  - 避免把大段说明文字放进封面图里，主页文案会补充说明
- 背景和留白
  - 如果截图本身边缘太杂，建议导出时保留适量留白
  - 对于横图，优先选择视觉重心明确的一屏
- 字体与乱码
  - 如果服务器截图容易出现中文乱码，优先在本地导出封面图后再放入 homepage
  - 不要依赖运行时临时加载字体来生成最终封面

推荐接入流程：

1. 将子项目 clone 到独立目录
2. 让站点路由直接指向该项目目录
3. 在 `homepage/site/assets/projects/` 中准备封面图
4. 在 `site/projects.json` 中新增一条项目记录
5. 在主页中读取并展示对应项目数据

示例：

```json
[
  {
    "slug": "choose-your-city",
    "name": "Choose Your City",
    "subtitle": "城市选择交互页面",
    "description": "通过问答匹配城市风格的交互网页。",
    "tags": ["网页", "交互", "实验"],
    "cover": "/assets/projects/choose-your-city-cover.png",
    "ratio": "wide",
    "gallery": [
      "/projects/choose-your-city/assets/cover.png",
      "/projects/choose-your-city/assets/full.png"
    ],
    "url": "/projects/choose-your-city/",
    "repo": "https://github.com/Davinccci/ChooseYourCity",
    "status": "online"
  }
]
```

说明：

- `online`：会在 homepage 首页展示
- `draft`：保留在数据里，但当前不会在首页展示
- `archived`：保留存档用途，当前不会在首页展示

## Decoupling Rule

- `homepage` 只维护主页页面、项目元数据和主页使用的封面图
- 每个项目在自己的目录里单独开发和维护
- homepage 引用项目时，只修改 `homepage` 目录下的内容
- 项目本身不需要为了接入 homepage 再复制一份到主页仓库
- 首页展示区不再手写项目卡片，统一由 `projects.json` 渲染

## Project Structure

`site/index.html`

- 网站主页面
- 包含页面结构、样式、滚动切换脚本和项目数据渲染逻辑

`site/projects.json`

- 子项目接入数据示例
- 作为主页项目展示的数据源

`site/assets/projects/`

- homepage 自己维护的项目封面图目录
- 首页项目卡只依赖这里的封面图

`Caddyfile`

- 当前站点的 Caddy 配置文件
- `/projects/*` 会直接映射到 `/home/admin/projects/*`

## Notes

- 项目已初始化为 git 仓库，默认分支为 `main`
- 运行时目录 `caddy_config/` 和 `caddy_data/` 已被 `.gitignore` 忽略

## Repository

GitHub: <https://github.com/Davinccci/homepage>
