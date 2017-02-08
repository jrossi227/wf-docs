## Adding Tags

To add tags to one or more entities:

1. Open an entity browser by selecting **Browse \> \<entity\>**, where **\<entity\>** is **Alerts**, **Dashboards**, **Events**, or **Sources**.
1. Choose which entities to tag. Do one of:
    - Check the checkboxes next to the entities and click the **+ Tag** button.
    - Click **+tag** below an entity.
1. In the Add Tag dialog, do one of:
    - Optionally type a tag name to filter the list of tags. Click the tag.
    - Click the **Create Tag** button at the bottom:
    1. Type a tag name. Tag names are case sensitive. For example, the tags **MyApp** and **myapp** are stored as distinct tags.
    1. Click **Add**.

## Searching for Tags
To search for a tag, type in the Search box below the Tags heading in the filter bar at the left of an entity browser. As you type in the Search box, the list of tags below is filtered by the search string. When you search for tags, the search process is case insensitive. For example, searching for the tag **myapp** returns **MyApp** and **myapp**. Similarly, searching for the tag **MyApp** returns **MyApp** and **myapp**.

## Filtering by Tags
To filter by a tag, click a tag icon ![agents tag](images/agents_tag.png#inline) in the filter bar or below an entity.

## Tag Paths
All tag types support the ability to organize tags in a hierarchy. The hierarchy is defined by separating tag components with a dot **'.'**. For example, **MyService.MyApp**.

### Searching Tag Paths
Trailing wildcards ".\*" are supported when searching tag paths. For example, to match all tags starting with **alertTagPath.**, enter **alertTagPath.\***. This string matches alerts named **alertTagPath.tpc1**, **alertTagPath.tpc1.tpc11**, etc. When creating maintenance windows you can use tag paths and wildcards to put a group of of alerts in maintenance.
