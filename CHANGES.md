# Spotless releases

### Version 3.0.0-SNAPSHOT - TBD ([javadoc](https://diffplug.github.io/spotless/javadoc/snapshot/), [snapshot](https://oss.sonatype.org/content/repositories/snapshots/com/diffplug/gradle/spotless/spotless/))

* Big push towards supporting incremental build.  Requires a lot of work for every FormatterStep to properly serialize its state, so this might take a little while.

### Version 2.4.0 - November 1st 2016 ([javadoc](https://diffplug.github.io/spotless/javadoc/2.4.0/), [jcenter](https://bintray.com/diffplug/opensource/spotless/2.4.0/view))

* If a formatter step throws an `Error` or any of its subclasses, such as the `AssertionError`s thrown by JUnit, AssertJ, etc. that error will kill the build ([#46](https://github.com/diffplug/spotless/issues/46))
	+ This allows custom rules like this:

```gradle
custom 'no swearing', {
	if (it.toLowerCase().contains('fubar')) {
		throw new AssertionError('No swearing!');
	}
}
```

### Version 2.3.0 - October 27th 2016 ([javadoc](https://diffplug.github.io/spotless/javadoc/2.3.0/), [jcenter](https://bintray.com/diffplug/opensource/spotless/2.3.0/view))

* When `spotlessCheck` fails, the error message now contains a short diff of what is neccessary to fix the issue ([#10](https://github.com/diffplug/spotless/issues/10), thanks to Jonathan Bluett-Duncan).
* Added a [padded-cell mode](https://github.com/diffplug/spotless/blob/master/PADDEDCELL.md) which allows spotless to band-aid over misbehaving rules, and generate error reports for these rules (See [#37](https://github.com/diffplug/spotless/issues/37) for an example).
* Character encoding is now configurable (spotless-global or format-by-format).
* Line endings were previously only spotless-global, they now also support format-by-format.
* Upgraded eclipse formatter from 4.6.0 to 4.6.1

### Version 2.2.0 - October 7th 2016 ([javadoc](https://diffplug.github.io/spotless/javadoc/2.2.0/), [jcenter](https://bintray.com/diffplug/opensource/spotless/2.2.0/view))

* Added support for [google-java-format](https://github.com/google/google-java-format).

```
spotless {
	java {
		googleJavaFormat()  // googleJavaFormat('1.1') to specify a specific version
	}
}
```

### Version 2.1.0 - October 7th 2016 ([javadoc](https://diffplug.github.io/spotless/javadoc/2.1.0/), [jcenter](https://bintray.com/diffplug/opensource/spotless/2.1.0/view))

* Added the method `FormatExtension::customLazyGroovy` which fixes the Groovy closure problem.

### Version 2.0.0 - August 16th 2016 ([javadoc](https://diffplug.github.io/spotless/javadoc/2.0.0/), [jcenter](https://bintray.com/diffplug/opensource/spotless/2.0.0/view))

* `spotlessApply` now writes out a file only if it needs to be changed (big performance improvement).
* Java import sorting now removes duplicate imports.
* Eclipse formatter now warns if the formatter xml contains multiple profiles.
* Updated eclipse formatter to Eclipse Neon (4.6).
* BREAKING CHANGE: Eclipse formatter now formats javadoc comments.
	+ You might want to look at the following settings in your `spotless.eclipseformat.xml`:

```
org.eclipse.jdt.core.formatter.join_lines_in_comments=true/false
org.eclipse.jdt.core.formatter.comment.format_javadoc_comments=true/false
org.eclipse.jdt.core.formatter.comment.format_line_comments=true/false
org.eclipse.jdt.core.formatter.comment.format_block_comments=true/false
```

The most important breaking change of 2.0 is the new default line ending mode, `GIT_ATTRIBUTES`.  This line ending mode copies git's behavior exactly.  This change should require no intervention from users, and should be significantly easier to adopt for users who are already using `.gitattributes` or the `core.eol` property.

If you aren't using git, you can still use `.gitattributes` files for fine-grained control of line endings.  If no git information is found, it behaves the same as PLATFORM_NATIVE (the old default).

Below is the algorithm used by git and spotless to determine the proper line ending for a file.  As soon as a step succeeds in finding a line ending, the other steps will not take place.

1. If the code is a git repository, look in the `$GIT_DIR/info/attributes` file for the `eol` attribute.
2. Look at the `.gitattributes` in the file's directory, going up the directory tree.
3. Look at the global `.gitattributes` file, if any.
4. Look at the `core.eol` property in the git config (looking first at repo-specific, then user-specific, then system-specific).
5. Use the PLATFORM_NATIVE line ending.

### Version 1.3.3 - March 10th 2016 ([jcenter](https://bintray.com/diffplug/opensource/spotless/1.3.3/view))

* Upgraded Eclipse formatter to 4.5.2, which fixes [37 bugs](https://bugs.eclipse.org/bugs/buglist.cgi?query_format=advanced&resolution=FIXED&short_desc=%5Bformatter%5D&short_desc_type=allwordssubstr&target_milestone=4.5.1&target_milestone=4.5.2) compared to the previous 4.5.0.
* If you have been using `custom 'Lambda fix', { it.replace('} )', '})').replace('} ,', '},') }`, you don't need it anymore.

### Version 1.3.2 - December 17th 2015 ([jcenter](https://bintray.com/diffplug/opensource/spotless/1.3.2/view))

* Spotless no longer clobbers package-info.java, fixes [#1](https://github.com/diffplug/spotless/issues/1).
* Added some infrastructure which allows `FormatterStep`s to peek at the filename if they really need to.

### Version 1.3.1 - September 22nd 2015 ([jcenter](https://bintray.com/diffplug/opensource/spotless/1.3.1/view))

* Bumped the FreshMark dependency to 1.3.0, because it offers improved error reporting.

### Version 1.3.0 - September 22nd 2015 ([jcenter](https://bintray.com/diffplug/opensource/spotless/1.3.0/view))

* Added native support for FreshMark.

### Version 1.2.0 - August 25th 2015 ([jcenter](https://bintray.com/diffplug/opensource/spotless/1.2.0/view))

* Updated from Eclipse 4.5 M6 to the official Eclipse 4.5 release, which fixes several bugs in the formatter.
* Fixed a bug in the import sorter which made it impossible to deal with "all unmatched type imports".
* Formatting is now relative to the project directory rather than the root directory.
* Improved the logging levels.

### Version 1.1 - May 14th 2015 ([jcenter](https://bintray.com/diffplug/opensource/spotless/1.1/view))

* No functional changes, probably not worth the time for an upgrade.
* First version which is available on plugins.gradle.org as well as jcenter.
* Removed some code that was copy-pasted from Durian, and added a Durian dependency.

### Version 1.0 - April 29th 2015 ([jcenter](https://bintray.com/diffplug/opensource/spotless/1.0/view))

* Initial release.

### Version 0.1 - April 28th 2015 ([jcenter](https://bintray.com/diffplug/opensource/spotless/0.1/view))

* First release, to test out that we can release to jcenter and whatnot.
