Wavefront is a high performance streaming analytics platform that has been designed for monitoring/optimization in the data center. The service is unique in its ability to scale to very high data ingestion rates and query loads. The result is a technology stack unique in its ability to scale horizontally to the largest data center needs while providing access to all of the granular (not aggregated) data collected for all time.

The major components of Wavefront include the Wavefront SaaS application, which facilitates economies of scale for deployment, flexibility, and time to value, the Wavefront <a href="#" data-toggle="tooltip" data-original-title="{{site.data.glossary.proxy}}">Proxy</a>, and collector agents (Wavefront or customer provided) which instrument hardware and software applications. The diagram below depicts each of these components.

![Wavefront architecture](images/wavefront_architecture.png)

It's possible to have several data centers from different locations feeding data into Wavefront versus traditional on premise solutions which can only provide a view of one location at a time. At a high level, the setup process typically consists of configuring an agent behind the firewall to accept the sensor data and then forwarding this data to Wavefront.
