# Voorbeeld Documentatie voor het huidige AO CMS

### type="tags"
### Module::FormTags()

### Options
- title
- name
- value
- tags
- config_tags
- groupable
- free_entries
- hide_label
- placeholder = "Voeg een tag toe \| false"
- uri = "api/complete-tags"
- class = "form-control"
- readonly = "false"
- where = "status:!=:1 \| status:1"
- order_by = "name"
- show_first = "1,3,2"
- prefix_by = "parent_id"
- prefix_by_value = "null"
- pivot = '{"number": 1}'

### Location

File:
- [/src/FormFields/FormTags/FormTagsModule.php]

