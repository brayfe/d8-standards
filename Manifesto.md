> The following page provides a highly opinionated prescription for building a Drupal 8 distribution. It is designed to be a straw man to be disputed, contradicted, ridiculed, and subsequently revised – in effect, a _[reculer pour mieux sauter](http://www.bartleby.com/81/14132.html)_ – which will eventually grow into a blueprint for an actual distribution design document.

## Composer

1.  The distribution repository should not include any built artifacts of Drupal core, contributed projects, or PHP libraries.
2.  The distribution repository should contain a `composer.json` file in the root  of the repository that will compose explicitly identified versions of:
    1.  Drupal Core, specifically Pantheon's "[drops-8](https://github.com/pantheon-systems/drops-8)" distribution for support of the Pantheon-based UT QuickSites and CMS Hosting Platform services
    2.  All contributed projects
        1.  All contrib patches should be managed in a `patches` subdirectory of the repository root and applied via the "scripts" method during `composer install`
    3.  All PHP libraries
    4. Custom standalone projects, as defined below
3. The "Distribution Kernel" -- meaning custom projects intrinsic to the functionality provided by the distribution -- must be directly included in the repository (e.g., `web/profiles/<install-profile-name>/modules`), rather than via the `composer.json`. Examples include the installation profile, "foundational" node content types defined by the distribution maintainers, Paragraphs types used by those node content types, user roles, and text format settings.
4. "Custom standalone" projects not intrinsic to the functionality provided by the distribution must be included via VCS. Examples include custom text format filters, layout editing tools, self-contained display elements (e.g., Announcements), Drupal overrides, analytics solutions, and 404 handling.

### Visualisation of 1 - 4
| Component | Use Composer | Don't use Composer |
| ------------- | ------------- | ------------- |
| Core | X | |
| Contrib | X | |
| Libraries | X  | |
| Distribution Kernel | | X |
| Custom Standalone | X | |

5.  The Drupal docroot must be located in a `web` subdirectory of the repository.
6.  A customized `settings.php` file should be committed directly to the repository to overwrite the default `settings.php` provided as part of Drupal Core.
7.  The distribution must be distributable to developers as-is (for a Composer-based workflow). Developers will not be prevented from using Composer for adding their own packages if desired; they would then need to manually maintain their own version of the repository root's `composer.json` file
8. Non-functional example files will be provided for `.gitignore`, `circle.yml`, `pantheon.yml`, and `composer.json` as part of the distribution.

**Reasoning:**

*   Using Composer for Drupal core and contributed modules reduces repository size and enforces the rule that core should not be hacked
*   Using Composer for contributed module patching ensures that patches will be applied, or will fail during the build process, prompting the developer to revisit the patch before committing contributed module changes.
*   Defining the explict version of core, contributed projects, and PHP libraries ensures that maintainers will have an idempotent built codebase when testing.
*   Distribution-intrinsic custom functionality should be included in the repository (as opposed to separate git repositories) (A) to avoid a situation where a single functional change requires pull requests on multiple repositories and (B) because there is  no compelling advantage to keeping custom code that will singly be used in the distribution somewhere other than the distribution's repository.
*   Defining the `.gitignore`, `circle.yml`, `pantheon.yml`, and `composer.json` files as example files allows the individual developer to use Composer himself/herself, while adding minimal work for the distribution maintainers (i.e., scripted conversion of example files to real files, and saving those changes back to the example files).

## Configuration Management

1.  No configuration should ever be written to the `sites/default/config` directory.
1.  All configuration relating to custom projects (modules/themes) must be defined in the modules' own `config/install` directory, and those modules should include a `features.yml` file that registers the configuration in Features.
    1.  Updates to configuration provided by custom projects should be deployed by adding a database update hook that does a `features-revert` on specific features. Update hooks should never use `features-revert-all` to avoid reverting features that individual developers have added to their own code.
1.  Configuration relating to general site defaults (site information) must be done in the installation profile via a method such as described here [https://drupal.stackexchange.com/questions/238189/setting-site-name-in-installation-profile](https://drupal.stackexchange.com/questions/238189/setting-site-name-in-installation-profile)

**Reasoning:**

*   Configuration Management, in the sense of what is generated via a `config-export` and stored in `sites/default/config`, is designed to be a complete representation in code of the site's live configuration. As such, it is a tool for individual sites, rather than for distributions.

## Content

1.  Content elements that will only always ever have a discrete number of instances on the site must be defined as plugin blocks. Examples of this include the sitewide social links, footer links, and sitewide announcement.
2.  Content elements that will never be displayed as a standalone page (read: it doesn't have a direct URL) must be defined as custom block types. Examples of this include Contact Info, Social Links, UT Newsreels, and Twitter Widgets.
3.  Content elements that are primarily intended for display on a single page must be defined as fields. Examples include Hero Photo, Flex Content Area, and WYSIWYG fields.
4.  Compound fields must be defined as [Paragraph](https://www.drupal.org/project/paragraphs) types, and added to content types as Paragraph entity references.

**Reasoning:**

*   The Paragraphs contributed modules is emerging as the _sine qua non_ for complex field combinations that work in conjunction with each other.

## Layout & Content Placement

1.  The distribution must provide one or more tools for arranging content on pages using the core `field_layout` paradigm, which limits such arrangement and layout to the "content" region of the given page. Such tools may include Panels, Display Suite, and/or a custom layout solution designed to provide a "Panels Lite" user experience.
2.  Such tools must allow placement of both resuable and single-use content.
3.  Such tools must be compatible with the Drupal core QuickEdit tool.
4.  The layouts which such tools generate must be revisionable, just as the content they contain is revisionable.
5.  Such tools must be compatible with moderated workflow.
6.  Such tools must depend on core or other modules to supply layouts, and must provide a UI to switch between these externally-supplied layouts.

**Reasoning:**

*   The `field_layout` approach to content placement is (a) in Core, (b) adopted by Panels and Display Suite and (c) avoids the pitfalls of UTDK 7.x's dependence on manipulating regions at the page level.

## Custom Functionality

1.  All custom functionality provided in the distribution must NOT rely on a custom theme for any of its baseline rendering. In other words, all custom functionality should be minimally usable with any Drupal theme.
2.  All tests for custom functionality should be written without dependence on a given theme. Example: a Behat test for a Contact Info block should not check that the block header text is a color defined by a given theme; it should check that the html structure defined by the component's template implementation is present.
3.  All tests for custom functionality should be written without dependence on a given installation profile. Example: a Behat test for a custom text format filter should not save a node of a content type that is defined by the installation profile, but rather should save a "basic page" or "article."
4.  All tests for custom functionality should be written without dependence on other custom functionality. Example: a Behat test for the Qualtrics text format filter should not assume the presence of a Responsive Tables text format filter.

## Theme

1.  The distribution should provide a custom theme which can be sub-themed.
1.  The custom theme will be dependent on the Bootstrap grid system.
1.  The custom theme should follow atomic design principles.
1.  The custom theme should be based on an application-agnostic, canonical styleguide which generates the application-specific theme implementation.
1.  The distribution repository should not include compiled CSS. This should be performed as a part of the `composer install` process.
1.  All tests for the theme should be done using visual regression comparison.

## The Ten Commandments of a Drupal 8 Distribution

> This section is intended to be a concise, high-level distillation of the above. It could be presented to stakeholders as a smoke-test for buy-in.

1.  The distribution shall be composed using Composer and distributed as a built application with a `web` subdirectory docroot.
2.  Updates to the distribution shall be applied by downloading the updated codebase and running database updates.
3.  The individual developer/site shall be responsible for using Configuration Management in a manner s/he sees fit.
4.  Reusable content shall be defined as Drupal blocks.
5.  Page-specific content shall be defined as Drupal fields; complex fields shall be defined as Paragraphs types.
6.  A layout tool shall be included which will allow content builders to choose between multiple page layouts and assign both reusable content and page-specific content on a per-page basis.
7.  A branded theme shall be provided that may be sub-themed.
8.  This branded theme shall follow principles of atomic design, and shall be constructed from an application-agnostic styleguide.
9.  Addition, configuration, and management of contributed modules shall be the responsibility of the individual developer, except for contributed modules that are dependencies to the distribution's custom functionality.
10.  Issues, feature requests, and in-progress changes shall be directly accessible by individual developers via Enterprise Github.
