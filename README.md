# VPM Related Posts

*VPM Related Posts* is a WordPress plugin for simple, no-frills related posts. Pass an ID and get a set of related posts. It's that easy.

A few caveats/tidbits you should be aware of:

+   Related posts are cached as transients for 48 hours (by default).
+   Relations are calculated using MySQL regexp, to prevent needing to reindex your post content as MySQL FULLTEXT. This means results are likely to be—at best—mediocre. MySQL wizards are welcomed to submit pull requests that improve this functionality; my attempt is a quick hack.
+   Due to the use of namespaces, PHP 5.3 is *required*.
+   Because it's developer oriented, this plugin will _probably_ never make it into the WordPress Repository. I suggest using Composer to add this dependency to your project.

## Usage

The namespaced function `\VanPattenMedia\relatedPosts( $postID )` will return an array of `WP_Post` objects.

```php
<?php
$relatedPosts = \VanPattenMedia\relatedPosts( get_the_id() );

if ( is_array( $relatedPosts ) && ! empty( $relatedPosts ) ) {
	foreach( $relatedPosts as $relatedPost ) {
		echo '<ul>';
		echo '<li><a href="' . get_the_permalink( $relatedPost->ID ) . '">' . $relatedPost->post_title . '</a></li>';
		echo '</ul>';
	}
}
```

## Filters

There are a number of filters provided to make it easier to adjust the query and results.

Filter Name                             | Description
----------------------------------------|-----------------------------------------------------------------------------------------------
`vpm_related_posts_stopwords`           | An array of stopwords that will be filtered from all searches
`vpm_related_posts_contentprepared`     | A tag-stripped, lower-cased version of your content, before filtering stopwords
`vpm_related_posts_regexfilter`         | The regex string that will be passed into `preg_replace` to filter unneeded text/characters
`vpm_related_posts_searchregexp`        | The regexp we'll pass to `$wpdb->prepare` to get our list of related posts
`vpm_related_posts_searchcount`         | An int of the number of posts to return. Default is 5
`vpm_related_posts_wpdbquery`           | The full query that will be passed into `$wpdb->prepare`
`vpm_related_posts_transientexpiration` | An int representing the transient expiration time in seconds. Default is 2 days/172800 seconds

## License

**Copyright (c) 2015 [Van Patten Media Inc.](https://www.vanpattenmedia.com/) All rights reserved.**

Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:

*   Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.
*   Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.
*   Neither the name of the organization nor the names of its contributors may be used to endorse or promote products derived from this software without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
