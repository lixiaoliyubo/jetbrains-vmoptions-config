# JetBrains IDE JVM 调优配置参考

IntelliJ IDEA 与 DataGrip 的 `*.vmoptions` 配置参考，**只含合法 JVM 运行参数**（内存、垃圾回收、代码缓存等），用于优化 IDE 在大项目下的性能与稳定性。

## ⚠️ 关于清理说明

原始 `idea64.exe.vmoptions` / `datagrip64.exe.vmoptions` 中曾包含以下两类行，**本仓库已全部移除，不予收录**：

- `-javaagent:<path>/ja-netfilter.jar` —— 第三方 agent 注入，属于授权绕过补丁；
- `--add-opens=java.base/jdk.internal.org.objectweb.asm=ALL-UNNAMED`（及 `.tree`）—— 配合上述注入的模块开放参数。

理由：这类配置用于绕过软件授权验证，违反 JetBrains 许可协议，且第三方 agent 存在安全风险。本仓库仅提供合法调优参数。

> 需要合法授权：JetBrains 提供开源项目免费 License、学生/教育优惠、订阅三种途径，请通过官方渠道获取。

## 参数说明

| 参数 | 含义 | 建议 |
| --- | --- | --- |
| `-Xms128m` | JVM 初始堆内存 | 保持默认即可 |
| `-Xmx2048m` | JVM 最大堆内存 | 设为物理内存的 1/4，8G 内存机器建议 2G |
| `-XX:ReservedCodeCacheSize=512m` | JIT 代码缓存 | 大项目可调至 512m 以上 |
| `-XX:+UseG1GC` | 启用 G1 垃圾回收器 | 大项目更平稳 |
| `-XX:SoftRefLRUPolicyMSPerMB=50` | 软引用存活策略 | 默认值 |
| `-XX:+HeapDumpOnOutOfMemoryError` | OOM 时生成堆转储 | 便于排查内存问题 |
| `-Dsun.java2d.metal=true` | macOS Metal 渲染 | 按系统实际情况调整 |

## 使用方法

**推荐（用户级配置，升级不被覆盖）**：IDE 菜单 `Help → Edit Custom VM Options`，按需要复制本仓库参数。

**不推荐（安装目录配置，升级会被覆盖）**：直接编辑 IDE 安装目录 `bin/*.exe.vmoptions`。

修改后需重启 IDE 生效。

## 文件

- `idea64.exe.vmoptions` —— IntelliJ IDEA 配置（清理版）
- `datagrip64.exe.vmoptions` —— DataGrip 配置（清理版）
