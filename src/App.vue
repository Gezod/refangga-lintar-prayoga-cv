<script setup>
import { ref, computed, onMounted, onUnmounted, nextTick, h } from 'vue'
import {
  Globe, Terminal, Copy, Printer,
  ChevronDown, ChevronUp, ExternalLink,
  Code2, Layout, Database, Wrench, Layers,
  Eye, Search, PenTool, Hammer, FlaskConical, TrendingUp, Repeat,
  Cpu, Zap, ShieldCheck, Rocket, ArrowRight, Check,
  Maximize2, Activity, Layers2,
  User, FolderGit2, Brain, RotateCw, Send, Menu, X, Sparkles
} from '@lucide/vue'
import TechIcon from './components/TechIcon.vue'
import ImageModal from './components/ImageModal.vue'

// ─── BRAND ICONS ────────────────────────────────────────────────
const Github = (props) => h('svg', {
  xmlns: 'http://www.w3.org/2000/svg',
  width: props.size || 20, height: props.size || 20,
  viewBox: '0 0 24 24', fill: 'none', stroke: 'currentColor',
  'stroke-width': '2', 'stroke-linecap': 'round', 'stroke-linejoin': 'round'
}, [
  h('path', { d: 'M15 22v-4a4.8 4.8 0 0 0-1-3.5c3 0 6-2 6-5.5.08-1.25-.27-2.48-1-3.5.28-1.15.28-2.35 0-3.5 0 0-1 0-3 1.5-2.64-.5-5.36-.5-8 0C6 2 5 2 5 2c-.3 1.15-.3 2.35 0 3.5A5.403 5.403 0 0 0 4 9c0 3.5 3 5.5 6 5.5-.39.49-.68 1.05-.85 1.65-.17.6-.22 1.23-.15 1.85v4' }),
  h('path', { d: 'M9 18c-4.51 2-5-2-7-2' })
])

const Linkedin = (props) => h('svg', {
  xmlns: 'http://www.w3.org/2000/svg',
  width: props.size || 20, height: props.size || 20,
  viewBox: '0 0 24 24', fill: 'none', stroke: 'currentColor',
  'stroke-width': '2', 'stroke-linecap': 'round', 'stroke-linejoin': 'round'
}, [
  h('path', { d: 'M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4v-7a6 6 0 0 1 6-6z' }),
  h('rect', { width: '4', height: '12', x: '2', y: '9' }),
  h('circle', { cx: '4', cy: '4', r: '2' })
])

// ─── STATE ──────────────────────────────────────────────────────
const activeFilter = ref('All')
const activeLoopStep = ref('design')
const expandedPrinciple = ref('01')
const terminalTyped = ref(false)
const copiedLink = ref(null)
const showScrollTop = ref(false)

// ─── POWERFUL NAVIGATION STATE & SCROLLSPY ─────────────────────
const activeSection = ref('hero')
const scrollProgress = ref(0)
const isMobileMenuOpen = ref(false)

const navItems = [
  { id: 'identity', num: '01', label: 'Identity', icon: User, tip: 'Whoami & Summary' },
  { id: 'stack', num: '02', label: 'Tech Stack', icon: Cpu, tip: '17 Core Technologies' },
  { id: 'projects', num: '03', label: 'Selected Work', icon: FolderGit2, tip: 'Case Studies', badge: '3 CASES' },
  { id: 'principles', num: '04', label: 'Principles', icon: Brain, tip: 'How I Think' },
  { id: 'loop', num: '05', label: 'Loop', icon: RotateCw, tip: '7-Stage Cycle' },
  { id: 'contact', num: '06', label: 'Contact', icon: Send, tip: 'Get in Touch' }
]

// Active screenshot selection for each project
const activeScreenshot = ref({
  bsis: 0,
  etemp: 0,
  erp: 0
})

// Lightbox Modal State
const modalState = ref({
  isOpen: false,
  image: { url: '', title: '', caption: '', projectTitle: '' }
})

function openModal(imgObj, projectTitle) {
  modalState.value = {
    isOpen: true,
    image: { ...imgObj, projectTitle }
  }
}

function closeModal() {
  modalState.value.isOpen = false
}

// ─── PROFILE DATA ──────────────────────────────────────────────
const profile = {
  name: 'Refangga Lintar Prayoga',
  tagline: 'ENGINEER THE SYSTEM. ELEVATE THE OUTCOME.',
  roles: ['SOFTWARE ENGINEER', 'SYSTEMS THINKER', 'PROBLEM SOLVER'],
  headline: 'I turn complex workflows into practical digital solutions.',
  subheadline: 'Building web applications, connecting systems, and improving the way people work.',
  education: 'Information Systems Graduate — Telkom University Surabaya',
  location: 'Surabaya, Indonesia',
  links: {
    github: { url: 'https://github.com/Gezod', label: 'github.com/Gezod' },
    portfolio: { url: 'https://my-portofolio-refangga-lintar-prayo.vercel.app/', label: 'Interactive Portfolio' },
    linkedin: { url: 'https://linkedin.com/in/refangga-lintar-prayoga-2a473a318/', label: 'LinkedIn Profile' }
  }
}

const terminalLines = [
  { key: 'name', value: 'Refangga Lintar Prayoga' },
  { key: 'background', value: 'Information Systems Graduate' },
  { key: 'institution', value: 'Telkom University Surabaya' },
  { key: 'role', value: 'Full Stack Web Developer' },
  { key: 'focus', value: 'Applications | Automation | Data | Quality' },
  { key: 'status', value: 'Available for Engineering Roles' }
]

const summary = [
  'Information Systems graduate from Telkom University Surabaya, passionate about building reliable software that solves real operational bottlenecks.',
  'Combines robust technical implementation with a deep understanding of business workflows — focusing not just on shipping features, but on why they exist, how system components interact, and how to maximize reliability.',
  'Core engineering capabilities center on PHP, Laravel, relational databases, responsive frontends, REST API integration, and automated testing, while continually expanding into modern reactive frameworks and IoT telemetry.'
]

const mantra = 'Understand the problem. Design the system. Build the solution. Validate the result.'

// ─── SKILLS DATA WITH ICONS ─────────────────────────────────────
const skillCategories = [
  { id: 'All', label: 'All Technologies', icon: Layers },
  { id: 'Backend', label: 'Backend', icon: Code2 },
  { id: 'Frontend', label: 'Frontend & UI', icon: Layout },
  { id: 'Data', label: 'Data & Storage', icon: Database },
  { id: 'DevTools', label: 'Dev Tools', icon: Wrench }
]

const skills = [
  { name: 'PHP', category: 'Backend', type: 'Language · Server-Side' },
  { name: 'Laravel', category: 'Backend', type: 'MVC Framework · APIs' },
  { name: 'Node.js', category: 'Backend', type: 'Runtime · Microservices' },
  { name: 'Go', category: 'Backend', type: 'Concurrency · Systems' },
  { name: 'HTML5', category: 'Frontend', type: 'Semantic Markup' },
  { name: 'CSS3', category: 'Frontend', type: 'Modern Styling · Grid' },
  { name: 'JavaScript', category: 'Frontend', type: 'ES6+ Logic · Async' },
  { name: 'Bootstrap', category: 'Frontend', type: 'Responsive Components' },
  { name: 'Vue.js', category: 'Frontend', type: 'Reactive SPA · State' },
  { name: 'MySQL', category: 'Data', type: 'Relational Database' },
  { name: 'MariaDB', category: 'Data', type: 'High Performance SQL' },
  { name: 'PostgreSQL', category: 'Data', type: 'Advanced Relational DB' },
  { name: 'MongoDB', category: 'Data', type: 'Document NoSQL Storage' },
  { name: 'Git', category: 'DevTools', type: 'Version Control' },
  { name: 'GitHub', category: 'DevTools', type: 'CI/CD · Collaboration' },
  { name: 'Docker', category: 'DevTools', type: 'Containerization' },
  { name: 'Postman', category: 'DevTools', type: 'API Testing & Specs' }
]

const disciplines = [
  'Full Stack Development',
  'REST APIs',
  'Database Design',
  'System Analysis',
  'Software Testing',
  'Workflow Automation'
]

const filteredSkills = computed(() => {
  if (activeFilter.value === 'All') return skills
  return skills.filter(s => s.category === activeFilter.value)
})

// ─── FEATURED PROJECTS (CASE STUDIES) ───────────────────────────
const projects = [
  {
    id: 'bsis',
    title: 'BSIS — Attendance & Payroll System',
    subtitle: 'Digitizing employee administration through an enterprise information system.',
    badge: 'ENTERPRISE SYSTEM',
    badgeColor: 'border-red-200 bg-red-50 text-red-700',
    accentColor: '#FF2D20',
    live: 'https://vyntra-bsis.cfd/',
    source: 'https://github.com/Gezod/bsis-presensi-penggajian',
    stack: [
      { name: 'Laravel', role: 'Backend Framework' },
      { name: 'PHP', role: 'Server Logic' },
      { name: 'MySQL', role: 'Relational DB' },
      { name: 'Bootstrap', role: 'Responsive UI' }
    ],
    challenge: 'Attendance and payroll administration demands strictly structured records, consistent overtime calculations, multi-tiered leave approvals, and dependable audit trails. The goal was to centralize manual spreadsheet procedures into an automated web application.',
    contributions: [
      'Developed end-to-end web features using Laravel and PHP with modular service architecture.',
      'Implemented automated employee attendance calculation, shift tracking, and leave quota management.',
      'Engineered payroll calculation algorithms with automated tax deductions, allowances, and pay slips.',
      'Designed normalized relational schemas in MySQL/MariaDB ensuring relational integrity.',
      'Constructed responsive management dashboards using Bootstrap, custom CSS, and JavaScript.',
      'Managed complete SDLC lifecycle from requirements analysis, testing, to cloud deployment.'
    ],
    screenshots: [
      {
        url: '/projects/bsis/dashboard.png',
        title: 'Executive Dashboard',
        caption: 'Centralized overview of real-time employee attendance status, monthly metrics, and administrative quick actions.'
      },
      {
        url: '/projects/bsis/manajemen_cuti.png',
        title: 'Leave Management',
        caption: 'Multi-tiered leave request workflow, quota balance tracking, and manager approval ledger.'
      },
      {
        url: '/projects/bsis/penggajian.png',
        title: 'Automated Payroll',
        caption: 'Precision salary calculation module with allowances, deductions, and downloadable pay slip records.'
      }
    ]
  },
  {
    id: 'etemp',
    title: 'E-Temperature — Real-Time IoT Temperature Monitoring',
    subtitle: 'Real-time telemetry streaming and machine health tracking dashboard.',
    badge: 'IOT TELEMETRY PLATFORM',
    badgeColor: 'border-teal-200 bg-teal-50 text-teal-700',
    accentColor: '#0D9488',
    live: 'https://e-temp.my.id/',
    source: 'https://github.com/Gezod/temperature-monitoring',
    stack: [
      { name: 'Vue.js', role: 'Reactive Frontend' },
      { name: 'JavaScript', role: 'Real-Time Telemetry' },
      { name: 'MySQL', role: 'Time-Series Logs' },
      { name: 'REST APIs', role: 'Sensor Ingestion' }
    ],
    challenge: 'Industrial machinery and sensitive facilities require continuous thermal surveillance to eliminate critical overheating, unexpected downtime, and equipment damage. Needed a real-time web telemetry platform that visualizes sensor fluctuations, detects anomalies, and logs historical readings reliably.',
    contributions: [
      'Built interactive real-time telemetry dashboards visualizing temperature curves and threshold warnings.',
      'Engineered structured machine sensor data logging tables with status flags, pagination, and date-range filters.',
      'Implemented secure operator authentication, credential hashing, and role-based portal security.',
      'Engineered clean REST API endpoints for seamless ingestion and processing of machine telemetry packets.',
      'Optimized lightweight responsive charts for smooth client rendering without browser lag.',
      'Configured public cloud deployment with domain routing, SSL security, and high-availability uptime.'
    ],
    screenshots: [
      {
        url: '/projects/e_temp/temperature_tends.png',
        title: 'Temperature Trends & Analytics',
        caption: 'Live multi-sensor temperature telemetry curves, threshold limit indicators, and real-time trend analytics.'
      },
      {
        url: '/projects/e_temp/data_suhu_mesin.png',
        title: 'Sensor Data Logging Table',
        caption: 'Detailed machine telemetry historical database, sensor status classifications, and audit logs.'
      },
      {
        url: '/projects/e_temp/login.png',
        title: 'Operator Gateway & Auth',
        caption: 'Secure portal authentication interface protecting operational controls and telemetry configuration.'
      }
    ]
  },
  {
    id: 'erp',
    title: 'ERP - ELECTROHUB — Enterprise Retail Operations Platform',
    subtitle: 'Unified enterprise resource planning platform integrating General Affair, multi-branch procurement, marketing, and inventory logistics.',
    badge: 'ENTERPRISE ERP PLATFORM',
    badgeColor: 'border-indigo-200 bg-indigo-50 text-indigo-700',
    accentColor: '#4F46E5',
    live: 'https://www.erp-electrohub.my.id/',
    source: 'https://github.com/Gezod/erp-electrohub',
    stack: [
      { name: 'Vue.js', role: 'Vue 3 Reactive SPA' },
      { name: 'Tailwind CSS', role: 'Modular UI System' },
      { name: 'Pinia', role: 'State Management' },
      { name: 'ApexCharts', role: 'Operational Analytics' }
    ],
    challenge: 'ElectroHub operates an omnichannel gadget & tech retail network across multiple metropolitan branches and logistics warehouses. Disjointed spreadsheet tracking across General Affair (GA), branch requisitions, critical stock replenishment, and multi-tier purchase approvals caused operational friction and delayed financial visibility. The objective was to engineer a high-performance web ERP system with real-time analytics to streamline branch-to-HQ workflows.',
    contributions: [
      'Architected and engineered a modular Single Page Application (SPA) using Vue 3, Vite, and Pinia centralized state stores.',
      'Developed specialized operational dashboards for General Affair (GA), Marketing, and Logistics with real-time KPI indicators.',
      'Integrated interactive data visualization with ApexCharts to monitor monthly purchasing trends, branch request quotas, and budget allocations.',
      'Constructed multi-branch procurement workflows, critical inventory tracking (<= 10 units threshold alerts), and automated approval routing.',
      'Designed responsive administrative interfaces using Tailwind CSS with a clean design system and quick-action toolkits.',
      'Configured production deployment on cloud infrastructure with custom domain routing (erp-electrohub.my.id) and SSL security.'
    ],
    screenshots: [
      {
        url: '/projects/erp/dashboard-erp.png',
        title: 'Executive Operations Portal',
        caption: 'Centralized daily operations command center featuring real-time task counters, schedule metrics, and rapid-access shortcuts.'
      },
      {
        url: '/projects/erp/ga-erp.png',
        title: 'General Affair & Budget Analytics',
        caption: 'Interactive GA management dashboard displaying operational expense trends, multi-tier purchase commitments, and critical stock threshold alarms.'
      },
      {
        url: '/projects/erp/marketing-erp.png',
        title: 'Marketing & Branch Request Center',
        caption: 'Regional branch requisition tracker visualizing order accumulation across stores, purchase orders, and asset transfer pipelines.'
      }
    ]
  }
]

// ─── ENGINEERING PRINCIPLES ─────────────────────────────────────
const principles = [
  {
    id: '01',
    title: 'SYSTEMS THINKING',
    subtitle: 'See beyond individual features',
    description: 'I consider the relationships between business requirements, application logic, database structures, and user interactions. A reliable system depends on how its components work together.',
    icon: Cpu
  },
  {
    id: '02',
    title: 'AUTOMATION FIRST',
    subtitle: 'Eliminate unnecessary repetition',
    description: 'I look for opportunities to transform repetitive manual processes into structured digital workflows that are easier to manage, validate, and maintain.',
    icon: Zap
  },
  {
    id: '03',
    title: 'QUALITY ENGINEERING',
    subtitle: 'Verify, investigate, improve',
    description: 'I value systematic testing, debugging, and validation. Finding the root cause of a defect matters more than simply hiding its symptoms.',
    icon: ShieldCheck
  },
  {
    id: '04',
    title: 'CONTINUOUS EVOLUTION',
    subtitle: 'Stay curious, stay accountable',
    description: 'I continue to explore new technologies while strengthening my fundamentals. I value adaptability, integrity, collaboration, and the willingness to learn from mistakes.',
    icon: Rocket
  }
]

// ─── ENGINEERING LOOP ───────────────────────────────────────────
const loopSteps = [
  { id: 'observe', label: 'OBSERVE', icon: Eye, desc: 'Watch how users actually interact with existing systems. Identify friction points, manual bottlenecks, and unexpressed needs through direct observation.' },
  { id: 'analyze', label: 'ANALYZE', icon: Search, desc: 'Deconstruct the problem domain into technical constraints, data dependencies, and root causes before committing to any architecture.' },
  { id: 'design', label: 'DESIGN', icon: PenTool, desc: 'Architect schemas, API contracts, component hierarchies, and failure modes with scalability and maintainability prioritized from day one.' },
  { id: 'build', label: 'BUILD', icon: Hammer, desc: 'Implement clean, well-commented, and modular code following established engineering standards, design patterns, and idiomatic syntax.' },
  { id: 'test', label: 'TEST', icon: FlaskConical, desc: 'Rigorously validate edge cases, data sanitization, integration handshakes, and response times under real-world scenarios.' },
  { id: 'improve', label: 'IMPROVE', icon: TrendingUp, desc: 'Measure system performance, gather operational feedback, simplify unnecessary complexity, and strengthen reliability.' },
  { id: 'repeat', label: 'REPEAT', icon: Repeat, desc: 'Engineering is a disciplined continuous cycle. Every iteration refines the solution and compounds architectural knowledge.' }
]

// ─── LEARNING & DIRECTION ───────────────────────────────────────
const learningAreas = [
  { title: 'Full Stack Development', desc: 'Modern web architectures, responsive frontends, resilient backends, and decoupled REST/GraphQL APIs.' },
  { title: 'Digital Systems & IoT', desc: 'Information systems, network administration, hardware telemetry ingestion, and distributed services.' },
  { title: 'Software Quality Assurance', desc: 'Unit testing, regression verification, automated pipelines, root-cause debugging, and performance profiling.' },
  { title: 'Professional Engineering', desc: 'Cross-functional collaboration, technical documentation, operational integrity, and user-focused delivery.' }
]

const targetRoles = [
  'Full Stack Web Development',
  'Laravel / PHP Engineering',
  'Web Application Development',
  'Information Systems Development',
  'Business Process Automation'
]

// ─── ACTIONS ────────────────────────────────────────────────────
function printCV() {
  window.print()
}

async function copyToClipboard(text, key) {
  try {
    await navigator.clipboard.writeText(text)
    copiedLink.value = key
    setTimeout(() => { copiedLink.value = null }, 2000)
  } catch {
    const el = document.createElement('textarea')
    el.value = text
    document.body.appendChild(el)
    el.select()
    document.execCommand('copy')
    document.body.removeChild(el)
    copiedLink.value = key
    setTimeout(() => { copiedLink.value = null }, 2000)
  }
}

function togglePrinciple(id) {
  expandedPrinciple.value = expandedPrinciple.value === id ? null : id
}

function setLoopStep(id) {
  activeLoopStep.value = activeLoopStep.value === id ? null : id
}

function scrollToTop() {
  window.scrollTo({ top: 0, behavior: 'smooth' })
}

function onScroll() {
  const winScroll = document.documentElement.scrollTop || document.body.scrollTop
  const height = document.documentElement.scrollHeight - document.documentElement.clientHeight
  scrollProgress.value = height > 0 ? Math.min(100, Math.max(0, (winScroll / height) * 100)) : 0
  showScrollTop.value = winScroll > 400

  // ScrollSpy detection for active section
  const sections = ['hero', 'identity', 'stack', 'projects', 'principles', 'loop', 'contact']
  const scrollPos = winScroll + 160

  for (let i = sections.length - 1; i >= 0; i--) {
    const el = document.getElementById(sections[i])
    if (el && el.offsetTop <= scrollPos) {
      activeSection.value = sections[i]
      break
    }
  }
}

function scrollToSection(id) {
  isMobileMenuOpen.value = false
  const el = document.getElementById(id)
  if (el) {
    const yOffset = -76
    const y = el.getBoundingClientRect().top + window.pageYOffset + yOffset
    window.scrollTo({ top: y, behavior: 'smooth' })
  }
}

// ─── LIFECYCLE ──────────────────────────────────────────────────
onMounted(() => {
  setTimeout(() => { terminalTyped.value = true }, 500)

  window.addEventListener('scroll', onScroll, { passive: true })
  onScroll()

  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          entry.target.classList.add('animate-fade-in-up')
          observer.unobserve(entry.target)
        }
      })
    },
    { threshold: 0.1, rootMargin: '0px 0px -40px 0px' }
  )

  nextTick(() => {
    document.querySelectorAll('.observe-section').forEach(el => {
      observer.observe(el)
    })
  })
})

onUnmounted(() => {
  window.removeEventListener('scroll', onScroll)
})
</script>

<template>
  <div class="min-h-screen bg-slate-50 text-slate-900 selection:bg-teal-100 selection:text-teal-900 font-sans">

    <!-- ═══════════════════════════════════════════════════════════
         TOP FLOATING ARCHITECTURAL DOCK & NAVIGATION
         ═══════════════════════════════════════════════════════════ -->
    <nav class="fixed top-0 left-0 right-0 z-40 bg-white/85 backdrop-blur-md border-b border-slate-200/90 shadow-2xs no-print transition-all">
      
      <!-- Real-Time Gradient Scroll Progress Indicator -->
      <div class="h-[2px] bg-slate-100 w-full overflow-hidden">
        <div
          class="h-full bg-gradient-to-r from-teal-500 via-cyan-400 to-emerald-400 transition-all duration-150 ease-out"
          :style="{ width: scrollProgress + '%' }"
        ></div>
      </div>

      <div class="max-w-7xl mx-auto px-4 sm:px-6 py-2.5 flex items-center justify-between gap-3">
        
        <!-- Left: Brand / Systems Identity Badge -->
        <button
          @click="scrollToSection('hero')"
          class="flex items-center gap-2 group text-left cursor-pointer shrink-0"
        >
          <div class="flex items-center font-mono text-xs font-bold bg-slate-900 text-teal-300 px-2.5 py-1 rounded-md shadow-2xs group-hover:bg-teal-700 group-hover:text-white transition-all">
            <span>SYS://</span>
            <span class="cursor-blink text-teal-400">_</span>
          </div>
          <div>
            <div class="flex items-center gap-1.5">
              <span class="text-xs sm:text-sm font-extrabold tracking-tight text-slate-900 group-hover:text-teal-700 transition-colors">
                REFANGGA.CV
              </span>
              <span class="hidden sm:inline-flex items-center gap-1 px-1.5 py-0.2 rounded-full text-[9px] font-mono font-bold bg-emerald-50 text-emerald-700 border border-emerald-200">
                <span class="w-1.5 h-1.5 rounded-full bg-emerald-500 animate-pulse"></span>
                ACTIVE
              </span>
            </div>
            <p class="hidden sm:block text-[10px] font-mono text-slate-600 leading-none">
              SYSTEMS IN MOTION
            </p>
          </div>
        </button>

        <!-- Center: Creative Architectural Segmented Capsule Dock (Desktop) -->
        <div class="hidden xl:flex items-center p-1 rounded-full bg-slate-100/90 border border-slate-200 shadow-inner gap-0.5">
          <button
            v-for="item in navItems"
            :key="item.id"
            @click="scrollToSection(item.id)"
            class="group relative px-3 py-1.5 rounded-full text-xs font-semibold transition-all duration-200 flex items-center gap-2 cursor-pointer active:scale-95"
            :class="activeSection === item.id
              ? 'bg-white text-slate-900 shadow-sm border border-slate-200/90'
              : 'text-slate-600 hover:text-slate-900 hover:bg-white/60'"
            :title="item.tip"
          >
            <!-- Monospace Section Number Badge -->
            <span
              class="font-mono text-[10px] font-bold px-1.5 py-0.2 rounded-md transition-all"
              :class="activeSection === item.id
                ? 'bg-teal-600 text-white shadow-2xs'
                : 'bg-slate-200/80 text-slate-600 group-hover:bg-teal-100 group-hover:text-teal-800'"
            >
              {{ item.num }}
            </span>

            <!-- Section Icon -->
            <component
              :is="item.icon"
              :size="13"
              class="transition-colors"
              :class="activeSection === item.id ? 'text-teal-600' : 'text-slate-600 group-hover:text-slate-700'"
            />

            <!-- Section Label -->
            <span>{{ item.label }}</span>

            <!-- Special Pulse Badge for Selected Work -->
            <span
              v-if="item.badge"
              class="px-1.5 py-0.2 text-[9px] font-mono font-bold rounded-full transition-all"
              :class="activeSection === item.id
                ? 'bg-teal-100 text-teal-800 border border-teal-300'
                : 'bg-slate-200/80 text-slate-600 group-hover:bg-teal-100 group-hover:text-teal-800'"
            >
              {{ item.badge }}
            </span>

            <!-- Active Bottom Micro Glow Line -->
            <span
              v-if="activeSection === item.id"
              class="absolute -bottom-1 left-1/2 -translate-x-1/2 w-4 h-[2px] bg-teal-500 rounded-full shadow-[0_0_8px_#0D9488]"
            ></span>
          </button>
        </div>

        <!-- Medium Desktop (lg to xl): Compact Creative Dock -->
        <div class="hidden lg:flex xl:hidden items-center p-1 rounded-full bg-slate-100/90 border border-slate-200 shadow-inner gap-0.5">
          <button
            v-for="item in navItems"
            :key="'md-' + item.id"
            @click="scrollToSection(item.id)"
            class="px-2.5 py-1 rounded-full text-xs font-semibold transition-all flex items-center gap-1.5 cursor-pointer active:scale-95"
            :class="activeSection === item.id ? 'bg-white text-slate-900 shadow-xs border border-slate-200' : 'text-slate-600 hover:text-slate-900 hover:bg-white/60'"
          >
            <span class="font-mono text-[10px] text-teal-600 font-bold">{{ item.num }}</span>
            <span>{{ item.label }}</span>
            <span v-if="item.badge" class="px-1 py-0.1 text-[9px] font-mono rounded bg-teal-100 text-teal-800">2</span>
          </button>
        </div>

        <!-- Right: Print Action & Mobile Toggle -->
        <div class="flex items-center gap-2 shrink-0">
          
          <!-- Print / Export Button -->
          <button
            @click="printCV"
            class="group flex items-center gap-2 px-3 sm:px-3.5 py-1.5 rounded-lg text-xs font-mono font-bold bg-teal-600 hover:bg-teal-700 text-white shadow-xs hover:shadow transition-all duration-200 active:scale-95 cursor-pointer"
            title="Print CV or Save as A4 PDF (Ctrl+P)"
          >
            <Printer :size="14" class="group-hover:rotate-6 transition-transform" />
            <span class="hidden sm:inline">PRINT / PDF</span>
            <span class="sm:hidden">PRINT</span>
          </button>

          <!-- Mobile Menu Drawer Toggle -->
          <button
            @click="isMobileMenuOpen = !isMobileMenuOpen"
            class="lg:hidden p-2 rounded-lg bg-slate-100 hover:bg-slate-200 text-slate-700 border border-slate-200 transition-colors cursor-pointer"
            :aria-label="isMobileMenuOpen ? 'Close menu' : 'Open menu'"
          >
            <X v-if="isMobileMenuOpen" :size="18" />
            <Menu v-else :size="18" />
          </button>

        </div>

      </div>

      <!-- Mobile Dropdown Creative Drawer -->
      <Transition name="expand">
        <div
          v-if="isMobileMenuOpen"
          class="lg:hidden bg-white/98 border-t border-slate-200 px-4 py-3 shadow-xl backdrop-blur-md"
        >
          <div class="grid grid-cols-2 gap-2">
            <button
              v-for="item in navItems"
              :key="'mobile-' + item.id"
              @click="scrollToSection(item.id)"
              class="flex items-center gap-2.5 p-2.5 rounded-xl border transition-all text-left cursor-pointer active:scale-95"
              :class="activeSection === item.id ? 'bg-teal-50 border-teal-300 text-teal-900 shadow-2xs font-bold' : 'bg-slate-50 border-slate-200 text-slate-700 hover:bg-slate-100 font-medium'"
            >
              <span
                class="w-6 h-6 rounded-md font-mono text-[10px] flex items-center justify-center font-bold"
                :class="activeSection === item.id ? 'bg-teal-600 text-white' : 'bg-white border border-slate-200 text-slate-600'"
              >
                {{ item.num }}
              </span>
              <div class="min-w-0 flex-1">
                <div class="text-xs truncate flex items-center justify-between">
                  <span>{{ item.label }}</span>
                  <span v-if="item.badge" class="text-[9px] font-mono font-bold px-1 rounded bg-teal-200/80 text-teal-900 ml-1">
                    {{ item.badge }}
                  </span>
                </div>
                <div class="text-[10px] text-slate-600 truncate font-mono">
                  {{ item.tip }}
                </div>
              </div>
            </button>
          </div>
        </div>
      </Transition>

    </nav>

    <!-- ═══════════════════════════════════════════════════════════
         MAIN CONTENT AREA
         ═══════════════════════════════════════════════════════════ -->
    <main class="max-w-6xl mx-auto px-4 sm:px-6 pt-20 pb-20">

      <!-- ─── HERO / IDENTITY BANNER ────────────────────────────── -->
      <header id="hero" class="pt-8 sm:pt-14 pb-14 circuit-bg border-b border-slate-200/80 mb-14">
        <div class="observe-section">
          
          <div class="flex flex-col-reverse lg:flex-row lg:items-center lg:justify-between gap-8 lg:gap-12">
            
            <!-- Left Column: Personal Brand & Headlines -->
            <div class="flex-1 min-w-0">
              
              <!-- Tagline -->
              <div class="inline-flex items-center gap-2 px-3 py-1 rounded-full bg-teal-50 border border-teal-200 mb-5">
                <span class="w-1.5 h-1.5 rounded-full bg-teal-600"></span>
                <span class="font-mono text-xs font-bold tracking-widest text-teal-800">
                  {{ profile.tagline }}
                </span>
              </div>

              <!-- Full Name -->
              <h1 class="text-3xl sm:text-5xl lg:text-6xl font-extrabold tracking-tight text-slate-900 leading-tight mb-4">
                {{ profile.name }}
              </h1>

              <!-- Core Roles -->
              <div class="flex flex-wrap items-center gap-2 mb-6">
                <span
                  v-for="(role, i) in profile.roles"
                  :key="role"
                  class="font-mono text-xs font-semibold tracking-wider text-slate-600"
                >
                  {{ role }}
                  <span v-if="i < profile.roles.length - 1" class="mx-2 text-teal-500 font-bold">·</span>
                </span>
              </div>

              <!-- Headline & Subheadline -->
              <p class="text-lg sm:text-xl font-medium text-slate-800 mb-2 leading-relaxed max-w-2xl">
                "{{ profile.headline }}"
              </p>
              <p class="text-sm text-slate-600 mb-5 max-w-2xl leading-relaxed">
                {{ profile.subheadline }}
              </p>

              <!-- Education & Location Badge -->
              <div class="flex flex-wrap items-center gap-3 text-xs font-medium text-slate-600 mb-8">
                <span class="inline-flex items-center gap-1.5 px-3 py-1.5 rounded-md bg-white border border-slate-200 shadow-2xs">
                  🎓 {{ profile.education }}
                </span>
                <span class="inline-flex items-center gap-1.5 px-3 py-1.5 rounded-md bg-white border border-slate-200 shadow-2xs">
                  📍 {{ profile.location }}
                </span>
              </div>

              <!-- Quick Links & Contact CTAs -->
              <div class="flex flex-wrap items-center gap-3">
                <a
                  :href="profile.links.github.url"
                  target="_blank"
                  rel="noopener noreferrer"
                  class="inline-flex items-center gap-2 px-4 py-2.5 rounded-lg text-sm font-semibold bg-slate-900 text-white hover:bg-slate-800 hover:shadow transition-all duration-200"
                >
                  <Github :size="16" />
                  GitHub
                </a>
                <a
                  :href="profile.links.portfolio.url"
                  target="_blank"
                  rel="noopener noreferrer"
                  class="inline-flex items-center gap-2 px-4 py-2.5 rounded-lg text-sm font-semibold bg-white border border-slate-300 text-slate-700 hover:text-teal-700 hover:border-teal-500 hover:bg-teal-50/50 shadow-2xs transition-all duration-200"
                >
                  <Globe :size="16" />
                  Portfolio
                </a>
                <a
                  :href="profile.links.linkedin.url"
                  target="_blank"
                  rel="noopener noreferrer"
                  class="inline-flex items-center gap-2 px-4 py-2.5 rounded-lg text-sm font-semibold bg-white border border-slate-300 text-slate-700 hover:text-blue-700 hover:border-blue-500 hover:bg-blue-50/50 shadow-2xs transition-all duration-200"
                >
                  <Linkedin :size="16" />
                  LinkedIn
                </a>

                <!-- Copy Quick Links Button -->
                <button
                  @click="copyToClipboard(`Refangga Lintar Prayoga\nPortfolio: ${profile.links.portfolio.url}\nGitHub: ${profile.links.github.url}\nLinkedIn: ${profile.links.linkedin.url}`, 'links')"
                  class="inline-flex items-center gap-1.5 px-3.5 py-2.5 rounded-lg text-xs font-mono font-medium transition-all duration-200"
                  :class="copiedLink === 'links' ? 'bg-emerald-100 text-emerald-800 border border-emerald-300' : 'bg-slate-100 hover:bg-slate-200 text-slate-700 border border-slate-200'"
                >
                  <Check v-if="copiedLink === 'links'" :size="14" />
                  <Copy v-else :size="14" />
                  {{ copiedLink === 'links' ? 'COPIED!' : 'COPY LINKS' }}
                </button>
              </div>

            </div>

            <!-- Right Column: Profile Photo with Architectural Ring -->
            <div class="flex justify-center lg:justify-end shrink-0">
              <div class="relative group">
                <!-- Outer subtle glow -->
                <div class="absolute -inset-2 rounded-full bg-gradient-to-tr from-teal-400/20 via-cyan-400/15 to-emerald-400/20 blur-md opacity-80 group-hover:opacity-100 transition-opacity duration-300"></div>

                <!-- Double Ring Container -->
                <div class="relative w-44 h-44 sm:w-52 sm:h-52 lg:w-60 lg:h-60 rounded-full p-1.5 bg-white border-2 border-teal-500 shadow-xl transition-transform duration-300 group-hover:scale-102">
                  <img
                    src="/foto_profile.png"
                    :alt="profile.name"
                    class="w-full h-full rounded-full object-cover bg-slate-100"
                  />
                  <!-- Corner System Tag -->
                  <div class="absolute bottom-1 right-1 bg-white border border-slate-200 rounded-full p-1.5 shadow-md text-teal-600">
                    <Activity :size="16" />
                  </div>
                </div>
              </div>
            </div>

          </div>

        </div>
      </header>

      <!-- ─── SECTION 01 / ENGINEERING IDENTITY ─────────────────── -->
      <section id="identity" class="mb-16 sm:mb-20 observe-section">
        <div class="flex items-center gap-3 mb-6">
          <span class="section-index">01</span>
          <h2 class="text-sm font-mono font-bold tracking-widest text-slate-700 uppercase">
            ENGINEERING IDENTITY & SUMMARY
          </h2>
          <div class="h-px flex-1 bg-slate-200"></div>
        </div>

        <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
          
          <!-- Terminal Widget ($ whoami) -->
          <div class="terminal-window print-avoid-break">
            <div class="terminal-bar">
              <span class="terminal-dot bg-red-400"></span>
              <span class="terminal-dot bg-amber-400"></span>
              <span class="terminal-dot bg-emerald-400"></span>
              <span class="ml-2 font-mono text-xs text-slate-400">refangga@sys-arch: ~ $ whoami</span>
            </div>
            <div class="terminal-body space-y-2">
              <div class="flex items-center gap-2 text-slate-100 font-bold mb-3">
                <span class="text-teal-400">$</span>
                <span>whoami --verbose</span>
                <span class="cursor-blink text-teal-400">▋</span>
              </div>
              <div
                v-for="line in terminalLines"
                :key="line.key"
                class="flex flex-col sm:flex-row sm:items-center gap-1 sm:gap-3 text-xs"
              >
                <span class="text-teal-400 font-mono min-w-[110px]">{{ line.key }}:</span>
                <span class="text-slate-200 font-medium">{{ line.value }}</span>
              </div>
              <div class="pt-3 border-t border-slate-800 text-[11px] text-slate-400 font-mono">
                // System status: READY · Workflows optimized · Clean code delivered
              </div>
            </div>
          </div>

          <!-- Executive Summary Card -->
          <div class="glow-card p-6 sm:p-7 flex flex-col justify-between print-avoid-break">
            <div>
              <h3 class="text-base font-bold text-slate-900 mb-3 flex items-center gap-2">
                <span>Executive Summary</span>
                <span class="text-xs font-mono font-normal text-teal-600 bg-teal-50 px-2 py-0.5 rounded border border-teal-200">
                  Profile Brief
                </span>
              </h3>
              <div class="space-y-3 text-sm text-slate-600 leading-relaxed">
                <p v-for="(para, i) in summary" :key="i">
                  {{ para }}
                </p>
              </div>
            </div>

            <!-- Core Mantra -->
            <div class="mt-6 pt-4 border-t border-slate-100 bg-slate-50/60 -mx-6 -mb-6 p-4 rounded-b-xl">
              <span class="text-[10px] font-mono tracking-widest text-teal-700 font-bold uppercase block mb-1">
                CORE ENGINEERING MANTRA
              </span>
              <p class="font-mono text-xs text-slate-800 italic font-medium">
                "{{ mantra }}"
              </p>
            </div>
          </div>

        </div>
      </section>

      <!-- ─── SECTION 02 / THE ENGINEERING STACK ────────────────── -->
      <section id="stack" class="mb-16 sm:mb-20 observe-section">
        <div class="flex items-center gap-3 mb-6">
          <span class="section-index">02</span>
          <h2 class="text-sm font-mono font-bold tracking-widest text-slate-700 uppercase">
            THE ENGINEERING STACK & DISCIPLINES
          </h2>
          <div class="h-px flex-1 bg-slate-200"></div>
        </div>

        <!-- Category Filters -->
        <div class="flex flex-wrap items-center gap-2 mb-6 no-print">
          <button
            v-for="cat in skillCategories"
            :key="cat.id"
            @click="activeFilter = cat.id"
            class="tab-filter inline-flex items-center gap-2 px-3.5 py-1.5 rounded-lg text-xs font-mono font-semibold transition-all duration-200"
            :class="activeFilter === cat.id ? 'active' : 'bg-white text-slate-700 border border-slate-200 hover:border-teal-500 hover:text-teal-700 shadow-2xs'"
          >
            <component :is="cat.icon" :size="14" />
            <span>{{ cat.label }}</span>
          </button>
        </div>

        <!-- Skills Grid with Brand Icons -->
        <div class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 gap-3 mb-8">
          <div
            v-for="skill in filteredSkills"
            :key="skill.name"
            class="skill-badge group p-3 flex items-center justify-between"
          >
            <div class="flex items-center gap-2.5 min-w-0">
              <TechIcon :name="skill.name" :size="20" />
              <div class="min-w-0">
                <span class="text-xs font-bold text-slate-900 group-hover:text-teal-700 block truncate">
                  {{ skill.name }}
                </span>
                <span class="text-[10px] text-slate-600 block truncate">
                  {{ skill.category }}
                </span>
              </div>
            </div>
          </div>
        </div>

        <!-- Disciplines Section -->
        <div class="bg-white border border-slate-200 rounded-xl p-5 mb-3 shadow-2xs">
          <h3 class="text-xs font-mono font-bold tracking-wider text-slate-500 uppercase mb-3">
            ENGINEERING DISCIPLINES & EXPERTISE
          </h3>
          <div class="flex flex-wrap gap-2">
            <span
              v-for="d in disciplines"
              :key="d"
              class="discipline-pill"
            >
              {{ d }}
            </span>
          </div>
        </div>

        <p class="font-mono text-xs text-slate-600 italic">
          * The technologies above represent tools I have engineered solutions with, across different projects and systems.
        </p>
      </section>

      <!-- ─── SECTION 03 / SELECTED ENGINEERING WORK ────────────── -->
      <section id="projects" class="mb-16 sm:mb-20 observe-section">
        <div class="flex items-center gap-3 mb-6">
          <span class="section-index">03</span>
          <h2 class="text-sm font-mono font-bold tracking-widest text-slate-700 uppercase">
            SELECTED ENGINEERING WORK · FEATURED CASE STUDIES
          </h2>
          <div class="h-px flex-1 bg-slate-200"></div>
        </div>

        <div class="space-y-12">
          
          <!-- Individual Project Card -->
          <div
            v-for="proj in projects"
            :key="proj.id"
            class="glow-card overflow-hidden border border-slate-200 shadow-sm print-avoid-break"
          >
            <!-- Project Header Bar -->
            <div class="p-6 sm:p-7 border-b border-slate-100 bg-gradient-to-r from-slate-50/70 via-white to-slate-50/50">
              <div class="flex flex-col md:flex-row md:items-start justify-between gap-4">
                
                <div>
                  <div class="flex flex-wrap items-center gap-2 mb-2">
                    <span class="px-2.5 py-0.5 rounded text-[10px] font-mono font-bold tracking-wide border uppercase" :class="proj.badgeColor">
                      {{ proj.badge }}
                    </span>
                    <span class="inline-flex items-center gap-1 text-[11px] font-mono text-emerald-700 bg-emerald-50 px-2 py-0.5 rounded border border-emerald-200 font-semibold">
                      <span class="w-1.5 h-1.5 rounded-full bg-emerald-500 animate-pulse"></span>
                      PRODUCTION LIVE
                    </span>
                  </div>

                  <h3 class="text-xl sm:text-2xl font-extrabold text-slate-900 tracking-tight">
                    {{ proj.title }}
                  </h3>
                  <p class="text-sm text-slate-600 mt-1 max-w-3xl leading-relaxed">
                    {{ proj.subtitle }}
                  </p>
                </div>

                <!-- External Action Links -->
                <div class="flex flex-wrap items-center gap-2 shrink-0">
                  <a
                    :href="proj.source"
                    target="_blank"
                    rel="noopener noreferrer"
                    class="inline-flex items-center gap-1.5 px-3 py-1.5 rounded-lg text-xs font-mono font-semibold bg-white border border-slate-300 text-slate-700 hover:text-slate-900 hover:border-slate-400 shadow-2xs transition-all active:scale-95"
                  >
                    <Github :size="14" />
                    <span>SOURCE CODE</span>
                  </a>
                  <a
                    :href="proj.live"
                    target="_blank"
                    rel="noopener noreferrer"
                    class="inline-flex items-center gap-1.5 px-3.5 py-1.5 rounded-lg text-xs font-mono font-bold bg-teal-600 text-white hover:bg-teal-700 shadow-xs transition-all active:scale-95"
                  >
                    <Globe :size="14" />
                    <span>LIVE APP</span>
                    <ExternalLink :size="12" />
                  </a>
                </div>

              </div>

              <!-- Tech Stack Badges with Icons -->
              <div class="flex flex-wrap items-center gap-2 mt-4 pt-3 border-t border-slate-100">
                <span class="text-xs font-mono text-slate-600 font-semibold mr-1">STACK:</span>
                <span
                  v-for="stk in proj.stack"
                  :key="stk.name"
                  class="inline-flex items-center gap-1.5 px-2.5 py-1 rounded-md bg-white border border-slate-200 text-xs font-medium text-slate-800 shadow-2xs"
                >
                  <TechIcon :name="stk.name" :size="14" />
                  <span class="font-bold">{{ stk.name }}</span>
                  <span class="text-[10px] text-slate-600">({{ stk.role }})</span>
                </span>
              </div>
            </div>

            <!-- Product Screenshot Showcase (Browser Mockup) -->
            <div class="p-6 sm:p-7 bg-slate-900/3 border-b border-slate-100">
              <div class="bg-white rounded-xl border border-slate-200 shadow-md overflow-hidden">
                
                <!-- Browser Bar -->
                <div class="flex items-center justify-between px-4 py-2.5 bg-slate-100 border-b border-slate-200">
                  <div class="flex items-center gap-2">
                    <span class="w-3 h-3 rounded-full bg-red-400"></span>
                    <span class="w-3 h-3 rounded-full bg-amber-400"></span>
                    <span class="w-3 h-3 rounded-full bg-emerald-400"></span>
                    <span class="ml-2 font-mono text-[11px] text-slate-500 bg-white px-3 py-0.5 rounded border border-slate-200 font-medium truncate max-w-[200px] sm:max-w-md">
                      {{ proj.live }}
                    </span>
                  </div>

                  <button
                    @click="openModal(proj.screenshots[activeScreenshot[proj.id]], proj.title)"
                    class="inline-flex items-center gap-1 px-2.5 py-1 rounded text-[11px] font-mono text-slate-600 hover:text-teal-700 hover:bg-slate-200 transition-colors"
                    title="Inspect Full Image"
                  >
                    <Maximize2 :size="12" />
                    <span>ZOOM / INSPECT</span>
                  </button>
                </div>

                <!-- Active Screenshot Display -->
                <div
                  class="relative aspect-16/9 sm:aspect-21/9 max-h-[380px] bg-slate-950 flex items-center justify-center overflow-hidden cursor-pointer group"
                  @click="openModal(proj.screenshots[activeScreenshot[proj.id]], proj.title)"
                >
                  <img
                    :src="proj.screenshots[activeScreenshot[proj.id]].url"
                    :alt="proj.screenshots[activeScreenshot[proj.id]].title"
                    class="w-full h-full object-contain transition-transform duration-300 group-hover:scale-101"
                  />
                  <!-- Hover Overlay Indicator -->
                  <div class="absolute inset-0 bg-slate-900/40 opacity-0 group-hover:opacity-100 flex items-center justify-center transition-opacity text-white font-mono text-xs font-semibold gap-2">
                    <Maximize2 :size="16" /> Click to enlarge screenshot
                  </div>
                </div>

                <!-- Screenshot Tabs Selector -->
                <div class="grid grid-cols-3 divide-x divide-slate-200 border-t border-slate-200 bg-slate-50">
                  <button
                    v-for="(screen, idx) in proj.screenshots"
                    :key="screen.title"
                    @click="activeScreenshot[proj.id] = idx"
                    class="p-3 text-left transition-all relative"
                    :class="activeScreenshot[proj.id] === idx ? 'bg-white shadow-2xs' : 'hover:bg-slate-100/80'"
                  >
                    <div
                      v-if="activeScreenshot[proj.id] === idx"
                      class="absolute top-0 left-0 right-0 h-0.5 bg-teal-600"
                    ></div>
                    <span
                      class="block font-mono text-[10px] font-bold uppercase tracking-wider"
                      :class="activeScreenshot[proj.id] === idx ? 'text-teal-700' : 'text-slate-600'"
                    >
                      VIEW 0{{ idx + 1 }}
                    </span>
                    <span
                      class="block text-xs font-bold truncate mt-0.5"
                      :class="activeScreenshot[proj.id] === idx ? 'text-slate-900' : 'text-slate-600'"
                    >
                      {{ screen.title }}
                    </span>
                  </button>
                </div>

                <!-- Caption text for current screenshot -->
                <div class="px-4 py-2 bg-slate-50/60 border-t border-slate-100 text-[11px] text-slate-500 font-mono flex items-center gap-2">
                  <span class="text-teal-600 font-bold">INFO:</span>
                  <span>{{ proj.screenshots[activeScreenshot[proj.id]].caption }}</span>
                </div>

              </div>
            </div>

            <!-- The Challenge & What I Engineered Breakdown -->
            <div class="grid grid-cols-1 lg:grid-cols-12 divide-y lg:divide-y-0 lg:divide-x divide-slate-100">
              
              <!-- Challenge Column -->
              <div class="lg:col-span-5 p-6 sm:p-7 bg-slate-50/40">
                <div class="inline-flex items-center gap-1.5 text-xs font-mono font-bold text-slate-500 uppercase tracking-wider mb-3">
                  <span class="w-1.5 h-1.5 rounded-full bg-amber-500"></span>
                  THE ARCHITECTURAL CHALLENGE
                </div>
                <p class="text-sm text-slate-700 leading-relaxed">
                  {{ proj.challenge }}
                </p>
              </div>

              <!-- What I Engineered Column -->
              <div class="lg:col-span-7 p-6 sm:p-7">
                <div class="inline-flex items-center gap-1.5 text-xs font-mono font-bold text-teal-700 uppercase tracking-wider mb-3">
                  <span class="w-1.5 h-1.5 rounded-full bg-teal-600"></span>
                  WHAT I ENGINEERED (KEY CONTRIBUTIONS)
                </div>
                <ul class="space-y-2.5">
                  <li
                    v-for="(item, i) in proj.contributions"
                    :key="i"
                    class="flex items-start gap-2.5 text-xs sm:text-sm text-slate-700 leading-relaxed"
                  >
                    <ArrowRight :size="14" class="text-teal-600 shrink-0 mt-1" />
                    <span>{{ item }}</span>
                  </li>
                </ul>
              </div>

            </div>

          </div>

        </div>
      </section>

      <!-- ─── SECTION 04 / HOW I THINK (PRINCIPLES) ─────────────── -->
      <section id="principles" class="mb-16 sm:mb-20 observe-section">
        <div class="flex items-center gap-3 mb-6">
          <span class="section-index">04</span>
          <h2 class="text-sm font-mono font-bold tracking-widest text-slate-700 uppercase">
            HOW I THINK · ENGINEERING PRINCIPLES
          </h2>
          <div class="h-px flex-1 bg-slate-200"></div>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
          <div
            v-for="p in principles"
            :key="p.id"
            @click="togglePrinciple(p.id)"
            class="glow-card p-6 cursor-pointer border transition-all duration-200 print-avoid-break"
            :class="expandedPrinciple === p.id ? 'border-teal-500 bg-teal-50/20 shadow-sm' : 'border-slate-200 hover:border-slate-300'"
          >
            <div class="flex items-start justify-between">
              <div class="flex items-center gap-3">
                <span class="w-10 h-10 rounded-xl bg-teal-100/70 border border-teal-200 flex items-center justify-center text-teal-700 shrink-0">
                  <component :is="p.icon" :size="20" />
                </span>
                <div>
                  <span class="font-mono text-xs font-bold text-teal-600">{{ p.id }}</span>
                  <h3 class="text-sm font-bold text-slate-900 tracking-tight">
                    {{ p.title }}
                  </h3>
                </div>
              </div>

              <button class="text-slate-400 hover:text-slate-600 no-print">
                <ChevronUp v-if="expandedPrinciple === p.id" :size="16" />
                <ChevronDown v-else :size="16" />
              </button>
            </div>

            <p class="font-mono text-xs text-slate-500 mt-3 mb-2 font-medium">
              {{ p.subtitle }}
            </p>

            <Transition name="expand">
              <p
                v-if="expandedPrinciple === p.id"
                class="text-xs sm:text-sm text-slate-600 leading-relaxed border-t border-slate-100 pt-3 mt-2"
              >
                "{{ p.description }}"
              </p>
            </Transition>
          </div>
        </div>
      </section>

      <!-- ─── SECTION 05 / ENGINEERING CYCLE & PUBLIC LEARNING ──── -->
      <section id="loop" class="mb-16 sm:mb-20 observe-section">
        <div class="flex items-center gap-3 mb-6">
          <span class="section-index">05</span>
          <h2 class="text-sm font-mono font-bold tracking-widest text-slate-700 uppercase">
            LEARNING IN PUBLIC & ENGINEERING CYCLE
          </h2>
          <div class="h-px flex-1 bg-slate-200"></div>
        </div>

        <!-- Continuous Engineering Loop Visualizer -->
        <div class="glow-card p-6 sm:p-8 mb-8 border border-slate-200 print-avoid-break">
          <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-2 mb-6">
            <div>
              <span class="text-xs font-mono font-bold text-teal-600 uppercase tracking-widest block">
                SYSTEMS IN MOTION
              </span>
              <h3 class="text-base font-bold text-slate-900">
                The Continuous Engineering Workflow Pipeline
              </h3>
            </div>
            <span class="text-xs font-mono text-slate-600">
              Click any stage to inspect Refangga's approach
            </span>
          </div>

          <!-- Pipeline Nodes -->
          <div class="flex flex-wrap items-center justify-between gap-2 mb-6 pb-2 border-b border-slate-100">
            <template v-for="(step, i) in loopSteps" :key="step.id">
              <button
                @click="setLoopStep(step.id)"
                class="loop-node flex flex-col items-center gap-1.5 px-3 py-2.5 rounded-xl border transition-all"
                :class="activeLoopStep === step.id ? 'bg-teal-600 text-white border-teal-600 shadow-sm' : 'bg-slate-50 text-slate-700 border-slate-200 hover:border-teal-400 hover:bg-teal-50/50'"
              >
                <component :is="step.icon" :size="16" />
                <span class="font-mono text-[11px] font-bold tracking-wider">{{ step.label }}</span>
              </button>

              <ArrowRight
                v-if="i < loopSteps.length - 1"
                :size="14"
                class="text-slate-300 hidden md:block"
              />
            </template>
          </div>

          <!-- Active Step Inspection Card -->
          <div v-if="activeLoopStep" class="p-4 rounded-xl bg-teal-50/60 border border-teal-200">
            <div class="flex items-center gap-2 mb-1">
              <span class="font-mono text-xs font-bold text-teal-800 uppercase">
                PHASE STAGE: {{ loopSteps.find(s => s.id === activeLoopStep)?.label }}
              </span>
            </div>
            <p class="text-xs sm:text-sm text-slate-700 leading-relaxed">
              {{ loopSteps.find(s => s.id === activeLoopStep)?.desc }}
            </p>
          </div>
        </div>

        <!-- Professional Development Areas -->
        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
          <div
            v-for="area in learningAreas"
            :key="area.title"
            class="glow-card p-5 border border-slate-200 print-avoid-break"
          >
            <h4 class="text-sm font-bold text-slate-900 mb-1.5 flex items-center gap-2">
              <span class="w-1.5 h-1.5 rounded-full bg-teal-500"></span>
              {{ area.title }}
            </h4>
            <p class="text-xs text-slate-600 leading-relaxed">
              {{ area.desc }}
            </p>
          </div>
        </div>
      </section>

      <!-- ─── SECTION 06 / CAREER DIRECTION & CALL TO ACTION ────── -->
      <section id="contact" class="mb-10 observe-section">
        <div class="flex items-center gap-3 mb-6">
          <span class="section-index">06</span>
          <h2 class="text-sm font-mono font-bold tracking-widest text-slate-700 uppercase">
            CAREER DIRECTION & OBJECTIVES
          </h2>
          <div class="h-px flex-1 bg-slate-200"></div>
        </div>

        <!-- Direction & Target Roles Card -->
        <div class="glow-card p-6 sm:p-8 mb-8 border border-slate-200 print-avoid-break">
          <p class="text-sm text-slate-700 leading-relaxed mb-6 max-w-4xl">
            Interested in opportunities where I can contribute to useful software, solve real engineering bottlenecks,
            and grow alongside a team that values thoughtful implementation. My goal is to build solutions that are not only
            functional, but maintainable, understandable, and deeply useful to the people and operations that depend on them.
          </p>

          <h3 class="text-xs font-mono font-bold tracking-wider text-slate-500 uppercase mb-3">
            TARGET ROLES & SPECIALIZATIONS PURSUED
          </h3>
          <div class="flex flex-wrap gap-2">
            <span
              v-for="role in targetRoles"
              :key="role"
              class="discipline-pill"
            >
              {{ role }}
            </span>
          </div>
        </div>

        <!-- Closing CTA Box -->
        <div class="glow-card p-8 sm:p-12 text-center border-2 border-teal-500/40 bg-gradient-to-b from-teal-50/40 via-white to-slate-50 circuit-bg-dense print-avoid-break">
          
          <div class="inline-flex items-center gap-1.5 px-3 py-1 rounded-full bg-teal-100 text-teal-800 text-xs font-mono font-semibold mb-4">
            <span>READY TO COLLABORATE</span>
          </div>

          <h2 class="text-2xl sm:text-4xl font-extrabold text-slate-900 tracking-tight mb-3">
            LET'S ENGINEER SOMETHING MEANINGFUL.
          </h2>
          <p class="text-sm text-slate-600 max-w-xl mx-auto mb-8 leading-relaxed">
            Good engineering connects technology to real operational outcomes. Available for Full Stack, Laravel, and Web Application Development roles.
          </p>

          <!-- Contact Action Links -->
          <div class="flex flex-wrap items-center justify-center gap-3 mb-8">
            <a
              :href="profile.links.github.url"
              target="_blank"
              rel="noopener noreferrer"
              class="inline-flex items-center gap-2 px-5 py-3 rounded-xl text-sm font-semibold bg-slate-900 text-white hover:bg-slate-800 hover:shadow-md transition-all active:scale-95"
            >
              <Github :size="16" />
              GitHub (@Gezod)
            </a>
            <a
              :href="profile.links.portfolio.url"
              target="_blank"
              rel="noopener noreferrer"
              class="inline-flex items-center gap-2 px-5 py-3 rounded-xl text-sm font-semibold bg-teal-600 text-white hover:bg-teal-700 hover:shadow-md transition-all active:scale-95"
            >
              <Globe :size="16" />
              Portfolio Website
            </a>
            <a
              :href="profile.links.linkedin.url"
              target="_blank"
              rel="noopener noreferrer"
              class="inline-flex items-center gap-2 px-5 py-3 rounded-xl text-sm font-semibold bg-white border border-slate-300 text-slate-800 hover:text-blue-700 hover:border-blue-400 shadow-sm transition-all active:scale-95"
            >
              <Linkedin :size="16" />
              LinkedIn Profile
            </a>
          </div>

          <!-- Footer Motto Triad -->
          <div class="flex flex-wrap items-center justify-center gap-4 sm:gap-8 pt-6 border-t border-slate-200">
            <span
              v-for="motto in ['THINK DEEPER.', 'BUILD BETTER.', 'MAKE IT MATTER.']"
              :key="motto"
              class="font-mono text-xs font-extrabold tracking-[0.25em] text-teal-800"
            >
              {{ motto }}
            </span>
          </div>

        </div>

      </section>

    </main>

    <!-- ═══════════════════════════════════════════════════════════
         PAGE FOOTER
         ═══════════════════════════════════════════════════════════ -->
    <footer class="border-t border-slate-200 bg-white py-6 text-center text-xs text-slate-600 font-mono no-print">
      <div class="max-w-6xl mx-auto px-4 sm:px-6 flex flex-col sm:flex-row items-center justify-between gap-3">
        <p>
          SYS://REFANGGA.CV — Refangga Lintar Prayoga · Information Systems Graduate
        </p>
        <p>
          Designed with "Systems in Motion" · Vue.js 3 + Tailwind CSS
        </p>
      </div>
    </footer>

    <!-- ═══════════════════════════════════════════════════════════
         SCROLL TO TOP BUTTON
         ═══════════════════════════════════════════════════════════ -->
    <Transition name="fade">
      <button
        v-if="showScrollTop"
        @click="scrollToTop"
        class="fixed bottom-6 right-6 w-10 h-10 rounded-xl bg-white border border-slate-200 text-teal-700 shadow-md flex items-center justify-center hover:bg-slate-50 hover:shadow-lg transition-all z-30 no-print"
        title="Scroll to Top"
      >
        <ChevronUp :size="18" />
      </button>
    </Transition>

    <!-- ═══════════════════════════════════════════════════════════
         LIGHTBOX IMAGE MODAL
         ═══════════════════════════════════════════════════════════ -->
    <ImageModal
      :is-open="modalState.isOpen"
      :image="modalState.image"
      @close="closeModal"
    />

  </div>
</template>

<style scoped>
.expand-enter-active,
.expand-leave-active {
  transition: all 0.25s cubic-bezier(0.16, 1, 0.3, 1);
  overflow: hidden;
}
.expand-enter-from,
.expand-leave-to {
  opacity: 0;
  max-height: 0;
  transform: translateY(-4px);
}
.expand-enter-to,
.expand-leave-from {
  opacity: 1;
  max-height: 250px;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.2s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
