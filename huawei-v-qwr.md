---
title: 某不知名网友怒斥华为，究竟发生了什么
date: 2021-06-21 13:32:57
---

虽然国内新闻平台比较后知后觉，但是想必大家都看到了类似于这样的[报道]。报道中故事被显著的简化：一个提交 trivial patch 的华为员工被开发者社区怒斥。这个瓜显然不好啃，我大概简述一下我吃到的瓜长啥样。

## 导火索的 patch
作为导火索的邮件是[这一封][邮件]。邮件中 Zhen Lei 想要提交一个 patch 以移除一个分配内存失败的警告。理由是如果能触发这个警告的情况下，call trace 应该能有效的提示开发者发生了什么。

这个 patch 被礼貌的拒绝，理由是作为测试套件的一部分，尽可能的详细并不是一件坏事。移除它完全没必要。

到这里结束的话，问题其实不大。在 LKML 中一直有 patch 被拒绝，这种无关紧要的一行修改被拒绝了就被拒绝了。但是这位 patch 作者接下来进行了反驳：

 - 我觉得这个错误提示对于 debug 完全没用；
 - 删除它能省下来一点编译出来的二进制的体积；
 - 如果发生了 OOM 导致这个错误被触发，那么除了这个 OOM 之外后续测试结果会失去意义。
 
这就犯了大忌——干了浪费维护者时间的事情。因为对于这种相当 trivial 的修改，作为维护者的 David Sterba 显然会比 patch 作者了解这个 patch 的影响。在这种情况下，除非理由足够的把握，否则去 argue 只是浪费维护者的时间。维护者们最讨厌的就是被浪费时间，因为他们很忙很忙，每天要 review 一堆 patch，要是这种无意义还有人来争论的 patch 太多就没时间干正事了。

然后 Qu Wenruo 跟进并且反驳了上述争论，~~然后越看越气，~~指出这人提交 patch 前就不知道开发流程，没仔细研究就来提交 patch，属于混 KPI 行为。

## 指责的邮件
然后大概是翻阅了当事人 Zhen Lei 的邮件，发现这类 trivial 的 patch 不止这一个。作为一个老鸟，抢这类低垂的果实不是太好的事情，因为这些简单的问题对于新手可能是他们接触 Linux 社区的第一步。这些 patch 都被老鸟抢干净了，那么 Linux 社区吸引新人的能力就会下降。另外，使用公司邮箱去跟维护者进行无意义的争论是很影响声望的事情。（参照之前明尼苏达大学的事件，有趣的是，那次事件也是 patch 被维护者礼貌拒绝，然后无意义争论惹怒维护者导致被翻旧帐）

然后 Qu Wenruo 向 LKML [发送了邮件][原邮件]直接的指出了不满，原邮件网上有大量翻译，这里我连引用都懒得。但是这一句话

> But what you guys are doing is really KPI grabbing, I have already see 
> several maintainers arguing with you on such "cleanups", and you're 
> always defending yourself to try to get those patches merged.

就是冲突的关键。华为有人为了 KPI 提交无意义甚至是可能起到反作用的 patch，在被维护者拒绝后**试图抗议**（废话，patch 不被合并自己 KPI 就没了），然后引起了社区志愿者们的不满。

---

这事情大概华为会有人道歉吧，问题也不是很大。但愿华为会改进内部的评价机制，让员工不至于为了一两行的 patch 去浪费社区志愿者的时间。

[报道]: https://www.cnbeta.com/articles/tech/1143079.htm
[原邮件]: https://lore.kernel.org/linux-btrfs/e78add0a-8211-86c3-7032-6d851c30f614@suse.com/T/#u
[邮件]: https://lore.kernel.org/linux-btrfs/55b0c70b-f0c1-07e2-f8dd-073f4fdc8f07@gmx.com/T/#t
