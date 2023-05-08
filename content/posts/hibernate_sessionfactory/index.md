---
title: "在JAVA中建立Hibernate SessionFactory的三種方法"
date: 2023-05-08T11:00:25+08:00
lastmod: 2023-05-08T11:00:25+08:00
draft: false
author: "Divazone King"
authorLink: ""
tags: ["Hibernate", "Java"]
categories: ["coding_life"]
---

方法有這三種

1. 使用hibernate.cfg.xml文件建立Hibernate SessionFactory
2. 使用Metatada和ServiceRegistry class在Hibernate中建立SessionFactory
3. 從JPA的EntityManager中獲得Hibernate SessionFactory

## 1. 使用hibernate.cfg.xml文件建立Hibernate SessionFactory

這是官方建議的方法，hibernate.cfg.xml內容如下:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-configuration PUBLIC 
  "-//Hibernate/Hibernate Configuration DTD 3.0//EN" 
  "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
  <hibernate-configuration>
    <session-factory>
      <property name="connection.driver_class">com.mysql.cj.jdbc.Driver</property>
      <!-- property name="connection.driver_class">com.mysql.jdbc.Driver</property -->
      <property name="connection.url">jdbc:mysql://localhost/hibernate_examples</property>
      <property name="connection.username">root</property>
      <property name="connection.password">password</property>
      <property name="connection.pool_size">3</property>
      <property name="dialect">org.hibernate.dialect.MySQL8Dialect</property>
      <property name="current_session_context_class">thread</property>
      <property name="show_sql">true</property>
      <property name="format_sql">true</property>
      <property name="hbm2ddl.auto">update</property>
      <!-- mapping  class="com.mcnz.jpa.examples.Player" / -->
  </session-factory>
</hibernate-configuration>
```

接著在就可以在JAVA內去建立Hibernate SessionFactory:

```java
public static Session getCurrentSessionFromConfig() {
  // SessionFactory in Hibernate 5 example
  Configuration config = new Configuration();
  config.configure();
  // local SessionFactory bean created
  SessionFactory sessionFactory = config.buildSessionFactory();
  Session session = sessionFactory.getCurrentSession();
  return session;
}
```

## 2. 使用Metatada和ServiceRegistry class在Hibernate中建立SessionFactory

個人覺得用xml檔來儲存像是資料庫位址與帳密實在是太危險，好在能夠使用一個HashMap來把資料儲存起來傳送給ServiceRegistry，這樣就不需要去建立hibernate.cfg.xml了

```java
public static Session getCurrentSession() {
  // Hibernate 5.4 SessionFactory example without XML
  Map<String, String> settings = new HashMap<>();
  settings.put("connection.driver_class", "com.mysql.jdbc.Driver");
  settings.put("dialect", "org.hibernate.dialect.MySQL8Dialect");
  settings.put("hibernate.connection.url", 
    "jdbc:mysql://localhost/hibernate_examples");
  settings.put("hibernate.connection.username", "root");
  settings.put("hibernate.connection.password", "password");
  settings.put("hibernate.current_session_context_class", "thread");
  settings.put("hibernate.show_sql", "true");
  settings.put("hibernate.format_sql", "true");

  ServiceRegistry serviceRegistry = new StandardServiceRegistryBuilder()
                                    .applySettings(settings).build();

  MetadataSources metadataSources = new MetadataSources(serviceRegistry);
  // metadataSources.addAnnotatedClass(Player.class);
  Metadata metadata = metadataSources.buildMetadata();

  // here we build the SessionFactory (Hibernate 5.4)
  SessionFactory sessionFactory = metadata.getSessionFactoryBuilder().build();
  Session session = sessionFactory.getCurrentSession();
  return session;
}
```

## 3. 從JPA的EntityManager中獲得Hibernate SessionFactory

直接使用JPA去取得 hibernate session內的資料

```java
public static SessionFactory getCurrentSessionFromJPA() {
  // JPA and Hibernate SessionFactory example
  EntityManagerFactory emf = 
      Persistence.createEntityManagerFactory("jpa-tutorial");
  EntityManager entityManager = emf.createEntityManager();
  // Get the Hibernate Session from the EntityManager in JPA
  Session session = entityManager.unwrap(org.hibernate.Session.class);
  SessionFactory factory = session.getSessionFactory();
  return factory;
}
```


## 參考資料 
 
 [Build a Hibernate SessionFactory by example](https://www.theserverside.com/blog/Coffee-Talk-Java-News-Stories-and-Opinions/3-ways-to-build-a-Hibernate-SessionFactory-in-Java-by-example?utm_source=pocket_saves)