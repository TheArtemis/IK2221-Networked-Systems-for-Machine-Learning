1. Paper Summary

This paper talks about middleboxes in enterprise networks, like firewalls, IDS, proxies, WAN optimizers, VPN gateways, etc. These are used for security and performance reasons, but they also make the network more expensive and harder to manage. The authors basically say that this situation is similar to what happened before moving IT infrastructure to the cloud, and they ask if middleboxes could also be outsourced in the same way.

Their proposal is called APLOMB, which is a cloud-based architecture where traffic is redirected to middlebox services running in the cloud. The goal is to get the same functionality as on-premise middleboxes, but with less complexity on the enterprise side and without adding too much latency or overhead.

The design is based on a survey of 57 enterprise networks, which is actually one of the main parts of the paper. From this, they show that companies use a lot of middleboxes (sometimes as many as routers) and spend a lot of money and effort managing them. There are also frequent issues like misconfigurations, overload, or even physical failures, which makes the whole system quite fragile.

Based on this, they design APLOMB as a sort of compromise. Enterprises use simple gateways that tunnel traffic to cloud PoPs, where the actual middlebox processing happens. The intelligence is mostly in a centralized controller in the cloud, which manages policies, scaling, and routing decisions.

They implement a prototype using EC2, VPN tunnels, and software middleboxes, and evaluate it using real traffic. The results show that more than 90% of middlebox hardware can be outsourced, with only a small increase in latency (around 1 ms on average) and some bandwidth overhead (around 3-4%), which they consider acceptable, even if it probably depends on the use case.

2. Top 3 Contributions

The first contribution is the survey of 57 enterprise networks. This is useful because it gives actual data about how middleboxes are used in practice, how much they cost, and what kind of problems they cause. For example, they show that the number of middleboxes is often similar to routers, and that failures are quite common, especially due to misconfiguration or overload.

The second contribution is the exploration of the design space for outsourcing middleboxes. The paper doesn't just propose a solution directly, but it also discusses different trade-offs, like where to place components or how complex the system should be. APLOMB is presented as a "middle ground" that tries to balance performance, simplicity, and functionality, even if it's not perfect.

The third contribution is the implementation and evaluation. They build a working prototype on EC2 and test it with real traffic and traces. The results show that it is possible to move most middlebox functionality to the cloud while keeping latency and bandwidth overhead relatively low, which suggests that the idea could actually work in practice, at least in some scenarios.

3. Most Glaring Problems

Even if the idea is interesting, there are some important issues that are not fully addressed.

One big concern is security and trust. Since traffic is sent to the cloud, sensitive data is handled by third-party providers, but the paper kind of assumes that this is fine. It doesn't really go deep into problems like data leakage, compromised middleboxes, or legal restrictions. In reality, this could be a major limitation, especially for certain companies or types of traffic.

Another issue is performance in less ideal conditions. The evaluation shows small latency increases on average, but the system always relies on sending traffic through cloud PoPs. The paper doesn't really explore worst-case scenarios or situations where latency is very critical. Even a few milliseconds could matter in some applications, so this part felt a bit optimistic.

There is also the dependency on the centralized controller. It is responsible for many things like routing decisions and scaling, so if something goes wrong there, it could affect the whole system. The paper mentions this but doesn't go into much detail about failure handling or recovery.

Finally, even if APLOMB is supposed to simplify things, it still introduces new components like gateways and tunnels that need to be managed. So the complexity is reduced in some areas but added in others, and the paper doesn’t really quantify this trade-off clearly. It would have been nice to see a more detailed comparison with existing setups.