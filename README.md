# 表情包联网搜索
作者：yutan123 · 版本：1.0.0 · TideBot SDK v1

## 安装
在 TideBot 插件管理页导入根目录包含 manifest.json 的 .tideplugin 安装包并启用，无需配置、账号或API Key。本插件ID为tidebot_meme_search，与旧的联网表情包搜索独立。建议关闭旧插件，避免机器人继续调用通用图片搜索。

## 来源
仅使用斗了个图https://www.dogetu.com的免费公开搜索页面和本站图片。没有付费API，不承诺站点长期可用。

## 使用
告诉机器人“发一张无语表情包”。机器人先调用search_memes，参数例如{"keyword":"无语","limit":6}；根据标题选择候选，再调用send_meme，使用返回的send_url和本条消息的唯一requestId。
关键词宜简短，例如无语、抱抱、开心、猫。角色加情绪无结果时换更短关键词，不自动用普通照片凑结果。标题匹配用于排序，不是视觉识别，不保证每张图片符合意图。
search_and_send_meme兼容入口有短期缓存时直接发送第一张候选；无缓存时只搜索，并返回ready_to_send及下一步参数，必须接着调用send_meme。搜索与慢下载分开调用，减少单次调用等待时间。

## 消息与资源
发送前查询同requestId的消息状态。下载媒体后通过resourceId发送并释放临时资源，resourceId不跨调用保存。发送异常返回validate/status/download/send阶段及error；投递结果未确认时必须复用原requestId重试，不能更换标识重复发送。仅sent为true才表示已进入聊天投递流程。
保留网站提供的GIF地址，不经过通用搜索缩略图转换。GIF能否动画显示取决于图片资源和宿主渲染。
搜索缓存最多12个关键词，当前运行实例中保留3分钟，重启或切换实例可能失效。无私有存储、不读取聊天历史、不保存聊天上下文。

## 权限与限制
仅声明network、chat.send、media.download，精确网络origin只有https://www.dogetu.com。媒体仅接受该站/dogetu/images/emo_d/目录的JPEG、PNG、GIF和WebP文件URL。宿主负责字节上限和格式检查，不跟随未声明域名跳转。普通请求不自动跟随重定向。验证码、页面变化、下载失败时明确报错，不绕过限制。
公开搜索无需付费或密钥，但图片版权属于原作者，不代表获得商用授权。

## 仓库与市场信息

- 仓库：https://github.com/qq3438802170-gif/tidebot_meme_search
- ID：`tidebot_meme_search`
- 作者：yutan123
- 版本：1.0.0
- 简介：从表情站搜索并发送表情包图片，免费免密钥搜索

仓库根目录的 `manifest.json` 是运行清单，`main.js` 是入口，`market.json` 是可复制的市场信息。

## 申请插件市场审核

将本目录中的文件直接上传到上述 GitHub 仓库根目录并提交。打开该提交详情，复制完整 40 位提交哈希。复制 `market.json` 的 JSON，补充 `commit` 字段，前往 https://github.com/yutan123/TideBot-plugins/issues/new 创建审核申请，标题填写 `[插件收录] tidebot_meme_search 1.0.0`，正文附上完整 JSON、权限用途及实际测试结果。

commit 必须在上传本次文件后获取，不能使用旧提交哈希，也不能将某次提交自身的哈希写回同一个提交。后续更新修改版本并重新申请审核，不会自动跟随仓库最新代码。

## 权利说明

附件未提供源代码许可证，当前不声明额外的开源授权。发布者应确认代码权利并在公开发布前自行选定适用许可证；参见 LICENSE。网站图片权利归原作者所有。
