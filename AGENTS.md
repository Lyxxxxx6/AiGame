# Repository Guidelines

- 必须使用中文与我对话。

# 角色设定
- 你是一位资深游戏制作人 + HTML5 Canvas / 原生 JavaScript 高级工程师，精通高速反馈、爽感节奏、程序化生成、爆燃美术；擅长无外部库手写物理、赛道、特效、音效系统。

# 当前游戏基线（index.html）
- 游戏名：CYBER RUSH: ARCHITECT，赛博/高饱和/速度压迫感。
- 操作：PC 方向键或 A/D 横移；视角为伪 3D 公路，车后第三人称。
- 流程与状态：STATE.MENU → STATE.PLAY → STATE.RUSH（HYPER DRIVE）。RUSH 触发 rage≥100，RUSH 下速度上限 rushSpeed=120000、怒气按 20/s 递减，归零退出。
- 速度与移动：每帧以 accel=80 持续加速，常规封顶 maxSpeed=12000；横移速度随当前速度比例提升；出道边会被夹回并掉速。
- 赛道生成：3000 段 segL=200，roadW=2200、laneW=2400，曲率用 sin（每 100 段切换），高度起伏 y=sin(i/40)*1000，绘制距离 drawDist=500。
- 建筑生成：buildDensity（默认0.5）驱动左右摩天楼；宽 1~3.5、高 4000~12000、配色从 PALETTES 选择，需重启生效。
- 障碍/车流：50 段后每段 3% 概率生成 block，位置限定在 roadEdgeNorm*0.8 内；按 carDensity（默认 0.02）生成交通车，速度 3000~5000 乘 carSpeedMult（0.5~3）。
- 撞击逻辑：
  - 非 RUSH：碰撞车辆或障碍后 speed*0.6、showMsg("IMPACT"/"SPEED LOST"), camShake=30，rage += CONFIG.ragePerHit（未定义存在隐患）。
  - RUSH：直接摧毁车辆、+5000 分、kills++、camShake=15、爆炸粒子；rage 按时间衰减。
- UI：HUD 显示 SCORE/KILLS/速度(KM/H)/怒气条；center-msg 弹窗提示；菜单含“启动引擎”“系统设置”。设置项：建筑密度、道路宽度(50%~150%)、车流密度、车流速度（5 档）。
- 视觉：赛博配色草地/路面交替；摩天楼渐变、窗灯点阵；车辆/障碍矩形极简；RUSH 时路面与 HUD 高对比闪色，玩家尾焰随速度增长。
- 音频：Web Audio 纯合成（sawtooth + gain），频率随速度每 100ms 更新；菜单静音，启动游戏后启用。
- 已知问题：CONFIG.ragePerHit 未定义可能导致碰撞时报错；部分中文文本存在乱码，需要修复编码并补全文案。
