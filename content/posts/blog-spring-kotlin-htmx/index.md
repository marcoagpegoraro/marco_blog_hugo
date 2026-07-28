---
  author: Marco Pegoraro
  title: Spring Kotlin with HTMX - Creating a Blog Application
  date: 2026-07-28 11:30:00
  description: ""
  tags: ["Spring", "Kotlin", "HTMX", "Security"] 
  header_image: /posts/blog-spring-kotlin-htmx/banner.webp
---

## Introduction

*Disclaimer: No generative AI was used to write this blog post, nor the application code.*

This blog post will demonstrate how to create a blog application with the possibility to create, edit and delete posts. It will contain authentication so the admin can manage the posts and also user authentication so readers can leave coments in the posts. For that, it should be able to handle a database integration.

For this application we will use Spring Initialzr to create the scaffolding and add the following dependencies: 

|Dependency|Observation|
|:---|:---:|
|Kotlin|Modern programming language that also runs in the JVM, aims to be a fun alternative to Java, also it is fully interoperable.|
|Spring Security|Provides the security foundation into the application already defined and ready to use by default, only requiring the specific configuration.|
|HTMX|JS library that allows a SPA behaivor in a traditional server application using only HTML custom tags.|
|Flyway|Provides an easy way to manage migrations using plain SQL.|
|Thymeleaf|Template Engine|
|GraalVM|Generate the application using a native image instead of running in the JVM, saving resources and start up time.|
|Actuator|To monitore the application using standarized http paths.|
|JPA/Postgres|The database connection.|

[Click here to be redirected to Spring Initialzr with the dependencies already loaded](https://start.spring.io/#!type=maven-project&language=kotlin&platformVersion=4.0.7&packaging=jar&configurationFileFormat=properties&jvmVersion=25&groupId=dev.pegoraro&artifactId=spring-kotlin-htmx-security-blog&packageName=dev.pegoraro.spring-kotlin-htmx-security-blog&dependencies=web,devtools,thymeleaf,security,htmx,validation,postgresql,h2,actuator,flyway,opentelemetry,data-jpa,native)


With the application downloaded, open it in IntelliJ IDEA.

*For some reason, the downloaded project was using JDK 24 and not the 25, so i've changed manually in the pom.xml the java.version to 25 and the kotlin.version to 2.4.0. In your case, you might not need this.*

You should be able to build and run the application in this stage (it will use a default spring security password and the H2 database).

To log in, open the http://localhost:8080 url and then type the username "user" and the password that it is in the run console.

To keep it simple, lets just **temporarelly** coment the spring security dependency and also the open telemetry one, so we can worry only about the development of the application and then when the application starts to take form, then we add those dependencies again.

# Coding the models

I will follow the hexagonal pattern to name the packages in the application, so we will have ports and adapters in the app, and the adapters could be either an input or an output adapter.

For the persistence layer, i will create then a package ```adapters.out``` and inside of it, four packages for each integration.


For this blog application we will create in total 4 models:

adapters/out/comment/Post.kt:
```java
@Entity
class Post(
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    var id: Long? = null,
    @Column(nullable = false)
    var title: String = "",
    var subtitle: String? = null,
    @Column(columnDefinition = "TEXT")
    var body: String? = null,
    @Column(nullable = false)
    var isDraft: Boolean = true,
    @Column(nullable = false)
    var language: String = "en",
    @ManyToMany(fetch = FetchType.LAZY)
    @JoinTable(name = "post_tag",joinColumns = [ JoinColumn(name = "post_id")], inverseJoinColumns = [JoinColumn(name = "tag_id")],uniqueConstraints = [UniqueConstraint(name = "uk_post_tag",columnNames = ["post_id", "tag_id"])])
    var tags: MutableSet<Tag> = mutableSetOf(),
    @CreationTimestamp
    @Column(nullable = false, updatable = false)
    var createdAt: OffsetDateTime? = null,
    var publishedAt: OffsetDateTime? = null,
    @UpdateTimestamp
    var updatedAt: OffsetDateTime? = null
)
```

adapters/out/comment/Tag.kt:
```java
@Entity
class Tag(
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    var id: Long? = null,
    @Column(nullable = false)
    var name: String = "",
    @ManyToMany(mappedBy = "tags", fetch = FetchType.LAZY)
    var posts: MutableSet<Post> = mutableSetOf(),
    @CreationTimestamp
    @Column(nullable = false, updatable = false)
    var createdAt: OffsetDateTime? = null,
    @UpdateTimestamp
    var updatedAt: OffsetDateTime? = null
)
```

adapters/out/comment/Comment.kt:
```java
@Entity
class Comment(
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    var id: Long? = null,
    @Column(nullable = false, columnDefinition = "TEXT")
    var body: String = "",
    @ManyToOne(fetch = FetchType.LAZY, optional = false)
    @JoinColumn(name = "post_id", nullable = false)
    var post: Post? = null,
    @ManyToOne(fetch = FetchType.LAZY, optional = false)
    @JoinColumn(name = "author_id", nullable = false)
    var author: AppUser? = null,
    @CreationTimestamp
    @Column(nullable = false, updatable = false)
    var createdAt: OffsetDateTime? = null,
    @UpdateTimestamp
    var updatedAt: OffsetDateTime? = null
)
```

adapters/out/comment/AppUser.kt:
```java
@Entity
class AppUser(
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    var id: Long? = null,
    @Column(nullable = false, unique = true)
    var username: String = "",
    @Column(nullable = false)
    var passwordHash: String = "",
    @Enumerated(EnumType.STRING)
    @Column(nullable = false)
    var role: Role = Role.USER,
    @Column(nullable = false)
    var enabled: Boolean = true
)

// And this enum in other file inside the same package:
enum class Role {
    USER,
    ADMIN
}
```