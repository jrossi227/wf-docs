Wavefront is a high performance streaming analytics platform designed for monitoring/optimization.  The service is unique in its ability to scale to very high data ingestion rates and query loads. The result is a technology stack unique in its ability to scale horizontally while providing access to all of the granular (not aggregated) data collected for all time.

The major components of Wavefront include the *Wavefront SaaS application*, which facilitates economies of scale for deployment, flexibility, and time to value and the Wavefront Proxy.  The *Wavefront Proxy* is the interface to collector *agents*, which instrument hardware and software applications. The Wavefront application can also collect metrics directly from external metrics services such as those provided by Amazon Web Services. The diagram below depicts each of these components.

![Wavefront architecture](images/wavefront_architecture.png)

It's possible to have several data centers from different locations feeding data into Wavefront versus traditional on premise solutions which can only provide a view of one location at a time.  At a high level, the setup process typically consists of configuring collectors to send data to one or more Wavefront Proxies and then configuring the Wavefront Proxies to forward this data to the Wavefront application. The following sections describe these components in more detail.
