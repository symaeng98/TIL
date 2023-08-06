### A vs CNAME

```java
aodtns.tistory.com -> 211.231.99.250 (A record)
aodtns.com -> aodtns.tistory.com (CNAME)
```

**A record**는 **직접적으로 IP가 할당**되어 있기 때문에 **IP가 변경되면 직접적으로 도메인에 영향**을 미치지만,
**CNAME**은 **도메인에 도메인이 매핑**되어 있기 때문에 **IP의 변경에 직접적인 영향을 받지 않는다**