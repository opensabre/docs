<section class="os-home-hero os-home-hero-v2">
  <div class="os-home-copy">
    <div class="os-home-mark">Spring Cloud 2024 · Framework 0.4</div>
    <h1>Opensabre</h1>
    <p class="os-home-lead">面向 Spring Cloud 微服务的基础框架与治理套件，把工程规范、通用 starter、审计日志、限次治理和基础应用沉淀成可直接复用的体系。</p>
    <div class="os-home-actions">
      <a href="#/framework/manual/QUICKSTART.md">快速开始</a>
      <a href="#/framework/README.md" class="secondary">查看框架文档</a>
    </div>
    <div class="os-home-proof">
      <span>JDK 17+</span>
      <span>Spring Boot 3.4</span>
      <span>Spring Cloud 2024</span>
      <span>Governance SDK</span>
    </div>
  </div>
  <div class="os-architecture-panel os-architecture-panel-v2" aria-label="Opensabre architecture">
    <div class="os-panel-top">
      <span>opensabre stack</span>
      <strong>0.4.0</strong>
    </div>
    <div class="os-stack-base">opensabre-base-dependencies</div>
    <div class="os-stack-lanes">
      <div>
        <strong>Web</strong>
        <span>webmvc</span>
        <span>webflux</span>
      </div>
      <div>
        <strong>Runtime</strong>
        <span>boot</span>
        <span>rpc</span>
      </div>
      <div>
        <strong>Data</strong>
        <span>persistence</span>
        <span>cache</span>
      </div>
      <div>
        <strong>Governance</strong>
        <span>audit</span>
        <span>ratelimit</span>
      </div>
    </div>
    <div class="os-stack-flow">
      <span>config</span>
      <span>register</span>
      <span>eda</span>
      <span>sysadmin</span>
    </div>
  </div>
</section>

<section class="os-entry-grid">
  <a href="#/framework/README.md" class="os-entry-card">
    <strong>服务框架</strong>
    <span>查看 starter 模块、依赖组合、基础能力和治理 SDK。</span>
  </a>
  <a href="#/baseapp/README.md" class="os-entry-card">
    <strong>基础应用</strong>
    <span>了解组织、授权、系统管理等基础服务的业务边界。</span>
  </a>
  <a href="#/PROJECT.md" class="os-entry-card">
    <strong>项目清单</strong>
    <span>快速定位 framework、sysadmin、organization、examples 和 docs 仓库。</span>
  </a>
</section>

## 近期更新

<div class="os-release-strip">
  <div>
    <strong>0.4.0</strong>
    <span>新增 governance starter，审计日志和限次能力以 SDK 方式接入，统一调用 sysadmin 管理。</span>
  </div>
  <div>
    <strong>0.3.0</strong>
    <span>新增 WebFlux starter，重构 WebMVC / WebFlux / opensabre-web 的模块边界。</span>
  </div>
</div>

## 能力地图

| 能力 | 入口 | 说明 |
| --- | --- | --- |
| WebMVC / WebFlux | [框架模块设计](#/framework/architecture/MODULES.md) | 按技术栈选择 Web starter，减少应用侧手动补依赖。 |
| 审计与限次 | [治理 SDK](#/framework/manual/governance.md) | `@Audit`、`@RateLimit`、AOP、自动装配，sysadmin 统一入库和计数。 |
| 持久化 | [数据持久化](#/framework/manual/persistence.md) | MyBatis-Plus、分页、SQL 拦截和持久化异常处理。 |
| 多级缓存 | [多级缓存](#/framework/manual/cache.md) | JetCache Redis 多级缓存默认配置。 |
| 注册与配置 | [注册中心](#/framework/manual/discovery.md) / [配置中心](#/framework/manual/config.md) | Nacos 注册发现与配置中心集成。 |
| 远程调用 | [远程调用](#/framework/manual/rpc.md) | OpenFeign、LoadBalancer、Sentinel 相关配置。 |

## 推荐阅读路径

1. 阅读 [快速入门](#/framework/manual/QUICKSTART.md)，创建一个最小 WebMVC 服务。
2. 阅读 [框架模块设计](#/framework/architecture/MODULES.md)，确认应用需要引入哪些 starter。
3. 阅读 [治理 SDK](#/framework/manual/governance.md)，按需接入审计日志和限次能力。
4. 阅读 [版本说明](#/framework/VERSONS.md)，了解 0.3 与 0.4 的升级内容。
