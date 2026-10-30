# 前端检查与测试

启动器使用 Biome 统一检查和格式化 React 前端、三语消息以及相关构建与测试配置；Vitest 运行 TypeScript 单元测试。仓库根目录提供相同的 `lint`、`format` 和 `test:unit` 转发入口。

## 常用命令

在 `launcher/` 执行：

```sh
npm run lint
npm run lint:fix
npm run format:check
npm run format
npm run i18n:check
npm run i18n:audit
npm run test:unit
npm run test:unit:watch
npm test
```

`npm test` 先运行 Vitest，再运行既有 Node UI 契约套件。`test:ui-rewrite` 保留原有入口，浏览器与原生 Runtime 测试继续使用各自的专项命令。Vitest 的发现范围是 `tests/vitest/**/*.test.ts`，独立配置避免测试时触发 React Router 发布构建或重复发现 Node 测试。

`test:unit` 和应用类型检查先生成 Paraglide。工具类型检查同时覆盖 Vitest 配置与 TypeScript 测试。CI 的 UI 任务执行 Biome、词条检查和 Vitest 后，再执行原有行为套件。

## Biome 的边界

`biome.json` 列出 React 前端、消息文件和本轮迁移工具的检查范围。生成的 Paraglide、缓存、构建产物、旧测试夹具和游戏源码不进入前端格式化；原站 `public/styles.css` 保留原文。

检查使用当前推荐规则。已有生命周期所有者的 Hook 依赖与部分组合控件的语义规则保留为警告，供后续按组件审查；不会自动增删依赖数组而重启 Runtime、抽屉或事件订阅。装饰 SVG 标明 `aria-hidden`，可访问名称由所属按钮提供。

项目的设置层叠依赖 CSS `!important` 和既有选择器顺序，因此关闭对应两条样式偏好规则。受控输入编辑器保留 `autoFocus`，协议文本解析保留控制字符正则。已知的内联 SVG、启动脚本与样式在指定文件内保留原有 HTML 通道；帮助正文和译文仍经过内容清洗器。Runtime 检查的 `finally` 负责关闭本轮检查实例，其清理异常规则保留为警告。

## 词条与内容回归

Vitest 覆盖三语键集合、插值、语言隔离、已知错误翻译、连接状态、帮助正文结构与链接、玩法标签交互、富文本清洗、兼容帮助的 ES5 输出及硬编码审计。

`i18n:audit` 解析 TSX，检查 JSX 正文、可访问标签和中文字符串。品牌、原作标题、键位、单位、备案号及协议分类标识有明确豁免；用户输入与原生错误诊断按各自来源处理。已知协议错误在展示边界映射到 Paraglide，不改动校验字符串或联机协议。Runtime 就绪超时使用独立错误类型标识，中英日的导入恢复与错误展示顺序一致，不依赖提示文字。

远程浏览器验收：

```sh
npm run test:content-i18n:browser -- <候选地址>
npm run test:paraglide:browser -- <候选地址>
```

构建环境使用 Node.js 24，并通过锁文件安装依赖。
