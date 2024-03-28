# Hibernate三種查詢資料的方式


## 1. HQL（Hibernate Query Language）

```java
@Override 

public SysUser findUserByLoginName(String pLoginName) {
    String hql = "from SysUser as u where u.loginName = ?";
    List<SysUser> users = getHibernateTemplate().find(hql, pLoginName); 
    return users.isEmpty() ? null : users.get(0);
}
```

## 2. SQL（Structured Query Language）

```java
static List sql() {
 　 Session s = HibernateUtil.getSession();
    Query q = s.createSQLQuery("select * from user").addEntity(User.class);
    List<User> rs = q.list();
    s.close();
    return rs;

}
```

## 3. QBC（Query By Criteria）

```java
//查詢符合條件的帳號
Criteria c=s.createCriteria(Admin.class);
c.add(Restrictions.eq("aname",name));
c.add(Restrictions.eq("apassword", password));
List<Admin> list=c.list();

//列出分頁前10項
Criteria criteria = session.createCriteria(Customer.class);
criteria.addOrder( Order.asc("name") ); //排序方式
criteria.setFirstResult(0);
criteria.setMaxResults(10);
List result = criteria.list（）
```
