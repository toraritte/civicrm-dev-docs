# Extension Structure

## Extension Files
The [civix](/extensions/civix.md) command line tool will generate the following structure
for you:

-   ***info.xml*** is a manifest that describes your extension – the
    name, license, version number, etc. You should edit most information
    in this file. The information contained in this file will also be used if published on civicrm.org
-   ***myextension.php*** stores source code for all your hooks. It
    includes a few default hook implementations which will make
    development easier. You can add and remove hooks as you wish. (Note:
    This file name is different in each module – it is based the
    module's *[short-name](/extensions/index.md#extension-names)*.)
-   ***myextension.civix.php*** contains auto-generated helper
    functions. These deal with common problems like registering your
    module in the template include-path. `civix` may automatically
    overwrite this file, so generally do not edit it.

## Extension Directory Structure

In addition, civix creates some empty directories. These directories are
reminiscent of the directory structure in CiviCRM core:

-   ***CRM/Myextension/*** stores PHP class files. Classes in this
    folder should be prefixed with "CRM\_Myextension\_"
-   ***templates/*** stores Smarty templates.
-   ***xml/*** stores XML configuration files (such as URL routes and schema xml)
-   ***build/*** stores exportable .zip files

When adding files into these directories it is advisable to follow similar patterns to that in CiviCRM Core
e.g. BAO files should go in "CRM\_Myextension\BAO\", likewise with Form and Page. This ensures that for developers
that seek to modify or improve the extension files can be found in standard locations.

## The Big `E`

There are many times when you need to reference something from the
extension -- e.g.  its name, its file-path, or its translated messages.

For example, this code displays a translated message (using the translation
data for this extension):

```php
if ($welcoming) {
  CRM_Core_Session::setStatus(ts('Hello world!', array(
    'domain' => 'org.civicrm.myextension',
  )));
} else {
  CRM_Core_Session::setStatus(ts('Goodbye cruel world!', array(
    'domain' => 'org.civicrm.myextension',
  )));
}
```

Repeatedly entering the name of the extension is a bit tiresome.

For code generated by `civix` v17.08.0+ (or suitably
[upgraded](https://github.com/totten/civix/blob/master/UPGRADE.md)), `civix`
includes the `E` helper which provides easier access:

```php
use CRM_Mymodule_ExtensionUtil as E;

if ($welcoming) {
  CRM_Core_Session::setStatus(E::ts('Hello world!'));
} else {
  CRM_Core_Session::setStatus(E::ts('Goodbye cruel world!'));
}
```

Note that `E` is an alias.  It stands for "extension" -- as in "the current
extension that I'm writing".  It's a very thin class that provides small
helpers for looking up your extension's resources, e.g.

 * `E::ts($text)` -- Translate a string (using the extensions' translation file)
 * `E::path($file)` -- Get the path to a resource file (within this extension)
 * `E::url($file)` -- Get the URL to a resource file (within this extension)
 * `E::findClass($suffix)` -- Get the full name of a class (within this extension)
 * `E::LONG_NAME` -- The full length key for this extension (eg `org.example.myextension`)
 * `E::SHORT_NAME` -- The abbreviated key for this extension (eg `myextension`)
