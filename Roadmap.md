# D7 to D8 Functionality Roadmap

> This page proposes a transitional roadmap from our Drupal 7 distribution to a not-yet-built Drupal 8 distribution, based on our latest findings.

## Custom Functionality

**Summary:**
* Replace 7 custom modules with contributed module approximations
* Remove 4 custom modules
* Replace all custom compound field modules with Paragraph type configuration


| Drupal 7 component  | D8 Equivalent | Notes |
| ------------- | ------------- | ------------- |
| utexas_announcement | (contrib) Bootstrap Site Alert  | https://www.drupal.org/project/bootstrap_site_alert|
| utexas_google_tag_manager | (contrib) google_analytics  | |
| utexas_google_cse | (contrib) google_cse  | https://www.drupal.org/project/google_cse|
| utexas_page_builder | (contrib) layout_per_node | In Alpha|
| utexas_tablesaw_filter  | (contrib) responsive_tables_filter  | |
| utlogin | (contrib) simplesamlphp_auth  | Minimally tested in D8|
| utexas_twitter_widget | (contrib) twitter_profile_widget  | |
| utexas_fonts  | Convert to library  | |
| Custom compound fields  | Convert to paragraphs types | |
| utexas_contact_info | Custom block type | |
| utexas_social_accounts  | Custom block type | |
| Roles (configuration) | Custom features | |
| utexas_admin  | Custom module | Much of the existing code will be irrelevant in D8|
| utexas_beacon | Custom module | |
| utexas_qualtrics_filter | Custom module | In Alpha|
| utexas_saml_auth_helper | Custom module | |
| utexas_utdk_deploy  | Custom module | Still needed to differentiate QS & UTDK|
| content_type_team_member  | Custom node type  | |
| content_type_standard_page  | Custom node type  | |
| utexas_event  | Custom node type & view | |
| utexas_news | Custom node type & view | |
| utexas_social_sharing | Custom plugin block | |
| utexas_devel  | Remove  | |
| utexas_menu | Remove  | |
| utexas_navigation404  | Remove  | |
| content_type_landing_page | Remove in favor of single general-purpose page type | |

## Contributed Functionality (QS-only)

**Summary:**
* Of the 51 contributed modules listed below, this proposes removing 24.

| Drupal 7 component  | D8 Equivalent | Notes |
| ------------- | ------------- | ------------- |
admin_views | remove | obsolete
calendar | remove | Not yet available/obsolete |
ckeditor | remove | In Core |
ckeditor_link | Replace with linkit | Linkit seems more comprehensive than editor_advanced_link or ckeditor_entity_link |
ckeditor_link_file | Replace with linkit | |
context | remove | obsolete for custom functionality |
context_field | remove | obsolete |
ctools | keep | Apparently needed by path_auto |
ctr | remove | obsolete for custom functionality |
date | remove | In Core |
diff | keep | Still needed for comparing revisions |
draggableviews | keep | Needs to be tested |
entity | remove | In Core |
entity_view_mode | remove | In Core |
entityreference | remove | In Core
features | keep
features_extra | remove | obsolete |
feeds | remove | obsolete for custom functionality |
field_collection | replace with paragraphs | |
fieldblock | remove | obsolete |
file_entity | remove | obsolete |
globalredirect | merged into redirect | |
job_scheduler | remove | obsolete |
libraries | remove | obsolete
link | remove | In Core |
media | keep | (Hopefully will be in Core) |
media_ckeditor | unknown | |
media_youtube | unknown | |
menu_block | keep | |
metatag | keep | |
module_filter | remove | In Core |
node_clone | unknown | 8 version still in development |
path_auto | keep | |
pathologic | unknown | 8 version still in development |
readonlymode | keep | RC status currently |
redirect | keep | |
role_delegation | keep | Alpha status currently |
simplesamlphp_auth | keep | |
smartcrop | remove | Use Responsive Images (core) |
taxonomy_access_fix | remove | obsolete |
term_merge | remove | no equivalent |
token | keep | |
transliteration | remove | no equivalent |
underscore | remove | handled by libraries |
userprotect | keep | |
video_filter | keep | |
view_unpublished | keep | In Alpha currently |
views | remove | In Core |
views_bulk_operations | keep | |
workbench | keep | |
xml_sitemap | keep | |

