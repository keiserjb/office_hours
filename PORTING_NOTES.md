# Office Hours Module - Porting from Drupal 7 to Backdrop CMS

## Overview
This document outlines the changes made to port the Office Hours module from Drupal 7 to Backdrop CMS.

## Key Changes Made

### 1. Info File Updates (office_hours.info)
- Removed Drupal 7 specific `core = 7.x` and replaced with `backdrop = 1.x`
- Added `type = module` directive
- Removed Drupal.org packaging metadata
- Date and date_api dependencies remain for functionality (date API is used in widget)

### 2. Module File Updates (office_hours.module)
- Updated `hook_theme()` to use `backdrop_get_path()` instead of `drupal_get_path()`
- Changed `hook_theme()` signature to not require `$existing`, `$type`, `$theme` parameters
- Updated attached asset paths to use `backdrop_get_path()`
- Maintained field hooks which are compatible with Backdrop

### 3. Include File Updates
- **office_hours.formatter.inc**: Updated `drupal_alter()` → `backdrop_alter()`, updated variable_get references
- **office_hours.theme.inc**: Updated `drupal_strlen()`, `drupal_add_js()`, `drupal_html_id()`, `drupal_render()` to Backdrop equivalents
- **office_hours.field.inc**: Converted all `drupal_map_assoc()` → `backdrop_map_assoc()`
- **office_hours.elements.inc**: Updated `drupal_array_get_nested_value()` → `backdrop_array_get_nested_value()`
- **office_hours.widget.inc**: Converted `drupal_map_assoc()` → `backdrop_map_assoc()`
- **office_hours.install**: Converted `variable_get/set/del()` → `state_get/set/delete()`
- **office_hours_handler_filter_hours.inc**: Converted `drupal_map_assoc()` → `backdrop_map_assoc()`
- **office_hours.api.php**: Converted `drupal_map_assoc()` → `backdrop_map_assoc()`

### 4. Removed/Disabled Features
- Feeds integration file is still present but may need additional porting
- Views API hooks should work as Views module in Backdrop is similar to Drupal 7

## API Compatibility

### Compatible Hooks
The following hooks are compatible between Drupal 7 and Backdrop CMS:
- `hook_field_info()` ✓
- `hook_field_widget_info()` ✓
- `hook_field_widget_form()` ✓
- `hook_field_formatter_info()` ✓
- `hook_field_formatter_view()` ✓
- `hook_element_info()` ✓
- `hook_field_schema()` ✓
- `hook_views_api()` ✓
- `hook_field_views_data_alter()` ✓
- `hook_field_create_field()` ✓ (new in porting)

### Function Replacements

| Drupal 7 | Backdrop CMS |
|----------|--------------|
| `drupal_get_path()` | `backdrop_get_path()` |
| `drupal_get_library()` | `backdrop_get_library()` |
| `drupal_alter()` | `backdrop_alter()` |
| `drupal_strlen()` | `backdrop_strlen()` |
| `drupal_add_js()` | `backdrop_add_js()` |
| `drupal_html_id()` | `backdrop_html_id()` |
| `drupal_render()` | `render()` |
| `drupal_array_get_nested_value()` | `backdrop_array_get_nested_value()` |
| `drupal_map_assoc()` | `backdrop_map_assoc()` |
| `variable_get()` | `config_get()` or `state_get()` |
| `variable_set()` | `config_set()` or `state_set()` |
| `variable_del()` | `config_delete()` or `state_delete()` |

### Configuration Management
Note: Database schema remains compatible. Field storage in Backdrop works the same way as Drupal 7 for this module.

## Testing Checklist

- [ ] Module enables without errors
- [ ] Field type appears in field creation form
- [ ] Field widget displays correctly in content edit forms
- [ ] Field values save correctly to database
- [ ] Field formatter displays correctly on content pages
- [ ] Views integration works (if Views module is enabled)
- [ ] Test with different time formats (12h/24h)
- [ ] Test with different day formats (short/long)
- [ ] Test compress option
- [ ] Test multi-hour support

## Major Fixes Applied

1. **Field Cardinality**: Added `hook_field_create_field()` to force office_hours fields to unlimited cardinality on creation.
2. **Field UI Integration**: Modified form_alter hooks to hide and enforce unlimited cardinality in the field settings UI.
3. **Update Hook**: Added `office_hours_update_1000()` to fix existing fields that were created with cardinality 1.
4. **Form Validation**: Updated validation functions to properly handle empty time blocks.
5. **Comments**: Removed all Drupal 7 and Drupal 8 specific comments, replacing with Backdrop-appropriate documentation.
6. **Function API Updates**: Converted all remaining Drupal functions to their Backdrop equivalents.

## Known Issues / Future Work

1. **Feeds Integration**: The `office_hours.feeds.inc` file may require additional work if Feeds module is used.
2. **Entity Metadata**: The module references `entity_metadata_field_verbatim_get` which is compatible with Backdrop.
3. **Multiple Blocks Per Day**: The module supports the concept but some UI refinements may be beneficial.

## Installation Steps

1. Ensure the module is in `/modules/office_hours/`
2. Clear Backdrop cache at `/admin/config/development/performance`
3. Go to `/admin/modules`
4. Enable "Office Hours" module
5. Run updates (if upgrading from earlier versions): `/core/update.php`
6. Go to Structure > Content Types > Add field
7. Select "Office hours" as field type
8. The field will automatically be configured with unlimited cardinality
9. Configure display and other settings as needed

## Module Files

- `office_hours.module` - Main module file
- `office_hours.info` - Module metadata
- `office_hours.install` - Installation and schema
- `office_hours.api.php` - API documentation
- `office_hours.views.inc` - Views integration
- `office_hours.feeds.inc` - Feeds integration (optional)
- `includes/office_hours.field.inc` - Field definition
- `includes/office_hours.widget.inc` - Widget implementation
- `includes/office_hours.formatter.inc` - Field formatter
- `includes/office_hours.elements.inc` - Form element definitions
- `includes/office_hours.theme.inc` - Theme functions
- `includes/office_hours_handler_filter_*.inc` - Views filter handlers
- `js/office_hours.widget.js` - Widget JavaScript
- `js/office_hours.formatter.js` - Formatter JavaScript
- `office_hours.css` - Stylesheet

## Support

For Backdrop CMS specific issues or questions, refer to:
- [Backdrop CMS Documentation](https://docs.backdropcms.org/)
- [Backdrop CMS Community Forum](https://forum.backdropcms.org/)
