# NOSQL

## CAP+BASE

### CAP

- C: Consistency 强一致性
- A: Availability 可用性
- P : Partition tolerance 分区容错性

**只可同时满足两条**

不同组合的功能

- CA - 单点集群,满足一致性,可用性的系统,通常可扩展性不高
- CP - 满足一致性,分区容错性,通常性能不高
- AP - 满足可用,分区容错,通常对一致性要求不高



`sql`数据库,如`mySql`,`Oracle`为CA ,强一致性和高可用

常用`nosql`数据库,如`mongodb`,`redis`,满足CP,强一致性,分区容错性

AP为大多数网站架构的选择



### BASE

- Basically Available 基本可用
- Soft state 软状态
- Eventually consistent 最终一致性

> 思想是通过让系统放松对某一时刻的数据一致性要求,来换取高性能.
>
> 由于大型系统的地域分布和对性能得要求,不能采用分布式事务.则用BASE来解决

