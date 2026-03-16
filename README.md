# homepage

Davinci 的个人项目主页。

这是一个静态单页网站，当前包含：

- 首页主视觉
- GitHub 跳转入口
- 下滑切换的子项目展示区

## Project Integration Format

后续每个子项目建议独立维护，并通过统一的项目数据格式接入主页。

推荐目录规范：

- 项目源码目录：`/home/admin/projects/<slug>/`
- 静态部署目录：`/srv/projects/<slug>/`
- 线上访问路径：`/projects/<slug>/`

主页项目数据示例见：

`site/projects.json`

建议每个项目至少提供以下字段：

- `slug`：项目唯一标识，用于部署路径
- `name`：项目名称
- `subtitle`：项目副标题
- `description`：主页展示用简短说明
- `tags`：项目标签数组
- `cover`：主页默认展示图
- `gallery`：项目展示图数组
- `url`：项目线上访问地址
- `repo`：项目 GitHub 仓库地址
- `status`：项目状态，例如 `draft` 或 `online`

推荐图片资源规范：

- `cover` 和 `gallery` 都使用静态可访问路径
- 图片优先放在 `/projects/<slug>/assets/` 下
- 比例尽量统一，建议 `16:10` 或 `4:3`
- 格式优先 `jpg` 或 `webp`

推荐接入流程：

1. 将子项目 clone 到独立目录
2. 将可部署的静态文件发布到 `/srv/projects/<slug>/`
3. 为项目准备封面图和展示图
4. 在 `site/projects.json` 中新增一条项目记录
5. 在主页中读取并展示对应项目数据

## Project Structure

`site/index.html`

- 网站主页面
- 包含页面结构、样式和滚动切换脚本

`site/projects.json`

- 子项目接入数据示例
- 后续可作为主页项目展示的数据源

`Caddyfile`

- 当前站点的 Caddy 配置文件

## Notes

- 项目已初始化为 git 仓库，默认分支为 `main`
- 运行时目录 `caddy_config/` 和 `caddy_data/` 已被 `.gitignore` 忽略

## Repository

GitHub: <https://github.com/Davinccci/homepage>
