```xml
<model>
	<configuration>
		<title>People</title>
		<titleOne>Person</titleOne>
		<icon>fa fa-user</icon>
	</configuration>

	<fields>
		<field type="text" name="first_name" title="First name"/>
		<field type="text" name="surname" title="Surname"/>
		<field type="text" name="email" title="E-mail address"/>
		<field type="text" name="phone" title="Phonenumber"/>

		<field type="tags" name="Parents" title="Parents"
				relation_on="App\Models\Person" relation_name="name"
				where="active:1" order_by="name"/>
				
		<field type="tags" name="Pets" title="Pets"
				relation_on="App\Models\Pets" relation_name="name"
				where="active:1" order_by="name"/>
		
		<field type="select" name="active" title="Active" 
				options="config:options.boolean" value="1"/>
		
	</fields>

	<lists>
		<list name="overview">
			<field name="first_name"/>
			<field name="surname"/>
			<field name="email"/>
			<field name="active"/>
		</list>
		
		<list name="add">
		
		<list>
	</lists>
</model>
```