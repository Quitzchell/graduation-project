```xml
<model>
    <configuration>
        <title>People</title>
        <titleOne>Person</titleOne>
        <icon>fa fa-users</icon>
    </configuration>

    <fields>
        <!-- Personal information -->
        <field type="text" name="name" title="First Name" col="4"/>
        <field type="text" name="surname" title="Surname" col="4"/>
        <field type="date" name="date_of_birth" title="Birth Date" col="4"/>
        <field type="date" name="date_of_death" title="Death Date" col="4"/>
        <field type="select" name="gender" title="Gender"
               options="config:options.gender"/>

        <!-- Relations -->
        <field type="custom_tags" name="relationships" title="Relationships"
               reverse_relation="reverseRelationships" relation_on="App\Models\Person"
               exclude_self="true" relation_name="full_name" order_by="name"/>
        <field type="custom_tags" name="parents" title="Parents"
               relation_on="App\Models\Person" exclude_self="true"
               relation_name="full_name" order_by="name" max="2"/>

        <!-- General -->
        <field type="select" name="active" title="Active"
               options="config:options.boolean" value="1"/>

        <!-- Dynamic -->
        <field type="dynamic" name="relationship_names" title="Relationships"/>
        <field type="dynamic" name="parent_names" title="Parents"/>
    </fields>

    <lists>
        <list name="overview">
            <field name="name"/>
            <field name="surname"/>
            <field name="active"/>
        </list>

        <list name="info">
            <field name="name"/>
            <field name="surname"/>
            <field name="date_of_birth"/>
            <field name="gender"/>
            <field name="relationship_names"/>
            <field name="parent_names"/>
            <field name="active"/>
        </list>

        <list name="add">
            <field name="name"/>
            <field name="surname"/>
            <field name="date_of_birth"/>
            <field name="gender"/>
            <field name="relationships"/>
            <field name="parents"/>

            <field name="active"/>
        </list>

        <list name="edit">
            <field name="name"/>
            <field name="surname"/>
            <field name="date_of_birth"/>
            <field name="gender"/>
            <field name="relationships"/>
            <field name="parents"/>

            <field name="active"/>
        </list>
    </lists>
</model>
```
