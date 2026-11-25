# 火柴人视频 stickman-video-director（菲菲适配，2026-11-17 实测）

源：@CrazyKaomei 帖（2026-11-16）+ 仓库 github.com/kaomei/stickman-video-director（6,635 star，MIT）。
上游完整 Skill 在同目录 UPSTREAM-SKILL.md（2026-11-17 快照，A8 之前；上游现版已含 A8，见 fork jackspierrow/stickman-video-director），核心 references 已存同目录 references/（含 A7 四段提示词规范、A8 吉卜力生产规范+角色画风提示词、橡皮管生产/验收、B1 极简线条、风格目录、分镜模板）。
全仓 658MB 主要是预览素材，未整仓克隆（稀疏克隆实测卡住）；缺的 reference 按需从上游 raw 补拉。

## 两条引擎（选管线时先分清）
| 引擎 | 本质 | 成本 | 丝滑度 | 本机走法 |
|---|---|---|---|---|
| A 模型引擎（A1-A8） | 分镜提示词喂视频模型直出连续镜头 | 烧视频模型额度，越长越贵、要抽卡 | 高，真连续运动 | Muse 视频工具生成；作者原路线是 Google Flow（Omni Flash，720p/10s/场），本机无 Flow 账号 |
| B 代码引擎（B1-B6） | 分层素材+代码逐帧动画，本地渲染 | 零视频额度，只吃 Agent 额度 | 取决于逐帧密度，本质与 Remotion 同族 | 本地 Python PIL 逐帧 + ffmpeg（样片路线）；或 Remotion |

风格：A1 经典黑白 / A2 现代流行 / A3 史前洞穴 / A4 温馨叙事 / A5 历史卡通 / A6 琥珀 noir / A7 橡皮管 / A8 吉卜力画风；B1 极简线条 / B2 彩色手绘 / B3 复古电视 / B4 生存惊悚 / B5 角色讲解 / B6 动感 Meme。

## A7 橡皮管铁律（上游验收规范，摘）
- 四段式提示词：Block1 固定角色 + Block2 固定画风/色板，必须逐字复用（见 references/a7-prompt-contract.md），只换 Block3 场景 + Block4 分时动作/镜头。
- 角色：细线火柴人、白圆脸、两撮头发、细线躯干四肢，**不许穿衣服、不许填充躯干**。
- 色板：低饱和彩色——鼠尾草灰绿、珍珠灰、灰褐、象牙白 + 少量褪色砖红点缀。**不是黑白、不是棕褐 sepia**，不要重划痕/暗角。
- 参考图走「素材/参考」位定身份与画法，不是首帧；厨房参考图存 references/a7-kitchen-ref.jpg。
- 验收看真片：角色漂移、节拍顺序、接触关系不过关就是不过关，提示词对了不算数。

## A8 吉卜力画风（上游 2026-11-21 新增，2026-11-25 并入本 skill）
- 来源：上游 A8 · Studio Ghibli Style（吉卜力画风），Model Engine（A）。生产规范见 references/a8-production-guide.md，固定人物+画风提示词见 references/a8-summer-two-roles-prompts.md（两段英文 CHARACTER / ART DIRECTION 须逐字复用）。上游 fork 已含 A8；本次只补 A8，A1–A7/B1–B6 不动。
- 画面：手绘水粉质感背景，深蓝天空与大海，耸立积云，阳光照晒的石墙，硬边赛璐珞阴影；扁平赛璐珞火柴人：浅象牙色圆脸、两点小黑眼、一笔短嘴、细墨线四肢；简洁工作服可作身份标识，但四肢须保持可见。
- 晴天色板（上游已验证示例）：深钴蓝天空、青绿远景、阳光奶油色表面、叶绿、少量朱红点缀、冷蓝紫阴影。雨/夜/室内可用自己的配色与光源，人物与道具须始终可读。
- 四段式提示词（结构同 A7，Block 3 不同）：Block1 固定角色（逐字复用）+ Block2 固定画风/场景（逐字复用）+ Block3 站位与接触（起点、工作区、可走路线、道具位置，手在明确时刻接触道具并保持连续）+ Block4 分时动作/镜头（触发→发力→接触→结果→收势）。⚠️注意：A7 的 Block3 是场景，A8 的 Block3 是站位与接触——不要套错。
- 续拍：同场续拍用上一段合格保留区间的末尾帧作下一段首帧，生成后复查接点（停顿、动作重来、方向反转、道具跳位）；新场景复用人物+画法段落重写场景与动作。
- 禁止：代码绘制的火柴人、SVG 骨架图、临时站姿示意图作角色参考或首帧，也不随 Skill 打包。
- 适用：旅行故事、日常工作趣事、奇幻短篇、历史文化故事。上游示例：Mountain Workshop（山地工坊）、Seaside Messenger（海边信使）；样片截图（assets/a8/ 两张）未随 skill 存本地，需要时从上游 fork 取。
- 本机实测 demo：Leo 快递员四场 30 秒（16:9），Bayi 版提示词与配音方案在 ~/workspace/shorts/a8-leo-kurir-ghibli/prompts-bayi.md，完整 A8 规格存档 ~/workspace/x-simpan/2026-11-24-avery-ghibli-stickman-a8.md。注意：2026-11-25 实测视频生成服务对本账号报 403 区域封锁（video_not_available），A8 成片制作待服务恢复。
- 另记（上游同批更新，未并入）：B7 Pixel Theater、B8 Greek Mythology；模式改名 Director→Model Engine（A）、Autopilot→Code Engine（B）。

## 本机实测（2026-11-17，同题：闹钟吓醒→踩袜滑倒→喝咖啡复活）
- B 样片：~/workspace/stickman-lab/b-sample/stickman-b1-sample.mp4，1280x720/30fps/16.5s/559KB，PIL 逐帧流式喂 ffmpeg，渲染 26.3 秒，零成本。单场景横移贯穿三幕，字幕烧录，无右下角文字。
- A 样片 v2（交付版）：~/workspace/stickman-lab/a-sample/stickman-a7-sample-v2.mp4，1280x704/24fps/10s/6.7MB，Muse 视频工具 + 厨房参考图 + 四段规范生成。色板和细线身体基本对版；残留问题：6 秒处躯干线变粗/脸空白、结尾特写脸漂移且咖啡节拍跑到出门后，严格 A7 验收不算全过。
- A 样片 v1（已淘汰）：纯文字提示词、棕褐黑白 + 穿睡衣填充躯干，角色三幕三变。教训：A 模式不带参考图、不按四段规范，出片必漂。
- 结论：要丝滑/丰富选 A（付额度、要验收返修）；要便宜/可控/角色稳选 B（逐帧密度决定丝滑上限）。
- **主人定版（2026-11-17 看完双样片）：后面发的 A7 画面更精致。以后火柴人成品优先 A 模型引擎，B 只作免费打样/兜底，不再默认 B 出成品。**
- 长片实测（2026-11-17，《凌晨2点精神，下午2点断电》腺苷科普，67.5s）：分场生成 6 条×10s（每场一梗）→ Aria 中文配音分段对齐 → ffmpeg 拼接+烧字幕+响度 -14.9 LUFS → 抽帧 8 张全过。分场后节拍不再乱序；脸随场景有夸张变形（搞笑题材可接受）。成品 ~/workspace/stickman-lab/adenosine/adenosine-final.mp4（16:9）+ adenosine-vertical.mp4（9:16 模糊垫底版）。单条整片生成只适合 ≤10s，长片必须分场。
- **节奏定版（2026-11-18 主人打回 v1「节奏太慢、停顿怪怪的」后定）**：①取消静默片头卡，标题改烧进第一场画面上方，配音 0 秒起；②文案写满，每场约 55–60 字，配音 speed 112；③每场画面用 setpts 变速压到该场配音时长+0.15s，配音首尾相接零静默；④配音段时长离群必查（实测一稿 8 秒文案被 TTS 念成 23 秒、句间塞 1.1–1.45 秒停顿，silencedetect 可查；同文案重念会命中缓存坏稿，须改几个字破缓存再生成）。v2《你以为你在刷手机？其实手机在刷你》多巴胺篇 53.5s，成品 ~/workspace/stickman-lab/dopamine/dopamine-final.mp4。

## B-Remotion 现成骨架 stickman-story（2026-11-20 主人令克隆实测，源 @soundofdragon 回复帖指路）
- 仓库 ahmadaryanto/stickman-story（64 星，MIT，commit 4f2262c），原仓+实测在 ~/workspace/stickman-story-lab/repo（596KB，审计干净：源码无 exec/eval/fetch，依赖只有 Remotion 4.0.535 + React 19 + TS）。
- 定位：B 代码引擎的现成 Remotion 骨架，补 PIL 路线没有的组件库。竖版 1080×1920/30fps；`src/rig/` 火柴人骨架+姿势（idle/walk/sit/typing/point/cheer/mix）+3 套换装，`Camera2D` 平移/缩放/跟随，`CaptionTrack` 字幕轨，`Furniture` 桌椅/显示器/白板，3 个示例（走路 5s / 办公室 12s / 微纪录片 11s）。
- 实测（2026-11-20）：`npm ci` 258 包 34 秒；typecheck 0 错；still 5 张亲查——人物/换装/坐姿打字/字幕/流程图场景全对版，无水印、无右下角文字；Example03 全片渲染 2 分 02 秒出 1080×1920/30fps/11.0s/735KB，成品 ~/workspace/stickman-story-lab/repo/out/example03-mini-documentary.mp4。
- **本机适配补丁（已打在 lab 仓 palette.ts，勿回退）**：上游用 @remotion/google-fonts 在无头 Chromium 里拉 fonts.gstatic.com，本机走出口代理报 ERR_SSL_PROTOCOL_ERROR（curl 正常、浏览器不行）；FONT 已改本地字体栈 `'Noto Sans CJK SC', 'DejaVu Sans', sans-serif`。渲染须带 `--browser-executable=$HOME/.cache/ms-playwright/chromium-1248/chrome-linux64/chrome`。
- 工作法（上游精华，值得复用）：REUSE → RESKIN → NEW，先复用现成场景、只改颜色/文字/位置/帧数，新代码是最后手段；CLAUDE.md 只做短索引按需读文件——这是省 agent token 的关键，正对 @soundofdragon 原帖「一条片烧掉半周 Muse 额度」的坑。
- 短板写死：作者自称简化教育版、非完整生产引擎；画风是暗底+填充躯干的圆头小人（不是细线火柴人），人物在竖版画面里偏小、留白多，要近景须自己调 Camera2D zoom；只有室内家具，没有户外/恐怖场景。
- **分工定版**：科普讲解/流程解说/办公室类短片要便宜可控，用这个骨架（B）；恐怖故事、丧尸这类氛围叙事（如 @soundofdragon 的电梯 7 楼 180 秒横版片，抽帧实查是精细插画背景+大头角色+电影感打光）是 A 模型引擎的活，这个骨架做不出，别拿它硬套。成品优先 A 的旧定版不变。

## 现代火柴人故事片（A 引擎·叙事风，2026-11-20 首测《电梯总停在没人的楼层》）
- 题材源 @soundofdragon《电梯 7 楼》：主人点头练中文版。做法：不用 A7 橡皮管 blocks，另立现代故事风固定 blocks（女主：大圆白脸+黑马尾+青绿毛衣+米色托特包；保安大爷：圆白脸+圆眼镜+灰胡子+棕开衫；画风：手绘精细背景、深夜蓝+暖黄台灯、电影感悬疑转温暖），全片逐字复用 + 原作帧作参考图定身份/画风，6 场 ×10s 分场生成。
- 成品 ~/workspace/stickman-lab/elevator/elevator-final.mp4：1280×720/24fps/57.9s/22MB，响度实测 -14.5 LUFS，配音 Aria speed 112、六段 11.5/9.4/8.2/9.5/9.4/9.0s，生产包 kit.md + build.py 在同目录。
- 抽帧验收：S1–S5 画风与原作同档（办公室加班、电梯开空层、捂嘴+黑影逼近、大爷浇花、相框老太太全对版）；短板如实记：S6 画风变扁（平涂感明显强于前五场）、S4 大爷特写脸偏写实与 S5/S6 圆白脸不完全一致、场景内 ELEVATOR/FIRE EXIT 英文标识是模型自带（非字幕）。
- **TTS 新坑（2026-11-20 实证）**：整段 54 字配音两次都生成 22.7 秒坏稿——第一句后塞 8.5 秒静默，改几个字破缓存也没用；解法是按句拆成两段分别生成再本地 concat，得到干净 9.4 秒。以后配音时长离群先 silencedetect，拆段重录优先于改字重录。

## 《电梯》v2 修订（2026-11-20 主人令两项一起做）
- S6 重做：改用本片 S5/S2 的成品帧作参考图（不再用原作帧）+ 提示词明写「不要扁平极简卡通、要宽镜侧面构图」，新 S6 与前五场同档（锈墙细节、蹲放粉花、双人侧脸、结尾两盆花并排+暖光门缝全对版）。教训：同片返修定画风优先用本片已过关的帧当参考，比外部参考更贴。
- BGM：本机无音乐生成工具，改用 numpy 纯合成（make_bgm.py 在 elevator 目录）：前 39.2 秒 D 小调悬疑（低音 drone+稀疏钢琴单音+远处八音盒泛音），39.2 秒起转 F 大调八音盒旋律+软钢琴和弦，转折点对齐 S5 真相揭晓；BGM 本体 -21.4 LUFS、混音后成片 -14.3 LUFS，旁白清楚。成品 elevator-final-v2.mp4（57.95s/23MB），v1 保留在 elevator-final.mp4。

## 附录 A8 · 两份制作指南全文（2026-11-25 并入）

说明：以下两份文件全文照录自 references/a8-production-guide.md 与 references/a8-summer-two-roles-prompts.md，内容未改动，仅作全文收录。合并 PDF（skill dari hextor）只收录 SKILL.md 正文，故将指南全文附于此，PDF 读者无需另取 references/。

### 附录 A8-1 · a8-production-guide.md 全文

# A8 吉卜力画风 · Studio Ghibli Style

第一段默认在 Flow 用[固定人物与画风提示词](a8-summer-two-roles-prompts.md)纯文生。同场动作需要续拍时，用上一段的合格尾帧开启下一段。角色保持浅象牙色简单圆脸、两点小黑眼、一笔嘴、干净脸颊与细黑线身体。身份服饰、场景配色与动作随故事调整。

## 画风参考

- [海边信使样片截图](../assets/a8/coastal-courier-style.jpg)对照手绘街景和海天的纵深、清晰明暗、平涂角色和道具动作的可读性。[山地工坊样片截图](../assets/a8/mountain-workshop-style.jpg)对照木材笔触、室内外光影、地形层次和工作道具与人物的关系。
- 从开头到结尾使用同一套二维赛璐珞画法。保留干净的墨线、角色平涂、背景的水粉干刷质感、清楚的色块与有方向的硬边阴影。用有用的前景、中景动作与远景变化建立空间。场景内选一两处能参与动作的细节，例如风带动棚布、信封和衣角，让画面有生活感。
- 雨景、夜景和室内可以有自己的配色与光源。即使切换天气、时间或机位，人物和动作道具仍要从背景中读得清楚，背景细节与前中远景要有层次。两张样片截图用于核对画法。本片主角的长相和服饰由本片人物描述确定，站位和姿势随分镜变化。

## 按四块写生成提示词

1. **人物**　同一故事的每个主角镜头逐字复用同一人物段落，包含头形、五官、身线粗细、关节连接点和服饰识别特征。场景和动作另写。领口、外套和工具带应留出足够空间，让细线身体和关节在主要动作中可辨认。
2. **画风与场景**　每个生成片段复用固定的画法与质感段落，再写该场景的主色块、光向、前中远景及可见的环境变化。让环境动作与人物任务有关，例如风先卷起信封，才引出伸手去接的动作。
3. **站位与接触**　写清角色起点、工作区、可走的路线和道具位置。用斜侧全身或中远景先交代长桌、桌角和开放过道。人物沿可见过道绕过桌角，停稳后再伸手；需要从桌后出现时，明确遵守前后遮挡，直到走出桌沿。手应在明确的时刻接触道具，并随持握保持连续。
4. **动作与镜头**　按时间写清触发、发力、接触、结果和收势。每个镜头围绕一个主要动作安排视线、重心和姿态变化，再选与动作有关的衣摆、领巾、工具或环境做跟随动作。写清它们什么时候动、朝哪里动、什么时候落下来。需要走位后拉绳或取物时，可拆为交代站位的全身镜头和手部特写。跨镜头保留动作方向、视线和道具状态。

海边信使的写法示例。0–2 秒，镜头沿斜坡跟拍，棚布被风掀起，信使骑出阴影进入阳光。2–4 秒，前轮颠过石块，信封从挎包飞到空中。4–6 秒，沿着信封的方向切到侧面跟拍，信使一手扶车、一手接信，红领巾继续随同一股风摆动。其他场景按实际道具和环境设计自己的动作。

## 片段续拍与角色参考图

- **同场续拍**　按正常速度看完上一段原片，在拟保留区间的末尾选一帧。人物脸、身体连接、姿势、道具接触、光影和构图都要通过检查。原片最后一帧有拉扯、断肢或道具漂移时，往前选最近的合格帧，上段也剪到对应接点。将原画面单帧放入 Flow 的首帧输入，保留比例和构图。
- **写下一段动作**　从该帧的站位、视线、手部接触、重心、道具运动与镜头方向继续写。例如上一段结束时信使刚夹住信封，下一段从手指收拢、前轮继续下坡、红领巾沿原风向摆动接起。生成前记录原片、取帧时间和输入方式，生成后复查接点是否停顿、动作重来、方向反转或道具跳位。拼接时剪去重复起始帧或多余停顿，并按正常速度复查两段衔接。
- **独立新场景**　可用同一人物和画法段落重新文生，按新构图写场景和动作。主角在新构图或多人场景需要额外保持身份时，可用已确认的合格原片选帧，或与 A8 画法一致的生成图、用户提供的手绘图，作为可选的角色参考。角色参考只绑定到对应人物，明确记录参考版本。
- 代码绘制的火柴人、SVG 骨架图和临时站姿示意图禁止作为角色参考或首帧提交，也不随 Skill 打包。随 Skill 收录的两张样片截图用于审片对照。

## 多人镜头

- 先标出主角、配角和背景人物。为配角安排不同的服饰标记、轮廓和站位，同一位配角跨镜头沿用自己的标记。主角保留定稿的头身比例和标志服饰。参考图只绑定到它对应的人物。
- 写清谁从哪里进入、朝哪里看、谁持有什么道具，以及谁在前景或后景。比如主角走到门口并挥手，蓝帽配角负责扶门，窗内客人各自在桌边。需要交接时，给手部接触和道具易主一个清楚的动作。
- 多人同框时把主动作留在清楚的轮廓上，让配角和环境承担反应。两人交错时明确前后遮挡。开门、让行、交接挤在一起而看不清时，可拆为连贯的全身镜头和动作特写。

## 可复用的人物连续性提示

```text
Animate the hero as a hand-inked stick figure within the painted 2D cel scene.
Use a plain ivory circle head with two tiny black dot eyes, one short mouth stroke and blank cheeks. Preserve that design through turns, changes in gaze and expression.
The entire torso is one thin ink stroke, the same weight as the arms and legs. The top of the stroke touches the underside of the head. Both arms branch from the shoulder point. The two legs branch from a visible Y-shaped hip.
Keep those connections continuous through the whole movement. Show the painted background between the limbs and keep the neck and hips readable against it.
Keep the costume's identifying details and the main joints readable. Show anticipation, weight shifts and recovery, with fabric or props following the action where appropriate.
Stage the performer and solid props on readable planes. Show the open walking path around furniture and the point where the hand touches a prop.
```

使用角色参考图的多人镜头可加上以下结构。按实际分镜填写人物、站位和动作。

```text
Use the attached character reference for [hero]. Keep the hero's face, thin limbs, proportions, and costume markers consistent through the shot.
Give each supporting character distinct costume markers. [Hero] starts at [position] and [action]. [Supporting character] stays at [position] and [action]. The [prop] belongs to [character] until [visible handoff, if any]. Maintain the same character identities and foreground/background order through overlaps.
```

## MV 剪辑

- 先在连续音乐轨上标出第一句人声、换句、重音与收束。生成前写清每个关键乐句对应的可见人物动作或反应，以及前后镜头怎样接上。尽早建立主角要做的事，在情绪转折和收束处看得见人物的选择与结果。关键停顿可以保留，让动作和音乐一起落下。
- 以远景交代空间，用近景看清角色反应、道具或触碰。窗光、水面、布帘和蒸汽等环境变化要配合镜头里的动作与情绪，例如布帘被风掀开后露出一碗热汤。
- 同一段人物动作可从通过验收的同一条原片取连续的两个区间，重新构图后用作反应镜头和更宽的后续镜头。剪完按正常速度检查动作是否跳回、乐句是否被切断。音乐沿用独立完整轨道，风声和动作音效按叙事需要另行混入。

## 原始素材验收

Flow 出片后，先以正常速度看完整原片，再逐帧检查转身、弯腰、迈步、伸手、人物交错和拟剪切点。按本片定稿的人物描述验收人物，使用过角色图的镜头也对照该图。在原片的起始、中段、末尾及切镜或场景变化处用 A8 样片截图和相邻镜头检查画风。用已确认的海边信使动态样片对照表演节奏。以下项目均通过的区间才可用。

| 项目 | 通过标准 |
| --- | --- |
| 脸型 | 主角为浅象牙色简单圆脸、两个小黑点眼、一笔嘴，脸颊干净。眨眼和转头保持同一轮廓与五官画法。出现腮红、额外眉鼻、脸颊圈或五官漂移的区间退回。 |
| 身体连接 | 头颈接到细线躯干，手臂从肩部连接，双腿接到髋部，发力时仍连续。合理遮挡前后保留同一结构。头颈、肩或髋可见断开、肢体缺失或增生、躯干变成实心粗块的区间退回。 |
| 穿模与接触 | 脚在可走的地面或台阶上落地，绕开桌面和柜台，过门时保持正确遮挡。拿取、拉动与交接能看清接触点，持握后道具持续随手移动。穿过实物、滑脚、脱手悬浮或道具跳位的区间退回。 |
| 动作表现 | 正常速度能看清视线、姿态或重心变化，以及关键动作的起势和结果。故事要求时，衣物、道具或环境有相应的跟随动作。主要动作像僵硬的平移、长时间站桩或多个部件同时摆动时退回。 |
| 多人一致性 | 主角的脸、头身比例和服饰识别点保持稳定，配角的标记、动作与道具归属可辨。交错前后能追踪每个人。轻微衣摆或纹样差异可接受，前提是身份识别、动作与肢体连接都清楚。 |
| 画风一致性 | 满幅手绘背景、平涂人物、墨线与干刷肌理一致，明暗分区清楚，前中远景有层次。可按场景变换颜色与光源。出现白边、明显的 3D 塑料质感、人形化身体，或一层灰绿雾抹平场景与人物轮廓的区间退回。 |

- 对每条原片记录参考版本、检查区间和结果，失败时记录时间点及问题。以通过、退回、待查标明状态。可保留已通过且动作完整的区间。剪辑点以正常速度复查方向、接触和空间连续性。
- 返修聚焦问题区间。脸或服饰漂移时先改清人物描述与出场安排，有连续性需要时再使用合格的手绘角色图。动作僵硬时补清触发、重心、动作结果和跟随时机。断开时先调整服饰遮挡和动作幅度，穿模时先明确走位、前后关系与接触点。画风偏移时对照样片重新写该场景的配色、光向和空间提示。保留已确认的画面，返修后复查问题区间和相邻剪辑点。成片上的补画或遮挡处理只计入那次剪辑修复，Flow 原片仍按实际表现记录。

## 成片验收

- 正常速度看完整成片。并排核对跨场景、跨光线的镜头，人物依然是同一套细线画法，画面仍有样片的手绘层次和清晰光影。每个切点检查动作方向、角色站位、道具状态与声音衔接。
- 关键情节须由可见的人物动作、反应或互动推进。能看懂谁在做什么、哪里发生了变化、最后怎样收束。环境和道具特写可以交代空间、连接动作或给情绪留白。连续关键段落只有空景或道具，导致主角的任务或选择读不出来时退回。MV 还要核对动作与乐句、重拍或情绪转折的对应。

### 附录 A8-2 · a8-summer-two-roles-prompts.md 全文

# A8 吉卜力画风 · 夏日手绘色调试片

Omni 1.2 Flash · 纯文生 · 720p · 6 秒 · 16:9

独立新场景沿用以下人物与画法段落。同场续拍时，从上一段已通过的保留区间末尾取帧，用作下一段首帧。续写当时的道具状态、人物动作和镜头方向。详见 [A8 制作、原片及成片验收](a8-production-guide.md#片段续拍与角色参考图)。

## 固定人物

CHARACTER. One original hand-inked stick figure animated within the painted 2D scene. The head is a simple pale ivory circle, finely outlined in near-black ink. The only marks inside are two tiny solid black dot eyes and one short expressive black mouth stroke. Both cheeks are plain ivory, the same color as the forehead. No blush, cheek circles, freckles, nose, ears, or face shadows. Keep the round head design through turns and expressions. A slender connected ink-line neck, torso, arms and legs, tiny simple hands and narrow shoes. Compact workwear specific to the role leaves the limbs visible as the character shifts weight, acts and settles.

## 固定画法与晴天色彩示例

沿用二维墨线、平涂人物、水粉干刷、清晰硬边阴影、前中远景层次与满幅构图。下面的晴天色彩是已验证的场景示例，换天气、时段或地点时写对应的色块和光源。

ART DIRECTION. Original full-bleed hand-inked 2D adventure animation, sunny painted summer colors. Deep cobalt sky, turquoise distance, sunlit buttercream surfaces, leafy greens, tiny vermilion accents, cool blue-violet cast shadows. Hand-painted gouache scenery with delicate dry-brush grain, legible silhouettes, hard-edged cel shadows on objects, flat unshaded face. Stage action on distinct foreground and distant planes with wind and gentle parallax. Edge-to-edge picture in every shot, no white margins, borders, panels, captions or text.

## 山顶气象观测员

SCENE AND ACTION. Mountain weather observer on a sunny ridge above a sea of clouds. Small navy peaked field cap, short field vest. An orange windsock snaps sideways in a gust. Start medium-wide, the observer braces on the timber deck, reaches up to free the snagged windsock cord; the sock fills and points downwind. Cut on the pull to a waist-up three-quarter view, the observer turns toward camera with a tiny satisfied smile. Keep the face clearly visible through the turn.

## 海港修船匠

SCENE AND ACTION. Harbor shipwright beside a vermilion wooden dinghy, sunlit quay, turquoise water reflecting moving light. Small navy knit cap, short blue canvas work apron. Start in a medium profile as the shipwright steadies a loose wooden peg, raises a small wooden mallet and taps it flush. Cut on impact to a waist-up three-quarter view; the shipwright lowers the mallet and turns toward camera with a tiny satisfied smile, boat bobbing behind. Keep the face clearly visible through the turn.
