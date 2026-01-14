# -----
《荒桥求生》
基于 Unreal Engine 5 开发的第三人称丧尸生存射击游戏。玩家需要在布满废弃车辆与路障的荒桥上，抵御尸潮的进攻并生存下去。

项目简介：
本项目是一个使用Unreal Engine 5构建的FPS射击游戏。
游戏背景设定在一个末世废土环境中，玩家被困在一座跨海大桥上，必须利用手中的武器在感染者的包围中杀出重围。

功能实现细节
1.角色与动画系统
混合空间：创建了 BS_IdleRun，利用一维混合空间实现了角色从站立、行走、跑步到冲刺的平滑动画过渡。
动画蓝图：构建了完整的状态机 (State Machine)，处理跳跃（起跳/滞空/落地）逻辑，并使用瞄准偏移 (Aim Offset) 让角色上半身随视角上下移动。
动作蒙太奇：通过 Slot 节点实现了射击反冲、换弹动作与身体移动的解耦，支持“移动射击”与“上半身播放换弹动画”。

2.射击与战斗机制
射线检测 (Line Trace)：使用 LineTraceByChannel 实现即时命中（Hitscan）判定，根据物理材质（Physical Material）区分击中特效（击中地面会有烟尘，击中丧尸会有血迹）。
动态弹道散布：实现了准星扩散逻辑，连续射击或移动时准星会变大，降低射击精度。
伤害判定系统：基于 ApplyDamage 和 Event AnyDamage 框架。
受击反馈：实现了摄像机抖动 (Camera Shake) 和受击UI红屏闪烁，增强战斗打击感。

3. 智能 AI 系统 (Artificial Intelligence)
行为树架构 (Behavior Tree & Blackboard)：摒弃了简单的 Tick 逻辑，使用行为树管理 AI 状态
Roaming：在 NavMesh 导航网格范围内随机寻找巡逻点。
Chasing：利用 PawnSensing 组件感知玩家视野与声音，自动切换至追逐状态。
Attacking：到达攻击距离后触发 Attack Task，播放攻击动画并判定伤害。

4. 场景搭建与环境艺术 (Level Art & Environment)
自定义关卡 - 荒桥 (The Bridge)
模块化搭建：使用基础网格体拼接出长距离的大桥结构，并利用 Blocking Volume 处理空气墙边界。
环境叙事：手动布置了大量废弃载具（Tank/Cars）和路障资产，通过物体堆叠营造“堵塞”和“混乱”的末日逃亡现场。
3D UI 集成：使用 Text 3D 插件在场景中植入立体文字（如“INFECTED”），使其自然融入环境而非简单的贴图。
光照与渲染 (Lumen & Post-Process)：调整 Exponential Height Fog（指数高度雾）营造远处不可见的压抑感。使用 Post Process Volume 调整色调映射 (Tonemapper)，降低饱和度，增强冷色调的惊悚氛围。

操作说明
按键功能W / A / S / D移动 (Move)
Shift冲刺 (Sprint)
鼠标左键 (LMB)
射击 (Fire)
鼠标右键 (RMB)
瞄准 (Aim)
R换弹 (Reload)
Space跳跃 (Jump）
