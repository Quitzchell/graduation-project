# Voorbeeld CMS schema voor een AllesOnline object

```xml
<model>
    <configuration>
        <title>Blogposts</title>
        <titleOne>Blogpost</titleOne>
        <icon>fa fa-newspaper-o</icon>
    </configuration>

    <fields>
        <!-- Dynamic fields -->
        <field type="dynamic" name="category_name" title="Category"/>

        <!-- General fields-->
        <field type="text" name="title" title="Title"/>
        <field type="textarea" name="excerpt" title="Excerpt"/>
        <field type="media-item" name="image" title="Image"/>
        <field type="relation" name="category_id" title="Category"
               relation_on="App\Models\Category" relation_name="name"/>
        <field type="date" name="published_at" title="Published at"/>
        <field type="select" name="published" title="Published" options="config:options.boolean"/>
        <field type="blocks" name="blocks" title="Content" blocks="common/*"/>
    </fields>

    <lists>
        <list name="default">
            <field name="title"/>
            <field name="excerpt"/>
        </list>
        <list name="add">
            <field name="title"/>
            <field name="excerpt"/>
            <field name="image"/>
            <field name="category_id"/>
            <field name="published_at"/>
            <field name="published"/>
            <field name="blocks"/>
        </list>
        <list name="edit">
            <field name="title"/>
            <field name="excerpt"/>
            <field name="image"/>
            <field name="category_id"/>
            <field name="published_at"/>
            <field name="published"/>
            <field name="blocks"/>
        </list>
        <list name="info">
            <field name="title"/>
            <field name="excerpt"/>
            <field name="category_name"/>
            <field name="published_at"/>
            <field name="published"/>
        </list>
        <list name="overview">
            <field name="title"/>
            <field name="category_name"/>
        </list>
        <list name="category_relation">
            <field name="title"/>
            <field name="category_name"/>
            <field name="published_at"/>
            <field name="published"/>
        </list>
    </lists>
</model>
```
