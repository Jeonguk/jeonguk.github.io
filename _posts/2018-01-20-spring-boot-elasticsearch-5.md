---
layout: post
title:  "Spring boot elasticsearch 5.5 configuration"
date:   2018-01-20 19:25:10 +0700
categories: [spring, nosql, elasticsearch]
---


## Spring boot elasticsearch 5.5 configuration

* Maven

```
<dependency>
    <groupId>org.elasticsearch</groupId>
    <artifactId>elasticsearch</artifactId>
    <version>5.5.1</version>
</dependency>
<dependency>
    <groupId>org.elasticsearch.client</groupId>
    <artifactId>transport</artifactId>
    <version>5.5.1</version>
</dependency>
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-api</artifactId>
    <version>2.8.2</version>
</dependency>
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-core</artifactId>
    <version>2.8.2</version>
</dependency>
```

* application.properties

```
spring.data.elasticsearch.cluster-nodes=172.0.0.1:9300
spring.data.elasticsearch.local=false
spring.data.elasticsearch.properties.transport.tcp.connect_timeout=60s
```

* ElasticsearchConfiguration

{% highlight java %}

import java.net.InetAddress;
import java.net.UnknownHostException;

import org.elasticsearch.client.transport.TransportClient;
import org.elasticsearch.common.transport.InetSocketTransportAddress;
import org.elasticsearch.transport.client.PreBuiltTransportClient;
import org.elasticsearch.common.settings.Settings;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.DisposableBean;
import org.springframework.beans.factory.FactoryBean;
import org.springframework.beans.factory.InitializingBean;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Configuration;

@Configuration
public class ElasticsearchConfiguration implements FactoryBean<TransportClient>, InitializingBean, DisposableBean {
	private static final Logger logger = LoggerFactory.getLogger(ElasticsearchConfiguration.class);

	@Value("${spring.data.elasticsearch.cluster-nodes}")
	private String clusterNodes;

	private TransportClient transportClient;
	private PreBuiltTransportClient preBuiltTransportClient;

	@Override
	public void destroy() throws Exception {
		try {
			logger.info("Closing elasticSearch client");
			if (transportClient != null) {
				transportClient.close();
			}
		} catch (final Exception e) {
			logger.error("Error closing ElasticSearch client: ", e);
		}
	}

	@Override
	public TransportClient getObject() throws Exception {
		return transportClient;
	}

	@Override
	public Class<TransportClient> getObjectType() {
		return TransportClient.class;
	}

	@Override
	public boolean isSingleton() {
		return false;
	}

	@Override
	public void afterPropertiesSet() throws Exception {
		buildClient();
	}

	protected void buildClient() {
		try {
			preBuiltTransportClient = new PreBuiltTransportClient(settings());

			String InetSocket[] = clusterNodes.split(":");
			String address = InetSocket[0];
			Integer port = Integer.valueOf(InetSocket[1]);
			transportClient = preBuiltTransportClient.addTransportAddress(new InetSocketTransportAddress(InetAddress.getByName(address), port));

		} catch (UnknownHostException e) {
			logger.error(e.getMessage());
		}
	}

	/**
	 * 初始化默认的client
	 */
	private Settings settings() {
//		Settings settings = Settings.builder().put("cluster.name", clusterName).put("client.transport.sniff", true).build();
		Settings settings = Settings.builder().put("cluster.name", "elasticsearch").build();
		return settings;
	}
}
{% endhighlight %}

* RestController

{% highlight java %}

import org.elasticsearch.action.get.GetResponse;
import org.elasticsearch.client.transport.TransportClient;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/api/search")
public class SearchRestController {

	@Autowired
	private TransportClient client;

	@RequestMapping(value = "/client/{articleId}")
	public GetResponse test(@PathVariable String articleId) {
		GetResponse response = client.prepareGet("information", "article", articleId).get();
		return response;
	}
}
{% endhighlight %}
