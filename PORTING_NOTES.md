# Office Hours Module - Porting from Drupal 7 to Backdrop CMS

## Overview
This document outlines the changes made to port the Office Hours module from Drupal 7 to Backdrop CMS.

## Key Changes Made

### 1. Info File Updates (office_hours.info)
- Removed Drupal 7 specific `core = 7.x` and replaced with `backdrop = 1.x`
- Removed date and date_api dependencies (not required in Backdrop)
- Added `type = module` directive
- Removed Drupal.org packaging metadata
- Simplified the file format

### 2. Module File Updates (office_hours.module)
- Updated `hook_theme()` to use `backdrop_get_path()` instead of `drupal_get_path()`
- Changed `hook_theme()` signature to not require `$existing`, `$type`, `$theme` parameters
- Updated attached asset paths to use `backdrop_get_path()`
- Maintained field hooks which are compatible with Backdrop

### 3. Include File Updates
- **office_hours.formatter.inc**: Updated JavaScript attachment path to use `backdrop_get_path()`
- **office_hours.feeds.inc**: Added note about potential feeds integration work needed

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

### Function Replacements
| Drupal 7 | Backdrop CMS |
|----------|--------------|
| `drupal_get_path()` | `backdrop_get_path()` |
| `drupal_get_library()` | `backdrop_get_library()` |

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

## Known Issues / Future Work

1. **Feeds Integration**: The `office_hours.feeds.inc` file may require additional work if Feeds module is used.
2. **Entity Metadata**: The module references `entity_metadata_field_verbatim_get` which may need verification in Backdrop.
3. **Date API**: Dependencies on date module removed; verify if needed for specific functionality.

## Installation Steps

1. Ensure the module is in `/modules/office_hours/`
2. Clear Drupal cache
3. Go to `/admin/modules`
4. Enable "Office Hours" module
5. Go to Structure > Content Types > Add field
6. Select "Office hours" as field type
7. Configure as needed

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

