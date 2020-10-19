---
title: "Test Containers"
date: 2020-06-05T19:21:52+02:00
draft: true
---

https://dzone.com/articles/implementing-integration-tests-using-testcontainer

https://phauer.com/2019/focus-integration-tests-mock-based-tests/#execution-speed

https://www.testcontainers.org/test_framework_integration/manual_lifecycle_control/


????

public abstract class AbstractIntegrationTest {

    public static MySQLContainer mysql = new MySQLContainer();

    static {
        mysql.start();
    }
}