```
public class MybatisTest {

    private InputStream in;
    private SqlSession sqlSession;
    private IUserDao userDao;

    @Before
    public void init() throws Exception{
        in = Resources.getResourceAsStream("mybatis.xml");
        SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
        sqlSession = factory.openSession();
        userDao = sqlSession.getMapper(IUserDao.class);

    }

    @After
    public void destroy()throws Exception{
        sqlSession.commit();
        sqlSession.close();
        in.close();
    }

    @Test
    public void testFindAll() throws Exception{
        List<User> users = userDao.findAll();
        for(User user:users){
            System.out.println(user);
        }
    }

    @Test
    public void testSaveUser() throws Exception{
        User user = new User();
        user.setName("小张");
        user.setBirthday(new Date());
        user.setGender(2);
        user.setAddress("天津");
        userDao.saveUser(user);
        System.out.println(user);
    }
    @Test
    public void testUpdateUser() throws Exception{
        User user = new User();
        user.setId(9);
        user.setName("小张");
        user.setBirthday(new Date());
        user.setGender(2);
        user.setAddress("南京");
        userDao.updateUser(user);
    }

    @Test
    public void testDeleteUser() throws Exception{
        userDao.deleteUser(2);
    }

    @Test
    public void testFindOne() throws Exception{
       User user= userDao.findById(1);
       System.out.println(user);
    }

    @Test
    public void testFindByName() throws Exception{
        List<User> users= userDao.findByName("小%");
        System.out.println(users);
    }

    @Test
    public void testFindTotal() throws Exception{
         Integer total= userDao.findTotal();
        System.out.println(total);
    }

    @Test
    public void testFindByVo() throws Exception{
        QueryVo vo = new QueryVo();
        User user = new User();
        user.setName("小王");
        vo.setUser(user);
        List<User> users = userDao.findUserByVo(vo);
        System.out.println(users);
    }

}

```

mybatis.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--配置环境-->
    <environments default="mysql">
        <environment id="mysql">
            <!--配置事务的类型-->
            <transactionManager type="JDBC"></transactionManager>
            <!--配置数据源(连接池)-->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://127.0.0.1:3306/demo?characterEncoding=utf-8"/>
                <property name="username" value="root"/>
                <property name="password" value="baxiang"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="mappers/IUserDao.xml"></mapper>
    </mappers>
</configuration>

```
IUserDao.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="dao.IUserDao">
    <select id="findAll" resultType="domain.User">
        select * from user;
    </select>

    <resultMap id="userMap" type="domain.User">
        <!--主键字段的对应-->
        <id property="id" column="id"></id>
        <result property="name" column="name"></result>
        <result property="address" column="address"></result>
        <result property="gender" column="gender"></result>
    </resultMap>

    <insert id="saveUser" parameterType="domain.User">
        <selectKey keyProperty="id" keyColumn="id" resultType="java.lang.Integer" order="AFTER">
            select LAST_INSERT_ID()
        </selectKey>
        insert into user(name,gender,birthday,address)value (#{name},#{gender},#{birthday},#{address})
    </insert>

    <update id="updateUser" parameterType="domain.User">
        update user set name=#{name},address=#{address},gender=#{gender},birthday=#{birthday}
        where id=#{id}
    </update>

    <delete id="deleteUser" parameterType="java.lang.Integer">
        DELETE  from user where id = #{id}
    </delete>

    <select id="findById" parameterType="java.lang.Integer" resultType="domain.User">
        select * from  user where id = #{id}
    </select>

    <select id="findByName" parameterType="String" resultMap="userMap">
        select * from  user where name LIKE #{name}
    </select>

    <select id="findTotal" resultType="java.lang.Integer">
        select COUNT(*) FROM user
    </select>

    <select id="findUserByVo" parameterType="domain.QueryVo" resultType="domain.User">
        select * from  user where name LIKE #{user.name}
    </select>

```
接口类
```
/**
 * 用户持久层接口
 */
public interface IUserDao {

    List<User> findAll();
    void saveUser(User user);
    void updateUser(User user);
    void deleteUser(Integer id);
    User findById(Integer id);
    List<User> findByName(String name);
    Integer findTotal();
    List<User> findUserByVo(QueryVo vo);
}

```
User
```
public class User implements Serializable {

    private Integer id;
    private String name;
    private int gender;
    private Date birthday;
    private String address;
    

    public Date getBirthday() {
        return birthday;
    }

    public void setBirthday(Date birthday) {
        this.birthday = birthday;
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getGender() {
        return gender;
    }

    public void setGender(int gender) {
        this.gender = gender;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", gender=" + gender +
                ", birthday=" + birthday +
                ", address='" + address + '\'' +
                '}';
    }
}

```
