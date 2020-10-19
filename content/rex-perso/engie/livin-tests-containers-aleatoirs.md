---
title: "Livin Tests Containers Aleatoirs"
date: 2020-06-08T18:26:27+02:00
draft: true
---

## Objectifs pedagogiques

- Introduire complexité accidentelle et entropie
- Utilisation des tests fixtures
- Comment implémenter le pattern singleton

## Recos tests containers

- Ne pas utiliser des FixedHostPortGenericContainer <https://github.com/testcontainers/testcontainers-java/issues/1005>
- Une seule instance

https://www.testcontainers.org/test_framework_integration/manual_lifecycle_control/#singleton-containers

https://github.com/testcontainers/testcontainers-java/blob/7ba06573feb6561c6fc857cd5b53ff1449808da9/examples/singleton-container/src/test/java/com/example/AbstractIntegrationTest.java#L5


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




public abstract class AbstractIntegrationTest {

    @ClassRule
    public static GenericContainer mongo = new GenericContainer("mongo:latest").withExposedPorts(27017);

    public static class Initializer implements ApplicationContextInitializer<ConfigurableApplicationContext> {
        @Override
        public void initialize(ConfigurableApplicationContext configurableApplicationContext) {
            TestPropertyValues.of("spring.data.mongodb.uri=mongodb://" + mongo.getContainerIpAddress() + ":" + mongo.getMappedPort(27017) + "/mongo-dakar").applyTo(configurableApplicationContext);
            log.debug("New property : " + configurableApplicationContext.getEnvironment().getProperty("spring.data.mongodb.uri"));//=mongodb://" + mongo.getContainerIpAddress() + ":" + mongo.getMappedPort(27017) + "/mongo-dakar");
        }
    }
}



https://dzone.com/articles/implementing-integration-tests-using-testcontainer




https://bsideup.github.io/posts/debugging_containers/

https://github.com/testcontainers/testcontainers-java/blob/master/examples/spring-boot/src/test/java/com/example/AbstractIntegrationTest.java
https://github.com/testcontainers/testcontainers-java/issues/256
https://github.com/silaev/mongodb-replica-set/



```log
16:09:17.525 [main] DEBUG org.testcontainers.containers.output.WaitingConsumer - STDOUT: 2020-06-15T14:09:17.533+0000 I NETWORK  [initandlisten] waiting for connections on port 27017
16:09:17.525 [main] INFO 🐳 [mongo:4.0.10] - Container mongo:4.0.10 started in PT1.122313S
16:09:17.526 [main] DEBUG org.testcontainers.containers.MongoDBContainer - Initializing a single node node replica set...
16:09:17.526 [main] DEBUG org.testcontainers.containers.ExecInContainerPattern - /jovial_tesla: Running "exec" command: mongo --eval rs.initiate();
16:09:17.526 [main] DEBUG com.github.dockerjava.core.command.AbstrDockerCmd - Cmd: cae75aeaf001eb14bf7c6220d9258bd4d45a6ea5599213a0d91221c06170a0b9,<null>,true,true,<null>,<null>,<null>,{mongo,--eval,rs.initiate();},<null>,<null>
16:09:17.753 [main] DEBUG com.github.dockerjava.core.command.AbstrDockerCmd - Cmd: 849bc63ccd58fca31b87cfbb2049d0f8265b5050ce5f9429a74cd6f76d6d159d
16:09:17.754 [main] DEBUG com.github.dockerjava.core.exec.InspectExecCmdExec - GET: com.github.dockerjava.okhttp.OkHttpWebTarget@af9a89f
16:09:17.758 [main] DEBUG org.testcontainers.containers.MongoDBContainer - MongoDB shell version v4.0.10

connecting to: mongodb://127.0.0.1:27017/?gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("5ac70f19-8789-47b2-9769-b23d3267094f") }

MongoDB server version: 4.0.10

{
	"ok" : 0,
	"errmsg" : "This node was not started with the replSet option",
	"code" : 76,
	"codeName" : "NoReplicationEnabled"
}

16:09:17.758 [main] DEBUG org.testcontainers.containers.MongoDBContainer - Awaiting for a single node replica set initialization up to 60 attempts
16:09:17.758 [main] DEBUG org.testcontainers.containers.ExecInContainerPattern - /jovial_tesla: Running "exec" command: mongo --eval var attempt = 0; while(db.runCommand( { isMaster: 1 } ).ismaster==false) { if (attempt > 60) {quit(1);} print('An attempt to await for a single node replica set initialization: ' + attempt); sleep(100);  attempt++;  }
16:09:17.758 [main] DEBUG com.github.dockerjava.core.command.AbstrDockerCmd - Cmd: cae75aeaf001eb14bf7c6220d9258bd4d45a6ea5599213a0d91221c06170a0b9,<null>,true,true,<null>,<null>,<null>,{mongo,--eval,var attempt = 0; while(db.runCommand( { isMaster: 1 } ).ismaster==false) { if (attempt > 60) {quit(1);} print('An attempt to await for a single node replica set initialization: ' + attempt); sleep(100);  attempt++;  }},<null>,<null>
16:09:17.951 [main] DEBUG com.github.dockerjava.core.command.AbstrDockerCmd - Cmd: cd8338f1cabab1b0815530041802b294d848cfa9329d50195581800cb14b3b82
16:09:17.951 [main] DEBUG com.github.dockerjava.core.exec.InspectExecCmdExec - GET: com.github.dockerjava.okhttp.OkHttpWebTarget@5bcb04cb
16:09:17.955 [main] DEBUG org.testcontainers.containers.MongoDBContainer - MongoDB shell version v4.0.10

connecting to: mongodb://127.0.0.1:27017/?gssapiServiceName=mongodb

2020-06-15T14:09:17.926+0000 E QUERY    [js] Error: network error while attempting to run command 'isMaster' on host '127.0.0.1:27017'  :
connect@src/mongo/shell/mongo.js:344:17
@(connect):2:6

16:09:17.955 [main] ERROR org.testcontainers.containers.MongoDBContainer - A single node replica set was not initialized in a set timeout: 60 attempts
16:09:17.955 [main] ERROR 🐳 [mongo:4.0.10] - Could not start container
org.testcontainers.containers.MongoDBContainer$ReplicaSetInitializationException: A single node replica set was not initialized in a set timeout: 60 attempts
	at org.testcontainers.containers.MongoDBContainer.checkMongoNodeExitCodeAfterWaiting(MongoDBContainer.java:90)
	at org.testcontainers.containers.MongoDBContainer.initReplicaSet(MongoDBContainer.java:112)
	at org.testcontainers.containers.MongoDBContainer.containerIsStarted(MongoDBContainer.java:51)
	at org.testcontainers.containers.GenericContainer.containerIsStarted(GenericContainer.java:657)
	at org.testcontainers.containers.GenericContainer.tryStart(GenericContainer.java:477)
	at org.testcontainers.containers.GenericContainer.lambda$doStart$0(GenericContainer.java:325)
	at org.rnorth.ducttape.unreliables.Unreliables.retryUntilSuccess(Unreliables.java:81)
	at org.testcontainers.containers.GenericContainer.doStart(GenericContainer.java:323)
	at org.testcontainers.containers.GenericContainer.start(GenericContainer.java:311)
	at org.testcontainers.containers.GenericContainer.starting(GenericContainer.java:1022)
	at org.testcontainers.containers.FailureDetectingExternalResource$1.evaluate(FailureDetectingExternalResource.java:29)
	at org.junit.rules.RunRules.evaluate(RunRules.java:20)
	at org.junit.runners.ParentRunner.run(ParentRunner.java:363)
	at org.springframework.test.context.junit4.SpringJUnit4ClassRunner.run(SpringJUnit4ClassRunner.java:190)
	at org.junit.runners.Suite.runChild(Suite.java:128)
	at org.junit.runners.Suite.runChild(Suite.java:27)
	at org.junit.runners.ParentRunner$3.run(ParentRunner.java:290)
	at org.junit.runners.ParentRunner$1.schedule(ParentRunner.java:71)
	at org.junit.runners.ParentRunner.runChildren(ParentRunner.java:288)
	at org.junit.runners.ParentRunner.access$000(ParentRunner.java:58)
	at org.junit.runners.ParentRunner$2.evaluate(ParentRunner.java:268)
	at org.junit.runners.ParentRunner.run(ParentRunner.java:363)
	at org.junit.runner.JUnitCore.run(JUnitCore.java:137)
	at com.intellij.junit4.JUnit4IdeaTestRunner.startRunnerWithArgs(JUnit4IdeaTestRunner.java:68)
	at com.intellij.rt.junit.IdeaTestRunner$Repeater.startRunnerWithArgs(IdeaTestRunner.java:33)
	at com.intellij.rt.junit.JUnitStarter.prepareStreamsAndStart(JUnitStarter.java:230)
	at com.intellij.rt.junit.JUnitStarter.main(JUnitStarter.java:58)
16:09:17.999 [main] ERROR 🐳 [mongo:4.0.10] - Log output from the failed container:
about to fork child process, waiting until server is ready for connections.

forked process: 29

2020-06-15T14:09:16.872+0000 I CONTROL  [main] ***** SERVER RESTARTED *****

2020-06-15T14:09:16.876+0000 I CONTROL  [main] Automatically disabling TLS 1.0, to force-enable TLS 1.0 specify --sslDisabledProtocols 'none'

2020-06-15T14:09:16.882+0000 I CONTROL  [initandlisten] MongoDB starting : pid=29 port=27017 dbpath=/data/db 64-bit host=cae75aeaf001

```
