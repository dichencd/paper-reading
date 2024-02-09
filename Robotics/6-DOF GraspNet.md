# 6-DOF GraspNet: Variational Grasp Generation for Object Manipulation
## elegant way of iterative refinement
用learning 做decision making 时候，在实际环境中总会遇到一些constraints。传统的pnc 中基于optimization的方法通常会用mpc, 在mpc 中先formulate这些constraints， 然后通过external cost 当成soft constraint 或者是真正的hard constraints，可以“系统性”handle 这个问题。但是问题就来了
1. 这样的方法即使对于传统control/planning stack 来说就是对的吗？
2. 即使适合于传统stack, 同样的方法在learning中真的就可以照搬过来了吗？ 

传统的deterministic decision making 中， 
$$
\dot{x} = f(x, u), \space y = g(x, u), \space \text{s.t.} \space x \in X, y \in Y, u \in U, \space (x,y,u) \in XYU
$$
因为系统dynamics 是已知的，在一些特定情况下我们可以通过设计$u = h(y)$ 获得一个解析解来保证constraint satisfaction。而更多的情况我们无法获得这样的解析解，只能通过discretize system dynamics + mpc 的方式来handle constraints。听起来很直观，很简单，但是很多人（特别是只上过一些简单control 课程 并只做过简单project调用mpc的人），都会忽视mpc 里面隐藏的一个大坑"RECURSIVE FEASIBILITY"： 在时间t 求解的一个some form of optimization function $L(x_{...},u_{...}), \text{s.t. input/observation/control constraints} $ 是不能保证在下一时间t+1时候还有解的。
究其原因，我的一个粗略的理解是要保证系统的recursive feasibility (也就是任何时间系统都能有机会满足constraint)的关键是control invariant set。 而这个control invariant set 通常比上述decision making 中定义的constraint 要小很多。

介绍完了这个背景，回到说我为什么觉得用learning做decision making 中imitation learning cost 上加一个some cost related to constraints 是一个非常不靠谱的事情。首先一个原因当然是recursive feasibility 问题。因为我们的decision 不管是以control 还是planning 的形式，都是一个finite horizon 的。我们没法解一个全局问题（同时涉及未来不可预测），来保证constraint satisfaction。 还有一个是有imitation learning 还要用这个additional cost 中positive sample & negative sample 问题。同时，个人的另一个观点是在learning 的方法中，我们是在从一个high dimensional space map 到我们想要的output low dimensional space. 我们的constraint related cost 是在low dimensional space, 期望他去properly space 这样high dimensional feature space, 同时在有一个imitation learning 的cost 在，不是一个properly defined problem (maybe work, maybe not), from 解optimization 角度还取决于initialization & gradient update。 没有说一定不work，但是一个东西如果你没想清楚他为什么会work那很大概率他是不会work的。

回到这篇文章本身，文章本身的整个框架其实并不令人惊讶，其中的output generator + output evaluator 甚至refinement 可能在很多自动驾驶有关的文章和公司都能类似的影子。但是除了生成器用vae 来减少对数据分布假设的依赖之外，我觉得这个文章值得人思考的是如何做output evaluator 和对于output refinement 设计逻辑的思考。

### grasp pose evaluation (there is another recent similar paper motivates me to think about occ as evaluator)
- grasp sampling 
- Success of a grasp pose depends on the <mark>relative pose</mark> of the grasp with respect to the object
- represent a grasp g in a way more closely tied to the object point cloud --  unified point cloud $X \cup X_g$, use pointnet++ relative info
- negative samples: the grasps that have similar pose to a positive grasp but that are either in collision with the object or are too far from the object to grasp the object (gan related?)
### Iterative Grasp Pose Refinement
a large portion of the rejected grasps can be close to successful ones
we are looking for a refining transformation ∆g that increases the proba- bility of success, i.e., P (s = 1 | g + ∆g) > P (s = 1 | g).



