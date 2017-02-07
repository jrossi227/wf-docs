A *tag* is custom metadata that provides application-specific meaning to metrics and Wavefront entities such as alerts, dashboards, events, and sources. Tags group together metrics and entities according to categories you define.

The primary use of tags is to limit the number metrics and entities you are querying or working with at once. Limiting the number of metrics reduces query time. Limiting the number of entities reduces information overload.

In queries, Wavefront supports filtering metrics with **source** and **point** tags and filtering events with **alert** and **event** tags. In the Wavefront UI and API you can specify alert, dashboard, event, and source tags to filter those entities in their respective browsers and during API invocations. In the Wavefront UI, tags display as gray labeled icons ![agents tag](images/agents_tag.png#inline) in the filter bar and below each entity in the entity browser.
