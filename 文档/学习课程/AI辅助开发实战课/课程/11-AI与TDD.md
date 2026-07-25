# AI + TDD：验收条件、Red-Green-Refactor、单元/契约/集成测试

导航：上一篇 [[10-项目长期记忆]] · [[VibeCoding课程首页]] · [[VibeCoding课程总纲]] · 下一篇 [[12-核心执行循环]]

> [!summary] 核心问题
> Agent 生成代码很快，但它需要一个比“看起来没问题”更短、更客观的反馈回路。TDD 把 Spec 先变成失败测试，再让每次修改都接受明确裁判，特别适合约束高速度的 AI 实现。


## 测试是 Agent 最短的外部反馈

Red-Green-Refactor 的顺序是：

1. **Red**：写一个表达业务行为的测试，运行并确认它因缺少该行为而失败。
2. **Green**：只写让当前测试通过的最小实现。
3. **Refactor**：在测试持续通过的前提下改进结构和命名。

必须确认 Red。若测试一开始就绿，可能是行为已经存在、断言没有覆盖目标，或测试根本没有执行。让 Agent 同时生成实现和测试再宣称通过，会失去测试驱动设计的约束。

测试层按风险选择：

| 层 | 订单取消验证 | 速度 |
|---|---|---:|
| 领域单元测试 | 状态表、原因校验、领域事件 | 快 |
| 应用/契约测试 | 幂等、事务、HTTP 状态与错误码 | 中 |
| 集成测试 | JPA 乐观锁、唯一键、outbox 持久化 | 较慢 |
| 端到端测试 | API 到退款沙箱的关键路径 | 慢，数量少 |

## 先写领域失败测试

```java
@Test
void paid_order_enters_canceling_and_emits_refund_request() {
    Order order = Order.paid("O-200");

    CancelDecision decision =
            order.requestCancel("R-2", "买错商品", "U-8");

    assertThat(order.getStatus()).isEqualTo(OrderStatus.CANCELING);
    assertThat(decision.refundRequired()).isTrue();
    assertThat(decision.requestId()).isEqualTo("R-2");
}
```

第一次运行应因 `requestCancel` 不存在或状态仍为 `PAID` 而失败。然后只实现状态规则与返回值，不顺手创建 Controller、数据库表和 Worker。

第二个测试保护拒绝路径：

```java
@Test
void shipped_order_is_not_cancelable() {
    Order order = Order.shipped("O-300");

    assertThatThrownBy(() ->
            order.requestCancel("R-3", "不想要了", "U-8"))
        .isInstanceOf(OrderNotCancelableException.class)
        .hasMessageContaining("SHIPPED");
}
```

应用层再验证同一 `requestId` 只保存一次退款任务；JPA 集成测试验证唯一约束和并发，而不是在纯单元测试里模拟数据库锁。

## 可以怎样做

1. 从 Spec 选一个最小场景，只写一个失败测试。
2. 运行精确测试类，保存失败原因。
3. 检查失败是否来自缺失行为，而非环境、拼写或夹具错误。
4. 写最小实现并再次运行。
5. 在绿灯下重构，随后运行相邻测试。
6. 对状态、错误、幂等、并发和外部失败逐层重复。

## 动手梳理

为 `CREATED` 订单取消写一个 JUnit 测试，断言：

- 状态变为 `CANCELED`。
- 不需要退款。
- 审计记录包含 `requestId` 和原状态。

再为相同 `requestId` 写一个应用层测试，断言 outbox 数量仍为 1。说明为什么第二个测试不能只断言 `refundGateway` 被调用一次：它无法证明数据库中的幂等事实。

## 自查清单

- [ ] 每个新行为都先看到预期 Red。
- [ ] 测试断言业务结果，不只验证 Mock 调用。
- [ ] 领域、契约和集成风险放在合适层。
- [ ] Green 阶段没有超出当前测试范围的大实现。
- [ ] 重构后目标测试和相邻测试仍通过。

## 实战速查卡

### 什么时候查这篇文章

- Agent 一次生成大量实现后才补测试。
- 测试只覆盖成功路径。
- 不知道该写单元、契约还是集成测试。
- 重构时没有安全网。

### 先做什么

- 从一条 Spec 规则选最小切片。
- 先写失败测试并确认失败原因。
- 写最少实现使其通过。
- 整理结构但保持测试全绿。
- 再扩展下一条规则。

### 可直接借鉴的方法

Red：测试因目标行为缺失而失败。Green：最小实现通过。Refactor：消除重复并保持行为不变。优先领域单元测试，再按跨层风险补契约和集成测试。

### 常见误区

- 让测试一开始就通过。
- 同时改测试和实现导致失去裁判。
- 只断言 HTTP 200。
- 大量模拟内部细节。
- 测试失败后不读原始错误。

### 完成证据

- 可复现的失败测试。
- 最小通过实现。
- 关键状态、幂等、并发和失败分支。
- 目标测试与相关测试输出。

### 延伸阅读

[[06-最小可执行Spec]] · [[12-核心执行循环]] · [[13-证据驱动调试]]

## 结语

测试为 Agent 提供最短的外部反馈，也把业务规则变成可重复执行的证据。延伸阅读：[[12-核心执行循环]]。

---

相关项目文档：[[规格评审清单]]
