提纲：
- 自我介绍
    - 阿里，云原生应用平台，应用交付和管理，大规模集群管理
    - CoreOS, K8s commiter, k8s Operator 发起人
- OAM：自包含、关注点分离的云原生应用定义
    - [构建以应用为中心的 Kubernetes](https://zhuanlan.zhihu.com/p/183857762)
        - k8s 统一了底层基础设施 (Compute, Network, Storage)，但是在应用层上没有进一步往上抽象。大家在 k8s 上主要以使用单个 workload 为主。一旦能力多了，整个管理和扩展的难度会增大，且用户体验也不够友好。
        - 尝试往应用层做抽象，做统一的云原生应用管理架构。
    - 云原生应用管理架构的领域建模:
        -  [Clean architecture](https://medium.com/@hatajoe/clean-architecture-in-go-4030f11ec1b1): 可扩展、易标准化的架构
            - 越往里，依赖越低，抽象程度越高，接口设计越稳定
        - 核心层为通用的应用模型 (OAM) ⇒ PaaS 平台基础能力 ⇒ 用户展示和体验层
        ![img/oam-domain-arch.jpg](./img/oam-domain-arch.jpg)
    - OAM 基本概念
        - WorkloadDefinition: [cloneset workload definition](https://github.com/oam-dev/catalog/blob/master/workloads/cloneset/workloadDefinition.yaml)
        - TraitDefinition: [ingress trait definition](https://github.com/oam-dev/catalog/blob/master/traits/ingresstrait/config/oam/trait_definition.yaml)
        - [Component](https://github.com/oam-dev/samples/tree/master/2.ServiceTracker_App/Components)
        - [ApplicationConfiguration](https://github.com/oam-dev/samples/blob/master/2.ServiceTracker_App/ApplicationConfiguration/tracker-app-config-managed.yaml)
            - 自包含：所有服务组件和运维能力都在一个定义里。
            - 结构化：traits attached to components，避免了 label selector 等关联资源的方式，让用户使用更简单，体验也更友好。
				![oam-simplify-view.png](.//img/oam-simplify-view.png)

        - Demo
            - Install [KinD](https://kind.sigs.k8s.io/docs/user/quick-start/#installation)
            - Follow the [README](./oam/README.md) to deploy OAM app
    - 关注点分离:
![oam-workflow.png](./img/oam-workflow.png)
- CUE configuration language
    - [K8s yaml 管理的窘境](https://blog.cedriccharly.com/post/20191109-the-configuration-complexity-curse/)
        - Templating, Overlay, DSL
        - 简单统一 ⇒ 复杂多样 ⇒ 简单统一
    - [CUE 用法入门](https://cuelang.org/docs/tutorials/tour/intro/)
    - [用 CUE 管理 k8s yaml](https://github.com/cuelang/cue/blob/v0.2.2/doc/tutorial/kubernetes/README.md)
- CUE 跟 OAM 的关系
    - OAM 是服务器端的更高级别抽象，达到以应用为中心。
    - CUE 是客户端工具，为了更高效准确地编写和管理配置文件。
    - OAM 可以通过 CUE 透出更简单易用的模板：CUE templates ⇒ GitOps/IaC ⇒ OAM
