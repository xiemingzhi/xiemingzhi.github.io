---
title: Spring Boot Different ways to start an application
date: 2020-08-07T09:06:20+08:00
---

# Spring Boot - Different ways to start an application

Using ConfigurableApplicationContext

```
@SpringBootApplication
public class KafkaApplication {

    public static void main(String[] args) throws Exception {

        ConfigurableApplicationContext context = SpringApplication.run(KafkaApplication.class, args);

        MessageProducer producer = context.getBean(MessageProducer.class);
        MessageListener listener = context.getBean(MessageListener.class);
		...
	}
}
```

Implement CommandLineRunner

```
@SpringBootApplication
public class DemoApplication implements CommandLineRunner {
	
	@Autowired
	CassandraConfiguration cassConfig;

	@Autowired
	PersonRepository repository;
	
	public static void main(String[] args) {
		SpringApplication.run(DemoApplication.class, args);
	}

	@Override
	public void run(String... arg0) throws Exception {
		Iterable<Person> persons = repository.findAll();
		for (Person person : persons) {
			LOGGER.info("person id {}, name {}", person.getId(), person.getName());
		}
		cassConfig.cassandraSession().destroy();
	}
}
```
	
Bean CommandLineRunner

```
@SpringBootApplication
public class DatastoreRepositoryExample {

	@Autowired
	private SingerRepository singerRepository;

	public static void main(String[] args) {
		SpringApplication.run(DatastoreRepositoryExample.class, args);
	}

	@Bean
	public CommandLineRunner commandLineRunner() {
		return (args) -> {
			System.out.println("Remove all records from 'singers' kind");
			this.singerRepository.deleteAll();
		};
	}
}
```
