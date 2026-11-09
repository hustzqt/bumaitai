# 埋汰 项目上下文 v1.2.2
> 本文件是 AI 协作的开机内存。每个新对话第一轮必须完整阅读本文件。
> 最后更新:【待补:日期】

## 产品定位
名称:埋汰(Maitai)
一句话:不嫌你埋汰,陪你从最小一件事开始。
形态:单文件 index.html(Tailwind CDN + 原生 JS,无后端)
用户群:抑郁/低能量人群

## 产品哲学(最高判据,凌驾于一切功能便利性)
1. 不诊断——不判断用户是什么病
2. 不施压——不要求变好,不说"你应该""加油"
3. 不强制——所有功能可跳过,情绪标签可选
4. 隐私是底线——数据全本地,AI不看,可一键抹除
5. 微光定义——"我做的",不是"我经历的"(对话内容不能存为微光)
6. 危机处理——全屏热线页是有意设计,只软化文案不弱化结构

## 技术架构
五层安全体系:
InputGuard(crisisKeywords负向断言/jailbreakPatterns)
→ OutputGuard(禁说教/限100字)
→ SafeResponseLibrary(27场景,含"对AI不满"元场景)
→ ConversationMonitor(负面≥3推热线/对话≥20推休息/2小时重置)
→ CrisisRouter + RollbackSwitch(危机全屏页/AI一键关闭)

关键兼容层:
- SafeStorage:localStorage 包装,Quick Look/隐私模式静默降级
- hashData:crypto.subtle 缺失时走 djb2 降级(非 HTTPS 也能用,HTTPS 硬需求仅在未来 PWA 的 sw)
- 100vh+100dvh 双写(iOS 聊天页高度)
- 开场视频:3秒超时降级文字版
- ⚠️ Tailwind CDN 是样式命脉,离线/微信内置直开会裸页面

## 导航系统(v1.2.2)
- VALID_PAGES 白名单 + NAV_HIGHLIGHT_MAP(card 归属 home 高亮)
- 底部导航无 inline onclick,data-page 属性
- click 单决策点(捕获阶段)+ 白名单前置守卫 + 220ms navLocked 冷却
- 特意不用 pointerdown 主路径(会抑制合成 click 且断键盘可达性)——勿重构
- .nav-item 含 touch-action: manipulation(消灭双击检测,iOS 9.3+ 支持)

## 数据层
STORAGE_KEYS:entries / settings / return_count / safety_events
丢烦恼方案B:只存次数+日期(maitai_throw_dates),不存内容
导出 JSON 含 throwCount/throwDates,不含烦恼内容

## 版本号四处同步
1. 设置页文案(含构建时间戳,格式 v1.2.2 · 0612)
2. RollbackSwitch.config.version
3. exportData 的 version 字段
4. 构建时间戳 BUILD_STAMP
⚠️ 时间戳随版本走,避免 v1.2.3 配旧时间戳

## 已裁决禁止项(勿擅自重开)
- ❌ 丢烦恼合并进聊聊(等朋友B使用数据)
- ❌ 对话存为微光(违背微光定义)
- ⏸ iOS PWA 导出下载问题(记入 v1.3.0)
- ⏸ 小窝元素密度(等真机渲染)
- ⏸ 隐私模式"不安装PWA"提示(待裁决清单)

## 待办
🔴 部署给朋友B真机测(先问B网络条件定托管路径);补视频资源;PWA基础三件套
🟡 等B反馈:v1.3.0是否接安全大模型;丢烦恼合并决策;小窝视觉
🟢 发布路径:Vercel小范围→ICP备案→阿里云ECS+HTTPS+备案号→APP备案+软著→PWA→华为/小米商店

## 协作机制(本项目专用)
- 一个任务卡 = 一个对话,验收打勾即归档
- 新对话首轮做交接考试(10题9对为线)
- 每次裁决立即落档 DECISIONS.md(我主动输出三行,用户粘贴)
- 关键方案(事件/安全/并发/加密)双源验证
- AI硬事实(版本号/API签名)发布前抽查官方文档
