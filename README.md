# 专站表情包搜索
作者：yutan123 · 版本：1.0.0 · TideBot SDK v1

## 安装
在TideBot插件管理页导入本ZIP并启用，无需配置、账号或API Key。本插件ID为tidebot_meme_search，与旧的联网表情包搜索独立。建议关闭旧插件，避免机器人继续调用通用图片搜索。

## 来源
仅使用斗了个图https://www.dogetu.com的免费公开搜索页面和本站图片。没有付费API，不承诺站点长期可用。

## 使用
告诉机器人“发一张无语表情包”。机器人先调用search_memes，参数例如{"keyword":"无语","limit":6}；根据标题选择候选，再调用send_meme，使用返回的send_url和本条消息的唯一requestId。
关键词宜简短，例如无语、抱抱、开心、猫。角色加情绪无结果时换更短关键词，不自动用普通照片凑结果。标题匹配用于排序，不是视觉识别，不保证每张图片符合意图。
search_and_send_meme兼容入口有短期缓存时直接发送第一张候选；无缓存时只搜索，并返回ready_to_send及下一步参数，必须接着调用send_meme。搜索与慢下载分开调用，避免超过宿主15秒预算。

## 消息与资源
发送前查询同requestId的消息状态。下载媒体后通过resourceId发送并释放临时资源，resourceId不跨调用保存。发送异常返回validate/status/download/send阶段及error；投递结果未确认时必须复用原requestId重试，不能更换标识重复发送。仅sent为true才表示已进入聊天投递流程。
保留网站提供的GIF地址，不经过通用搜索缩略图转换。GIF能否动画显示取决于图片资源和宿主渲染。
搜索缓存最多12个关键词，当前运行实例中保留3分钟，重启或切换实例可能失效。无私有存储、不读取聊天历史、不保存聊天上下文。

## 权限与限制
仅声明network、chat.send、media.download，精确网络origin只有https://www.dogetu.com。媒体仅接受该站/dogetu/images/emo_d/目录的JPEG、PNG、GIF和WebP文件URL。宿主负责字节上限和格式检查，不跟随未声明域名跳转。普通请求不自动跟随重定向。验证码、页面变化、下载失败时明确报错，不绕过限制。
公开搜索无需付费或密钥，但图片版权属于原作者，不代表获得商用授权。
