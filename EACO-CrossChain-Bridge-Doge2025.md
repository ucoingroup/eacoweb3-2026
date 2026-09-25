# EACO 生态跨链方案 · 技术文件

**文档版本**：v0.1  
**编制日期**：2025年3月  
**适用对象**：地球村 Web3 开发者与社区成员

---

## 一、项目概述

**EACO（Earth's Best Coin = E = EACO）** 是一个基于 Solana 的 AI + RWA + Web3 加密资产项目，合约地址为 `DqfoyZH96RnvZusSp3Cdncjpyp3C74ZmJzGhjmHnDHRH`。

其核心理念为：

> **EACO = Energy（能量）× Attitude（态度）× Cooperation（协作）× Optimization（优化）**

即“正能量 × 积极态度 × 全球协作 × 持续优化”，目标是通过与 100 个认知模型结合，提升个人认知、组织效率、社会文明与地球可持续发展。

**地球币 Earthcoin（EAC）** 是 2013 年 12 月 20 日在加拿大多伦多发布的基于 Scrypt 算法的全球性加密货币，PoW 共识机制，总量 135 亿 EAC。电脑钱包最新版本为 v2.1.4，支持 IPFS 网关配置与多签地址及交易。

---

## 二、跨链技术架构

### 2.1 核心机制：Eaco Routes Any-to-Any Swaps

EACO 生态跨链的核心依托 **Eaco Routes API** 的“任意代币跨链兑换”功能，该功能已于 2026 年 8 月正式上线，默认采用 **LayerZero** 作为结算配置，初期覆盖所有 EACO 支持的 EVM 链及 Solana。

跨链流程分为**三步原子化操作**：

**第一步 · 源链原子交换**  
用户在源链上的输入代币通过 **EacoSwapGateway** 合约与本地 DEX 或 propAMM，被**原子化地**交换为稳定币（USDT/USDC）。这一过程编码为“本地闪兑意图”（Flash Intent），在单笔交易中同时完成证明、提款和路由执行。

**第二步 · 跨链稳定币转移**  
稳定币输出通过 EACO 现有的跨链稳定币通道转移至目标链。该通道**无需信任、无需许可**，是整个跨链架构的中间结算层。求解器（Solver）在整个过程中**仅需持有稳定币**，无需持有任何长尾代币库存。

**第三步 · 目标链兑换**  
目标链上第二个闪兑意图将收到的稳定币通过链上 DEX 兑换为用户的目标代币，并**直接交付给收款地址**。

**原子化保障**：所有步骤要么全部执行完成，要么全部取消，**不会出现资金卡在半路的情况**。每个环节在单个区块内完成结算，**无中间托管状态**。

### 2.2 结算层：LayerZero 消息传递

LayerZero 为 Eaco Routes 的 Any-to-Any 兑换提供默认结算配置，通过其消息传递层实现跨链意图结算。这意味着 EACO 的跨链不仅支持 EVM 链之间的互操作，还通过 LayerZero 的扩展能力覆盖 Solana，实现 EVM 生态与 Solana 生态之间的资产流转。

### 2.3 DOGE 原生跨链：Wormhole NTT + Sunrise

DOGE 进入 Solana 生态的路径与上述稳定币通道不同，采用的是 **Wormhole 原生代币转移（NTT）框架**，配合 **Psy Protocol** 和 **RISC Zero** 构建的**零知识证明（ZKP）** 来验证链间交易。

与传统“包装代币”模式不同，NTT 框架转移的是 **DOGE 的“原始代币身份”**，而非合成替代品。这意味着 Solana 上的 DOGE **保留其全部原始属性**，包括元数据控制权和供应政策。

**Sunrise 流动性网关**由 Wormhole Labs 开发，为每个进入 Solana 的外部资产分配**唯一的标准化铸造地址**，从根本上解决多桥接导致流动性碎片化和价格混乱的问题。DOGE 在 Solana 上的官方标准合约地址为：

`DoGEV7LASBkQbibMc5k5vKnTZoMg423GpJ5QtJEGfm7R`

该集成已将 DOGE 全部约 **350 亿美元**的流通供应迁移至 Solana，是加密历史上最大的单资产跨链集成案例。

---

## 三、多资产跨链路径

| 目标资产 | 合约地址（Solana） | 跨链路径 | 难度 | 关键说明 |
|---|---|---|---|---|
| **SOL** | `So111...1112` | Solana 原生 DEX | 低 | EACO/SOL 在 Pumpfun 有交易对，直接在 Solana 生态内兑换 |
| **USDT** | `9gP2...qiLa` | Eaco Routes 稳定币通道 | 低 | 最直接路径，EACO/USDT 池经 Meteora DLMM 提供流动性 |
| **DOGE** | `DoGEV7...fm7R` | Wormhole NTT + Sunrise | 高 | 原生跨链，非包装代币。全流通供应已迁移至 Solana |
| **wBTC** | `3NZ9J...qmJh` | Eaco Routes → Solana → 兑换 | 中 | 可经 ECHO 等协议将 wBTC 统一为 aBTC 后在 DeFi 中操作 |
| **wETH** | `7vfCX...voxs` | Eaco Routes → Solana → 兑换 | 中 | wETH 在 Solana 上由 Wormhole 桥接 |
| **wBNB** | `9gP2...qiLa` | BSC DEX 或 Eaco Routes | 中 | EACO 在 BSC 上有合约，需注意 BSC 合约合规状态 |
| **EAC 地球币** | `Dqfoy...nDHRH` | SOL DEX 兑换或 DOGE Routes | 中 | EAC 在 SOL 上以 EACO 合约形式存在 |
| **TRX** | `Gbbes...kKc` | LI.FI / Allbridge | 中 | Allbridge 连接 TRON 与 Solana 等 12 条链 |

**TRX 跨链方案**：LI.FI 已集成 4 座桥支持 TRON 连接，包括 GasZip（意图桥接至 60 条链）和 Allbridge（连接 TRON 至 Solana、Ethereum、Sui 等 12 条链）。MetaMask 也已原生支持 TRON 与 Solana 之间的跨链兑换。

---

## 四、安全评估与风险提示

### 4.1 EACO 代币安全特性

EACO 代币合约设计具备以下安全属性：

- **不可冻结（Non-Freezable）**：合约拥有者无权冻结任何持有者的代币账户，代币转移不受限制
- **不可增发（Non-Mintable）**：代币总量在初始发行后固定，无法继续创建新代币
- **元数据不可篡改**：代币元数据在创建后锁定，不可被修改

这些设计保障了持有者的资产自主权，避免了中心化控制带来的冻结与通胀风险。

### 4.2 流动性建设

EACO 目前在 Solana 上的流动性池规模仍在成长阶段，24 小时交易量有待提升。为支持更大规模的跨链操作，EACO 生态正在持续推进以下流动性建设工作：

- **Meteora DLMM 池扩展**：通过 EACO DAO Pool 持续注入流动性
- **多交易对建设**：在 Pumpfun、Raydium、Jupiter 等 Solana 生态 DEX 上拓展 EACO 交易对
- **社区流动性激励**：鼓励社区成员为 EACO 交易对提供流动性

建议用户在进行大额跨链操作前，先查看实时流动性深度，合理规划交易规模。

### 4.3 Wormhole NTT 安全模型

DOGE 跨链依赖 Wormhole 基础设施的安全性。NTT 框架通过零知识证明实现了无需信任的验证，用户**受制于 Wormhole 底层桥接架构的安全协议**。建议关注 Wormhole 官方安全公告，及时了解协议更新。

---

## 五、操作指南

### 5.1 通用跨链流程

1. 连接钱包（Phantom / Backpack / Solflare）
2. 选择源资产与目标资产
3. 调用 Eaco Routes API 获取报价（单次调用可完成多链路径规划）
4. 确认交易，系统自动执行三步原子化流程
5. 在目标链查收到账资产

### 5.2 DOGE 跨链专属流程

1. 访问 Portal Bridge（portalbridge.com）
2. 连接 Dogecoin 钱包（MyDoge / Ledger）和 Solana 钱包（Phantom / Backpack）
3. 选择 DOGE 作为源资产、Solana 作为目标网络
4. 系统自动锁定 DOGE 并在 Solana 上铸造等量原生 DOGE
5. 在 Jupiter、Raydium 或 Kamino Swap 上进行交易

### 5.3 安全操作原则

- **小额测试先行**：首次使用任何桥，先以极小金额测试完整流程
- **验证合约地址**：交易前务必核对官方合约地址，避免仿冒代币
- **使用硬件钱包**：管理跨链资产时优先使用 Ledger 等硬件钱包
- **确认链与合约一致性**：确保目标链与合约地址匹配后再操作
- **关注流动性深度**：大额操作前查看目标交易对的实时流动性

---

## 六、常用链接

| 资源 | 链接 |
|---|---|
| EACO (Orb Markets) | https://orbmarkets.io/token/DqfoyZH96RnvZusSp3Cdncjpyp3C74ZmJzGhjmHnDHRH |
| DOGE (Orb Markets) | https://orbmarkets.io/token/DoGEV7LASBkQbibMc5k5vKnTZoMg423GpJ5QtJEGfm7R |
| SOL (Orb Markets) | https://orbmarkets.io/token/So11111111111111111111111111111111111111112 |
| wBTC (Orb Markets) | https://orbmarkets.io/token/3NZ9JMVBmGAqocybic2c7LQCJScmgsAZ6vQqTDzcqmJh |
| wETH (Orb Markets) | https://orbmarkets.io/token/7vfCXTUXx5WJV5JADk17DUJ4ksgau7utNKj4b963voxs |
| wBNB (Orb Markets) | https://orbmarkets.io/token/9gP2kCy3wA1ctvYWQk75guqXuHfrEomqydHLtcTCqiLa |
| USDT (Orb Markets) | https://orbmarkets.io/token/9gP2kCy3wA1ctvYWQk75guqXuHfrEomqydHLtcTCqiLa |
| 地球币电脑钱包 | https://github.com/Sandokaaan/Earthcoin/releases/tag/v.2.1.4 |
| EACO SWAP | https://ucoingroup.github.io/eacoSWAP/ |
| EACO DAO Pool | https://app.meteora.ag/dammv2/H4P6RUcyQPG7yCrSyE51P4sfj32aTJC2qE86hodCH1Cc |

---

*本文件基于公开技术资料整理，不构成投资建议。EACO 代币不可冻结、不可增发，流动性需要增加，参与前请充分评估自身风险承受能力。*