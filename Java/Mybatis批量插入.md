## foreach 方式插入

``` java
@Test
public void testInsertBatch() throws Exception {
    long start = System.currentTimeMillis();
    List<User> list = new ArrayList<>();
    User user;
    for (int i = 0; i < 10000; i++) {
        user = new User();
        user.setId("test" + i);
        user.setName("name" + i);
        user.setDelFlag("0");
        list.add(user);
    }
    userService.insertBatch(list);
    long end = System.currentTimeMillis();
    System.out.println("---------------" + (start - end) + "---------------");
}

```

``` xml
<insert id="insertBatch">
    INSERT INTO t_user
            (id, name, del_flag)
    VALUES
    <foreach collection ="list" item="user" separator =",">
         (#{user.id}, #{user.name}, #{user.delFlag})
    </foreach >
</insert>

```