```xml
<model>
    <configuration>
        <title>People</title>
        <titleOne>Person</titleOne>
        <icon>fa fa-user</icon>
    </configuration>

    <fields>
        <!-- Personal information -->
        <field type="text" name="name" title="Name" col="4"/>
        <field type="text" name="surname" title="Surname" col="4"/>
        <field type="date" name="birth_date" title="Date Of Birth" col="4"/>

        <!-- Relations -->
        <field type="relation" name="partner_id" title="partner"
               relation_on="App\Models\Person" relation_name="full_name"
               order_by="name" relation_id="partner_id"/>
        <field type="tags" name="parents" title="Parents"
               relation_on="App\Models\Person" relation_name="full_name"
               order_by="name" max="2"/>
        <field type="tags" name="pets" title="Pets"
               relation_on="App\Models\Pet" relation_name="name"
               order_by="name"/>
        <field type="select" name="gender" title="Gender"
               options="config:options.gender"/>

        <!-- General -->
        <field type="select" name="active" title="Active"
               options="config:options.boolean" value="1"/>

        <!-- Dynamic -->
        <field type="dynamic" name="partner_name" title="Partner"/>
        <field type="dynamic" name="parent_names" title="Parents"/>
        <field type="dynamic" name="pet_names" title="Parents"/>
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
            <field name="birth_date"/>
            <field name="gender"/>
            <field name="partner_name"/>
            <field name="parent_names"/>
            <field name="pet_names"/>
            <field name="active"/>
        </list>

        <list name="add">
            <field name="name"/>
            <field name="surname"/>
            <field name="birth_date"/>
            <field name="gender"/>
            <field name="partner_id"/>
            <field name="parents"/>
            <field name="pets"/>
            <field name="active"/>
        </list>

        <list name="edit">
            <field name="name"/>
            <field name="surname"/>
            <field name="birth_date"/>
            <field name="gender"/>
            <field name="partner_id"/>
            <field name="parents"/>
            <field name="pets"/>
            <field name="active"/>
        </list>
    </lists>
</model>
```
