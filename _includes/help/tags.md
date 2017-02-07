A tag is custom metadata that provides application-specific meaning to metrics and Wavefront entities such as alerts, dashboards, events, and sources. Tags group together metrics and entities according to categories you define.

The primary use of tags is to limit the number metrics and entities you are querying or working with at once. Limiting the number of metrics reduces query time. Limiting the number of entities reduces information overload.

In queries, Wavefront supports filtering metrics with **source** and **point** tags and filtering events with **alert** and **event** tags. In the Wavefront UI and API you can specify alert, dashboard, event, and source tags to filter those entities in their respective browsers and during API invocations. In the Wavefront UI, tags display as gray labeled icons ![agents tag](images/agents_tag.png#inline) in the filter bar and below each entity in the entity browser.

To add tags to one or more entities:

1. To add tags to one or more entities:
1. Open an entity browser by selecting **Browse \> \<entity\>**, where **\<entity\>** is **Alerts**, **Dashboards**, **Events**, or **Sources**.
1. Choose which entities to tag:
    - Check the checkboxes next to the entities and click the **+ Tag** button.
    - Click **+tag** below an entity.
1. In the Add Tag dialog:
    - Optionally type a tag name to filter the list of tags. Click the tag.
    - Click the Create Tag button at the bottom:
    1. Type a tag name. Tag names are case sensitive. For example, the tags **MyApp** and **myapp** are stored as distinct tags.
    1. Click **Add**.

To search for a tag, type in the Search box below the Tags heading in the filter bar at the left of an entity browser. As you type in the Search box, the list of tags below is filtered by the search string. When you search for tags, the search process is case insensitive. For example, searching for the tag **myapp** returns **MyApp** and **myapp**. Similarly, searching for the tag **MyApp** returns **MyApp** and **myapp**.

To filter by a tag, click a tag icon ![agents tag](images/agents_tag.png#inline) in the filter bar or below an entity.
