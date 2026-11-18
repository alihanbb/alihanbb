<h1 align="center">Alihan Berat Çelik</h1>
<h3 align="center">Backend & Cloud-Native Developer · Software Architecture · Distributed Systems</h3>

<p align="center">
  <a href="https://www.kozmozinovasyon.com"><img src="https://img.shields.io/badge/Kozmoz%20İnovasyon-kozmozinovasyon.com-F97316?style=for-the-badge&logo=googlechrome&logoColor=white" alt="Website"/></a>
  <a href="https://www.linkedin.com/in/alihan-%C3%A7elik/"><img src="https://img.shields.io/badge/LinkedIn-Alihan%20Çelik-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white" alt="LinkedIn"/></a>
  <img src="https://img.shields.io/badge/📍-Kırıkkale,%20Türkiye-555?style=for-the-badge" alt="Location"/>
</p>

<p align="center">
  <a href="#-english">🇬🇧 English</a> · <a href="#-türkçe">🇹🇷 Türkçe</a>
</p>

---

## 🇬🇧 English

I design and build **backend systems end to end** — from the domain model and service boundaries to messaging, data, deployment and observability. I think in terms of **systems**: how the parts talk, how they fail, how they recover and how you see what is happening inside them.

- 🧱 **Architecture:** Clean Architecture, DDD, CQRS, Vertical Slice, Modular Monolith and Microservices — chosen per problem, not by habit.
- 🔁 **Distributed systems:** event-driven design, Saga orchestration, Outbox/Inbox, idempotent consumers, Kafka, RabbitMQ, Redis Streams, Azure Service Bus.
- ☁️ **Platform:** Docker, Kubernetes, GitOps with Argo CD, Azure Functions, CI/CD with GitHub Actions.
- 🔭 **Observability:** OpenTelemetry, Jaeger, Prometheus, Grafana, ELK — every request traceable across services.
- 🤖 **AI in production:** agentic workflows with human-in-the-loop gates, local LLMs (Ollama), Semantic Kernel, LangGraph.
- 🚀 I build real products at **[Kozmoz İnovasyon](https://www.kozmozinovasyon.com)** — from system design to the app in the store.

## 🇹🇷 Türkçe

**Backend sistemlerini uçtan uca** tasarlayıp geliştiriyorum — domain modeli ve servis sınırlarından mesajlaşmaya, veriye, dağıtıma ve gözlemlenebilirliğe kadar. Yazılıma **sistem mühendisliği** gözüyle bakıyorum: parçalar nasıl konuşur, nasıl hata verir, nasıl toparlanır ve içeride ne olduğunu nasıl görürsün.

- 🧱 **Mimari:** Clean Architecture, DDD, CQRS, Vertical Slice, Modular Monolith ve Mikroservisler — alışkanlıkla değil, probleme göre seçilir.
- 🔁 **Dağıtık sistemler:** event-driven tasarım, Saga orkestrasyonu, Outbox/Inbox, idempotent tüketiciler, Kafka, RabbitMQ, Redis Streams, Azure Service Bus.
- ☁️ **Platform:** Docker, Kubernetes, Argo CD ile GitOps, Azure Functions, GitHub Actions ile CI/CD.
- 🔭 **Gözlemlenebilirlik:** OpenTelemetry, Jaeger, Prometheus, Grafana, ELK — her istek servisler arasında izlenebilir.
- 🤖 **Üretimde yapay zekâ:** insan onay kapılı ajan iş akışları, yerel LLM'ler (Ollama), Semantic Kernel, LangGraph.
- 🚀 **[Kozmoz İnovasyon](https://www.kozmozinovasyon.com)** çatısı altında gerçek ürünler geliştiriyorum — sistem tasarımından mağazadaki uygulamaya kadar.

---

## 🏗️ How I Design Systems · Sistemleri Nasıl Tasarlıyorum

**EN:** A typical platform I build: an edge layer and a BFF in front, services that own their data, events for facts and queues for commands, and one trace for every request.
**TR:** Kurduğum tipik bir platform: önde edge katmanı ve BFF, kendi verisine sahip servisler, olaylar için event, komutlar için kuyruk ve her istek için tek bir trace.

```mermaid
flowchart LR
    U["Web / Mobile<br/>Clients"] --> E["Edge<br/>Cloudflare WAF · Tunnel"]
    E --> B["Web BFF<br/>YARP · OIDC session"]

    subgraph S["Bounded Contexts · Servisler"]
        O["Ordering<br/>Saga · Outbox"]
        I["Inventory"]
        P["Payment"]
        C["Catalog<br/>Search"]
        N["Notification"]
    end

    B --> O
    B --> C
    O -- "facts · events" --> K[("Kafka")]
    K --> I
    K --> N
    O -- "commands" --> R[("RabbitMQ")]
    R --> P

    O --- DB1[("PostgreSQL")]
    C --- DB2[("MongoDB ·<br/>Elasticsearch")]
    O --- RD[("Redis")]

    S -. "traces · metrics · logs" .-> OT["OpenTelemetry<br/>Grafana · Jaeger · Prometheus"]
```

**EN:** Reliable messaging without distributed transactions — the Outbox pattern I use across my services:
**TR:** Dağıtık transaction olmadan güvenilir mesajlaşma — servislerimde kullandığım Outbox kalıbı:

```mermaid
sequenceDiagram
    participant API as API / Command Handler
    participant DB as Service DB
    participant W as Outbox Worker
    participant BR as Broker (Kafka / RabbitMQ)
    participant C as Consumer (Inbox)

    API->>DB: Save aggregate + outbox message (one transaction)
    W->>DB: Poll unpublished messages
    W->>BR: Publish event
    W->>DB: Mark as published
    BR->>C: Deliver event
    C->>C: Inbox check → process once (idempotent)
```

**EN:** Agentic GitOps — AI agents write to Git, never to the cluster; a human approves and Argo CD applies.
**TR:** Agentic GitOps — yapay zekâ ajanları cluster'a değil Git'e yazar; insan onaylar, Argo CD uygular.

```mermaid
flowchart LR
    A["AI Agent"] -->|"opens PR"| G["Git PR<br/>manifest change"]
    G --> M{"Merge Gate<br/>CI · Security · Human approval"}
    M -->|"merge"| R["main<br/>desired state"]
    R --> AR["Argo CD"]
    AR -->|"RollingUpdate"| K["Kubernetes"]
    K -->|"readiness fails"| RB["Auto rollback<br/>last stable version"]
```

---

## 🚀 Products I've Built · Geliştirdiğim Ürünler

| Product · Ürün | EN | TR | Architecture · Stack |
|---|---|---|---|
| [**Sıram**](https://www.kozmozinovasyon.com/products/siram) | Appointment, reminder and calendar app for barbers, salons and small businesses. | Berber, kuaför ve küçük işletmeler için randevu, hatırlatma ve takvim uygulaması. | Expo · React Native · TypeScript |
| [**Planyol**](https://www.kozmozinovasyon.com/products/planyol) | Multi-tenant project & field management for construction, architecture and engineering firms — tasks, site, finance, team. Offline-first mobile with sync. | İnşaat, mimarlık ve mühendislik firmaları için çok kiracılı proje & saha yönetimi — görev, saha, finans, ekip. Senkronizasyonlu çevrimdışı mobil. | NestJS · Prisma · PostgreSQL · Socket.IO · FCM · Expo · Turborepo monorepo |
| **AI-Do** | AI-assisted productivity app: tasks, Pomodoro, analytics and smart suggestions. Backend designed for 10k+ RPS. | Yapay zekâ destekli verimlilik uygulaması: görevler, Pomodoro, analitik ve akıllı öneriler. Backend 10k+ RPS için tasarlandı. | .NET 10 Modular Monolith · DDD · CQRS · Outbox · OpenTelemetry · React Native |
| **Shop Microservices** *(in progress · geliştiriliyor)* | DDD e-commerce platform of six bounded contexts on Kubernetes: Kafka + RabbitMQ, polyglot persistence, GitOps with Argo CD, Cloudflare edge. | Kubernetes üzerinde altı bounded context'li DDD e-ticaret platformu: Kafka + RabbitMQ, çoklu veri deposu, Argo CD ile GitOps, Cloudflare edge. | .NET · Kafka · RabbitMQ · PostgreSQL · MongoDB · Argo CD · KEDA · Vault |

---

## 📐 Architecture Projects · Mimari Projeler

| Project · Proje | EN | TR |
|---|---|---|
| [**Agentic GitOps & Argo CD**](https://github.com/Kozmoz-Inovasyon/agentic-gitops-argocd) | Reference architecture for safely bringing AI agents into the delivery pipeline with human-in-the-loop gates. | Yapay zekâ ajanlarını insan onay kapılarıyla teslim hattına güvenle dahil eden referans mimari. |
| [**Multi-Agent Orchestration**](https://github.com/Kozmoz-Inovasyon/evrak-orkestrasyonu) | Document pipeline orchestrated as a graph of agents with an approval gate and node-level retries. | Onay kapısı ve düğüm bazlı yeniden denemeyle graf olarak orkestre edilen evrak hattı. |
| [**ResultFlow**](https://github.com/alihanbb/ResultFlow) | Railway-Oriented Programming Result library for .NET 8 — chainable, allocation-aware, RFC 7807. | .NET 8 için Railway-Oriented Programming Result kütüphanesi — zincirlenebilir, düşük bellek tahsisli, RFC 7807. |
| [**Modular Monolith – Event Driven**](https://github.com/alihanbb/Modular_Monolith_EventDriven_MemoryStream) | Isolated modules with a custom in-memory event bus, Outbox and Inbox. | Özel in-memory event bus, Outbox ve Inbox ile izole modüller. |
| [**Aspire GenAI Microservices**](https://github.com/alihanbb/aspire-genai-microservice) | .NET Aspire microservices with Saga, Outbox, CQRS and Ollama-powered semantic search. | Saga, Outbox, CQRS ve Ollama destekli semantik arama ile .NET Aspire mikroservisleri. |
| [**.NET 9 OpenTelemetry Stack**](https://github.com/alihanbb/dotnet9-opentelemetry-stack) | Production-ready observability pipeline: OTel, ELK, Prometheus, Grafana, Jaeger. | Üretime hazır gözlemlenebilirlik hattı: OTel, ELK, Prometheus, Grafana, Jaeger. |
| [**Microservices with Redis Streams**](https://github.com/alihanbb/Microservice_RedisStream) | Event-driven services over Redis Streams with tracing, health checks and rate limiting. | Tracing, health check ve rate limiting ile Redis Streams üzerinden event-driven servisler. |
| [**Vertical Slice – End to End**](https://github.com/alihanbb/Vertical-Slice-Architechture---End-to-End) | Project management API with Vertical Slice Architecture and Domain Events. | Vertical Slice Architecture ve Domain Event'lerle proje yönetimi API'si. |
| [**Azure Serverless**](https://github.com/alihanbb/Azure_Serverless) | Event-driven order processing on Azure Functions with MassTransit, retries and circuit breaker. | MassTransit, retry ve circuit breaker ile Azure Functions üzerinde event-driven sipariş işleme. |
| [**GraphQL + CQRS + Clean Architecture**](https://github.com/alihanbb/dotnet-graphql-cqrs-clean-architechture) | GraphQL API with RabbitMQ events and Elasticsearch. | RabbitMQ olayları ve Elasticsearch ile GraphQL API. |
| [**Clean Architecture Template**](https://github.com/alihanbb/CleanArchitecture-Template) | .NET 9 boilerplate: JWT, caching, event bus, Docker Compose, tests. | .NET 9 şablonu: JWT, önbellek, event bus, Docker Compose, testler. |

---

## 🛠️ Tech Stack · Teknolojiler

**Languages · Diller**

![C#](https://img.shields.io/badge/C%23-512BD4?style=for-the-badge&logo=dotnet&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)

**Backend**

![.NET](https://img.shields.io/badge/.NET-512BD4?style=for-the-badge&logo=dotnet&logoColor=white)
![ASP.NET Core](https://img.shields.io/badge/ASP.NET%20Core-5C2D91?style=for-the-badge&logo=dotnet&logoColor=white)
![.NET Aspire](https://img.shields.io/badge/.NET%20Aspire-512BD4?style=for-the-badge&logo=dotnet&logoColor=white)
![NestJS](https://img.shields.io/badge/NestJS-E0234E?style=for-the-badge&logo=nestjs&logoColor=white)
![GraphQL](https://img.shields.io/badge/GraphQL-E10098?style=for-the-badge&logo=graphql&logoColor=white)
![SignalR](https://img.shields.io/badge/SignalR-0078D4?style=for-the-badge&logo=dotnet&logoColor=white)
![React Native](https://img.shields.io/badge/React%20Native-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)

**Messaging & Data · Mesajlaşma ve Veri**

![Kafka](https://img.shields.io/badge/Kafka-231F20?style=for-the-badge&logo=apachekafka&logoColor=white)
![RabbitMQ](https://img.shields.io/badge/RabbitMQ-FF6600?style=for-the-badge&logo=rabbitmq&logoColor=white)
![MassTransit](https://img.shields.io/badge/MassTransit-1E3A8A?style=for-the-badge)
![Azure Service Bus](https://img.shields.io/badge/Azure%20Service%20Bus-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![Elasticsearch](https://img.shields.io/badge/Elasticsearch-005571?style=for-the-badge&logo=elasticsearch&logoColor=white)

**Cloud, DevOps & Observability · Bulut, DevOps ve Gözlemlenebilirlik**

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![Argo CD](https://img.shields.io/badge/Argo%20CD-EF7B4D?style=for-the-badge&logo=argo&logoColor=white)
![Azure](https://img.shields.io/badge/Azure%20Functions-0062AD?style=for-the-badge&logo=azurefunctions&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white)
![Cloudflare](https://img.shields.io/badge/Cloudflare-F38020?style=for-the-badge&logo=cloudflare&logoColor=white)
![OpenTelemetry](https://img.shields.io/badge/OpenTelemetry-000000?style=for-the-badge&logo=opentelemetry&logoColor=white)
![Prometheus](https://img.shields.io/badge/Prometheus-E6522C?style=for-the-badge&logo=prometheus&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana-F46800?style=for-the-badge&logo=grafana&logoColor=white)
![Jaeger](https://img.shields.io/badge/Jaeger-66CFE3?style=for-the-badge&logo=jaeger&logoColor=black)

**AI · Yapay Zekâ**

![Ollama](https://img.shields.io/badge/Ollama-000000?style=for-the-badge&logo=ollama&logoColor=white)
![Semantic Kernel](https://img.shields.io/badge/Semantic%20Kernel-5C2D91?style=for-the-badge&logo=microsoft&logoColor=white)
![LangGraph](https://img.shields.io/badge/LangGraph-1C3C3C?style=for-the-badge&logo=langchain&logoColor=white)

---

## 🎯 Goals · Hedefler

| EN | TR |
|---|---|
| Grow into **software & platform architecture**: designing systems that scale, fail safely and stay observable. | **Yazılım ve platform mimarlığında** derinleşmek: ölçeklenen, güvenli şekilde hata veren ve gözlemlenebilir kalan sistemler tasarlamak. |
| Run production workloads on **Kubernetes with GitOps** end to end. | **Kubernetes ve GitOps** ile üretim iş yüklerini uçtan uca yönetmek. |
| Build **AI-native backends** where agents work behind clear, human-approved gates. | Ajanların net, insan onaylı kapılar arkasında çalıştığı **AI-native backend'ler** kurmak. |
| Share what I learn through open source and writing at [kozmozinovasyon.com](https://www.kozmozinovasyon.com/insights). | Öğrendiklerimi açık kaynak ve [kozmozinovasyon.com](https://www.kozmozinovasyon.com/insights) yazılarıyla paylaşmak. |

---

## 📊 GitHub Stats

<p align="center">
  <img src="https://github-readme-stats.vercel.app/api/top-langs/?username=alihanbb&layout=compact&hide=html&theme=transparent" alt="Top Languages" height="165"/>
</p>

---

## 📫 Contact · İletişim

<p align="left">
  <a href="https://www.linkedin.com/in/alihan-%C3%A7elik/"><img src="https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white" alt="LinkedIn"/></a>
  <a href="https://www.kozmozinovasyon.com"><img src="https://img.shields.io/badge/Website-kozmozinovasyon.com-F97316?style=for-the-badge&logo=googlechrome&logoColor=white" alt="Website"/></a>
  <a href="https://github.com/alihanbb"><img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub"/></a>
</p>

<p align="center"><i>"Make it work, make it right, make it observable."</i></p>
