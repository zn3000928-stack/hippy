<template>
  <div class="app">
    <!-- 背景光斑（多蓝渐变 + 模糊） -->
    <div class="bg">
      <div class="blob b1"></div>
      <div class="blob b2"></div>
      <div class="blob b3"></div>
      <div class="grid-noise"></div>
    </div>

    <!-- 顶部导航 -->
    <header class="nav glass">
      <div class="nav-left" @click="go('home')" style="cursor:pointer">
        <div class="logo">康膳打印</div>
        <span class="sub">中医辨体驱动 · 社区 AI 营养定制打印平台</span>
      </div>

      <nav class="nav-tabs">
        <button :class="{active:page==='home'}" @click="go('home')">官网</button>
        <button :class="{active:page==='library'}" @click="go('library')">资料库</button>
        <button :class="{active:page==='tcm'}" @click="go('tcm')">中医辨体</button>
        <button :class="{active:page==='platform'}" @click="go('platform')">平台</button>
        <button :class="{active:page==='community'}" @click="go('community')">社区</button>
      </nav>

      <div class="nav-right">
        
        <button class="btn ghost" @click="go('platform'); tab='devices'">设备参数</button>
        <button class="btn primary" @click="openCreateTask()">新建打印任务</button>
      </div>
    </header>

    <!-- 手机端底部导航 -->
    <nav class="bottom-nav glass">
      <button :class="{active:page==='home'}" @click="go('home')">
        <span class="i">⌂</span><span>官网</span>
      </button>
      <button :class="{active:page==='library'}" @click="go('library')">
        <span class="i">⌁</span><span>资料</span>
      </button>

      <button :class="{active:page==='tcm'}" @click="go('tcm')">
        <span class="i">✦</span><span>辨体</span>
      </button>
      <button :class="{active:page==='platform'}" @click="go('platform')">
        <span class="i">▦</span><span>平台</span>
      </button>
      <button :class="{active:page==='community'}" @click="go('community')">
        <span class="i">☰</span><span>社区</span>
      </button>
    </nav>

    <!-- Toast -->
    <div v-if="toastMsg" class="toast glass">
      <div class="t-title">提示</div>
      <div class="t-msg">{{ toastMsg }}</div>
    </div>

    <main class="main">

      <!-- 官网 -->
      <section v-if="page==='home'" class="page home">

        <!-- 官网页内导航（锚点） -->
        <div class="home-nav glass">
          <button @click="scrollTo('sec-hero')">总览</button>
          <button @click="scrollTo('sec-integrate')">集成</button>
          <button @click="scrollTo('sec-feature')">能力</button>
          <button @click="scrollTo('sec-case')">场景</button>
          <button @click="go('platform'); tab='ops'">运营</button>
        </div>

        <!-- Hero -->
        <section id="sec-hero" class="hero">
          <div class="hero-text">
            <div class="pill">FoodTech 流程</div>
            <h1>把体质与营养<br/>变成可执行的打印任务</h1>
            <p class="muted">
              面向社区站与养老机构，把“中医体质 → 营养策略 → 配方参数 → 设备约束 → 站点运营”做成闭环系统。<br/>
              你不只获得报告，而是获得“可打印、可追踪、可复盘”的服务链路。
            </p>

            <div class="hero-actions">
              <button class="btn primary" @click="go('tcm')">开始体质评估</button>
              <button class="btn ghost" @click="go('platform')">进入控制台</button>
              <button class="btn ghost" @click="go('platform'); tab='ingredients'">查看原料库</button>
            </div>

            <div class="kpis">
              <div class="kpi glass">
                <div class="kpi-num">{{ kpiSummary.menus }}</div>
                <div class="kpi-label">可组合菜单方案</div>
              </div>
              <div class="kpi glass">
                <div class="kpi-num">{{ kpiSummary.loss }}%</div>
                <div class="kpi-label">降低摩擦与流失</div>
              </div>
              <div class="kpi glass">
                <div class="kpi-num">{{ kpiSummary.upsell }}%</div>
                <div class="kpi-label">提升追加转化</div>
              </div>
              <div class="kpi glass">
                <div class="kpi-num">{{ kpiSummary.insight }}</div>
                <div class="kpi-label">运营可操作洞察</div>
              </div>
            </div>
          </div>
          



          <div class="hero-visual">
            <div class="promo ">
    <div class="inner-head">
   </div>
  </div>


            <div class="preview glass">
              <div class="pv-head">
                <div class="pv-title">平台预览 · 一屏看懂</div>
                <div class="pv-sub muted">站点状态 / 设备参数 / 推荐配方 / 运营告警</div>
              </div>

              <div class="pv-grid">
                <div class="pv-card glass">
                  <div class="pv-k">站点在线</div>
                  <div class="pv-v">{{ stationsOnline }}/{{ stations.length }}</div>
                  <div class="pv-tip muted">正常运行 · 自动告警</div>
                </div>
                <div class="pv-card glass">
                  <div class="pv-k">设备状态</div>
                  <div class="pv-v">{{ devicesOnline }}/{{ devices.length }}</div>
                  <div class="pv-tip muted">参数可调 · 清洗策略</div>
                </div>
                <div class="pv-card glass">
                  <div class="pv-k">低库存</div>
                  <div class="pv-v">{{ lowStockCount }}</div>
                  <div class="pv-tip muted">原料预警</div>
                </div>
                <div class="pv-card glass">
                  <div class="pv-k">今日任务</div>
                  <div class="pv-v">{{ queue.length }}</div>
                  <div class="pv-tip muted">排队 · 改派 · 复盘</div>
                </div>
              </div>

              <div class="pv-strip glass">
                <div class="strip-left">
                  <div class="strip-title">数据闭环</div>
                  <div class="muted small">体质 → 配方 → 设备 → 任务 → 运营</div>
                </div>
                <button class="btn primary sm" @click="go('platform'); tab='ops'">查看运营</button>
              </div>
            </div>

            <div class="hero-note muted">
             
            </div>
          </div>
        </section>


<div class="video-card video-section">
  <div class="video-head">
    <h2 class="video-title">How Our DietaryAI Transforms Your Data</h2>
    <p class="video-sub">辨体 → 营养映射 → 配方 → 打印执行 → 反馈闭环</p>
    <p class="video-sub2">让每一餐都“可解释、可追踪、可落地”的社区健康服务。</p>
  </div>

  <div class="video-wrap">
    <iframe
      class="video-iframe"
      :src="videoEmbedUrl"
      title="Demo Video"
      frameborder="0"
      allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
      allowfullscreen
    ></iframe>
  </div>
</div>


  <!-- ✅ 替换链接输入 -->
  <div v-if="showReplaceVideo" class="video-replace">
    <input class="input" v-model="videoUrlDraft" placeholder="粘贴 YouTube 链接（watch?v= / youtu.be / embed）" />
    <div class="video-replace-actions">
      <button class="btn primary sm" @click="applyVideoUrl()">应用</button>
      <button class="btn ghost sm" @click="cancelReplaceVideo()">取消</button>
    </div>
  </div>


<!-- ✅ 放大弹窗 -->
<div v-if="videoModalOpen" class="modal-mask" @click.self="closeVideoModal()">
  <div class="modal-card glass">
    <div class="modal-head">
      <div>
        <div class="modal-title">Food Printing Demo</div>
        <div class="muted small">辨体评估 · 配方生成 · 设备执行 · 反馈闭环</div>
      </div>
      <button class="btn ghost sm" @click="closeVideoModal()">关闭</button>
    </div>
    <div class="modal-video">
      <iframe
        class="video-iframe"
        :src="videoEmbedUrl"
        title="Demo Video Large"
        frameborder="0"
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
        allowfullscreen
      ></iframe>
    </div>
  </div>
</div>


        <!-- 集成三步 -->
        <section id="sec-integrate" class="section">
          <div class="section-head">
            <div class="pill">开始使用</div>
            <h2>开始集成：三步简单操作</h2>
            <p class="muted">提供对接清单：用户/站点/设备/配方/运营数据一体化。</p>
          </div>

          <div class="steps">
            <div class="step glass">
              <div class="step-num">1</div>
              <h3>请求访问</h3>
              <p class="muted">获取站点绑定与权限策略</p>
            </div>
            <div class="step glass">
              <div class="step-num">2</div>
              <h3>集成启动</h3>
              <p class="muted">同步用户与菜单，映射禁忌标签</p>
            </div>
            <div class="step glass">
              <div class="step-num">3</div>
              <h3>正式上线</h3>
              <p class="muted">配方→任务→设备→运营闭环</p>
            </div>
          </div>

          <div class="center" style="margin-top:16px">
            <button class="btn primary" @click="openQuickHelp()">生成对接清单 →</button>
          </div>
        </section>

        <!-- 能力总览 -->
        <section id="sec-feature" class="section">
          <div class="section-head">
            <div class="pill">能力总览</div>
            <h2>你要的模块都在</h2>
            <p class="muted">机构站点 / 用户 / 设备参数可调 / 原料库 / 配方编辑 / 任务队列 / 运营趋势与告警 / 社区。</p>
          </div>

          <div class="features">
            <div class="feature glass">
              <h3>中医辨体（36题）</h3>
              <p class="muted">问卷→评分→主次体质→策略→推荐配方。</p>
              <button class="btn ghost sm" @click="go('tcm')">去评估</button>
            </div>

            <div class="feature glass">
              <h3>机构 / 站点</h3>
              <p class="muted">社区站、养老机构、分站点与设备绑定。</p>
              <button class="btn ghost sm" @click="go('platform'); tab='org'">去管理</button>
            </div>

            <div class="feature glass">
              <h3>用户管理</h3>
              <p class="muted">老人/营养师/操作员，多角色与站点归属。</p>
              <button class="btn ghost sm" @click="go('platform'); tab='users'">看用户</button>
            </div>

            <div class="feature glass">
              <h3>设备参数可调</h3>
              <p class="muted">喷嘴/层高/温控/速度/回抽/压力阈值/搅拌/清洗周期。</p>
              <button class="btn ghost sm" @click="go('platform'); tab='devices'">调参数</button>
            </div>

            <div class="feature glass">
              <h3>原料库 + 库存</h3>
              <p class="muted">原料营养标签、库存预警、配方引用。</p>
              <button class="btn ghost sm" @click="go('platform'); tab='ingredients'">看原料</button>
            </div>

            <div class="feature glass">
              <h3>站点运营（更好看）</h3>
              <p class="muted">折线 + 渐变面积图 + 告警中心 + 动作建议。</p>
              <button class="btn ghost sm" @click="go('platform'); tab='ops'">看运营</button>
            </div>
          </div>
        </section>

        <!-- 场景 -->
        <section id="sec-case" class="section">
          <div class="section-head">
            <div class="pill">典型场景</div>
            <h2>社区养老场景的“可落地闭环”</h2>
            <p class="muted">从站点运营出发，反推设备约束与配方策略。</p>
          </div>

          <div class="scene-grid">
            <div class="scene glass">
              <div class="scene-title">社区站点 · 日常供餐</div>
              <div class="muted">高频、多批次、稳定出料与清洗策略优先。</div>
              <div class="chips" style="margin-top:10px">
                <span class="chip ghost">排队调度</span>
                <span class="chip ghost">低库存预警</span>
                <span class="chip ghost">成功率复盘</span>
              </div>
            </div>
            <div class="scene glass">
              <div class="scene-title">养老机构 · 个体化营养</div>
              <div class="muted">体质/疾病/质构等级需要配方版本管理。</div>
              <div class="chips" style="margin-top:10px">
                <span class="chip ghost">体质标签</span>
                <span class="chip ghost">过敏禁忌</span>
                <span class="chip ghost">版本回滚</span>
              </div>
            </div>
            <div class="scene glass">
              <div class="scene-title">运营管理 · 站点KPI</div>
              <div class="muted">告警、订单、成功率、耗材、人员培训一体化。</div>
              <div class="chips" style="margin-top:10px">
                <span class="chip ghost">告警中心</span>
                <span class="chip ghost">趋势图</span>
                <span class="chip ghost">动作建议</span>
              </div>
            </div>
          </div>

          <div class="center" style="margin-top:16px">
            <button class="btn primary" @click="go('platform')">现在进入控制台 →</button>
          </div>
        </section>

        <footer class="footer ">
          <div class="footer-grid">
            <div>
              <div class="logo small">foodini · style</div>
              <div class="muted">定制每一口营养 打印每一份安心</div>
              <div class="social">
                <span class="s">f</span><span class="s">in</span><span class="s">ig</span><span class="s">♪</span>
              </div>
            </div>
            <div>
              <div class="ft-title">SOLUTIONS</div>
              <div class="ft-link" @click="go('platform'); tab='ops'">FoodTech</div>
              <div class="ft-link" @click="go('platform'); tab='org'">Sites & Orgs</div>
              <div class="ft-link" @click="go('platform'); tab='devices'">Devices</div>
              <div class="ft-link" @click="go('community')">Community</div>
            </div>
            <div>
              <div class="ft-title">COMPANY</div>
              <div class="ft-link">Mission</div>
              <div class="ft-link">Privacy Policy</div>
              <div class="ft-link">Terms of Service</div>
            </div>
          </div>
          <div class="muted center" style="margin-top:16px">© 2025 康膳打印</div>
        </footer>
      </section>

<!-- 资料库 -->
<section v-if="page==='library'" class="page library">
  <div class="page-head">
    <div>
      <h2>资料库</h2>
      <p class="muted">科普知识 · 设备参数解释 · 项目成果（可持续补充）</p>
    </div>
    <div class="head-actions">
      <input class="input" style="width:260px" v-model="libraryQuery" placeholder="搜索：关键词/参数/成果" />
    </div>
  </div>

  <div class="tabs2 glass">
    <button :class="{active:libraryTab==='science'}" @click="libraryTab='science'">科普</button>
    <button :class="{active:libraryTab==='params'}" @click="libraryTab='params'">设备参数解释</button>
    <button :class="{active:libraryTab==='results'}" @click="libraryTab='results'">阶段成果</button>
  </div>

  <!-- 科普 -->
  <section v-if="libraryTab==='science'" class="grid2">
    <div class="glass panel">
      <div class="panel-head">
        <h3>基础科普</h3>
        <div class="muted small">Food Printing / 质构 / 安全</div>
      </div>
      <div class="list">
        <div class="row" v-for="a in filteredLibraryScience" :key="a.id" @click="openLibraryItem(a)">
          <div>
            <div class="title">{{ a.title }}</div>
            <div class="muted small">{{ a.brief }}</div>
          </div>
          <div class="chips">
            <span class="chip ghost" v-for="t in a.tags.slice(0,2)" :key="t">{{ t }}</span>
          </div>
        </div>
      </div>
    </div>

    <div class="glass panel">
      <div class="panel-head">
        <h3>阅读区</h3>
        <div class="muted small"></div>
      </div>

      <div v-if="!libraryActive" class="muted">选择左侧任意条目，我会在这里展示内容。</div>

      <div v-else class="glass inner">
        <div class="title">{{ libraryActive.title }}</div>
        <div class="chips" style="margin-top:10px">
          <span class="chip ghost" v-for="t in libraryActive.tags" :key="t">{{ t }}</span>
        </div>
        <div class="content">{{ libraryActive.content }}</div>
      </div>
    </div>
  </section>

  <!-- 参数解释 -->
  <section v-if="libraryTab==='params'" class="grid2">
    <div class="glass panel">
      <div class="panel-head">
        <h3>你最关心的 6 个参数</h3>
        <div class="muted small">喷头 / 层高 / 速度 / 温度 / 回抽 / 流量倍率</div>
      </div>

      <div class="list">
        <div class="row" v-for="p in filteredLibraryParams" :key="p.key" @click="openParam(p)">
          <div>
            <div class="title">{{ p.name }}</div>
            <div class="muted small">{{ p.oneLine }}</div>
          </div>
          <div class="chips">
            <span class="chip ghost">{{ p.unit }}</span>
            <span class="chip run">建议</span>
          </div>
        </div>
      </div>
    </div>

    <div class="glass panel">
      <div class="panel-head">
      


<!-- 对照表 -->
<div v-if="paramView==='table'" class="glass inner" style="margin-top:12px">
  <div class="inner-head">
    <h4>参数对照表（当前设备 vs 推荐范围）</h4>
    <button class="btn ghost sm" @click="logDeviceParams(getParamDevice())">记录一次（采样）</button>
  </div>

  <table class="table">
    <thead>
      <tr>
        <th>参数</th><th>当前值</th><th>推荐范围（演示）</th><th>作用/风险提示</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="r in paramCompareRows" :key="r.key">
        <td>{{ r.name }}</td>
        <td><span class="chip ghost">{{ r.value }}</span></td>
        <td class="muted">{{ r.range }}</td>
        <td class="muted small">{{ r.note }}</td>
      </tr>
    </tbody>
  </table>
</div>


        <h3>解释与建议</h3>
        <div class="muted small"></div>
      </div>

      <div v-if="!paramActive" class="muted">点左侧参数查看解释。</div>

      <div v-else class="glass inner">
        <div class="title">{{ paramActive.name }}</div>
        <div class="muted small" style="margin-top:6px">单位：{{ paramActive.unit }} · 常见范围：{{ paramActive.range }}</div>
        <div class="content" style="margin-top:10px">{{ paramActive.detail }}</div>

        <div class="glass inner" style="margin-top:12px">
          <div class="inner-head">
            <h4>调参口诀（演示）</h4>
            <button class="btn ghost sm" @click="copyParamTip(paramActive)">复制</button>
          </div>
          <ul class="muted">
            <li v-for="x in paramActive.tips" :key="x">{{ x }}</li>
          </ul>
        </div>
      </div>
    </div>
  </section>

  <!-- 成果 -->
  <section v-if="libraryTab==='results'" class="grid2">
    <div class="glass panel">
      <div class="panel-head">
        <h3>阶段成果</h3>
      </div>

      <div class="ops-cards">
        <div class="mini">
          <div class="muted small">已沉淀配方</div>
          <div class="big">{{ resultsSummary.recipes }}</div>
        </div>
        <div class="mini">
          <div class="muted small">已覆盖站点</div>
          <div class="big">{{ resultsSummary.stations }}</div>
        </div>
        <div class="mini">
          <div class="muted small">演示任务</div>
          <div class="big">{{ resultsSummary.tasks }}</div>
        </div>
      </div>

      <div class="glass inner" style="margin-top:12px">
        <div class="inner-head">
          <h4>里程碑</h4>
          <button class="btn ghost sm" @click="addMilestone()">新增里程碑</button>
        </div>
        <div class="list">
          <div class="row" v-for="m in filteredMilestones" :key="m.id">
            <div>
              <div class="title">{{ m.title }}</div>
              <div class="muted small">{{ m.date }} · {{ m.note }}</div>
            </div>
            <div class="chips">
              <span class="chip ghost">{{ m.type }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="glass panel">
      <div class="panel-head">
      </div>

      <div class="media-slot big" @click="pickResultImage()">
        <img v-if="resultsHeroImage" :src="resultsHeroImage" class="media-img" />
        <div v-else class="media-placeholder">
          <div class="muted small" style="margin-top:6px">点击上传成果展示图</div>
        </div>
      </div>

      <input ref="resultImgInput" type="file" accept="image/*" style="display:none" @change="onPickResultImage" />
      <div class="inline" style="margin-top:12px;justify-content:flex-end">
        <button class="btn ghost sm" @click="clearResultImage()">清除</button>
      </div>
    </div>
  </section>
</section>

      <!-- 中医辨体 -->
      <section v-if="page==='tcm'" class="page tcm">
        <div class="page-head">
          <div>
            <h2>中医体质评估（36题）</h2>
            <p class="muted">问卷 → 打分 → 主次体质 → 策略 → 推荐配方（可推送到配方编辑）。</p>
          </div>
          <div class="head-actions">
            <button class="btn ghost" @click="resetTCM()">重置</button>
            <button class="btn primary" @click="submitTCM()">提交评估</button>
          </div>
        </div>

        <div class="tcm-layout">
          <div class="glass tcm-card">
            <div class="tcm-top">
              <h3 style="margin:0">问卷</h3>
              <div class="muted small">已答：{{ answeredCount }}/{{ tcmQuestions.length }}</div>
            </div>

            <div class="q" v-for="q in tcmQuestions" :key="q.id">
              <div class="q-title">{{ q.title }}</div>
              <div class="q-options">
                <label class="opt" v-for="o in q.options" :key="o.v">
                  <input type="radio" :name="'q'+q.id" :value="o.v" v-model.number="tcmAnswers[q.id]" />
                  <span>{{ o.t }}</span>
                </label>
              </div>
            </div>
          </div>

          <div class="glass tcm-result">
            <h3>评分结果</h3>

            <div v-if="!tcmResult.ready" class="muted">
              选择问卷后点击「提交评估」。我会给出：<br/>
              ① 主体质 ② 次体质 ③ 策略建议 ④ 推荐配方入口
            </div>

            <div v-else>
              <div class="result-hero">
                <div class="badge">主：{{ tcmResult.primary }}</div>
                <div class="badge ghost">次：{{ tcmResult.secondary }}</div>
              </div>

              <div class="muted" style="margin:10px 0 16px">
                规则引擎演示（后续你可替换为 AI 模型 / 专家规则库）。
              </div>

              <div class="bars">
                <div class="bar-row" v-for="b in tcmResult.bars" :key="b.k">
                  <div class="bar-name">{{ b.k }}</div>
                  <div class="bar-track">
                    <div class="bar-fill" :style="{width: b.v + '%'}"></div>
                  </div>
                  <div class="bar-val">{{ b.v }}</div>
                </div>
              </div>

              <div class="glass advice">
                <h4>饮食与打印策略（建议）</h4>
                <ul>
                  <li v-for="a in tcmResult.advice" :key="a">{{ a }}</li>
                </ul>
                <div class="ad-actions">
                  <button class="btn primary sm" @click="createRecipeFromTCM()">生成推荐配方</button>
                  <button class="btn ghost sm" @click="go('platform'); tab='recipes'">去配方编辑</button>
                </div>
              </div>

              <div class="glass advice" style="margin-top:12px">
                <h4>设备参数提示（自动映射）</h4>
                <ul>
                  <li v-for="a in tcmResult.deviceHints" :key="a">{{ a }}</li>
                </ul>
                <div class="ad-actions">
                  <button class="btn ghost sm" @click="go('platform'); tab='devices'">查看设备参数</button>
                  <button class="btn ghost sm" @click="go('platform'); tab='ops'">查看站点运营</button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- 平台控制台 -->
      <section v-if="page==='platform'" class="page platform">
        <div class="page-head">
          <div>
            <h2>平台控制台</h2>
            <p class="muted"></p>
          </div>
          <div class="head-actions">
            <button class="btn ghost" @click="saveAll()">保存</button>
            <button class="btn primary" @click="openCreateTask()">新建任务</button>
          </div>
        </div>

        <div class="tabs2 glass">
          <button :class="{active:tab==='org'}" @click="tab='org'">机构/站点</button>
          <button :class="{active:tab==='users'}" @click="tab='users'">用户</button>
          <button :class="{active:tab==='devices'}" @click="tab='devices'">设备参数</button>
          <button :class="{active:tab==='ingredients'}" @click="tab='ingredients'">原料库</button>
          <button :class="{active:tab==='recipes'}" @click="tab='recipes'">配方编辑</button>
          <button :class="{active:tab==='queue'}" @click="tab='queue'">任务队列</button>
          <button :class="{active:tab==='ops'}" @click="tab='ops'">站点运营</button>
        </div>

        <!-- ORG -->
        <section v-if="tab==='org'" class="grid2">
          <div class="glass panel">
            <div class="panel-head">
              <h3>机构列表</h3>
              <button class="btn ghost sm" @click="addOrg()">新增机构</button>
            </div>

            <div class="list">
              <div class="row" v-for="o in orgs" :key="o.id" @click="selectOrg(o.id)" :class="{active: o.id===selectedOrgId}">
                <div>
                  <div class="title">{{ o.name }}</div>
                  <div class="muted small">{{ o.type }} · {{ o.city }}</div>
                </div>
                <div class="pill2">{{ orgStations(o.id).length }} 个站点</div>
              </div>
            </div>
          </div>

          <div class="glass panel" v-if="selectedOrg">
            <div class="panel-head">
              <h3>站点（{{ selectedOrg.name }}）</h3>
              <button class="btn ghost sm" @click="addStation()">新增站点</button>
            </div>


            <div class="list">
              <div class="row" v-for="s in orgStations(selectedOrg.id)" :key="s.id" @click="selectStation(s.id)" :class="{active: s.id===selectedStationId}">
                <div>
                  <div class="title">{{ s.name }}</div>
                  <div class="muted small">{{ s.address }}</div>
                </div>
                <div class="chips">
                  <span class="chip" :class="s.status==='正常'?'ok':(s.status==='告警'?'warn':'off')">{{ s.status }}</span>
                  <span class="chip ghost">{{ stationDevices(s.id).length }} 台设备</span>
                </div>
              </div>
            </div>

            <div v-if="selectedStation" class="glass inner">
              <div class="inner-head">
                <h4>站点概览</h4>
                <button class="btn ghost sm" @click="simulateAlarm()">模拟告警</button>
              </div>

              <div class="cards3">
                <div class="mini">
                  <div class="muted small">今日订单</div>
                  <div class="big">{{ stationKpi(selectedStation.id).orders }}</div>
                </div>
                <div class="mini">
                  <div class="muted small">成功率</div>
                  <div class="big">{{ stationKpi(selectedStation.id).success }}%</div>
                </div>
                <div class="mini">
                  <div class="muted small">告警</div>
                  <div class="big">{{ stationKpi(selectedStation.id).alerts }}</div>
                </div>
              </div>

              <div class="muted small" style="margin-top:10px">
                运营趋势和告警中心在「站点运营」Tab（折线+面积图）。
              </div>
            </div>
          </div>

          <div class="glass panel" v-else>
            <h3>提示</h3>
            <p class="muted">点击左侧机构查看站点与设备绑定。</p>
          </div>
        </section>

        <!-- USERS -->
        <section v-if="tab==='users'" class="grid2">
          <div class="glass panel">
            <div class="panel-head">
              <h3>用户列表</h3>
              <div class="inline">
                <select class="select" v-model="userFilter.role">
                  <option value="">全部角色</option>
                  <option value="老人">老人</option>
                  <option value="营养师">营养师</option>
                  <option value="操作员">操作员</option>
                </select>
                <button class="btn ghost sm" @click="addUser()">新增用户</button>
              </div>
            </div>

            <div class="list">
              <div class="row" v-for="u in filteredUsers" :key="u.id" @click="selectUser(u.id)" :class="{active:u.id===selectedUserId}">
                <div>
                  <div class="title">{{ u.name }}</div>
                  <div class="muted small">{{ u.role }} · 归属：{{ stationName(u.stationId) || '未绑定' }}</div>
                </div>
                <div class="chips">
                  <span class="chip ghost">体质：{{ (u.tcm?.primary)||'—' }}</span>
                  <span class="chip" :class="u.risk==='高'?'danger':(u.risk==='中'?'warn':'ok')">风险 {{ u.risk }}</span>
                </div>
              </div>
            </div>
          </div>

          <div class="glass panel" v-if="selectedUser">
            <div class="panel-head">
              <h3>用户档案</h3>
              <div class="inline">
                <button class="btn ghost sm" @click="bindUserToStation()">绑定站点</button>
                <button class="btn ghost sm" @click="go('tcm')">去辨体</button>
                <button class="btn primary sm" @click="openCreateTaskForUser(selectedUser.id)">为Ta建任务</button>
              </div>
            </div>

            <div class="glass inner">
              <div class="title">{{ selectedUser.name }}</div>
              <div class="muted small">
                角色：{{ selectedUser.role }}<br/>
                站点：{{ stationName(selectedUser.stationId) || '未绑定' }}<br/>
                年龄：{{ selectedUser.age || '—' }}<br/>
                备注：{{ selectedUser.note || '—' }}
              </div>
            </div>

            <div class="glass inner" style="margin-top:12px">
              <div class="inner-head">
                <h4>体质信息</h4>
                <button class="btn ghost sm" @click="mockUserTCM()">生成示例体质</button>
              </div>
              <div class="muted small">
                主：{{ selectedUser.tcm?.primary || '—' }} · 次：{{ selectedUser.tcm?.secondary || '—' }}
              </div>
              <div class="chips" style="margin-top:10px">
                <span class="chip ghost" v-for="t in (selectedUser.tags||[])" :key="t">{{ t }}</span>
              </div>
            </div>
          </div>

          <div class="glass panel" v-else>
            <h3>提示</h3>
            <p class="muted">选择一个用户查看档案。</p>
          </div>
        </section>

        <!-- DEVICES -->
        <section v-if="tab==='devices'" class="grid2">
          <div class="glass panel">
            <div class="panel-head">
              <h3>设备列表</h3>
              <div class="inline">
                <select class="select" v-model="deviceFilter.stationId">
                  <option value="">全部站点</option>
                  <option v-for="s in stations" :key="s.id" :value="s.id">{{ s.name }}</option>
                </select>
                <button class="btn ghost sm" @click="addDevice()">新增设备</button>
                
                

              </div>
            </div>

            <div class="list">
              <div class="row" v-for="d in filteredDevices" :key="d.id" @click="selectDevice(d.id)" :class="{active:d.id===selectedDeviceId}">
                <div>
                  <div class="title">{{ d.name }}</div>
                  <div class="muted small">喷嘴 {{ d.nozzle }}mm · 层高 {{ d.layerHeight }}mm · 速度 {{ d.speed }}mm/s</div>
                </div>
                <div class="chips">
                  <span class="chip" :class="d.status==='在线'?'ok':(d.status==='打印中'?'run':'off')">{{ d.status }}</span>
                  <span class="chip ghost">站点：{{ stationName(d.stationId) }}</span>
                </div>
              </div>
            </div>
          </div>

          <div class="glass panel" v-if="selectedDevice">
            <div class="panel-head">
              <h3></h3>
              <div class="inline">
                <div class=" inner">

<div class="inner-head">
  <h4>设备运行态  </h4>


  <div class="head-actions">
    <div class="inline">
      <button class="btn ghost sm" @click="pickDeviceStatusImage()" :disabled="!selectedDevice">上传状态图</button>
      <button class="btn ghost sm" @click="clearDeviceStatusImage()" :disabled="!selectedDevice">清除</button>
    </div>

    <div class="inline">
      <button class="btn ghost sm" @click="quickClean(selectedDevice?.id)" :disabled="!selectedDevice">一键清洗</button>
      <button class="btn ghost sm" @click="toggleDevice(selectedDevice?.id)" :disabled="!selectedDevice">切换在线</button>
      <button class="btn primary sm" @click="saveAll()" :disabled="!selectedDevice">保存参数</button>
    </div>
  </div>
</div>



  <div class="device-live-grid">
    <div class="device-live-left">
      <div class="muted small">当前进度</div>
      <div class="pbar">
        <div class="pbar-fill" :style="{width: (selectedDevice.progress||0) + '%'}"></div>
      </div>
      <div class="muted small" style="margin-top:8px">
        
        {{ (selectedDevice.progress||0) }}% · {{ selectedDevice.status==='打印中' ? '打印中（演示）' : '待机/在线' }}
      </div>

      <div class="chips" style="margin-top:10px">
        <span class="chip ghost">喷嘴 {{ selectedDevice.nozzle }}mm</span>
        <span class="chip ghost">层高 {{ selectedDevice.layerHeight }}mm</span>
        <span class="chip ghost">速度 {{ selectedDevice.speed }}mm/s</span>
        <span class="chip ghost">温控 {{ selectedDevice.temp }}°C</span>
      </div>

      <div class="muted small" style="margin-top:10px">
      </div>
    </div>

    <div class="media-slot" @click="pickDeviceStatusImage()">
      <img v-if="selectedDevice.statusImage" :src="selectedDevice.statusImage" class="media-img" />
      <div v-else class="muted small" style="margin-top:6px">
        设备状态图预留窗口
      </div>
    </div>
  </div>

  <input ref="deviceStatusImgInput" type="file" accept="image/*" style="display:none" @change="onPickDeviceStatusImage" />
</div>


              </div>
            </div>

            <div class="glass inner">
              <div class="title">{{ selectedDevice.name }}</div>
              <div class="muted small">站点：{{ stationName(selectedDevice.stationId) }} · 状态：{{ selectedDevice.status }}</div>
            </div>

            <div class="glass inner" style="margin-top:12px">
              <h4>核心参数</h4>
              <div class="param-form">
                <div class="f">
                  <div class="lab">喷嘴直径 (mm)</div>
                  <input class="input" type="number" step="0.1" min="0.4" max="2.0" v-model.number="selectedDevice.nozzle" />
                </div>
                <div class="f">
                  <div class="lab">层高 (mm)</div>
                  <input class="input" type="number" step="0.1" min="0.2" max="1.0" v-model.number="selectedDevice.layerHeight" />
                </div>
                <div class="f">
                  <div class="lab">温控 (°C)</div>
                  <input class="input" type="number" min="20" max="60" v-model.number="selectedDevice.temp" />
                </div>
                <div class="f">
                  <div class="lab">速度 (mm/s)</div>
                  <input class="input" type="number" min="5" max="80" v-model.number="selectedDevice.speed" />
                </div>
              </div>

              <h4 style="margin-top:14px">稳定性参数</h4>
              <div class="param-form">
                <div class="f">
                  <div class="lab">回抽 (mm)</div>
                  <input class="input" type="number" step="0.1" min="0" max="10" v-model.number="selectedDevice.retract" />
                </div>
                <div class="f">
                  <div class="lab">压力阈值</div>
                  <input class="input" type="number" step="1" min="10" max="200" v-model.number="selectedDevice.pressureLimit" />
                </div>
                <div class="f">
                  <div class="lab">搅拌模式</div>
                  <select class="select" v-model="selectedDevice.mixMode">
                    <option>关闭</option>
                    <option>间歇</option>
                    <option>持续</option>
                  </select>
                </div>
                <div class="f">
                  <div class="lab">清洗周期（单）</div>
                  <input class="input" type="number" min="1" max="20" v-model.number="selectedDevice.cleanCycle" />
                </div>
              </div>

              <div class="muted small" style="margin-top:10px">
                配方纤维风险高 → 建议喷嘴≥1.2mm、层高≥0.6mm、回抽↑；压力接近阈值 → 触发告警/改派。
              </div>
            </div>

            <div class="glass inner" style="margin-top:12px">
              <div class="inner-head">
                <h4>风险评估</h4>
                <button class="btn ghost sm" @click="autoEvaluateDevice(selectedDevice.id)">重新评估</button>
              </div>
              <div class="chips">
                <span class="chip" :class="riskClass(selectedDevice.clogRisk)">堵塞风险：{{ selectedDevice.clogRisk }}</span>
                <span class="chip ghost">材料模式：{{ selectedDevice.materialMode }}</span>
              </div>
              <ul class="muted" style="margin-top:10px">
                <li>纤维含量高：喷嘴≥1.2mm；层高≥0.6mm；速度降低；回抽增加。</li>
                <li>蛋白糊：温控 35~45°C；避免结块；建议搅拌间歇或持续。</li>
                <li>连续任务多：按清洗周期插入清洗任务。</li>
              </ul>
            </div>
          </div>

          <div class="glass panel" v-else>
            <h3>提示</h3>
            <p class="muted">选择一台设备查看并调节参数。</p>
          </div>
          
        </section>

        <!-- INGREDIENTS -->
        <section v-if="tab==='ingredients'" class="grid2">
          <div class="glass panel">
            <div class="panel-head">
              <h3>原料库</h3>
              <div class="inline">
                <input class="input" placeholder="搜索：原料名/标签" v-model="ingredientQuery" />
                <button class="btn ghost sm" @click="addIngredient()">新增原料</button>
              </div>
            </div>

            <div class="list">
              <div
                class="row"
                v-for="it in filteredIngredients"
                :key="it.id"
                @click="selectIngredient(it.id)"
                :class="{active: it.id===selectedIngredientId}"
              >
                <div>
                  <div class="title">{{ it.name }}</div>
                  <div class="muted small">
                    库存：{{ it.stock }} · 预警线：{{ it.lowStockLine }} · 纤维风险：{{ it.fiberRisk }}
                  </div>
                </div>
                <div class="chips">
                  <span class="chip" :class="it.stock<=it.lowStockLine?'warn':'ok'">
                    {{ it.stock<=it.lowStockLine?'低库存':'正常' }}
                  </span>
                  <span class="chip ghost">{{ it.category }}</span>
                </div>
              </div>
            </div>
          </div>

          <div class="glass panel" v-if="selectedIngredient">
            <div class="panel-head">
              <h3>原料详情</h3>
              <div class="inline">
                <button class="btn ghost sm" @click="restockIngredient(selectedIngredient.id)">补货 +10</button>
                <button class="btn primary sm" @click="saveAll()">保存</button>
              </div>
            </div>

            <div class="glass inner">
              <div class="param-form">
                <div class="f">
                  <div class="lab">名称</div>
                  <input class="input" v-model="selectedIngredient.name" />
                </div>
                <div class="f">
                  <div class="lab">分类</div>
                  <select class="select" v-model="selectedIngredient.category">
                    <option>谷物</option><option>薯类</option><option>蔬菜</option><option>水果</option>
                    <option>肉类</option><option>蛋奶</option><option>豆制品</option><option>调味</option><option>其他</option>
                  </select>
                </div>
                <div class="f">
                  <div class="lab">库存</div>
                  <input class="input" type="number" min="0" v-model.number="selectedIngredient.stock" />
                </div>
                <div class="f">
                  <div class="lab">低库存预警线</div>
                  <input class="input" type="number" min="0" v-model.number="selectedIngredient.lowStockLine" />
                </div>
              </div>

              <div class="param-form" style="margin-top:10px">
                <div class="f">
                  <div class="lab">纤维风险</div>
                  <select class="select" v-model="selectedIngredient.fiberRisk">
                    <option>低</option><option>中</option><option>高</option>
                  </select>
                </div>
                <div class="f">
                  <div class="lab">过敏/禁忌标签（逗号分隔）</div>
                  <input class="input" v-model="selectedIngredient.tagsText" placeholder="如：无乳制品, 无麸质, 含坚果" />
                </div>
              </div>

              <div class="glass inner" style="margin-top:12px">
                <div class="inner-head">
                  <h4>营养信息（演示）</h4>
                  <button class="btn ghost sm" @click="autoFillNutrition(selectedIngredient.id)">自动填充</button>
                </div>
                <div class="param-form">
                  <div class="f">
                    <div class="lab">蛋白 (g/100g)</div>
                    <input class="input" type="number" min="0" v-model.number="selectedIngredient.nutrition.protein" />
                  </div>
                  <div class="f">
                    <div class="lab">碳水 (g/100g)</div>
                    <input class="input" type="number" min="0" v-model.number="selectedIngredient.nutrition.carb" />
                  </div>
                  <div class="f">
                    <div class="lab">脂肪 (g/100g)</div>
                    <input class="input" type="number" min="0" v-model.number="selectedIngredient.nutrition.fat" />
                  </div>
                  <div class="f">
                    <div class="lab">能量 (kcal/100g)</div>
                    <input class="input" type="number" min="0" v-model.number="selectedIngredient.nutrition.kcal" />
                  </div>
                </div>
              </div>

              <div class="muted small" style="margin-top:10px">

              </div>
            </div>
          </div>

          <div class="glass panel" v-else>
            <h3>提示</h3>
            <p class="muted">选择一个原料进行编辑；或者新增原料。</p>
          </div>
        </section>

        <!-- RECIPES -->
        <section v-if="tab==='recipes'" class="grid2">
          <div class="glass panel">
            <div class="panel-head">
              <h3>配方编辑</h3>
              <div class="inline">
                <input class="input" placeholder="搜索：菜品名/标签" v-model="recipeQuery" />
                <button class="btn ghost sm" @click="addRecipe()">新增配方</button>
              </div>
            </div>

            <div class="list">
              <div
                class="row"
                v-for="r in filteredRecipes"
                :key="r.id"
                @click="selectRecipe(r.id)"
                :class="{active: r.id===selectedRecipeId}"
              >
                <div>
                  <div class="title">{{ r.name }}</div>
                  <div class="muted small">
                    质构：{{ r.texture }} · 粘度：{{ r.viscosity }} · 纤维风险：{{ recipeFiberRisk(r) }}
                  </div>
                </div>
                <div class="chips">
                  <span class="chip ghost">{{ r.category }}</span>
                  <span class="chip" :class="recipeFiberRisk(r)==='高'?'warn':(recipeFiberRisk(r)==='中'?'run':'ok')">
                    {{ recipeFiberRisk(r) }}
                  </span>
                </div>
              </div>
            </div>
          </div>

          <div class="glass panel" v-if="selectedRecipe">
            <div class="panel-head">
              <h3>配方详情</h3>
              <div class="inline">
                <button class="btn ghost sm" @click="duplicateRecipe(selectedRecipe.id)">复制一份</button>
                <button class="btn ghost sm" @click="pushRecipeToTask(selectedRecipe.id)">推送到任务</button>
                <button class="btn primary sm" @click="saveAll()">保存</button>
              </div>
            </div>

            <div class="glass inner">
              <div class="param-form">
                <div class="f">
                  <div class="lab">名称</div>
                  <input class="input" v-model="selectedRecipe.name" />
                </div>
                <div class="f">
                  <div class="lab">分类</div>
                  <select class="select" v-model="selectedRecipe.category">
                    <option>主食</option><option>菜</option><option>汤羹</option><option>甜品</option><option>功能餐</option>
                  </select>
                </div>
              </div>

              <div class="param-form" style="margin-top:10px">
                <div class="f">
                  <div class="lab">质构等级</div>
                  <select class="select" v-model="selectedRecipe.texture">
                    <option>糊状</option><option>软烂</option><option>半固体</option>
                  </select>
                </div>
                <div class="f">
                  <div class="lab">粘度水平</div>
                  <select class="select" v-model="selectedRecipe.viscosity">
                    <option>低</option><option>中</option><option>高</option>
                  </select>
                </div>
              </div>

              <div class="glass inner" style="margin-top:12px">
                <div class="inner-head">
                  <h4>原料配比</h4>
                  <button class="btn ghost sm" @click="addRecipeItem(selectedRecipe.id)">添加原料</button>
                </div>

                <div class="table">
                  <div class="tr th">
                    <div>原料</div>
                    <div>克数(g)</div>
                    <div>标签</div>
                    <div style="text-align:right">操作</div>
                  </div>

                  <div class="tr" v-for="(it, idx) in selectedRecipe.items" :key="it.key">
                    <div>
                      <select class="select" v-model="it.ingredientId">
                        <option disabled value="">选择原料</option>
                        <option v-for="ing in ingredients" :key="ing.id" :value="ing.id">{{ ing.name }}</option>
                      </select>
                    </div>
                    <div>
                      <input class="input" type="number" min="0" v-model.number="it.grams" />
                    </div>
                    <div class="muted small">
                      {{ ingredientTagsText(it.ingredientId) }}
                    </div>
                    <div style="text-align:right">
                      <button class="btn ghost sm" @click="removeRecipeItem(selectedRecipe.id, idx)">删除</button>
                    </div>
                  </div>
                </div>

                <div class="chips" style="margin-top:10px">
                  <span class="chip ghost">纤维风险：{{ recipeFiberRisk(selectedRecipe) }}</span>
                  <span class="chip ghost">过敏标签：{{ recipeAllergyTags(selectedRecipe).join('、') || '—' }}</span>
                </div>

                <div class="muted small" style="margin-top:8px">
                  *配方会做“禁忌校验”：用户若含过敏标签，会在任务创建时提示并建议替代配方。
                </div>
              </div>

              <div class="glass inner" style="margin-top:12px">
                <div class="inner-head">
                  <h4>打印参数建议（自动）</h4>
                  <button class="btn ghost sm" @click="applyRecipePrintHints(selectedRecipe.id)">一键套用</button>
                </div>

                <div class="param-form">
                  <div class="f">
                    <div class="lab">推荐喷嘴(mm)</div>
                    <input class="input" type="number" step="0.1" min="0.4" max="2.0" v-model.number="selectedRecipe.printHint.nozzle" />
                  </div>
                  <div class="f">
                    <div class="lab">推荐层高(mm)</div>
                    <input class="input" type="number" step="0.1" min="0.2" max="1.0" v-model.number="selectedRecipe.printHint.layerHeight" />
                  </div>
                  <div class="f">
                    <div class="lab">推荐温控(°C)</div>
                    <input class="input" type="number" min="20" max="60" v-model.number="selectedRecipe.printHint.temp" />
                  </div>
                  <div class="f">
                    <div class="lab">推荐速度(mm/s)</div>
                    <input class="input" type="number" min="5" max="80" v-model.number="selectedRecipe.printHint.speed" />
                  </div>
                </div>

                <div class="muted small" style="margin-top:10px">
                  规则：纤维风险越高 → 喷嘴/层高↑、速度↓；水分偏高 → 速度↓、回抽↑。
                </div>
              </div>
            </div>
          </div>

          <div class="glass panel" v-else>
            <h3>提示</h3>
            <p class="muted">选择一个配方进行编辑；或者新增配方。</p>
          </div>
        </section>

        <!-- QUEUE -->
        <section v-if="tab==='queue'" class="grid2">
          <div class="glass panel">
            <div class="panel-head">
              <h3>任务队列</h3>
              <div class="inline">
                <select class="select" v-model="queueFilter">
                  <option value="">全部状态</option>
                  <option value="排队">排队</option>
                  <option value="打印中">打印中</option>
                  <option value="完成">完成</option>
                  <option value="暂停">暂停</option>
                </select>
                <button class="btn primary sm" @click="openCreateTask()">新建任务</button>
              </div>
            </div>

            <div class="list">
              <div class="row" v-for="t in filteredQueue" :key="t.id" @click="selectTask(t.id)" :class="{active:t.id===selectedTaskId}">
                <div>
                  <div class="title">{{ t.title }}</div>
                  <div class="muted small">
                    <div class="inline" style="gap:8px;flex-wrap:wrap">
  <button class="btn sm" v-if="(t.status||'待开始')==='待开始'" @click="startTask(t.id, t.deviceId)">开始</button>
  <button class="btn ghost sm" v-if="t.status==='打印中'" @click="pauseTask(t.id)">暂停</button>
  <button class="btn sm" v-if="t.status==='暂停'" @click="resumeTask(t.id)">继续</button>
  <button class="btn ghost sm" v-if="t.status!=='完成'" @click="completeTask(t.id)">标记完成</button>
</div>

<div class="pbar" style="margin-top:10px">
  <div class="pbar-fill" :style="{width: (t.progress||0) + '%'}"></div>
</div>
<div class="muted small" style="margin-top:6px">
  状态：{{ t.status || '待开始' }} · 进度：{{ t.progress || 0 }}%
  <span v-if="t.deviceId"> · 设备：{{ deviceName(t.deviceId) }}</span>
</div>

                    站点：{{ stationName(t.stationId) }} · 设备：{{ deviceName(t.deviceId) }} · 配方：{{ recipeName(t.recipeId) }} · 数量：{{ t.qty }}
                  </div>
                </div>
                <div class="chips">
                  <span class="chip" :class="t.status==='完成'?'ok':(t.status==='打印中'?'run':(t.status==='暂停'?'warn':'ghost'))">{{ t.status }}</span>
                  <span class="chip ghost">{{ t.time }}</span>
                </div>
              </div>
            </div>
          </div>

          <div class="glass panel" v-if="selectedTask">
            <div class="panel-head">
              <h3>任务详情</h3>
              <div class="inline">
                <button class="btn ghost sm" @click="taskStart(selectedTask.id)">开始</button>
                <button class="btn ghost sm" @click="taskPause(selectedTask.id)">暂停</button>
                <button class="btn ghost sm" @click="taskComplete(selectedTask.id)">完成</button>
                <button class="btn primary sm" @click="saveAll()">保存</button>
              </div>
            </div>

            <div class="glass inner">
              <div class="title">{{ selectedTask.title }}</div>
              <div class="muted small">
                状态：{{ selectedTask.status }}<br/>
                站点：{{ stationName(selectedTask.stationId) }}<br/>
                设备：{{ deviceName(selectedTask.deviceId) }}<br/>
                服务对象：{{ userName(selectedTask.userId) || '—' }}<br/>
                配方：{{ recipeName(selectedTask.recipeId) }}<br/>
                数量：{{ selectedTask.qty }}<br/>
                备注：{{ selectedTask.note || '—' }}
              </div>
            </div>

            <div class="glass inner" style="margin-top:12px">
              <h4>改派 / 校验</h4>
              <div class="param-form">
                <div class="f">
                  <div class="lab">改派站点</div>
                  <select class="select" v-model="selectedTask.stationId" @change="syncTaskDeviceOptions(selectedTask.id)">
                    <option v-for="s in stations" :key="s.id" :value="s.id">{{ s.name }}</option>
                  </select>
                </div>
                <div class="f">
                  <div class="lab">改派设备</div>
                  <select class="select" v-model="selectedTask.deviceId">
                    <option v-for="d in devicesByStation(selectedTask.stationId)" :key="d.id" :value="d.id">{{ d.name }}</option>
                  </select>
                </div>
              </div>

              <div class="glass inner" style="margin-top:10px">
                <div class="inner-head">
                  <h4>禁忌/过敏校验</h4>
                  <button class="btn ghost sm" @click="validateTask(selectedTask.id)">重新校验</button>
                </div>
                <div class="chips">
                  <span class="chip" :class="selectedTask.validation?.ok?'ok':'warn'">
                    {{ selectedTask.validation?.ok ? '通过' : '提示' }}
                  </span>
                  <span class="chip ghost">{{ selectedTask.validation?.msg || '—' }}</span>
                </div>
              </div>
            </div>
          </div>

          <div class="glass panel" v-else>
            <h3>提示</h3>
            <p class="muted">选择一个任务查看详情；支持改派站点/设备与禁忌校验。</p>
          </div>
        </section>

        <!-- OPS -->
        <section v-if="tab==='ops'" class="grid2">
          <div class="glass panel">
            <div class="panel-head">
              <h3>站点运营</h3>
              <div class="inline">
                <select class="select" v-model="opsFilter.stationId" @change="regenTrend()">
                  <option value="">全站汇总</option>
                  <option v-for="s in stations" :key="s.id" :value="s.id">{{ s.name }}</option>
                </select>
                <button class="btn ghost sm" @click="regenTrend()">刷新趋势</button>
              </div>
            </div>

            <div class="ops-cards">
              <div class="mini">
                <div class="muted small">7日订单</div>
                <div class="big">{{ opsSummary.orders }}</div>
              </div>
              <div class="mini">
                <div class="muted small">平均成功率</div>
                <div class="big">{{ opsSummary.success }}%</div>
              </div>
              <div class="mini">
                <div class="muted small">告警数量</div>
                <div class="big">{{ opsSummary.alerts }}</div>
              </div>
            </div>

            <div class="glass inner" style="margin-top:12px">
              <div class="inner-head">
                <h4>趋势（7天）</h4>
                <div class="muted small">折线 + 渐变面积</div>
              </div>

              <svg class="ops-svg" viewBox="0 0 560 220" preserveAspectRatio="none">
                <defs>
                  <linearGradient id="g1" x1="0" y1="0" x2="0" y2="1">
                    <stop offset="0%" stop-color="rgba(30,94,255,.35)"/>
                    <stop offset="100%" stop-color="rgba(30,94,255,0)"/>
                  </linearGradient>
                </defs>

                <path :d="opsAreaPath" fill="url(#g1)"></path>
                <path :d="opsLinePath" fill="none" stroke="rgba(30,94,255,.95)" stroke-width="3" stroke-linecap="round"></path>

                <g v-for="(pt,i) in opsPoints" :key="i">
                  <circle :cx="pt.x" :cy="pt.y" r="5" fill="rgba(47,125,255,.95)"/>
                </g>
              </svg>

              <div class="ops-x">
                <div class="muted small" v-for="(d,i) in opsXLabels" :key="i">{{ d }}</div>
              </div>
            </div>
          </div>

          <div class="glass panel">
            <div class="panel-head">
              <h3>告警中心</h3>
              <button class="btn ghost sm" @click="simulateAlarm()">模拟一条</button>
            </div>

            <div class="list">
              <div class="row" v-for="a in alerts.slice(0,8)" :key="a.id">
                <div>
                  <div class="title">{{ a.title }}</div>
                  <div class="muted small">{{ a.stationName }} · 等级：{{ a.level }} · {{ a.time }}</div>
                </div>
                <div class="chips">
                  <span class="chip" :class="a.level==='高'?'danger':(a.level==='中'?'warn':'ok')">{{ a.level }}</span>
                  <button class="btn ghost sm" @click="suggestAction(a)">动作建议</button>
                </div>
              </div>
            </div>

            <div class="glass inner" style="margin-top:12px">
              <h4>运营建议（可落地）</h4>
              <ul class="muted">
                <li>低库存：自动创建补货工单 + 推荐替代配方。</li>
                <li>堵塞风险升高：降低速度/增大喷嘴/插入清洗任务/改派设备。</li>
                <li>成功率下降：追溯配方版本与设备参数变更记录。</li>
              </ul>
              <button class="btn primary sm" @click="go('platform'); tab='queue'">去看任务队列</button>
            </div>
          </div>
        </section>

      </section>

      <!-- 社区 -->
      <section v-if="page==='community'" class="page community">
        <div class="page-head">
          <div>
            <h2>社区贴吧</h2>
            <p class="muted">运营与共创：站点经验、配方分享、设备故障排查、用户反馈。</p>
          </div>
          <div class="head-actions">
            <input class="input" style="width:260px" placeholder="搜索帖子" v-model="postQuery" />
            <button class="btn primary" @click="openPostModal()">发帖</button>
          </div>
        </div>

        <div class="grid2">
          <div class="glass panel">
            <div class="panel-head">
              <h3>帖子列表</h3>
              <div class="muted small">共 {{ filteredPosts.length }} 条</div>
            </div>

            <div class="list">
              <div class="row" v-for="p in filteredPosts" :key="p.id" @click="selectPost(p.id)" :class="{active:p.id===selectedPostId}">
                <div>
                  <div class="title">{{ p.title }}</div>
                  <div class="muted small">{{ p.author }} · {{ p.time }} · 评论 {{ p.comments.length }}</div>
                </div>
                <div class="chips">
                  <span class="chip ghost" v-for="t in p.tags.slice(0,2)" :key="t">{{ t }}</span>
                  <span class="chip run">👍 {{ p.likes }}</span>
                </div>
              </div>
            </div>
          </div>

          <div class="glass panel" v-if="selectedPost">
            <div class="panel-head">
              <h3>帖子详情</h3>
              <div class="inline">
                <button class="btn ghost sm" @click="likePost(selectedPost.id)">点赞</button>
                <button class="btn ghost sm" @click="quickReply(selectedPost.id)">一键回复</button>
              </div>
            </div>

            <div class="glass inner">
              <div class="title">{{ selectedPost.title }}</div>
              <div class="muted small">{{ selectedPost.author }} · {{ selectedPost.time }}</div>
              <div class="content">{{ selectedPost.content }}</div>
              <div class="chips" style="margin-top:10px">
                <span class="chip ghost" v-for="t in selectedPost.tags" :key="t">{{ t }}</span>
              </div>
            </div>

            <div class="glass inner" style="margin-top:12px">
              <div class="inner-head">
                <h4>评论（{{ selectedPost.comments.length }}）</h4>
                <button class="btn ghost sm" @click="addComment(selectedPost.id)">发送</button>
              </div>
              <textarea class="textarea" v-model="newComment" placeholder="写点内容…（比如：参数怎么调/配方怎么过筛/怎么避免堵塞）"></textarea>

              <div class="comment" v-for="c in selectedPost.comments.slice().reverse()" :key="c.id">
                <div class="c-head">
                  <span class="c-author">{{ c.author }}</span>
                  <span class="muted small">{{ c.time }}</span>
                </div>
                <div class="c-body">{{ c.text }}</div>
              </div>
            </div>
          </div>

          <div class="glass panel" v-else>
            <h3>提示</h3>
            <p class="muted">选择一个帖子查看详情并评论。</p>
          </div>
        </div>
      </section>

    </main>

    <!-- QuickHelp Modal -->
    <div v-if="modals.quickHelp" class="modal-mask" @click.self="modals.quickHelp=false">
      <div class="modal glass">
        <div class="m-head">
          <div>
            <div class="m-title">对接清单（可直接复制）</div>
            <div class="muted small">用于你写“程序设计说明/平台集成文档”</div>
          </div>
          <button class="btn ghost sm" @click="modals.quickHelp=false">关闭</button>
        </div>

        <div class="m-body">
          <div class="glass inner">
            <h4>1）数据对象</h4>
            <ul class="muted">
              <li>机构 Org：机构信息、权限、站点列表</li>
              <li>站点 Station：地址、状态、KPI、设备绑定</li>
              <li>设备 Device：参数可调、清洗策略、告警</li>
              <li>用户 User：角色、体质、禁忌/过敏标签</li>
              <li>原料 Ingredient：库存、营养、标签、纤维风险</li>
              <li>配方 Recipe：原料引用、版本、打印建议参数</li>
              <li>任务 Task：排队、改派、复盘、成功率</li>
            </ul>
          </div>

          <div class="glass inner" style="margin-top:12px">
            <h4>2）闭环流程</h4>
            <ol class="muted">
              <li>辨体问卷 → 主次体质</li>
              <li>体质策略 → 推荐配方/替代配方</li>
              <li>配方 → 打印建议参数（喷嘴/层高/温控/速度）</li>
              <li>任务 → 设备执行（可改派）</li>
              <li>运营 → 趋势 + 告警 + 动作建议</li>
            </ol>
          </div>
        </div>

        <div class="m-foot">
          <button class="btn primary" @click="copyQuickHelp()">复制内容</button>
          <button class="btn ghost" @click="modals.quickHelp=false">完成</button>
        </div>
      </div>
    </div>

    <!-- CreateTask Modal -->
    <div v-if="modals.createTask" class="modal-mask" @click.self="modals.createTask=false">
      <div class="modal glass">
        <div class="m-head">
          <div>
            <div class="m-title">新建打印任务</div>
            <div class="muted small">支持禁忌校验与纤维风险提示</div>
          </div>
          <button class="btn ghost sm" @click="modals.createTask=false">关闭</button>
        </div>

        <div class="m-body">
          <div class="param-form">
            <div class="f">
              <div class="lab">站点</div>
              <select class="select" v-model="taskForm.stationId" @change="syncTaskFormDevice()">
                <option v-for="s in stations" :key="s.id" :value="s.id">{{ s.name }}</option>
              </select>
            </div>
            <div class="f">
              <div class="lab">设备</div>
              <select class="select" v-model="taskForm.deviceId">
                <option v-for="d in devicesByStation(taskForm.stationId)" :key="d.id" :value="d.id">{{ d.name }}</option>
              </select>
            </div>
          </div>

          <div class="param-form" style="margin-top:10px">
            <div class="f">
              <div class="lab">配方</div>
              <select class="select" v-model="taskForm.recipeId">
                <option v-for="r in recipes" :key="r.id" :value="r.id">{{ r.name }}</option>
              </select>
            </div>
            <div class="f">
              <div class="lab">服务对象（用户）</div>
              <select class="select" v-model="taskForm.userId">
                <option value="">（可选）</option>
                <option v-for="u in users" :key="u.id" :value="u.id">{{ u.name }} · {{ u.role }}</option>
              </select>
            </div>
          </div>

          <div class="param-form" style="margin-top:10px">
            <div class="f">
              <div class="lab">数量</div>
              <input class="input" type="number" min="1" v-model.number="taskForm.qty" />
            </div>
            <div class="f">
              <div class="lab">备注</div>
              <input class="input" v-model="taskForm.note" placeholder="如：软烂/低盐/加清洗" />
            </div>
          </div>

          <div class="glass inner" style="margin-top:12px">
            <div class="inner-head">
              <h4>即时校验</h4>
              <button class="btn ghost sm" @click="validateTaskForm()">校验</button>
            </div>
            <div class="chips">
              <span class="chip" :class="taskForm.validation.ok?'ok':'warn'">
                {{ taskForm.validation.ok ? '通过' : '提示' }}
              </span>
              <span class="chip ghost">{{ taskForm.validation.msg }}</span>
            </div>
          </div>
        </div>

        <div class="m-foot">
          <button class="btn primary" @click="createTask()">创建任务</button>
          <button class="btn ghost" @click="modals.createTask=false">取消</button>
        </div>
      </div>
    </div>

    <!-- Post Modal -->
    <div v-if="modals.post" class="modal-mask" @click.self="modals.post=false">
      <div class="modal glass">
        <div class="m-head">
          <div>
            <div class="m-title">发布帖子</div>
            <div class="muted small">建议标题明确：问题/经验/参数/配方</div>
          </div>
          <button class="btn ghost sm" @click="modals.post=false">关闭</button>
        </div>

        <div class="m-body">
          <div class="f">
            <div class="lab">标题</div>
            <input class="input" v-model="postForm.title" placeholder="例如：纤维堵塞怎么解决？喷嘴/层高怎么配？" />
          </div>
          <div class="f" style="margin-top:10px">
            <div class="lab">内容</div>
            <textarea class="textarea" v-model="postForm.content" placeholder="描述：站点/设备/原料/配方/参数/现象/你已尝试的方法…"></textarea>
          </div>
          <div class="f" style="margin-top:10px">
            <div class="lab">标签（逗号）</div>
            <input class="input" v-model="postForm.tagsText" placeholder="如：堵塞, 参数, 纤维, 清洗" />
          </div>
        </div>

        <div class="m-foot">
          <button class="btn primary" @click="createPost()">发布</button>
          <button class="btn ghost" @click="modals.post=false">取消</button>
        </div>
      </div>
    </div>

  </div>
</template>


<script>

const uid = () => Math.random().toString(36).slice(2, 10);
const nowStr = () => new Date().toLocaleString();

export default {
  name: "App",
  data() {
    
    return {
      videoUrl: "https://foodini.co/",
videoUrlDraft: "",
showReplaceVideo: false,
videoModalOpen: false,

      // ====== 资料库 ======
      paramView: "chart",        // chart | table
paramDeviceId: "",         // 当前查看哪台设备

      printEngineTimer: null,

libraryTab: "science",
libraryQuery: "",
libraryActive: null,
paramActive: null,
resultsHeroImage: "",

libraryScience: [],
libraryParams: [],
milestones: [],

      page: "home",
      tab: "org",

      toastMsg: "",
      toastTimer: null,



      kpiSummary: { menus: 150, loss: 28, upsell: 18, insight: 6 },

      // ====== TCM ======
      tcmTypes: ["平和", "气虚", "阳虚", "阴虚", "痰湿", "湿热", "血瘀", "气郁", "特禀"],
      tcmAnswers: {},
      tcmResult: { ready: false, primary: "", secondary: "", bars: [], advice: [], deviceHints: [] },
      tcmQuestions: [],

      // ====== 数据（localStorage 持久化） ======
      orgs: [],
      stations: [],
      devices: [],
      users: [],
      ingredients: [],
      recipes: [],
      queue: [],
      alerts: [],
      opsTrend: [],

      // selection
      selectedOrgId: "",
      selectedStationId: "",
      selectedDeviceId: "",
      selectedUserId: "",
      selectedIngredientId: "",
      selectedRecipeId: "",
      selectedTaskId: "",
      selectedPostId: "",

      // filters
      deviceFilter: { stationId: "" },
      userFilter: { role: "" },
      ingredientQuery: "",
      recipeQuery: "",
      queueFilter: "",
      opsFilter: { stationId: "" },

      // 社区
      posts: [],
      postQuery: "",
      newComment: "",

      // 弹窗
      modals: { quickHelp: false, createTask: false, post: false },

      // 新建任务
      taskForm: { stationId: "", deviceId: "", recipeId: "", userId: "", qty: 1, note: "", validation: { ok: true, msg: "—" } },

      // 发帖
      postForm: { title: "", content: "", tagsText: "" },
    };
  },

  computed: {
    videoEmbedUrl() {
  return this.normalizeToEmbed(this.videoUrl);
},

paramCompareRows() {
  const d = this.getParamDevice();
  const nozzle = Number(d?.nozzle || 1.2);
  const layerHeight = Number(d?.layerHeight || 0.6);
  const speed = Number(d?.speed || 25);
  const temp = Number(d?.temp || 38);
  const retract = Number(d?.retract || 1.5);
  const flowRate = Number(d?.flowRate || 1.0);

  return [
    { key:"nozzle", name:"喷头直径", value:`${nozzle} mm`, range:"0.8 ~ 1.6（高纤 ≥1.2）", note:"越小越精细但堵塞风险更高" },
    { key:"layerHeight", name:"层高", value:`${layerHeight} mm`, range:"0.4 ~ 0.8（高纤 ≥0.6）", note:"太低回压高，太高细节差" },
    { key:"speed", name:"速度", value:`${speed} mm/s`, range:"15 ~ 40", note:"过快可能断料/塌陷，过慢可能堆料" },
    { key:"temp", name:"温控", value:`${temp} °C`, range:"30 ~ 45", note:"太低粘稠易堵，太高易塌陷" },
    { key:"retract", name:"回抽", value:`${retract} mm`, range:"0 ~ 4", note:"减少滴料，过大可能断料" },
    { key:"flowRate", name:"流量倍率", value:`${flowRate} ×`, range:"0.85 ~ 1.15", note:"用于细调出料过多/过少" },
  ];
},


    // home preview
    stationsOnline() {
      return this.stations.filter(s => s.status === "正常").length;
    },
    devicesOnline() {
      return this.devices.filter(d => d.status === "在线" || d.status === "打印中").length;
    },
    lowStockCount() {
      return this.ingredients.filter(i => i.stock <= i.lowStockLine).length;
    },

    answeredCount() {
      return Object.keys(this.tcmAnswers || {}).filter(k => this.tcmAnswers[k] !== undefined && this.tcmAnswers[k] !== null).length;
    },

    selectedOrg() { return this.orgs.find(o => o.id === this.selectedOrgId); },
    selectedStation() { return this.stations.find(s => s.id === this.selectedStationId); },
    selectedDevice() { return this.devices.find(d => d.id === this.selectedDeviceId); },
    selectedUser() { return this.users.find(u => u.id === this.selectedUserId); },
    selectedIngredient() { return this.ingredients.find(i => i.id === this.selectedIngredientId); },
    selectedRecipe() { return this.recipes.find(r => r.id === this.selectedRecipeId); },
    selectedTask() { return this.queue.find(t => t.id === this.selectedTaskId); },
    selectedPost() { return this.posts.find(p => p.id === this.selectedPostId); },

    filteredDevices() {
      const sid = this.deviceFilter.stationId;
      return sid ? this.devices.filter(d => d.stationId === sid) : this.devices;
    },
    filteredUsers() {
      const r = this.userFilter.role;
      return r ? this.users.filter(u => u.role === r) : this.users;
    },

    filteredIngredients() {
      const q = (this.ingredientQuery || "").trim().toLowerCase();
      const arr = this.ingredients.map(i => ({
        ...i,
        tagsText: (i.tags || []).join(", "),
      }));
      if (!q) return arr;
      return arr.filter(i => {
        const tags = (i.tagsText || "").toLowerCase();
        return (i.name || "").toLowerCase().includes(q) || tags.includes(q) || (i.category || "").toLowerCase().includes(q);
      });
    },

    filteredRecipes() {
      const q = (this.recipeQuery || "").trim().toLowerCase();
      if (!q) return this.recipes;
      return this.recipes.filter(r => {
        const tags = (r.tags || []).join(",").toLowerCase();
        return (r.name || "").toLowerCase().includes(q) || tags.includes(q) || (r.category || "").toLowerCase().includes(q);
      });
    },

    filteredQueue() {
      const f = this.queueFilter;
      return f ? this.queue.filter(t => t.status === f) : this.queue;
    },

    opsSummary() {
      const baseOrders = this.opsTrend.reduce((a,b)=>a+b, 0);
      const orders = baseOrders;
      const success = Math.max(70, Math.min(98, Math.round(82 + (this.opsTrend[6] - this.opsTrend[0]) * 0.2)));
      const alerts = this.alerts.filter(a => !this.opsFilter.stationId || a.stationId === this.opsFilter.stationId).length;
      return { orders, success, alerts };
    },

    opsXLabels() {
      // 近7天（简化）
      const arr = [];
      const d = new Date();
      for (let i = 6; i >= 0; i--) {
        const x = new Date(d.getTime() - i * 86400000);
        arr.push(`${x.getMonth()+1}/${x.getDate()}`);
      }
      return arr;
    },

    opsPoints() {
      // 把 opsTrend 映射到 560x220 画布
      const W = 560, H = 220;
      const padX = 24, padY = 22;
      const innerW = W - padX * 2;
      const innerH = H - padY * 2;

      const vals = this.opsTrend.length ? this.opsTrend : [50,55,52,60,58,62,66];
      const minV = Math.min(...vals);
      const maxV = Math.max(...vals);

      const norm = (v) => {
        if (maxV === minV) return 0.5;
        return (v - minV) / (maxV - minV);
      };

      return vals.map((v, i) => {
        const x = padX + (innerW * (i / (vals.length - 1)));
        const y = padY + innerH * (1 - norm(v));
        return { x, y, v };
      });
    },

    opsLinePath() {
      const pts = this.opsPoints;
      if (!pts.length) return "";
      return "M " + pts.map(p => `${p.x.toFixed(1)} ${p.y.toFixed(1)}`).join(" L ");
    },

    opsAreaPath() {
      const pts = this.opsPoints;
      if (!pts.length) return "";
      const W = 560, H = 220;
      const baseY = H - 22; // padY
      return `M ${pts[0].x.toFixed(1)} ${baseY} ` +
        pts.map(p => `L ${p.x.toFixed(1)} ${p.y.toFixed(1)}`).join(" ") +
        ` L ${pts[pts.length-1].x.toFixed(1)} ${baseY} Z`;
    },

filteredLibraryScience() {
  const q = (this.libraryQuery || "").trim().toLowerCase();
  const arr = this.libraryScience || [];
  if (!q) return arr;
  return arr.filter(a =>
    (a.title||"").toLowerCase().includes(q) ||
    (a.brief||"").toLowerCase().includes(q) ||
    (a.content||"").toLowerCase().includes(q) ||
    (a.tags||[]).join(",").toLowerCase().includes(q)
  );
},
filteredLibraryParams() {
  const q = (this.libraryQuery || "").trim().toLowerCase();
  const arr = this.libraryParams || [];
  if (!q) return arr;
  return arr.filter(p =>
    (p.name||"").toLowerCase().includes(q) ||
    (p.oneLine||"").toLowerCase().includes(q) ||
    (p.detail||"").toLowerCase().includes(q)
  );
},
resultsSummary() {
  return {
    recipes: this.recipes.length,
    stations: this.stations.length,
    tasks: this.queue.length
  };
},
filteredMilestones() {
  const q = (this.libraryQuery || "").trim().toLowerCase();
  const arr = this.milestones || [];
  if (!q) return arr;
  return arr.filter(m =>
    (m.title||"").toLowerCase().includes(q) ||
    (m.note||"").toLowerCase().includes(q) ||
    (m.type||"").toLowerCase().includes(q)
  );
},


    filteredPosts() {
      const q = (this.postQuery || "").trim().toLowerCase();
      if (!q) return this.posts;
      return this.posts.filter(p => (p.title||"").toLowerCase().includes(q) || (p.content||"").toLowerCase().includes(q) || (p.tags||[]).join(",").toLowerCase().includes(q));
    },
  },

  mounted() {
    this.initTCMQuestions();
    this.loadAll();
    if (!this.orgs.length) this.seedAll();
    
    this.ensureSelections();
    this.regenTrend();
    this.seedLibraryIfEmpty();
  this.printEngineTimer = setInterval(this.tickPrintEngine, 800);
  if (!this.paramDeviceId && this.devices && this.devices[0]) this.paramDeviceId = this.devices[0].id;



  },

beforeUnmount() {
  if (this.progressTimer) clearInterval(this.progressTimer);
},

  methods: {
    tickDeviceProgress() {
  // 演示：只有“打印中”的设备进度才会动
  let changed = false;
  this.devices.forEach(d => {
    if (d.status === "打印中") {
      d.progress = Math.min(100, (d.progress || 0) + Math.floor(Math.random() * 6 + 2));
      changed = true;
      if (d.progress >= 100) {
        d.status = "在线";
        d.progress = 0;
      }
    }
  });
  if (changed) this.saveAll();
},
getParamDevice() {
  const id = this.paramDeviceId || (this.selectedDevice && this.selectedDevice.id);
  return this.devices.find(x => x.id === id) || this.devices[0] || null;
},

openVideoModal() { this.videoModalOpen = true; },
closeVideoModal() { this.videoModalOpen = false; },

toggleReplaceVideo() {
  this.showReplaceVideo = !this.showReplaceVideo;
  this.videoUrlDraft = this.videoUrl;
},
cancelReplaceVideo() {
  this.showReplaceVideo = false;
  this.videoUrlDraft = "";
},
applyVideoUrl() {
  const v = (this.videoUrlDraft || "").trim();
  if (!v) return this.toast?.("请输入链接");
  this.videoUrl = v;
  this.showReplaceVideo = false;
  this.toast?.("已替换视频链接");
  this.saveAll?.();
},

normalizeToEmbed(url) {
  if (!url) return "";
  const u = url.trim();
  if (u.includes("/embed/")) return u;

  const shortMatch = u.match(/youtu\.be\/([a-zA-Z0-9_-]{6,})/);
  if (shortMatch?.[1]) return `https://www.youtube.com/embed/${shortMatch[1]}`;

  const watchMatch = u.match(/[?&]v=([a-zA-Z0-9_-]{6,})/);
  if (watchMatch?.[1]) return `https://www.youtube.com/embed/${watchMatch[1]}`;

  return u;
},

drawParamChart() {
  const canvas = this.$refs.paramChart;
  const d = this.getParamDevice();
  if (!canvas || !d) return;

  const parent = canvas.parentElement;
  const w = Math.max(320, parent ? parent.clientWidth : 640);
  const h = 240;
  canvas.width = w;
  canvas.height = h;

  const ctx = canvas.getContext("2d");
  ctx.clearRect(0, 0, w, h);

  const history = Array.isArray(d.paramHistory) ? d.paramHistory.slice(-60) : [];
  if (history.length < 2) {
    ctx.fillStyle = "rgba(11,16,32,.65)";
    ctx.font = "14px sans-serif";
    ctx.fillText("暂无足够历史数据：开始打印或点击“记录一次（采样）”", 16, 36);
    return;
  }

  // 归一化到 0~1（6条线各自按范围映射，避免差距太大看不见）
  const series = [
    { key:"nozzle",     name:"喷头",     min:0.8,  max:1.6,  dash:[] },
    { key:"layerHeight",name:"层高",     min:0.4,  max:0.8,  dash:[6,4] },
    { key:"speed",      name:"速度",     min:15,   max:40,   dash:[2,4] },
    { key:"temp",       name:"温控",     min:30,   max:45,   dash:[10,4] },
    { key:"retract",    name:"回抽",     min:0,    max:4,    dash:[1,3] },
    { key:"flowRate",   name:"流量",     min:0.85, max:1.15, dash:[12,6] },
  ];

  const pad = { l:46, r:14, t:12, b:32 };
  const plotW = w - pad.l - pad.r;
  const plotH = h - pad.t - pad.b;

  // 坐标轴
  ctx.strokeStyle = "rgba(0,0,0,.10)";
  ctx.lineWidth = 1;
  ctx.beginPath();
  ctx.moveTo(pad.l, pad.t);
  ctx.lineTo(pad.l, pad.t + plotH);
  ctx.lineTo(pad.l + plotW, pad.t + plotH);
  ctx.stroke();

  // 轻网格
  ctx.strokeStyle = "rgba(0,0,0,.06)";
  for (let i=1;i<=4;i++){
    const y = pad.t + (plotH/5)*i;
    ctx.beginPath(); ctx.moveTo(pad.l, y); ctx.lineTo(pad.l+plotW, y); ctx.stroke();
  }

  const x = (i) => pad.l + (i/(history.length-1))*plotW;
  const yNorm = (v) => pad.t + (1 - v)*plotH;
  const clamp01 = (v)=>Math.max(0,Math.min(1,v));

  // 画 6 条线（同色不同虚线，保持克莱因蓝体系）
  series.forEach((s, idx) => {
    ctx.save();
    ctx.setLineDash(s.dash);
    ctx.strokeStyle = "rgba(30,94,255,.85)"; // 克莱因蓝系
    ctx.lineWidth = 2;

    ctx.beginPath();
    history.forEach((p, i) => {
      const raw = Number(p[s.key]);
      const vn = clamp01((raw - s.min) / (s.max - s.min));
      const xx = x(i);
      const yy = yNorm(vn);
      if (i === 0) ctx.moveTo(xx, yy);
      else ctx.lineTo(xx, yy);
    });
    ctx.stroke();
    ctx.restore();
  });

  // 图例（简洁）
  ctx.fillStyle = "rgba(11,16,32,.65)";
  ctx.font = "12px sans-serif";
  let lx = pad.l, ly = h - 10;
  const labels = series.map(s=>s.name).join(" · ");
  ctx.fillText(labels + "（同色不同线型）", lx, ly);
},


    pickDeviceStatusImage() {
  this.$refs.deviceStatusImgInput && this.$refs.deviceStatusImgInput.click();
},
onPickDeviceStatusImage(e) {
  const f = e.target.files && e.target.files[0];
  if (!f || !this.selectedDevice) return;
  const reader = new FileReader();
  reader.onload = () => {
    this.selectedDevice.statusImage = String(reader.result || "");
    this.toast("已更新设备状态图");
    this.saveAll();
  };
  reader.readAsDataURL(f);
},
clearDeviceStatusImage() {
  if (!this.selectedDevice) return;
  this.selectedDevice.statusImage = "";
  this.toast("已清除设备状态图");
  this.saveAll();
},

// ===== 打印引擎：任务 <-> 设备 进度联动 =====
ensureTaskShape(t) {
  // 兼容旧存档
  if (!t.status) t.status = "待开始";
  if (typeof t.progress !== "number") t.progress = 0;
  if (t.deviceId == null) t.deviceId = "";
  if (!t.startedAt) t.startedAt = 0;
  if (!t.pausedAt) t.pausedAt = 0;
  if (!t.pausedTotal) t.pausedTotal = 0;
  if (!t.estMs) t.estMs = 0;
  return t;
},

calcEstMs(task, device) {
  // “更像真机”的估时：跟克数、复杂度、设备速度相关（演示公式，可再调）
  const grams = Number(task.grams || task.weight || 150);
  const complexity = Number(task.complexity || 1.0); // 1~2
  const speed = Number(device?.speed || 25);         // mm/s
  // 基础 40s + 克数*0.9s + 复杂度系数 + 速度影响
  const base = 40000;
  const gPart = grams * 900;
  const cPart = base * (complexity - 1) * 0.8;
  const speedFactor = 25 / Math.max(10, speed); // 越快越短
  return Math.max(30000, (base + gPart + cPart) * speedFactor);
},

bindTaskToDevice(task, deviceId) {
  const d = this.devices.find(x => x.id === deviceId);
  if (!d) return;

  task.deviceId = d.id;
  d.activeTaskId = task.id;

  // 设备切到打印中并同步任务进度
  d.status = "打印中";
  d.progress = task.progress || 0;

  // 初次开始估时
  if (!task.estMs) task.estMs = this.calcEstMs(task, d);

  this.saveAll();
},

startTask(taskId, deviceId) {
  const t = this.queue.find(x => x.id === taskId);
  if (!t) return;
  this.ensureTaskShape(t);

  // 如果还没绑设备，必须绑（你也可以在 UI 里先选设备再开始）
  const did = deviceId || t.deviceId || (this.selectedDevice && this.selectedDevice.id) || (this.devices[0] && this.devices[0].id);
  if (!did) {
    this.toast("没有可用设备");
    return;
  }

  // 同一设备如果已有任务在打，先提示（演示：直接不让开）
  const d = this.devices.find(x => x.id === did);
  if (d && d.activeTaskId && d.activeTaskId !== t.id && d.status === "打印中") {
    this.toast("该设备正在执行其他任务");
    return;
  }

  // 绑定
  this.bindTaskToDevice(t, did);

  // 开始/续打
  if (!t.startedAt) t.startedAt = Date.now();
  if (t.status === "暂停" && t.pausedAt) {
    t.pausedTotal += (Date.now() - t.pausedAt);
    t.pausedAt = 0;
  }
  t.status = "打印中";

  this.toast("任务开始打印");
  this.saveAll();
},

pauseTask(taskId) {
  const t = this.queue.find(x => x.id === taskId);
  if (!t) return;
  this.ensureTaskShape(t);

  if (t.status !== "打印中") return;
  t.status = "暂停";
  t.pausedAt = Date.now();

  const d = this.devices.find(x => x.id === t.deviceId);
  if (d) d.status = "暂停";

  this.toast("已暂停");
  this.saveAll();
},

resumeTask(taskId) {
  const t = this.queue.find(x => x.id === taskId);
  if (!t) return;
  this.ensureTaskShape(t);

  if (t.status !== "暂停") return;
  // 走 startTask 的续打逻辑
  this.startTask(taskId, t.deviceId);
},

completeTask(taskId) {
  const t = this.queue.find(x => x.id === taskId);
  if (!t) return;
  this.ensureTaskShape(t);

  t.status = "完成";
  t.progress = 100;

  const d = this.devices.find(x => x.id === t.deviceId);
  if (d) {
    d.status = "在线";
    d.progress = 0;
    d.activeTaskId = "";
  }

  this.toast("任务完成");
  this.saveAll();
},

tickPrintEngine() {
  // 让任务进度驱动设备进度（真实联动）
  if (!Array.isArray(this.queue) || !this.queue.length) return;

  let changed = false;
  const now = Date.now();

  this.queue.forEach(t0 => {
    const t = this.ensureTaskShape(t0);
    if (t.status !== "打印中") return;

    const d = this.devices.find(x => x.id === t.deviceId);
    if (!d) return;

    if (!t.startedAt) t.startedAt = now;
    if (!t.estMs) t.estMs = this.calcEstMs(t, d);

    const elapsed = Math.max(0, now - t.startedAt - (t.pausedTotal || 0));
    const p = Math.min(100, Math.floor((elapsed / t.estMs) * 100));

    if (p !== t.progress) {
      t.progress = p;
      d.progress = p;
      d.status = "打印中";
      d.activeTaskId = t.id;

      // 记录参数历史（给资料库折线图用）
      this.logDeviceParams(d);

      changed = true;
    }

    // 自动完成
    if (t.progress >= 100) {
      t.status = "完成";
      d.status = "在线";
      d.progress = 0;
      d.activeTaskId = "";
      changed = true;
    }
  });

  if (changed) this.saveAll();
},

logDeviceParams(d) {
  if (!d) return;
  if (!Array.isArray(d.paramHistory)) d.paramHistory = [];

  d.paramHistory.push({
    t: Date.now(),
    nozzle: Number(d.nozzle || 1.2),
    layerHeight: Number(d.layerHeight || 0.6),
    speed: Number(d.speed || 25),
    temp: Number(d.temp || 38),
    retract: Number(d.retract || 1.5),
    flowRate: Number(d.flowRate || 1.0),
  });

  // 只保留最近 90 个点
  if (d.paramHistory.length > 90) d.paramHistory.splice(0, d.paramHistory.length - 90);
},

    // ---------- 基础 ----------
    seedLibraryIfEmpty() {
  // 兼容旧存档：没有就补齐
  if (!Array.isArray(this.libraryScience) || !this.libraryScience.length) {
    this.libraryScience = [
      {
        id: uid(),
        title: "食品3D打印：为什么适合适老化？",
        brief: "把质构、营养、禁忌与可追踪任务结合起来。",
        tags: ["适老化", "质构", "营养"],
        content:
`要点：
1）吞咽/咀嚼友好：糊状/软烂/半固体质构可控
2）营养可量化：配方克数 → 营养估算 → 任务记录
3）禁忌可校验：过敏/疾病标签贯穿原料→配方→任务
4）服务可复盘：成功率/告警/清洗策略形成闭环`
      },
      
      {
        id: uid(),
        title: "堵塞风险的常见原因与处理",
        brief: "纤维粒径、回抽、速度、喷嘴、压力阈值的组合问题。",
        tags: ["堵塞", "纤维", "回压"],
        content:
`常见原因：
- 纤维未打浆/未过筛，颗粒过大
- 喷嘴偏小、层高偏低，摩擦与回压升高
- 速度过快导致供料不稳定
- 回抽/流量倍率设置不匹配造成断料或积料
处理思路：
先把材料稳定（过筛/打浆）→ 再调喷嘴/层高/速度 → 最后用回抽/流量倍率做精修`
      },
      {
        id: uid(),
        title: "从辨体到可执行打印任务：闭环解释",
        brief: "辨体 → 策略 → 配方 → 参数 → 任务 → 运营复盘。",
        tags: ["闭环", "TCM", "运营"],
        content:
`闭环的价值在于“可执行”：
- 输出的不只是报告，而是一个可调参、可排队、可改派、可复盘的任务对象
- 设备参数与告警让运营者有抓手（速度/回抽/清洗周期等）`
      },
    ];
  }
  

  if (!Array.isArray(this.libraryParams) || !this.libraryParams.length) {
    this.libraryParams = [
      {
        key: "nozzle",
        name: "喷头直径（喷嘴）",
        unit: "mm",
        range: "0.8 ~ 1.6（高纤建议 ≥1.2）",
        oneLine: "影响堵塞风险与颗粒通过能力。",
        detail: "喷嘴越小，细节越好但堵塞风险越高；高纤/颗粒/银耳类建议加大喷嘴。",
        tips: ["高纤材料 → 喷嘴加大", "喷嘴变大后可适度提高速度", "喷嘴太大易塌陷 → 结合速度/流量调回去"]
      },
      {
        key: "layerHeight",
        name: "层高",
        unit: "mm",
        range: "0.4 ~ 0.8（高纤建议 ≥0.6）",
        oneLine: "影响成型稳定性与堵塞概率。",
        detail: "层高太低会提高摩擦与回压；层高太高会影响精细度但更稳。",
        tips: ["堵塞频发 → 层高上调", "想更细腻 → 层高下调但配合更大喷嘴", "层高变化后要同步看速度与流量倍率"]
      },
      {
        key: "speed",
        name: "速度",
        unit: "mm/s",
        range: "15 ~ 40（演示）",
        oneLine: "速度越快越省时，但供料与成型更难稳。",
        detail: "速度过快会造成断料/拉丝/塌陷；速度过慢可能导致局部堆料、回压上升。",
        tips: ["塌陷/渗水 → 降速", "断料/供料不稳 → 降速并检查回抽", "堵塞预警 → 先降速再调喷嘴"]
      },
      {
        key: "temp",
        name: "温度（料温/环境/喷头加热）",
        unit: "°C",
        range: "30 ~ 45（演示）",
        oneLine: "影响黏度、流动性与稳定性。",
        detail: "温度上升通常降低黏度，有利于出料但可能导致塌陷；温度过低易粘稠堵塞。",
        tips: ["太稠 → 温度上调", "太稀塌陷 → 温度下调 + 降速", "蛋白类注意避免结块区间"]
      },
      {
        key: "retract",
        name: "回抽",
        unit: "mm",
        range: "0 ~ 4（演示）",
        oneLine: "减少滴料与拉丝，但过大可能断料。",
        detail: "回抽用于停止出料时的回拉；过大可能导致断料、气泡、再启动不稳。",
        tips: ["滴料/拉丝 → 回抽上调", "断料 → 回抽下调 + 流量倍率微调", "回抽变化要看任务连续性与清洗策略"]
      },
      {
        key: "flowRate",
        name: "流量倍率",
        unit: "×",
        range: "0.85 ~ 1.15（演示）",
        oneLine: "相当于“出料量系数”，用于细调出料过多/过少。",
        detail: "倍率高：出料多，可能堆料/回压升；倍率低：出料少，可能空洞/断线。",
        tips: ["空洞/断线 → 倍率上调 0.02~0.05", "堆料/回压高 → 倍率下调 0.02~0.05", "倍率调完再看速度与回抽是否匹配"]
      },
    ];
  }

  if (!Array.isArray(this.milestones) || !this.milestones.length) {
    this.milestones = [
      { id: uid(), title: "完成平台闭环原型（辨体→配方→任务→运营）", date: "2025-12", type: "原型", note: "实现本地持久化与交互闭环" },
      { id: uid(), title: "沉淀 4 套可打印配方（演示）", date: "2025-12", type: "配方", note: "覆盖糊状/软烂/高纤风险" },
    ];
  }
},
openLibraryItem(a) {
  this.libraryActive = a;
},
openParam(p) {
  this.paramActive = p;
},
async copyParamTip(p) {
  const text = `${p.name}（${p.unit}）\n范围：${p.range}\n要点：${p.oneLine}\n建议：\n- ${p.tips.join("\n- ")}`;
  try {
    await navigator.clipboard.writeText(text);
    this.toast("已复制参数解释");
  } catch (e) {
    this.toast("复制失败：浏览器可能限制（可手动复制）");
  }
},
addMilestone() {
  this.milestones.unshift({
    id: uid(),
    title: "新里程碑",
    date: new Date().toISOString().slice(0,7),
    type: "进展",
    note: "演示数据（可自行编辑）"
  });
  this.toast("已新增里程碑");
  this.saveAll();
},
pickResultImage() {
  this.$refs.resultImgInput && this.$refs.resultImgInput.click();
},
onPickResultImage(e) {
  const f = e.target.files && e.target.files[0];
  if (!f) return;
  const reader = new FileReader();
  reader.onload = () => {
    this.resultsHeroImage = String(reader.result || "");
    this.toast("已更新成果展示图");
    this.saveAll();
  };
  reader.readAsDataURL(f);
},
clearResultImage() {
  this.resultsHeroImage = "";
  this.toast("已清除成果图");
  this.saveAll();
},

    go(p) {
      this.page = p;
      if (p === "platform" && !this.tab) this.tab = "org";
    },
    scrollTo(id) {
      const el = document.getElementById(id);
      if (!el) return;
      el.scrollIntoView({ behavior: "smooth", block: "start" });
    },

    toast(msg) {
      this.toastMsg = msg;
      if (this.toastTimer) clearTimeout(this.toastTimer);
      this.toastTimer = setTimeout(() => (this.toastMsg = ""), 2200);
    },

    // ---------- 存储 ----------
    saveAll() {
      const payload = {
        orgs: this.orgs,
        stations: this.stations,
        devices: this.devices,
        users: this.users,
        ingredients: this.ingredients,
        recipes: this.recipes,
        queue: this.queue,
        alerts: this.alerts,
        posts: this.posts,
        libraryScience: this.libraryScience,
        libraryParams: this.libraryParams,
        milestones: this.milestones,
        resultsHeroImage: this.resultsHeroImage,

      };
      localStorage.setItem("ksh_print_platform_v3", JSON.stringify(payload));
      this.toast("已保存（本地）");
    },
    loadAll() {
      const raw = localStorage.getItem("ksh_print_platform_v3");
      if (!raw) return;
      try {
        const p = JSON.parse(raw);
        this.orgs = p.orgs || [];
        this.stations = p.stations || [];
        this.devices = p.devices || [];
        this.users = p.users || [];
        this.ingredients = p.ingredients || [];
        this.recipes = p.recipes || [];
        this.queue = p.queue || [];
        this.alerts = p.alerts || [];
        this.posts = p.posts || [];
        this.libraryScience = p.libraryScience || [];
this.libraryParams = p.libraryParams || [];
this.milestones = p.milestones || [];
this.resultsHeroImage = p.resultsHeroImage || "";

      } catch (e) {}
    },

    ensureSelections() {
      this.selectedOrgId = this.selectedOrgId || (this.orgs[0]?.id || "");
      const ss = this.orgStations(this.selectedOrgId)[0];
      this.selectedStationId = this.selectedStationId || (ss?.id || "");
      this.selectedDeviceId = this.selectedDeviceId || (this.devices[0]?.id || "");
      this.selectedUserId = this.selectedUserId || (this.users[0]?.id || "");
      this.selectedIngredientId = this.selectedIngredientId || (this.ingredients[0]?.id || "");
      this.selectedRecipeId = this.selectedRecipeId || (this.recipes[0]?.id || "");
      this.selectedTaskId = this.selectedTaskId || (this.queue[0]?.id || "");
      this.selectedPostId = this.selectedPostId || (this.posts[0]?.id || "");

      this.taskForm.stationId = this.taskForm.stationId || (this.stations[0]?.id || "");
      this.taskForm.deviceId = this.taskForm.deviceId || (this.devicesByStation(this.taskForm.stationId)[0]?.id || "");
      this.taskForm.recipeId = this.taskForm.recipeId || (this.recipes[0]?.id || "");
      this.taskForm.userId = this.taskForm.userId || "";
      this.taskForm.qty = this.taskForm.qty || 1;
    },

    // ---------- ORG / Station ----------
    orgStations(orgId) { return this.stations.filter(s => s.orgId === orgId); },
    stationDevices(stationId) { return this.devices.filter(d => d.stationId === stationId); },
    stationName(id) { return this.stations.find(s => s.id === id)?.name || ""; },
    deviceName(id) { return this.devices.find(d => d.id === id)?.name || ""; },
    recipeName(id) { return this.recipes.find(r => r.id === id)?.name || ""; },
    userName(id) { return this.users.find(u => u.id === id)?.name || ""; },

    selectOrg(id) {
      this.selectedOrgId = id;
      const s = this.orgStations(id)[0];
      this.selectedStationId = s ? s.id : "";
    },
    selectStation(id) {
      this.selectedStationId = id;
      const d = this.stationDevices(id)[0];
      if (d) this.selectedDeviceId = d.id;
    },
    addOrg() {
      const o = { id: uid(), name: "新机构 " + (this.orgs.length + 1), type: "社区服务站", city: "衡阳" };
      this.orgs.unshift(o);
      this.selectedOrgId = o.id;
      this.toast("已新增机构");
      this.saveAll();
    },
    addStation() {
      if (!this.selectedOrgId) return this.toast("先选择机构");
      const s = {
        id: uid(),
        orgId: this.selectedOrgId,
        name: this.randomStationName(),
        address: "XX 路 XX 号",
        status: "正常",
        kpi: { orders: 12, success: 92, alerts: 0 },
      };
      this.stations.unshift(s);
      this.selectedStationId = s.id;
      this.toast("已新增站点");
      this.saveAll();
    },
    stationKpi(stationId) {
      const s = this.stations.find(x => x.id === stationId);
      return s?.kpi || { orders: 0, success: 0, alerts: 0 };
    },
    simulateAlarm() {
      const sid = this.selectedStationId || this.stations[0]?.id;
      const s = this.stations.find(x => x.id === sid);
      if (!s) return;
      s.status = "告警";
      s.kpi.alerts += 1;
      this.alerts.unshift({
        id: uid(),
        stationId: s.id,
        stationName: s.name,
        title: "喷嘴回压异常 / 堵塞风险上升",
        level: Math.random() > 0.7 ? "高" : "中",
        time: nowStr(),
      });
      this.toast("已生成告警");
      this.saveAll();
    },
randomPick(arr){ return arr[Math.floor(Math.random() * arr.length)]; },

randomUserName(){
  const surnames = ["李","王","张","刘","陈","杨","赵","黄","周","吴","徐","孙","马","朱","胡","郭","何","高","林","罗"];
  const suffix = ["爷爷","奶奶","叔叔","阿姨"];
  // 做成“李阿姨/张爷爷”这种适老风格
  let name = this.randomPick(surnames) + this.randomPick(suffix);

  // 避免重名（最多试 30 次）
  const used = new Set((this.users || []).map(u => u.name));
  let guard = 0;
  while (used.has(name) && guard++ < 30){
    name = this.randomPick(surnames) + this.randomPick(suffix);
  }
  return name;
},
randomStationName(){
  const areas = ["蒸湘","石鼓","雁峰","珠晖","南岳","衡东","衡山","祁东"];
  const suffix = ["社区站","康养站","养老服务站","营养驿站","打印站"];
  let name = this.randomPick(areas) + this.randomPick(suffix);

  const used = new Set((this.stations || []).map(s => s.name));
  let guard = 0;
  while (used.has(name) && guard++ < 30){
    name = this.randomPick(areas) + this.randomPick(suffix);
  }
  return name;
},

    // ---------- USERS ----------
    selectUser(id) { this.selectedUserId = id; },
    addUser() {
      const u = {
        id: uid(),
        name: this.randomUserName(),

        role: "老人",
        stationId: this.selectedStationId || "",
        age: 72,
        risk: "中",
        note: "演示数据",
        tcm: { primary: "", secondary: "" },
        tags: [],
      };
      this.users.unshift(u);
      this.selectedUserId = u.id;
      this.toast("已新增用户");
      this.saveAll();
    },
    bindUserToStation() {
      if (!this.selectedUser) return;
      if (!this.selectedStationId) return this.toast("先选择站点（在机构/站点Tab）");
      this.selectedUser.stationId = this.selectedStationId;
      this.toast("已绑定到当前站点");
      this.saveAll();
    },
    mockUserTCM() {
      if (!this.selectedUser) return;
      const primary = this.tcmTypes[Math.floor(Math.random() * this.tcmTypes.length)];
      const secondary = this.tcmTypes[Math.floor(Math.random() * this.tcmTypes.length)];
      this.selectedUser.tcm = { primary, secondary };
      this.selectedUser.tags = ["低盐", primary, secondary].slice(0, 3);
      this.selectedUser.risk = ["低","中","高"][Math.floor(Math.random()*3)];
      this.toast("已生成示例体质");
      this.saveAll();
    },
    openCreateTaskForUser(userId) {
      this.taskForm.userId = userId;
      this.openCreateTask();
    },

    // ---------- DEVICES ----------
    selectDevice(id) { this.selectedDeviceId = id; },
    addDevice() {
      const stationId = this.selectedStationId || this.stations[0]?.id;
      if (!stationId) return this.toast("请先创建站点");
      const d = {
        id: uid(),
        stationId,
        name: "打印机 " + (this.devices.length + 1),
        status: "在线",
        nozzle: 1.2,
        layerHeight: 0.6,
        temp: 38,
        speed: 28,
        retract: 2.0,
        pressureLimit: 90,
        mixMode: "间歇",
        materialMode: "糊状/软烂兼容",
        clogRisk: "中",
        cleanCycle: 6,
      };
      this.devices.unshift(d);
      this.selectedDeviceId = d.id;
      this.toast("已新增设备");
      this.saveAll();
    },
    riskClass(r) {
      return r === "高" ? "danger" : (r === "中" ? "warn" : "ok");
    },
    toggleDevice(deviceId) {
      const d = this.devices.find(x => x.id === deviceId);
      if (!d) return;
      d.status = d.status === "离线" ? "在线" : "离线";
      this.toast("已切换设备状态");
      this.saveAll();
    },
    quickClean(deviceId) {
      const d = this.devices.find(x => x.id === deviceId);
      if (!d) return;
      d.clogRisk = "低";
      this.toast("已执行清洗（演示）");
      this.saveAll();
    },
    autoEvaluateDevice(deviceId) {
      const d = this.devices.find(x => x.id === deviceId);
      if (!d) return;
      let score = 0;
      if (d.nozzle < 1.0) score += 2;
      if (d.layerHeight < 0.5) score += 1;
      if (d.speed > 40) score += 1;
      if (d.pressureLimit < 70) score += 1;
      d.clogRisk = score >= 4 ? "高" : (score >= 2 ? "中" : "低");
      this.toast("已重新评估堵塞风险");
      this.saveAll();
    },

    devicesByStation(stationId) {
      return this.devices.filter(d => d.stationId === stationId);
    },

    // ---------- INGREDIENTS ----------
    selectIngredient(id) { this.selectedIngredientId = id; },
    addIngredient() {
      const it = {
        id: uid(),
        name: "新原料 " + (this.ingredients.length + 1),
        category: "其他",
        stock: 12,
        lowStockLine: 6,
        fiberRisk: "中",
        tags: [],
        tagsText: "",
        nutrition: { protein: 0, carb: 0, fat: 0, kcal: 0 },
      };
      this.ingredients.unshift(it);
      this.selectedIngredientId = it.id;
      this.toast("已新增原料");
      this.saveAll();
    },
    restockIngredient(id) {
      const it = this.ingredients.find(x => x.id === id);
      if (!it) return;
      it.stock += 10;
      this.toast("已补货 +10");
      this.saveAll();
    },
    autoFillNutrition(id) {
      const it = this.ingredients.find(x => x.id === id);
      if (!it) return;
      // 演示：随机生成
      it.nutrition.protein = Math.round(Math.random()*10);
      it.nutrition.carb = Math.round(Math.random()*25);
      it.nutrition.fat = Math.round(Math.random()*8);
      it.nutrition.kcal = Math.round(40 + Math.random()*160);
      this.toast("已自动填充营养（演示）");
      this.saveAll();
    },
    ingredientTagsText(ingredientId) {
      const it = this.ingredients.find(i => i.id === ingredientId);
      if (!it) return "—";
      const tags = (it.tagsText || it.tags || []).join ? (it.tagsText ? it.tagsText : it.tags.join(", ")) : "";
      return tags || "—";
    },

    // ---------- RECIPES ----------
    selectRecipe(id) { this.selectedRecipeId = id; },
    addRecipe() {
      const r = {
        id: uid(),
        name: "新配方 " + (this.recipes.length + 1),
        category: "功能餐",
        texture: "软烂",
        viscosity: "中",
        tags: [],
        items: [{ key: uid(), ingredientId: "", grams: 100 }],
        printHint: { nozzle: 1.2, layerHeight: 0.6, temp: 38, speed: 28 },
      };
      this.recipes.unshift(r);
      this.selectedRecipeId = r.id;
      this.toast("已新增配方");
      this.saveAll();
    },
    duplicateRecipe(recipeId) {
      const r = this.recipes.find(x => x.id === recipeId);
      if (!r) return;
      const copy = JSON.parse(JSON.stringify(r));
      copy.id = uid();
      copy.name = r.name + "（复制）";
      copy.items = copy.items.map(it => ({ ...it, key: uid() }));
      this.recipes.unshift(copy);
      this.selectedRecipeId = copy.id;
      this.toast("已复制配方");
      this.saveAll();
    },
    addRecipeItem(recipeId) {
      const r = this.recipes.find(x => x.id === recipeId);
      if (!r) return;
      r.items.push({ key: uid(), ingredientId: "", grams: 50 });
      this.toast("已添加原料行");
      this.saveAll();
      
    },
    removeRecipeItem(recipeId, idx) {
      const r = this.recipes.find(x => x.id === recipeId);
      if (!r) return;
      r.items.splice(idx, 1);
      this.toast("已删除原料行");
      this.saveAll();
    },
    recipeFiberRisk(r) {
      if (!r || !r.items?.length) return "低";
      let score = 0;
      for (const it of r.items) {
        const ing = this.ingredients.find(x => x.id === it.ingredientId);
        if (!ing) continue;
        if (ing.fiberRisk === "高") score += 2;
        else if (ing.fiberRisk === "中") score += 1;
      }
      return score >= 4 ? "高" : (score >= 2 ? "中" : "低");
    },
    recipeAllergyTags(r) {
      const set = new Set();
      if (!r || !r.items) return [];
      for (const it of r.items) {
        const ing = this.ingredients.find(x => x.id === it.ingredientId);
        if (!ing) continue;
        const tags = ing.tagsText ? ing.tagsText.split(",") : (ing.tags || []);
        tags.map(x => x.trim()).filter(Boolean).forEach(t => set.add(t));
      }
      // 只保留“过敏/禁忌”类（演示：含“无/含/过敏”字样）
      return Array.from(set).filter(t => /无|含|过敏|低盐|低糖|低脂|麸质|乳|坚果|蛋|鱼虾/.test(t));
    },
    applyRecipePrintHints(recipeId) {
      const r = this.recipes.find(x => x.id === recipeId);
      if (!r) return;
      const fr = this.recipeFiberRisk(r);
      if (fr === "高") r.printHint = { nozzle: 1.4, layerHeight: 0.7, temp: 38, speed: 20 };
      else if (fr === "中") r.printHint = { nozzle: 1.2, layerHeight: 0.6, temp: 38, speed: 26 };
      else r.printHint = { nozzle: 1.0, layerHeight: 0.5, temp: 38, speed: 32 };
      this.toast("已套用打印建议参数");
      this.saveAll();
    },
    pushRecipeToTask(recipeId) {
      this.taskForm.recipeId = recipeId;
      this.openCreateTask();
      this.toast("已带入配方到任务表单");
    },

    // ---------- QUEUE / TASK ----------
    selectTask(id) { this.selectedTaskId = id; },
    taskStart(id) {
      const t = this.queue.find(x => x.id === id);
      if (!t) return;
      t.status = "打印中";
      this.toast("任务开始（演示）");
      this.saveAll();
    },
    taskPause(id) {
      const t = this.queue.find(x => x.id === id);
      if (!t) return;
      t.status = "暂停";
      this.toast("任务暂停（演示）");
      this.saveAll();
    },
    taskComplete(id) {
      const t = this.queue.find(x => x.id === id);
      if (!t) return;
      t.status = "完成";
      this.toast("任务完成（演示）");
      // KPI 演示：完成加订单
      const s = this.stations.find(x => x.id === t.stationId);
      if (s?.kpi) s.kpi.orders += 1;
      this.saveAll();
    },

    syncTaskDeviceOptions(taskId) {
      const t = this.queue.find(x => x.id === taskId);
      if (!t) return;
      const list = this.devicesByStation(t.stationId);
      if (list.length && !list.some(d => d.id === t.deviceId)) {
        t.deviceId = list[0].id;
      }
      this.toast("已同步站点设备列表");
      this.saveAll();
    },

    validateTask(taskId) {
      const t = this.queue.find(x => x.id === taskId);
      if (!t) return;
      const msg = this.validatePair(t.userId, t.recipeId);
      t.validation = msg;
      this.toast("已校验");
      this.saveAll();
    },

    validateTaskForm() {
      const msg = this.validatePair(this.taskForm.userId, this.taskForm.recipeId);
      this.taskForm.validation = msg;
      this.toast("已校验表单");
    },

    validatePair(userId, recipeId) {
      // 用户过敏/禁忌 vs 配方标签
      if (!userId) return { ok: true, msg: "未选择用户（跳过禁忌校验）" };
      const u = this.users.find(x => x.id === userId);
      const r = this.recipes.find(x => x.id === recipeId);
      if (!u || !r) return { ok: true, msg: "信息不足（跳过）" };
      const userTags = new Set([...(u.tags||[]), ...(u.allergy||[])].map(x => String(x)));
      const recipeTags = new Set(this.recipeAllergyTags(r));
      const conflict = [];
      recipeTags.forEach(t => {
        if (userTags.has(t)) conflict.push(t);
      });
      if (conflict.length) {
        return { ok: false, msg: `存在禁忌/过敏冲突：${conflict.join("、")}（建议替代配方）` };
      }
      return { ok: true, msg: "禁忌/过敏校验通过" };
    },

    syncTaskFormDevice() {
      const list = this.devicesByStation(this.taskForm.stationId);
      this.taskForm.deviceId = list[0]?.id || "";
    },

    openCreateTask() {
      this.ensureSelections();
      this.syncTaskFormDevice();
      this.taskForm.validation = { ok: true, msg: "点击校验可检查禁忌/过敏" };
      this.modals.createTask = true;
    },

    createTask() {
      // 任务标题：站点+配方
      const r = this.recipes.find(x => x.id === this.taskForm.recipeId);
      const s = this.stations.find(x => x.id === this.taskForm.stationId);
      const title = `${s?.name || "站点"} · ${r?.name || "配方"}`;

      const t = {
        id: uid(),
        title,
        stationId: this.taskForm.stationId,
        deviceId: this.taskForm.deviceId,
        recipeId: this.taskForm.recipeId,
        userId: this.taskForm.userId || "",
        qty: this.taskForm.qty || 1,
        status: "排队",
        note: this.taskForm.note || "",
        time: nowStr(),
        validation: this.validatePair(this.taskForm.userId, this.taskForm.recipeId),
      };
      this.queue.unshift(t);
      this.selectedTaskId = t.id;
      this.modals.createTask = false;
      this.toast("已创建任务（进入队列）");
      this.saveAll();
      this.tab = "queue";
      this.page = "platform";
    },

    // ---------- OPS Trend ----------
    regenTrend() {
      // 趋势数据：演示
      const a = [];
      let base = Math.floor(Math.random() * 18) + 55;
      for (let i = 0; i < 7; i++) {
        base += Math.floor(Math.random() * 12) - 5;
        base = Math.min(95, Math.max(22, base));
        a.push(base);
        
      }
      this.opsTrend = a;
    },

    suggestAction(a) {
      // 简单动作建议：基于告警标题
      let msg = "建议：插入清洗任务 + 速度降低 10% + 喷嘴增大 0.2mm";
      if ((a.title||"").includes("回压") || (a.title||"").includes("堵塞")) {
        msg = "建议：改派喷嘴≥1.2mm设备 + 插入清洗任务 + 回抽+0.5";
      }
      this.toast(msg);
    },

    // ---------- 社区 ----------
    selectPost(id) { this.selectedPostId = id; },
    openPostModal() {
      this.postForm = { title: "", content: "", tagsText: "" };
      this.modals.post = true;
    },
    createPost() {
      const title = (this.postForm.title || "").trim();
      const content = (this.postForm.content || "").trim();
      if (!title || !content) return this.toast("标题和内容不能为空");
      const tags = (this.postForm.tagsText || "").split(",").map(x => x.trim()).filter(Boolean);
      const p = {
        id: uid(),
        title,
        content,
        tags,
        author: "站点运营者",
        time: nowStr(),
        likes: 0,
        comments: [],
      };
      this.posts.unshift(p);
      this.selectedPostId = p.id;
      this.modals.post = false;
      this.toast("发布成功");
      this.saveAll();
    },
    likePost(id) {
      const p = this.posts.find(x => x.id === id);
      if (!p) return;
      p.likes += 1;
      this.toast("已点赞");
      this.saveAll();
    },
    addComment(postId) {
      const p = this.posts.find(x => x.id === postId);
      if (!p) return;
      const text = (this.newComment || "").trim();
      if (!text) return this.toast("评论不能为空");
      p.comments.push({ id: uid(), author: "营养师", time: nowStr(), text });
      this.newComment = "";
      this.toast("评论已发送");
      this.saveAll();
    },
    quickReply(postId) {
      this.newComment = "建议先做：原料过筛/打浆 → 喷嘴≥1.2mm → 层高≥0.6mm → 插入清洗任务；再根据回压告警调整速度与回抽。";
      this.toast("已填入一条常用回复");
      this.selectPost(postId);
    },

    // ---------- QuickHelp ----------
    openQuickHelp() {
      this.modals.quickHelp = true;
    },
    async copyQuickHelp() {
      const text =
`【对接清单】
1) 数据对象：Org/Station/Device/User/Ingredient/Recipe/Task/Alerts
2) 闭环：辨体→策略→配方→参数→任务→运营
3) 关键点：
- 原料：库存预警/营养/禁忌标签/纤维风险
- 配方：原料引用/版本/打印建议参数
- 任务：排队/改派/禁忌校验/复盘成功率
- 运营：趋势图+告警+动作建议
`;
      try {
        await navigator.clipboard.writeText(text);
        this.toast("已复制到剪贴板");
      } catch (e) {
        this.toast("复制失败：浏览器可能不允许（可手动复制）");
      }
    },

    // ---------- TCM ----------
    resetTCM() {
      this.tcmAnswers = {};
      this.tcmResult = { ready: false, primary: "", secondary: "", bars: [], advice: [], deviceHints: [] };
      this.toast("已重置");
    },

    initTCMQuestions() {
      const opt = [
        { t: "从不", v: 0 }, { t: "偶尔", v: 1 }, { t: "经常", v: 2 }, { t: "总是", v: 3 }
      ];
      const q = (id, title, map) => ({ id, title, map, options: opt });

      this.tcmQuestions = [
        q(1,"你是否经常怕冷、手脚发凉？（阳虚）",{阳虚:2}),
        q(2,"你是否喜欢热饮，遇冷易不适？（阳虚）",{阳虚:1}),
        q(3,"你是否容易腹泻或清晨腹泻？（阳虚）",{阳虚:2}),
        q(4,"你是否精神不振、畏寒、喜厚衣？（阳虚）",{阳虚:1}),

        q(5,"你是否容易疲乏、气短懒言？（气虚）",{气虚:2}),
        q(6,"你是否活动后出汗明显且恢复慢？（气虚）",{气虚:2}),
        q(7,"你是否说话声音偏低、易乏力？（气虚）",{气虚:1}),
        q(8,"你是否易感冒，抵抗力偏弱？（气虚）",{气虚:2}),

        q(9,"你是否口干、咽干、易上火？（阴虚）",{阴虚:2}),
        q(10,"你是否睡眠偏浅、夜间易醒？（阴虚）",{阴虚:1}),
        q(11,"你是否手心脚心发热或潮热？（阴虚）",{阴虚:2}),
        q(12,"你是否喜欢冷饮但喝多又不舒服？（阴虚）",{阴虚:1}),

        q(13,"你是否易困倦、头重如裹？（痰湿）",{痰湿:2}),
        q(14,"你是否偏爱油腻甜食且体重易增？（痰湿）",{痰湿:2}),
        q(15,"你是否舌苔厚腻或口中黏腻？（痰湿）",{痰湿:2}),
        q(16,"你是否痰多或胸闷？（痰湿）",{痰湿:1}),

        q(17,"你是否口苦口臭、易长痘？（湿热）",{湿热:2}),
        q(18,"你是否大便黏滞不爽或尿黄？（湿热）",{湿热:2}),
        q(19,"你是否怕热、容易出油出汗？（湿热）",{湿热:1}),
        q(20,"你是否偏爱辛辣烧烤且易上火？（湿热）",{湿热:2}),

        q(21,"你是否面色晦暗或易有瘀斑？（血瘀）",{血瘀:2}),
        q(22,"你是否常有刺痛、痛处固定？（血瘀）",{血瘀:2}),
        q(23,"你是否四肢麻木或循环差？（血瘀）",{血瘀:1}),
        q(24,"你是否唇色偏暗或舌有瘀点？（血瘀）",{血瘀:2}),

        q(25,"你是否容易烦躁、压力大、叹气？（气郁）",{气郁:2}),
        q(26,"你是否胸闷或情绪波动明显？（气郁）",{气郁:2}),
        q(27,"你是否食欲受情绪影响明显？（气郁）",{气郁:1}),
        q(28,"你是否睡眠受心情影响明显？（气郁）",{气郁:1}),

        q(29,"你是否有过敏史（药物/食物/花粉）？（特禀）",{特禀:2}),
        q(30,"你是否皮肤易过敏或荨麻疹？（特禀）",{特禀:2}),
        q(31,"你是否对气味/季节变化敏感？（特禀）",{特禀:1}),
        q(32,"你是否对海鲜/坚果/乳制品等易不适？（特禀）",{特禀:2}),

        q(33,"你是否精力充沛、睡眠较好？（平和）",{平和:1}),
        q(34,"你是否胃口稳定、消化较好？（平和）",{平和:1}),
        q(35,"你是否对冷热适应力较强？（平和）",{平和:1}),
        q(36,"你是否情绪稳定、压力应对良好？（平和）",{平和:1}),
      ];
    },

    submitTCM() {
      const score = {};
      this.tcmTypes.forEach(t => (score[t] = 0));

      for (const qq of this.tcmQuestions) {
        const v = Number(this.tcmAnswers[qq.id] ?? 0);
        const map = qq.map || {};
        Object.keys(map).forEach(k => {
          score[k] += v * map[k];
        });
      }

      // “平和”用反向抵消：越偏极端越不平和（演示）
      const total = Object.values(this.tcmAnswers).reduce((a,b)=>a + Number(b||0), 0);
      score["平和"] = Math.max(0, 60 - total * 1.6);

      const sorted = Object.entries(score).sort((a,b)=>b[1]-a[1]);
      const primary = sorted[0][0];
      const secondary = sorted[1][0];

      const bars = sorted.slice(0, 6).map(([k,v]) => ({ k, v: Math.min(100, Math.max(0, Math.round(v * 4))) }));

      this.tcmResult = {
        ready: true,
        primary,
        secondary,
        bars,
        advice: this.buildAdvice(primary, secondary),
        deviceHints: this.buildDeviceHints(primary),
      };
      this.toast(`评估完成：主 ${primary} / 次 ${secondary}`);
    },

    buildAdvice(p, s) {
      const a = [];
      const push = (x) => a.push(x);

      if (p === "阳虚") {
        push("温补优先：南瓜/红薯/小米/鸡肉糊类，避免生冷。");
        push("质构建议：软烂/糊状，温控 38~45°C 更稳定。");
      }
      if (p === "气虚") {
        push("益气健脾：山药/小米/豆制品（注意过敏）。");
        push("打印建议：黏度中等偏高，避免过稀塌陷。");
      }
      if (p === "阴虚") {
        push("滋阴润燥：银耳/百合/梨/鱼肉糊，少辛辣。");
        push("打印建议：含水率略高但要控速度，防渗水塌陷。");
      }
      if (p === "痰湿") {
        push("健脾祛湿：少油少糖，纤维要“打浆+过筛”。");
        push("打印建议：纤维风险高时喷嘴≥1.2mm，层高≥0.6mm。");
      }
      if (p === "湿热") {
        push("清淡少油：低盐低油，避免辛辣烧烤。");
        push("打印建议：低脂糊类更稳，注意保水与成型。");
      }
      if (p === "血瘀") {
        push("活血化瘀：注意均衡，避免久坐，饮食清淡规律。");
        push("打印建议：可加入“软烂颗粒”提升口感但需控粒径。");
      }
      if (p === "气郁") {
        push("舒缓为主：规律饮食，减少高糖高咖啡因刺激。");
        push("打印建议：菜单交互要“情绪友好”，提供选择掌控感。");
      }
      if (p === "特禀") {
        push("过敏管理：过敏源标签贯穿原料→配方→任务。");
        push("打印建议：同一菜品提供替代配方版本（无乳/无麸质等）。");
      }
      push(`次体质提示：${s} 可作为“微调层”策略。`);
      return a;
    },

    buildDeviceHints(p) {
      const a = [];
      if (p === "阳虚") a.push("温控建议 38~45°C，避免冷料粘度突变导致断料。");
      if (p === "阴虚") a.push("含水率偏高时建议降低速度、增大回抽，防止渗水塌陷。");
      if (p === "痰湿") a.push("纤维风险高：喷嘴≥1.2mm / 层高≥0.6mm / 回抽↑ / 压力阈值监控。");
      a.push("连续任务多：按清洗周期插入清洗任务，降低堵塞概率。");
      return a;
    },

    createRecipeFromTCM() {
      if (!this.tcmResult.ready) return this.toast("先提交评估");
      const p = this.tcmResult.primary;

      // 根据体质生成一个“可编辑的推荐配方”
      const pick = (...names) => this.ingredients.find(i => names.includes(i.name))?.id || "";

      let name = "推荐配方";
      let items = [];
      let texture = "软烂";
      let viscosity = "中";
      let category = "功能餐";

      if (p === "阳虚") {
        name = "南瓜鸡肉温补糊";
        texture = "糊状"; viscosity = "高";
        items = [
          { key: uid(), ingredientId: pick("南瓜", "红薯"), grams: 160 },
          { key: uid(), ingredientId: pick("鸡胸肉", "鸡肉"), grams: 80 },
          { key: uid(), ingredientId: pick("小米", "大米"), grams: 40 },
        ];
      } else if (p === "痰湿") {
        name = "山药薏米祛湿糊";
        texture = "糊状"; viscosity = "中";
        items = [
          { key: uid(), ingredientId: pick("山药"), grams: 120 },
          { key: uid(), ingredientId: pick("薏米", "燕麦"), grams: 60 },
          { key: uid(), ingredientId: pick("冬瓜", "胡萝卜"), grams: 80 },
        ];
      } else if (p === "阴虚") {
        name = "银耳百合润燥羹";
        texture = "糊状"; viscosity = "中";
        items = [
          { key: uid(), ingredientId: pick("银耳"), grams: 50 },
          { key: uid(), ingredientId: pick("百合"), grams: 50 },
          { key: uid(), ingredientId: pick("梨", "香蕉"), grams: 120 },
        ];
      } else if (p === "特禀") {
        name = "低敏山药小米糊";
        texture = "糊状"; viscosity = "中";
        items = [
          { key: uid(), ingredientId: pick("山药"), grams: 140 },
          { key: uid(), ingredientId: pick("小米"), grams: 70 },
          { key: uid(), ingredientId: pick("梨"), grams: 80 },
        ];
      } else {
        name = `体质推荐餐（${p}）`;
        items = [
          { key: uid(), ingredientId: this.ingredients[0]?.id || "", grams: 120 },
          { key: uid(), ingredientId: this.ingredients[1]?.id || "", grams: 80 },
        ];
      }

      const r = {
        id: uid(),
        name,
        category,
        texture,
        viscosity,
        tags: ["推荐", p],
        items: items.length ? items : [{ key: uid(), ingredientId: "", grams: 100 }],
        printHint: { nozzle: 1.2, layerHeight: 0.6, temp: 38, speed: 26 },
      };

      this.recipes.unshift(r);
      this.selectedRecipeId = r.id;
      this.applyRecipePrintHints(r.id);
      this.toast("已生成推荐配方（可继续编辑）");
      this.saveAll();
      this.go("platform");
      this.tab = "recipes";
    },

    // ---------- Seed ----------
    seedAll() {
      // Org
      const org1 = { id: uid(), name: "衡阳社区健康服务中心", type: "社区服务站", city: "衡阳" };
      const org2 = { id: uid(), name: "南华大学附属养老照护点", type: "养老机构", city: "衡阳" };
      this.orgs = [org1, org2];

      // Stations
      const s1 = { id: uid(), orgId: org1.id, name: "蒸湘社区站", address: "蒸湘区 XX 路", status: "正常", kpi: { orders: 42, success: 93, alerts: 1 } };
      const s2 = { id: uid(), orgId: org1.id, name: "石鼓社区站", address: "石鼓区 XX 路", status: "正常", kpi: { orders: 31, success: 91, alerts: 0 } };
      const s3 = { id: uid(), orgId: org2.id, name: "附属养老站", address: "南华大学附属院区", status: "正常", kpi: { orders: 55, success: 95, alerts: 2 } };
      this.stations = [s1, s2, s3];

      // Devices
      this.devices = [
        { id: uid(), stationId: s1.id, name: "KSP-01（双料仓）", status: "在线", nozzle: 1.2, layerHeight: 0.6, temp: 38, speed: 28, retract: 2.0, pressureLimit: 90, mixMode: "间歇", materialMode: "糊状/软烂兼容", clogRisk: "中", cleanCycle: 6 },
        { id: uid(), stationId: s1.id, name: "KSP-02（高纤模式）", status: "在线", nozzle: 1.4, layerHeight: 0.7, temp: 38, speed: 20, retract: 2.5, pressureLimit: 95, mixMode: "持续", materialMode: "高纤/颗粒兼容", clogRisk: "低", cleanCycle: 5 },
        { id: uid(), stationId: s2.id, name: "KSP-03（标准）", status: "在线", nozzle: 1.2, layerHeight: 0.6, temp: 38, speed: 26, retract: 2.0, pressureLimit: 85, mixMode: "间歇", materialMode: "糊状/软烂兼容", clogRisk: "中", cleanCycle: 6 },
        { id: uid(), stationId: s3.id, name: "KSP-04（机构版）", status: "打印中", nozzle: 1.2, layerHeight: 0.6, temp: 40, speed: 24, retract: 2.2, pressureLimit: 92, mixMode: "持续", materialMode: "机构高频任务", clogRisk: "中", cleanCycle: 5 },
      ];

      // Users
      this.users = [
        { id: uid(), name: "刘奶奶", role: "老人", stationId: s3.id, age: 78, risk: "中", note: "吞咽偏弱", tcm: { primary: "痰湿", secondary: "气虚" }, tags: ["低盐","痰湿"] },
        { id: uid(), name: "王爷爷", role: "老人", stationId: s3.id, age: 82, risk: "高", note: "血糖波动", tcm: { primary: "气虚", secondary: "血瘀" }, tags: ["低糖","气虚"] },
        { id: uid(), name: "陈阿姨", role: "老人", stationId: s1.id, age: 74, risk: "低", note: "偏怕冷", tcm: { primary: "阳虚", secondary: "平和" }, tags: ["阳虚"] },
        { id: uid(), name: "李营养师", role: "营养师", stationId: s1.id, age: 32, risk: "低", note: "负责配方审核", tcm: { primary: "", secondary: "" }, tags: [] },
        { id: uid(), name: "周操作员", role: "操作员", stationId: s2.id, age: 29, risk: "低", note: "负责设备维护", tcm: { primary: "", secondary: "" }, tags: [] },
      ];

      // Ingredients
      const ing = (name, category, stock, low, fiberRisk, tagsText, nutrition) => ({
        id: uid(),
        name, category, stock, lowStockLine: low,
        fiberRisk,
        tagsText,
        tags: (tagsText||"").split(",").map(x=>x.trim()).filter(Boolean),
        nutrition: nutrition || { protein: 0, carb: 0, fat: 0, kcal: 0 },
      });

      this.ingredients = [
        ing("南瓜","蔬菜", 20, 6, "中", "低盐", {protein:1,carb:7,fat:0,kcal:34}),
        ing("红薯","薯类", 18, 6, "中", "低脂", {protein:1,carb:20,fat:0,kcal:86}),
        ing("山药","薯类", 16, 5, "中", "低盐", {protein:2,carb:17,fat:0,kcal:75}),
        ing("薏米","谷物", 14, 5, "高", "无麸质", {protein:4,carb:70,fat:1,kcal:350}),
        ing("小米","谷物", 22, 6, "中", "无麸质", {protein:4,carb:73,fat:2,kcal:360}),
        ing("大米","谷物", 26, 7, "低", "无麸质", {protein:2,carb:78,fat:1,kcal:360}),
        ing("鸡胸肉","肉类", 12, 4, "低", "含蛋白", {protein:22,carb:0,fat:2,kcal:120}),
        ing("鳕鱼","肉类", 10, 4, "低", "含鱼虾", {protein:18,carb:0,fat:1,kcal:90}),
        ing("银耳","其他", 8, 3, "高", "低糖", {protein:1,carb:10,fat:0,kcal:50}),
        ing("百合","其他", 9, 3, "中", "低脂", {protein:2,carb:12,fat:0,kcal:60}),
        ing("梨","水果", 14, 4, "低", "低糖", {protein:0,carb:10,fat:0,kcal:40}),
        ing("香蕉","水果", 12, 4, "低", "低脂", {protein:1,carb:23,fat:0,kcal:90}),
        ing("冬瓜","蔬菜", 16, 5, "中", "低盐", {protein:1,carb:4,fat:0,kcal:18}),
        ing("胡萝卜","蔬菜", 18, 6, "中", "低盐", {protein:1,carb:10,fat:0,kcal:41}),
        ing("豆腐","豆制品", 11, 4, "低", "含大豆", {protein:8,carb:2,fat:4,kcal:76}),
        ing("牛奶","蛋奶", 9, 3, "低", "含乳制品", {protein:3,carb:5,fat:3,kcal:60}),
      ];

      // Recipes
      const rid = (name) => this.ingredients.find(i => i.name === name)?.id || "";
      this.recipes = [
        {
          id: uid(), name: "南瓜鸡肉温补糊", category: "功能餐", texture: "糊状", viscosity: "高", tags: ["阳虚","推荐"],
          items: [
            { key: uid(), ingredientId: rid("南瓜"), grams: 180 },
            { key: uid(), ingredientId: rid("鸡胸肉"), grams: 80 },
            { key: uid(), ingredientId: rid("小米"), grams: 40 },
          ],
          printHint: { nozzle: 1.2, layerHeight: 0.6, temp: 40, speed: 24 },
        },
        {
          id: uid(), name: "山药薏米祛湿糊", category: "功能餐", texture: "糊状", viscosity: "中", tags: ["痰湿","推荐"],
          items: [
            { key: uid(), ingredientId: rid("山药"), grams: 140 },
            { key: uid(), ingredientId: rid("薏米"), grams: 60 },
            { key: uid(), ingredientId: rid("冬瓜"), grams: 100 },
          ],
          printHint: { nozzle: 1.4, layerHeight: 0.7, temp: 38, speed: 20 },
        },
        {
          id: uid(), name: "银耳百合润燥羹", category: "汤羹", texture: "糊状", viscosity: "中", tags: ["阴虚","推荐"],
          items: [
            { key: uid(), ingredientId: rid("银耳"), grams: 60 },
            { key: uid(), ingredientId: rid("百合"), grams: 50 },
            { key: uid(), ingredientId: rid("梨"), grams: 120 },
          ],
          printHint: { nozzle: 1.2, layerHeight: 0.6, temp: 38, speed: 22 },
        },
        {
          id: uid(), name: "鳕鱼胡萝卜软烂餐", category: "菜", texture: "软烂", viscosity: "中", tags: ["高蛋白"],
          items: [
            { key: uid(), ingredientId: rid("鳕鱼"), grams: 120 },
            { key: uid(), ingredientId: rid("胡萝卜"), grams: 120 },
            { key: uid(), ingredientId: rid("大米"), grams: 50 },
          ],
          printHint: { nozzle: 1.2, layerHeight: 0.6, temp: 40, speed: 26 },
        },
      ];

      // Queue
      this.queue = [
        { id: uid(), title: `${s3.name} · 南瓜鸡肉温补糊`, stationId: s3.id, deviceId: this.devices[3].id, recipeId: this.recipes[0].id, userId: this.users[0].id, qty: 2, status: "打印中", note: "软烂", time: nowStr(), validation: { ok: true, msg: "禁忌校验通过" } },
        { id: uid(), title: `${s1.name} · 山药薏米祛湿糊`, stationId: s1.id, deviceId: this.devices[1].id, recipeId: this.recipes[1].id, userId: this.users[2].id, qty: 1, status: "排队", note: "加清洗", time: nowStr(), validation: { ok: true, msg: "禁忌校验通过" } },
        { id: uid(), title: `${s2.name} · 银耳百合润燥羹`, stationId: s2.id, deviceId: this.devices[2].id, recipeId: this.recipes[2].id, userId: "", qty: 3, status: "排队", note: "", time: nowStr(), validation: { ok: true, msg: "未选择用户（跳过禁忌校验）" } },
      ];

      // Alerts
      this.alerts = [
        { id: uid(), stationId: s3.id, stationName: s3.name, title: "连续任务过多：建议插入清洗任务", level: "中", time: nowStr() },
        { id: uid(), stationId: s1.id, stationName: s1.name, title: "原料低库存：薏米", level: "低", time: nowStr() },
      ];

      // Posts
      this.posts = [
        { id: uid(), title: "纤维堵塞怎么解决？", content: "我们站点用薏米/银耳时堵塞概率上升，大家喷嘴/层高/速度怎么配？", tags: ["堵塞","纤维","参数"], author: "周操作员", time: nowStr(), likes: 6, comments: [] },
        { id: uid(), title: "配方版本怎么管理更合理？", content: "同一个菜品要无乳/无麸质/低盐版本，建议怎么命名和回滚？", tags: ["配方","版本","运营"], author: "李营养师", time: nowStr(), likes: 3, comments: [] },
      ];

      this.selectedOrgId = org1.id;
      this.selectedStationId = s1.id;
      this.selectedDeviceId = this.devices[0].id;
      this.selectedUserId = this.users[0].id;
      this.selectedIngredientId = this.ingredients[0].id;
      this.selectedRecipeId = this.recipes[0].id;
      this.selectedTaskId = this.queue[0].id;
      this.selectedPostId = this.posts[0].id;

      this.saveAll();
    },
  },
};
</script>

<style>
:root{
  --k1:#1e5eff;
  --k2:#2f7dff;
  --k3:#7aa8ff;
  --bg1:#eaf1ff;
  --bg2:#f8fbff;

  --glass:rgba(255,255,255,.52);
  --border:rgba(255,255,255,.32);
  --text:#0b1020;
  --muted:#5a6477;
  --shadow:0 18px 50px rgba(30,94,255,.14);
}

*{box-sizing:border-box}
body{margin:0;font-family:-apple-system,BlinkMacSystemFont,"SF Pro Text","PingFang SC",sans-serif;color:var(--text)}
.app{min-height:100vh;position:relative;overflow:hidden;background:linear-gradient(130deg,var(--bg1),var(--bg2))}
.center{display:flex;justify-content:center;align-items:center}

.bg{position:fixed;inset:0;z-index:0;pointer-events:none}
.blob{position:absolute;filter:blur(60px);opacity:.55}
.blob.b1{width:520px;height:520px;left:-120px;top:-120px;background:radial-gradient(circle,var(--k2),transparent 60%)}
.blob.b2{width:620px;height:620px;right:-180px;top:120px;background:radial-gradient(circle,var(--k1),transparent 62%)}
.blob.b3{width:520px;height:520px;left:20%;bottom:-220px;background:radial-gradient(circle,var(--k3),transparent 60%)}
.grid-noise{
  position:absolute;inset:0;
  background:
    radial-gradient(circle at 20% 10%, rgba(30,94,255,.12), transparent 50%),
    radial-gradient(circle at 80% 30%, rgba(47,125,255,.10), transparent 52%),
    linear-gradient(transparent 0, rgba(255,255,255,.18) 80%, transparent 100%);
  opacity:.45;
}

.glass{
  background:var(--glass);
  backdrop-filter:blur(18px);
  -webkit-backdrop-filter:blur(18px);
  border:1px solid var(--border);
  border-radius:18px;
  box-shadow:var(--shadow);
}

.nav{
  position:sticky;top:0;z-index:10;
  display:flex;justify-content:space-between;align-items:center;
  padding:14px 22px;margin:14px;
}
.nav-left{display:flex;align-items:center;gap:10px}
.logo{font-weight:800;font-size:18px;color:var(--k1);letter-spacing:.5px}
.logo.small{font-size:16px}
.sub{font-size:12px;color:var(--muted);max-width:420px}

.nav-tabs button{
  background:none;border:none;margin:0 8px;
  font-size:14px;cursor:pointer;color:#1b2233;
  padding:10px 10px;border-radius:12px;
}
.nav-tabs button:hover{background:rgba(30,94,255,.06)}
.nav-tabs .active{color:var(--k1);font-weight:800;background:rgba(30,94,255,.09)}

.nav-right{display:flex;gap:10px}
.btn{
  border:none;cursor:pointer;
  padding:11px 16px;border-radius:14px;
  font-size:14px;
}
.btn.primary{background:linear-gradient(90deg,var(--k2),var(--k1));color:#fff}
.btn.ghost{background:transparent;border:1px solid rgba(30,94,255,.35);color:var(--k1)}
.btn.sm{padding:8px 12px;border-radius:12px;font-size:13px}

.bottom-nav{
  position:fixed;left:50%;transform:translateX(-50%);
  bottom:14px;z-index:12;
  display:none;gap:6px;
  padding:10px;
}
.bottom-nav button{
  border:none;background:transparent;cursor:pointer;
  display:flex;flex-direction:column;align-items:center;gap:4px;
  padding:10px 12px;border-radius:14px;color:#1b2233;font-weight:800;font-size:12px;
}
.bottom-nav button .i{font-size:16px}
.bottom-nav button.active{background:rgba(30,94,255,.12);color:var(--k1)}

.toast{
  position:fixed;right:18px;bottom:88px;z-index:99;
  width:280px;padding:12px 14px;
}
.t-title{font-weight:950}
.t-msg{margin-top:6px;color:var(--muted);font-size:13px;line-height:1.4}

.main{position:relative;z-index:1;padding:18px 22px 76px}
.page{max-width:1200px;margin:0 auto;padding:14px 0}
.page-head{display:flex;justify-content:space-between;align-items:flex-end;gap:16px;margin-bottom:14px}
.page-head h2{margin:0;font-size:28px}
.head-actions{display:flex;gap:10px;align-items:center}

.pill{
  display:inline-flex;align-items:center;justify-content:center;
  padding:6px 10px;border-radius:999px;
  color:var(--k1);background:rgba(30,94,255,.10);
  font-weight:800;font-size:12px;
}
.pill2{
  padding:6px 10px;border-radius:999px;
  background:rgba(0,0,0,.04);
  font-size:12px;color:var(--muted);
}
.muted{color:var(--muted)}
.muted.small{font-size:12px}
.big{font-size:22px;font-weight:900}

.home-nav{display:flex;gap:10px;flex-wrap:wrap;padding:10px 12px;margin:4px 0 10px}
.home-nav button{
  border:none;cursor:pointer;
  padding:10px 12px;border-radius:14px;
  background:rgba(0,0,0,.035);
  font-weight:800;color:#1b2233;
}
.home-nav button:hover{background:rgba(30,94,255,.10);color:var(--k1)}

.hero{display:grid;grid-template-columns:1.08fr .92fr;gap:34px;align-items:center;padding:14px 0 22px}
.hero h1{font-size:42px;line-height:1.08;margin:12px 0 8px}
.hero p{margin:0;line-height:1.65}
.hero-actions{display:flex;gap:14px;margin:18px 0 10px;flex-wrap:wrap}

.kpis{margin-top:16px;display:grid;grid-template-columns:repeat(4,1fr);gap:12px}
.kpi{padding:14px}
.kpi-num{font-weight:900;font-size:20px;color:#111}
.kpi-label{font-size:12px;color:var(--muted)}

.hero-visual{display:flex;flex-direction:column;gap:10px}
.preview{padding:16px}
.pv-head{margin-bottom:10px}
.pv-title{font-weight:900}
.pv-sub{font-size:12px}
.pv-grid{display:grid;grid-template-columns:repeat(2,1fr);gap:10px;margin-top:12px}
.pv-card{padding:12px}
.pv-k{font-size:12px;color:var(--muted);font-weight:800}
.pv-v{font-size:22px;font-weight:950;margin-top:4px}
.pv-tip{font-size:12px;margin-top:4px}
.pv-strip{margin-top:12px;padding:12px;display:flex;justify-content:space-between;align-items:center}
.strip-title{font-weight:900}
.hero-note{margin-top:6px;font-size:12px;line-height:1.5}

.section{margin-top:32px}
.section-head{text-align:center;margin-bottom:18px}
.section-head h2{margin:10px 0 8px;font-size:28px}

.steps{display:grid;grid-template-columns:repeat(3,1fr);gap:16px;margin-top:16px}
.step{padding:18px;text-align:center}
.step-num{
  width:44px;height:44px;border-radius:50%;
  background:rgba(30,94,255,.18);
  display:flex;align-items:center;justify-content:center;
  margin:0 auto 10px;color:var(--k1);font-weight:950
}

.features{display:grid;grid-template-columns:repeat(3,1fr);gap:16px}
.feature{padding:18px}
.feature h3{margin:0 0 8px}
.feature p{margin:0 0 10px;line-height:1.6}

.scene-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:16px}
.scene{padding:16px}
.scene-title{font-weight:900;margin-bottom:6px}

.footer{margin-top:46px;padding:18px}
.footer-grid{display:grid;grid-template-columns:1.2fr 1fr 1fr;gap:18px}
.ft-title{font-weight:950;margin-bottom:10px}
.ft-link{color:var(--muted);margin:8px 0;cursor:pointer}
.ft-link:hover{color:var(--k1)}
.social{display:flex;gap:10px;margin-top:10px}
.s{
  width:28px;height:28px;border-radius:10px;
  background:rgba(0,0,0,.04);
  display:flex;align-items:center;justify-content:center;
  color:#6c768a;font-weight:900
}

.tcm-layout{display:grid;grid-template-columns:1.1fr .9fr;gap:16px}
.tcm-card{padding:18px}
.tcm-top{display:flex;justify-content:space-between;align-items:center;margin-bottom:8px}
.q{padding:12px 0;border-bottom:1px dashed rgba(0,0,0,.08)}
.q:last-child{border-bottom:none}
.q-title{font-weight:900;margin-bottom:8px}
.q-options{display:flex;gap:10px;flex-wrap:wrap}
.opt{display:flex;gap:8px;align-items:center;background:rgba(0,0,0,.03);padding:8px 10px;border-radius:12px}
.opt input{accent-color:var(--k1)}

.tcm-result{padding:18px}
.result-hero{display:flex;gap:10px;flex-wrap:wrap;margin-bottom:10px}
.badge{padding:8px 12px;border-radius:999px;background:rgba(30,94,255,.14);color:var(--k1);font-weight:950}
.badge.ghost{background:rgba(0,0,0,.04);color:var(--muted);font-weight:850}

.bars{display:flex;flex-direction:column;gap:10px;margin-top:12px}
.bar-row{display:grid;grid-template-columns:90px 1fr 40px;gap:10px;align-items:center}
.bar-name{font-size:12px;color:var(--muted)}
.bar-track{height:10px;border-radius:999px;background:rgba(0,0,0,.06);overflow:hidden}
.bar-fill{height:100%;background:linear-gradient(90deg,var(--k2),var(--k1))}
.bar-val{font-size:12px;color:var(--muted);text-align:right}

.advice{padding:14px}
.advice h4{margin:0 0 8px}
.advice ul{margin:0;padding-left:18px}
.ad-actions{display:flex;gap:10px;flex-wrap:wrap;margin-top:10px}

.tabs2{display:flex;gap:8px;flex-wrap:wrap;margin:10px 0 14px;padding:10px 12px}
.tabs2 button{
  border:none;cursor:pointer;
  background:rgba(0,0,0,.035);
  padding:10px 12px;border-radius:14px;
  color:#1b2233;font-weight:900
}
.tabs2 button.active{background:rgba(30,94,255,.12);color:var(--k1)}

.grid2{display:grid;grid-template-columns:1fr 1fr;gap:16px}
.panel{padding:16px}
.panel-head{display:flex;justify-content:space-between;align-items:center;gap:10px;margin-bottom:10px}
.panel-head h3{margin:0}
.inline{display:flex;gap:10px;align-items:center}

.list{display:flex;flex-direction:column;gap:10px}
.row{
  padding:12px;border-radius:16px;
  border:1px solid rgba(0,0,0,.05);
  background:rgba(255,255,255,.55);
  display:flex;justify-content:space-between;gap:10px;
  cursor:pointer
}
.row:hover{border-color:rgba(30,94,255,.20)}
.row.active{border-color:rgba(30,94,255,.45);box-shadow:0 8px 24px rgba(30,94,255,.10)}
.title{font-weight:950}
.chips{display:flex;gap:8px;align-items:center;flex-wrap:wrap}
.chip{
  font-size:12px;padding:6px 10px;border-radius:999px;
  background:rgba(0,0,0,.05);color:var(--muted);font-weight:850
}
.chip.ghost{background:rgba(0,0,0,.03)}
.chip.ok{background:rgba(82,196,26,.14);color:#2c7a2c}
.chip.warn{background:rgba(250,173,20,.18);color:#8a5a00}
.chip.danger{background:rgba(255,77,79,.16);color:#b33a3a}
.chip.run{background:rgba(30,94,255,.14);color:var(--k1)}
.chip.off{background:rgba(0,0,0,.06);color:#6c768a}

.inner{padding:14px;margin-top:10px}
.inner-head{display:flex;justify-content:space-between;align-items:center;margin-bottom:10px}
.cards3{display:grid;grid-template-columns:repeat(3,1fr);gap:10px}
.ops-cards{display:grid;grid-template-columns:repeat(3,1fr);gap:10px}
.mini{background:rgba(255,255,255,.6);border:1px solid rgba(0,0,0,.05);border-radius:16px;padding:12px}

.input,.select,.textarea{
  width:100%;
  padding:11px 12px;
  border-radius:14px;
  border:1px solid rgba(0,0,0,.08);
  background:rgba(255,255,255,.7);
  outline:none;
}
.textarea{min-height:110px;resize:vertical}
.input:focus,.select:focus,.textarea:focus{border-color:rgba(30,94,255,.35)}

.param-form{display:grid;grid-template-columns:repeat(2,1fr);gap:12px}
.f .lab{font-size:12px;color:var(--muted);font-weight:900;margin:4px 0 6px}

/* 简单表格 */
.table{display:flex;flex-direction:column;gap:8px}
.tr{display:grid;grid-template-columns:1.2fr .6fr 1fr .5fr;gap:10px;align-items:center}
.tr.th{font-size:12px;color:var(--muted);font-weight:950}

/* OPS chart */
.ops-svg{width:100%;height:220px}
.ops-x{display:flex;justify-content:space-between;margin-top:8px;padding:0 6px}

/* Community */
.content{margin-top:10px;white-space:pre-wrap;line-height:1.65}
.comment{padding:10px 12px;border-radius:14px;background:rgba(0,0,0,.03);margin-top:10px}
.c-head{display:flex;justify-content:space-between;align-items:center}
.c-author{font-weight:950}
.c-body{margin-top:6px;line-height:1.6}

/* Modals */
.modal-mask{
  position:fixed;inset:0;z-index:200;
  background:rgba(10,16,32,.35);
  display:flex;align-items:center;justify-content:center;
  padding:16px;
}
.modal{width:min(860px, 96vw);max-height:90vh;overflow:auto;padding:16px}
.m-head{display:flex;justify-content:space-between;align-items:flex-start;gap:12px}
.m-title{font-weight:950;font-size:18px}
.m-body{margin-top:12px}
.m-foot{display:flex;justify-content:flex-end;gap:10px;margin-top:14px}

@media (max-width: 980px){
  .hero{grid-template-columns:1fr;gap:14px}
  .kpis{grid-template-columns:repeat(2,1fr)}
  .features{grid-template-columns:repeat(2,1fr)}
  .scene-grid{grid-template-columns:1fr}
  .steps{grid-template-columns:1fr}
  .tcm-layout{grid-template-columns:1fr}
  .grid2{grid-template-columns:1fr}
  .nav{flex-direction:column;align-items:flex-start;gap:10px}
  .nav-tabs{width:100%;display:flex;flex-wrap:wrap}
  .nav-right{width:100%;justify-content:flex-end}
  .bottom-nav{display:flex}
  .ops-cards{grid-template-columns:1fr}
}
</style>
<style scoped>
/* ……你原来的样式…… */

/* ✅ 放这里：第3点新增CSS */
.inner-head{
  display:flex;
  align-items:center;
  justify-content:space-between;
  gap:12px;
}
.head-actions{
  display:flex;
  align-items:center;
  justify-content:flex-end;
  gap:10px;
  flex-wrap:wrap;
}
.inline{
  display:flex;
  align-items:center;
  gap:8px;
  flex-wrap:wrap;
}
.video-card{
  max-width: 980px;
  margin: 28px auto 0;
  border-radius: 18px;
  background: rgba(255,255,255,.55);
  box-shadow: 0 18px 40px rgba(0,0,0,.12);
  overflow: hidden;
  border: 1px solid rgba(255,255,255,.7);
}

.video-wrap{
  position: relative;
  width: 100%;
  aspect-ratio: 16 / 9;
  background: #000;
}

.video-iframe{
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  border: 0;
}
.video-card{ padding:14px; border-radius:18px; }
.video-topbar{ display:flex; justify-content:space-between; gap:12px; margin-bottom:10px; align-items:flex-start; }
.video-title{ font-weight:900; font-size:16px; }
.slogan{ margin:8px 0 0; line-height:1.7; max-width:680px; }
.video-actions{ display:flex; gap:10px; flex-wrap:wrap; align-items:center; }

.video-replace{
  margin: 10px 0 12px;
  padding: 10px;
  border-radius: 14px;
  background: rgba(255,255,255,0.55);
  border: 1px solid rgba(100,140,255,0.18);
}
.video-replace .input{ width:100%; }
.video-replace-actions{ margin-top:10px; display:flex; gap:10px; justify-content:flex-end; }

.video-wrap{ border-radius:16px; overflow:hidden; box-shadow:0 18px 50px rgba(0,0,0,0.12); }
.video-iframe{ width:100%; aspect-ratio:16/9; display:block; }

.modal-mask{
  position:fixed; inset:0; z-index:9999;
  background:rgba(10,18,40,0.45);
  backdrop-filter: blur(10px);
  display:flex; align-items:center; justify-content:center;
  padding:18px;
}
.modal-card{ width:min(980px, 96vw); border-radius:20px; padding:14px; }
.modal-head{ display:flex; justify-content:space-between; align-items:center; padding:6px 6px 12px; gap:12px; }
.modal-title{ font-weight:900; font-size:14px; }
.modal-video{ border-radius:16px; overflow:hidden; }

/* ✅ 去掉“白色卡片”那层背景/边框/阴影（如果你用了 glass 类也能压住） */
.video-section{
  background: transparent !important;
  box-shadow: none !important;
  border: none !important;
  padding: 0 !important;
}

/* ✅ 文字放在视频上方并居中 */
.video-head{
  text-align: center;
  margin: 0 auto 14px;
  width: min(1100px, 96%);
}
.video-title{
  font-size: 22px;
  font-weight: 800;
  margin: 0 0 6px;
}
.video-sub{
  margin: 0;
  font-size: 13px;
  opacity: .8;
}
.video-sub2{
  margin: 6px 0 0;
  font-size: 13px;
  opacity: .8;
}

/* ✅ 视频区域居中 + 保留你现在的圆角质感 */
.video-wrap{
  width: min(1100px, 96%);
  margin: 0 auto;
  border-radius: 18px;
  overflow: hidden;
  box-shadow: 0 18px 50px rgba(0,0,0,.12);
}
.video-iframe{
  width: 100%;
  aspect-ratio: 16/9;
  display: block;
  border: 0;
}

</style>

