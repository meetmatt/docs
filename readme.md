# Table of Contents

* Getting Started
    * [About Framework](about/spiral.md)
    * [Installation](about/install.md)
    * [Versioning](about/semver.md)
    * [Contributing](about/contributing.md)
    * [LICENSE](license.md)
* Basics
    * [Application Workers](basic/workers.md)
    * [Application Structure](basic/structure.md)
    * [Default Configuration](basic/configuration.md)
    * [Console Commands](basic/commands.md)
* Framework
    * Design Approach
    * [Application Server](framework/application-server.md)
    * Kernel, Environment
    * [Container](framework/container.md)
    * [Bootloaders](framework/bootloaders.md)
    * [Config Objects](framework/config.md)
    * [IoC scopes](framework/scopes.md)
    * [Static Memory](framework/memory.md)
    * [Finalizers](framework/finalizers.md)
* Cookbook
    * Quick Start
    * Scaffolding
    * [Prototyping](cookbook/prototype.md)
    * Database Scaffolding
    * MVC Application
    * [Injectors](cookbook/injector.md)
    * [Domain Core and Controllers](cookbook/domain-core.md)
    * [Website Scraper](cookbook/scraper.md)
    * [Custom PSR-15 Handlers](cookbook/psr-15.md)
    * [Integrate Golang service to PHP](cookbook/golang-library.md)
    * [Custom Dispatcher](cookbook/custom-dispatcher.md)
    * Testing Application
* Components
    * [Files and Directories](component/files.md)
    * [Code Generation](component/reactor.md)
    * [Pagination](component/pagination.md)
    * [Static Analysis Tools](component/tokenizer.md)
    * [Prometheus Metrics](component/metrics.md)
    * Data Grids
* Security
    * [Data Encryption](security/encrypter.md)
    * [Validation](security/validation.md)
    * [Role Based Access Control](security/rbac.md)
    * [User Authentication](security/authentication.md)
* Console
    * [Installation and Configuration](console/configuration.md)
    * [User Commands](console/commands.md)
* HTTP
    * [Installation and Configuration](http/configuration.md)
    * [Request Lifecycle](http/lifecycle.md)
    * [Request and Response](http/request-response.md)
    * [Routing](http/routing.md)
    * [Error Pages](http/errors.md)
    * [Middleware](http/middleware.md)
    * [Golang Middleware](http/golang.md)
    * [Cookies](http/cookies.md)
    * Session
    * [CSRF protection](http/csrf.md)
* Filter / Request Form
    * [Installation and Configuration](filters/configuration.md)
    * [Filter Object](filters/filter.md)
    * [Composite Filters](filters/composite.md)
* Queue and Jobs
    * [Installation and Configuration](queue/configuration.md)
    * [Console Commands](queue/commands.md)
    * [Running Jobs](queue/jobs.md)
    * [Standalone Usage](queue/standalone.md)
* GRPC
    * Installation and Configuration
    * Generating Service Code
    * Passing Metadata and Errors
    * Golang Services
    * GRPC client code
* Views
    * [Installation and Configuration](views/configuration.md)
    * Rendering Views
    * [Plain PHP templates](views/native.md)
    * [Twig templates](views/twig.md)
* Stempler templates
    * Installation and Configuration
    * Basic Usage
    * Inheritance
    * Components and Props
    * Directives
    * [AST Modifications](stempler/visitors.md)
* Internalization
    * Installation and Configuration
    * Import and Export
    * Translate Views
    * Say Trait
* Databases
    * Installation and Configuration
    * Databases and Drivers
    * Database Isolation
    * Query Builders
    * [Transactions](database/transactions.md)
    * [Schema Introspection](database/introspection.md)
    * [Schema Declaration](database/declaration.md)
    * [Migrations](database/migrations.md)
    * [Errata](database/errata.md)
* Cycle ORM
    * [Installation and Configuration](cycle/configuration.md)
    * [Transactions](cycle/transactions.md)
    * [Full Documentation](cycle/documentation.md)
    * [Console Commands](cycle/commands.md)
* Event Broadcasting
    * Installation and Configuration
    * WebSocket Client
    * Distributed Locks
    * Standalone Usage
* Debug and Profiling
    * [Dumping Variables](debug/dumps.md)
    * RoadRunner Gotchas
    * XDebug
    * [Handling Exceptions](debug/exceptions.md)
    * Profiling (coming soon)
* Extensions
    * [Code Style](extension/code-style.md)
    * [Dotenv](extension/dotenv.md)   
    * [Monolog](extension/monolog.md)
    * [Sentry](extension/sentry.md)
